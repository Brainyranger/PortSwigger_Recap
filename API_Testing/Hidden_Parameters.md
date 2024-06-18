# Introduction 

Les paramètres cachés dans une API sont des paramètres qui ne sont pas documentés publiquement mais peuvent exister et influencer le comportement de l'API.

##  Analyser le Trafic Réseau

Utiliser des Outils d'Analyse :

Intercepteurs HTTP: Utilisez des outils comme Postman, Fiddler, ou les DevTools des navigateurs pour inspecter les requêtes et les réponses HTTP. Vous pourriez voir des paramètres supplémentaires utilisés par les applications clientes.

Requêtes Authentifiées: Observez les requêtes provenant d'utilisateurs authentifiés ou ayant des rôles spéciaux pour voir si des paramètres supplémentaires sont utilisés.

##  Expérimenter et Tester

Test de Variantes :

Paramètres Communément Utilisés: Essayez d'ajouter des paramètres couramment utilisés (comme debug, verbose, test) pour voir s'ils produisent des effets.
Exploration de Valeurs: Modifiez les valeurs de paramètres existants pour voir si des comportements non documentés se manifestent.