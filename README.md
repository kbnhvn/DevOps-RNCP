# DATA-AIR

## Présentation
La qualité de l'air est une préoccupation croissante pour la santé publique. Disposer d'un outil permettant de visualiser les données de qualité de l'air par région, ville ou polluant offre une valeur ajoutée significative pour le grand public et les décideurs. 

Cette application a été créée dans le cadre du **projet Fil Rouge** de la formation **Bootcamp Ingénieur DevOps** de **DATASCIENTEST**. Elle se base sur le sujet FastAPI Traefik. Elle est conçue pour fournir une solution intégrée et facile d'accès pour le suivi et la visualisation des données de qualité de l'air.

L'architecture de l'application repose sur une série de micro-services, chacun jouant un rôle spécifique dans le traitement, la gestion et la visualisation des données. Ces services comprennent :

- Traefik comme edge router, simplifiant le routage des requêtes HTTP et HTTPS vers les services internes.
- Une API *web* pour l'ajout de nouveaux utilisateurs et l'authentification.
- Une base de données Postgres pour stocker les informations des utilisateurs, avec un service pgAdmin pour la gestion de cette base.
- Le service data-air, qui récupère les données de pollution de l'air depuis une API externe pour les indexer dans une base de données Elasticsearch.
- Kibana offre une interface utilisateur pour la visualisation de ces données.
- Un serveur web Nginx pour la landing page et la page de connexion, améliorant l'accès utilisateur.

Pour maximiser la disponibilité, la performance et la scalabilité de l'application, elle est déployée sur un cluster K3s. K3s offre un environnement Kubernetes léger et facile à déployer, idéal pour des applications conteneurisées comme celle-ci. Ce choix de déploiement assure une gestion efficace des ressources et une haute disponibilité des services.

## Cahier des charges :
https://1drv.ms/w/s!ArK3W0SU4bw29gz5Em1yGjNG07H8?e=N2dZ83

## Images Docker
### Images docker créées à partir du projet initial :
- kbnhvn/web-dev : https://hub.docker.com/r/kbnhvn/web-dev
- kbnhvn/web-prod : https://hub.docker.com/r/kbnhvn/web-prod

### Image docker du service de récupération des données :
- kbnhvn/datafetcher : https://hub.docker.com/r/kbnhvn/datafetcher

### Image docker du serveur web :
- kbnhvn/webserver : https://hub.docker.com/r/kbnhvn/webserver

## Infrastructure :
Cette application est déployée sur le **cloud AWS** au sein d'un **cluster EKS**.
Son déploiement est automatisé à l'aide de l'outil **Terraform** et comporte son propre pipeline de CI/CD. Pour une préocupation de clarté et de maintenabilité, j'ai fais le choix d'isoler l'aplicatif de l'infrastructure. Ainsi le code source relatif à l'infrastructure possède son propre repository GitHub : https://github.com/kbnhvn/Terraform-RNCP

## Déploiement continu :
Le déploiement continu de cette application utilise un workflow GitHub Actions, scindé en deux pipelines : 
- Un pipeline pour le déploiement en environnement de développement *dev*
- Un pipeline pour le déploiement en environnement de production *prod*
Ces deux pipelines permettent les étapes de **build/run/test/push** des images dockers des différents microservices, avant le déploiement de l'application sur leur environnement respectif avec **Helm**.
Ce déploiement comprend également l'ajout d'un environnement de supervision avec **Prometheus** et **Grafana**, ainsi que **cert-manager** pour la gestion des **certificats SSL**.

**Le déploiement en production nécéssite un étape de validation manuelle**

## Notes et instructions de dev :
### Pour commencer :
- Créer un dossier projet
- Cloner le repo dans ce dossier :
  ```https://github.com/kbnhvn/FastAPI-Traefik.git .```

### Répartition du travail :
Les différentes tâches et leur avancement sont visibles sur Jira : https://dst-fastapi-traefik.atlassian.net/jira/software/projects/KAN/boards/1
**Chaque tâche est liée à une branche** !

Pour afficher le nom de la branche liée à une tâche :
  - Cliquer sur la carte d'une tâche
  - Passer la souris sur ```1 branche``` sur le panneau de droite, le nom de la branche s'affiche

Remarques: 
  - Si la branche n'existe pas, il est possible de la créer via ce même menu (et choisir : Branch from : develop )
  - Eviter de travailler à plusieur sur la même branche pour éviter les conflits

- Vérifier sur quelle branche on se trouve: ```git status```
- Récupérer les branches: ```git fetch```
- Changer de branche: ```git checkout```

(Voir le module complémentaire Git/Github, notebook 3 : Github - First steps pour les principales commandes git)

### Lorsqu'une tâche liée à une branche est terminée :
Ouvrir une pull request :
**Vérifier la branche** (en haut à gauche de la capture d'écran), cliquer sur **contribute** puis **open pull request**

![Capture d'écran 2024-03-13 103535](https://github.com/kbnhvn/FastAPI-Traefik/assets/22301011/fc596a2d-7070-4404-bb11-64021c738e29)

S'assurer ensuite qu'il y ai bien **base: develop** en haut à gauche, et ajouter une description des tâches effectuées.
Ajouter les membres du groupe en **reviewer** en cliquant sur l'icône en haut à droite. 
Cliquer ensuite sur **Open pull request**

![Capture d'écran 2024-03-13 103608](https://github.com/kbnhvn/FastAPI-Traefik/assets/22301011/9e9658d3-8246-4f42-9a3c-fd4655250338)


### Règles de nommage des fichiers
Pour les manifests yaml :
- ```micro service```-```ressource``` par exemple web-service ou db-secret



