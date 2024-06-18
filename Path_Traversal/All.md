# Introduction 

La traversée de répertoires, également connue sous le nom de path traversal, est une vulnérabilité de sécurité web qui permet à un attaquant d'accéder à des fichiers et répertoires situés en dehors du répertoire racine de l'application web. Cette vulnérabilité survient lorsque les entrées utilisateur ne sont pas correctement validées, permettant à un attaquant de manipuler les chemins de fichiers.

***Comment Fonctionne la Traversée de Répertoires?***
Le but de la traversée de répertoires est de naviguer dans les répertoires parent en utilisant des séquences de chemin comme ../ . Voici un exemple simple :

Requête Normale :

URL : http://example.com/showfile?filename=report.pdf
Le serveur récupère et affiche le fichier report.pdf situé dans le répertoire autorisé.
Requête Malveillante :

URL : http://example.com/showfile?filename=../../../../etc/passwd
Le serveur, ne validant pas correctement le chemin, accède au fichier passwd situé dans le répertoire /etc.