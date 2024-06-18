# Introduction 

La pollution des paramètres côté serveur (SSPP) est une vulnérabilité de sécurité web où un attaquant envoie plusieurs paramètres identiques avec des valeurs différentes dans une requête HTTP, exploitant la manière dont le serveur web ou l'application gère ces paramètres. Cette technique peut amener le serveur à traiter ces paramètres de manière inattendue, entraînant des failles de sécurité potentielles.

***Comment Fonctionne la Pollution des Paramètres Côté Serveur?***

Lorsqu'un serveur reçoit plusieurs paramètres identiques dans une requête, il peut les traiter de différentes manières en fonction de sa configuration et du langage de programmation utilisé. Voici quelques comportements possibles :

Utiliser la Première Valeur : Certains serveurs ou frameworks prennent la première occurrence du paramètre.

Utiliser la Dernière Valeur : D'autres peuvent utiliser la dernière occurrence du paramètre.
Concaténer les Valeurs : Certains systèmes peuvent concaténer toutes les valeurs reçues.
Créer des Tableaux : Certains langages ou frameworks peuvent créer des tableaux contenant toutes les valeurs des paramètres.