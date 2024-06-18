# Introduction  

L'assignation massive est une fonctionnalité de certains frameworks de développement web permettant d'assigner des valeurs à des propriétés d'objets ou de modèles à partir des données fournies par l'utilisateur en une seule opération.

***Qu'est-ce qu'une Vulnérabilité d'Assignation Massive?***

Une vulnérabilité d'assignation massive survient lorsque des paramètres non sécurisés peuvent être assignés à des propriétés d'un objet, permettant ainsi à un utilisateur malveillant de modifier des champs qu'il ne devrait pas pouvoir accéder ou changer. Par exemple, si un utilisateur peut définir des valeurs pour des champs sensibles comme isAdmin ou user_id à travers des formulaires ou des requêtes HTTP, cela peut entraîner des problèmes de sécurité majeurs.