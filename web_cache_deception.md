# Cours : Comprendre et Exploiter le Web Cache Deception (WCD)

---

## 1. Introduction au concept

Le **Web Cache Deception** est une vulnérabilité qui survient lorsqu'il y a une **discrépance** (une différence d'interprétation) entre la manière dont un **serveur de cache** et un **serveur d'origine** traitent une URL.

L'objectif de l'attaquant est de tromper le cache pour qu'il **stocke une réponse contenant des données sensibles** (clé API, profil utilisateur, jeton CSRF, etc.) dans un emplacement considéré comme public.

---

## 2. Les 3 piliers de la vulnérabilité

Pour qu'une attaque WCD réussisse, trois conditions doivent être réunies :

### 1️⃣ Une discrépance dans le mapping d'URL  
Les deux serveurs ne lisent pas l'URL de la même façon.

### 2️⃣ Une règle de cache basée sur l'URL  
Le cache utilise des **extensions** (`.js`, `.css`, `.png`) ou des **préfixes** (`/resources`, `/static`) pour décider quoi stocker.

### 3️⃣ Une page sensible accessible  
Le serveur d'origine renvoie des données privées même si l'URL est légèrement modifiée.

---

# 3. Analyse des techniques par niveau (Étude de cas des Labs)

---

## A. Niveau Débutant : La permissivité du chemin (Path Mapping)

### 🔎 Mécanisme
Le serveur d'origine ignore tout ce qui suit le chemin légitime.

### 🛠 Technique
Ajouter simplement une extension à l'URL :



### 🧠 Interprétation

**Origine :**  
> "Je connais `/my-account`, je sers la page."

**Cache :**  
> "Ça finit par `.js`, je stocke la page."

### 📌 Leçon
Ne jamais configurer un serveur pour qu'il accepte des segments de chemin arbitraires sur des endpoints sensibles.

---

## B. Niveau Intermédiaire : Les délimiteurs de chemin

### 🔎 Mécanisme
Utiliser des caractères spéciaux que le serveur d'origine voit comme des fins d'URL mais que le cache voit comme faisant partie du nom de fichier.

### 🛠 Technique

Exemple :

Caractères testés :


### 🧠 Interprétation

**Origine :**  
Le `;` coupe l'URL → il ne voit que `/my-account`.

**Cache :**  
Le `;` n'est pas un délimiteur → il voit l'extension `.js`.

### 📌 Leçon
Les listes de délimiteurs (`;`, `?`, `#`, `&`, etc.) doivent être synchronisées entre le cache et l'origine.

---

## C. Niveau Avancé : La normalisation (Conflit de nettoyage)

La **normalisation** est le processus de résolution des chemins relatifs comme : ../

---

### Cas 1 : Normalisation par l'Origine (Lab 3)

#### URL : 

/resources/..%2fmy-account

#### Comportement :

**Origine :**
- Décode `%2f` → `/`
- Résout `../`
- Sert `/my-account`

**Cache :**
- Ne décode pas
- Voit le préfixe `/resources/`
- Stocke la réponse

---

### Cas 2 : Normalisation par le Cache (Lab 4)

#### URL :

/my-account%23%2f%2e%2e%2fresources


#### Comportement :

**Origine :**
- S'arrête au délimiteur `%23` (`#`)
- Sert `/my-account`

**Cache :**
- Normalise tout
- Résout les `..`
- Croit que l'URL finale pointe vers `/resources`
- Stocke la réponse

---

# 4. Méthodologie d'audit (Checklist)

## ✅ Identifier les pages sensibles
Chercher :
- Clés API
- Emails
- Jetons CSRF
- Informations de profil

---

## ✅ Tester la permissivité

Ajouter :
/abc
;abc


Si la page charge toujours (**200 OK**) → risque potentiel.

---

## ✅ Détecter les règles de cache

Chercher :
- Dossiers : `/resources`, `/static`
- Extensions : `.js`, `.css`
- Header : `X-Cache: hit`

---

## ✅ Tester les délimiteurs

Utiliser une liste :
? ; # %23 & %26


Observer lesquels sont ignorés par le serveur d'origine.

---

## ✅ Tester la normalisation

Utiliser :
..%2f
..%252f
../


Observer comment les serveurs remontent dans les dossiers.

---

# 5. Mesures de remédiation (Défense)

## 🔐 1. Header Cache-Control

Utiliser :
Cache-Control: no-store
Cache-Control: private


Sur toutes les pages contenant des données sensibles.

---

## 🔄 2. Synchronisation

S'assurer que :

- Le cache et le serveur d'origine utilisent les mêmes règles de normalisation
- Les mêmes délimiteurs sont interprétés de la même manière

---

## 🚫 3. Strict Mapping

Configurer le serveur d'origine pour renvoyer :
404 Not Found


Si l'URL ne correspond pas exactement au chemin attendu.

---

## 📦 4. Vérification du Type

Le cache devrait vérifier que :

- Le `Content-Type` correspond à l'extension demandée  
- Ne pas stocker un `text/html` si l'URL finit par `.js`

---

# 🎯 Conclusion

Le Web Cache Deception repose toujours sur une **désynchronisation entre cache et origine**.

Corriger cette vulnérabilité revient à :

- Harmoniser les règles
- Rendre le mapping strict
- Empêcher le stockage de contenu sensible
- Configurer correctement les headers de cache

