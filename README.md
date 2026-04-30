# workshop-dvwa-docker_Georges_Florian
Saphii : Florian Georges

Schéma projet : 
<img width="1536" height="1024" alt="SchémaTP_docker" src="https://github.com/user-attachments/assets/ce853a18-c3b3-447d-ab91-c8557765f07b" />


Partie 1 – Déploiement
Mise en place de DVWA + MySQL avec Docker Compose
Accès via : http://localhost:8081

Partie 2 – Réseau
Création d’un réseau Docker isolé
MySQL non accessible depuis l’extérieur
Communication interne via le service db

Partie 3 – Reverse Proxy
Ajout de NGINX
Accès via : http://localhost:8080
DVWA non exposé directement

Partie 4 – Test de vulnérabilité

SQL Injection testée avec : 1 (dans <img width="1492" height="847" alt="image" src="https://github.com/user-attachments/assets/98ece23a-480d-4985-bcc7-ad52ca569f89" />

Résultat :

*Accès aux données utilisateurs

*Contournement de la logique de la requête


QUESTIONS : 

1-Pourquoi cette attaque fonctionne ?

Cette attaque fonctionne car l’application ne filtre pas les entrées utilisateur.
La valeur saisie est directement injectée dans la requête SQL, ce qui permet de modifier sa logique.
Ici, la condition '1'='1' est toujours vraie, donc la base de données retourne tous les résultats.

2-Quel est le rôle de la base de données dans cette faille ?

La base de données exécute la requête SQL telle qu’elle est reçue.
Elle ne vérifie pas si la requête est légitime, elle se contente de l’exécuter.
Dans ce cas, elle retourne des données sensibles (utilisateurs) à cause de la requête modifiée.

3-Docker protège-t-il contre ce type de vulnérabilité ?

Non, Docker ne protège pas contre les vulnérabilités applicatives.
Docker isole les services et sécurise l’infrastructure, mais ne corrige pas les failles dans le code.
Une application vulnérable reste exploitable même dans un conteneur Docker.


## Bonus

### phpMyAdmin

phpMyAdmin permet d’accéder au conteneur MySQL via le réseau Docker.  
Dans ce lab, la base `dvwa` est visible depuis phpMyAdmin.

Cependant, l’image `vulnerables/web-dvwa` démarre également un service MySQL/MariaDB interne, comme on peut le voir dans les logs (`Starting mysql`).  
Cela signifie que DVWA peut utiliser sa propre base interne au lieu de celle du conteneur `db`.

C’est pourquoi la base `dvwa` visible dans phpMyAdmin peut rester vide, même après l’initialisation via DVWA.

Ce comportement montre l’importance de bien configurer et vérifier les variables de connexion entre les services dans une architecture Docker.




