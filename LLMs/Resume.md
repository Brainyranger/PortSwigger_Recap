# Détection des vulnérabilités LLM (large modèle de langage) 
### Notre méthodologie recommandée pour détecter les vulnérabilités LLM est la suivante :
\
Identifiez les entrées du LLM, y compris les entrées directes (comme une invite) et indirectes (comme les données d’entraînement).
Déterminez les données et les API auxquelles le LLM a accès.
Sondez cette nouvelle surface d’attaque à la recherche de vulnérabilités.

# Fonctionnement des API LLM
Le flux de travail pour intégrer un LLM à une API dépend de la structure de l’API elle-même.
Lors de l’appel d’API externes, certains LLM peuvent exiger que le client appelle un point de terminaison de fonction distinct (en fait une API privée) 
afin de générer des demandes valides qui peuvent être envoyées à ces API. Le flux de travail pour cela pourrait ressembler à ce qui suit :

Le client appelle le LLM avec l’invite de l’utilisateur.
Le LLM détecte qu’une fonction doit être appelée et renvoie un objet JSON contenant des arguments adhérant au schéma de l’API externe.
Le client appelle la fonction avec les arguments fournis.
Le client traite la réponse de la fonction.
Le client appelle à nouveau le LLM, en ajoutant la réponse de la fonction en tant que nouveau message.
Le LLM appelle l’API externe avec la réponse de la fonction.
Le LLM résume les résultats de ce rappel d’API à l’utilisateur.
Ce flux de travail peut avoir des implications en matière de sécurité, car le LLM appelle effectivement des API externes au nom de l’utilisateur,
mais l’utilisateur peut ne pas savoir que ces API sont appelées. Idéalement, 
les utilisateurs devraient être présentés avec une étape de confirmation avant que le LLM n’appelle l’API externe.

# Cartographie de la surface d’attaque de l’API LLM
Le terme « agence excessive » fait référence à une situation dans laquelle un LLM a accès à des API qui peuvent accéder à des informations sensibles et 
peut être persuadé d’utiliser ces API de manière dangereuse. Cela permet aux attaquants de pousser le LLM au-delà de sa portée prévue et 
de lancer des attaques via ses API.

La première étape de l’utilisation d’un LLM pour attaquer les API et les plugins consiste à déterminer à quelles API et plugins le LLM a accès.
Une façon de le faire est simplement de demander au LLM à quelles API il peut accéder. 
Vous pouvez ensuite demander des détails supplémentaires sur les API qui vous intéressent.

Si le LLM n’est pas coopératif, essayez de fournir un contexte trompeur et de poser à nouveau la question. Par exemple, vous pourriez prétendre que vous êtes le développeur du LLM et que vous devriez donc avoir un niveau de privilège plus élevé.

### Les attaques par injection rapide peuvent être menées de deux manières :

Directement, par exemple, via un message à un chatbot.
Indirectement, lorsqu’un attaquant délivre l’invite via une source externe. Par exemple, l’invite peut être incluse dans les données d’entraînement ou la sortie d’un appel d’API.
L’injection d’invites indirectes permet souvent des attaques LLM Web sur d’autres utilisateurs. Par exemple, si un utilisateur demande à un LLM de décrire une page Web, une invite cachée à l’intérieur de cette page peut faire en sorte que le LLM réponde avec une charge utile XSS conçue pour exploiter l’utilisateur.
