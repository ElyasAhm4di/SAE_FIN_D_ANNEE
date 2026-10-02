Viking Transport
Application web de réservation et de gestion d'un réseau de transport
Projet réalisé dans le cadre des SAE 4, 5 et 6 du BUT Informatique, année universitaire 2025-2026.
L'objectif du projet est de concevoir et développer une application web pour Viking Transport, un réseau de cars régionaux normands. L'application permet aux voyageurs de consulter le réseau, rechercher un trajet, réserver un voyage et gérer leur compte. Elle fournit également un espace d'administration destiné au suivi des clients, des lignes et des statistiques.
Le projet a été développé par l'équipe DeviK, dans une démarche collaborative de conception, développement, gestion de base de données et organisation de projet.
Sommaire
- Présentation
- Objectifs
- Fonctionnalités
- Équipe
- Technologies
- Architecture du projet
- Base de données
- Installation
- Configuration de la connexion Oracle
- Lancement de l'application
- Utilisation
- Espace administrateur
- Organisation du développement
- Sécurité et bonnes pratiques
- Contexte pédagogique
- Limites
- Auteurs
Présentation
Viking Transport est une application web destinée à faciliter la consultation et la réservation de trajets en car à travers la Normandie.
L'application distingue principalement deux types d'utilisateurs :
- les voyageurs, qui peuvent consulter le réseau et effectuer des réservations ;
- les administrateurs, qui disposent d'outils de gestion et de statistiques.
L'application s'appuie sur une base de données Oracle contenant notamment les départements, communes, lignes, clients, tarifs, réductions, réservations, étapes et nœuds du réseau.
Objectifs
Le projet répond aux objectifs suivants :
- concevoir et exploiter une base de données relationnelle ;
- connecter une application web PHP à une base Oracle ;
- permettre la consultation des lignes et horaires ;
- proposer une recherche de trajet ;
- gérer les réservations ;
- gérer les comptes clients ;
- mettre en place un espace d'administration ;
- produire des statistiques à partir des données de réservation ;
- proposer une interface claire et cohérente ;
- organiser le développement au sein d'une équipe ;
- utiliser Git pour le suivi et la collaboration sur le projet.
Le projet s'inscrit dans le cadre des SAE 4, 5 et 6 du BUT Informatique et mobilise des compétences en développement, bases de données, gestion de projet et travail en équipe.
Fonctionnalités
Fonctionnalités accessibles aux visiteurs
Un utilisateur non connecté peut notamment :
- consulter la page d'accueil ;
- consulter les lignes du réseau ;
- consulter les horaires ;
- consulter les tarifs ;
- consulter la carte du réseau ;
- rechercher un trajet ;
- créer un compte ;
- effectuer une réservation.
Gestion du compte client
Un client peut :
- se connecter à son compte ;
- consulter son profil ;
- consulter ses informations personnelles ;
- consulter son historique de réservations ;
- effectuer une nouvelle réservation ;
- se déconnecter.
Recherche et réservation
La recherche de trajet permet de sélectionner notamment :
- une commune de départ ;
- une commune d'arrivée ;
- une date ;
- une ligne ou un itinéraire correspondant.
L'application exploite les données de la base afin de déterminer les informations nécessaires au trajet et à sa réservation.
Le système de réservation prend également en compte les informations tarifaires et le système de points associé aux clients.
Carte interactive
La page de carte utilise Leaflet et OpenStreetMap afin de représenter visuellement le réseau et ses itinéraires.
Elle permet de consulter les informations liées aux lignes et aux communes directement depuis une représentation cartographique.
Espace administrateur
L'application dispose d'un espace dédié à l'administration du réseau.
Tableau de bord
Le tableau de bord permet d'accéder aux principales fonctions d'administration :
- gestion des clients ;
- gestion des lignes ;
- statistiques.
Gestion des clients
L'administrateur peut :
- consulter la liste des clients ;
- modifier les informations d'un client ;
- supprimer un compte client lorsque cela est autorisé par l'application.
Gestion des lignes
L'administration permet notamment de :
- consulter les lignes ;
- modifier les informations d'une ligne ;
- consulter les arrêts associés ;
- ajouter ou modifier des horaires ;
- supprimer des horaires ;
- supprimer certains arrêts.
Statistiques
Le module de statistiques fournit plusieurs indicateurs issus de la base de données :
- meilleurs clients ;
- lignes les plus utilisées ;
- réservations par période ;
- chiffre d'affaires ;
- clients disposant du plus grand nombre de points ;
- heures de pointe ;
- trajets populaires.
Équipe
DeviK
Le projet a été réalisé par les huit membres suivants :
Membre
AHMADI Mohammad Elyas
CHAIGNON Nathan
CHUQUET Anaël
COLLET Léo
CONSTANTIN Thomas
GRENECHE Mathéo
GUILBERT Joan
PRENVEILLE Noé


L'équipe a choisi une organisation collaborative et polyvalente. Les fonctionnalités ont été réparties en fonction des compétences, des préférences et des besoins du projet, sans imposer une séparation rigide des rôles.
Technologies
Backend
- PHP
- PDO
- PDO_OCI pour la connexion à Oracle
- Sessions PHP pour la gestion des utilisateurs connectés
Frontend
- HTML5
- CSS3
- JavaScript
- Bootstrap 5.3
- Leaflet 1.9.4 pour la cartographie
Base de données
- Oracle Database
- SQL
- Modèle relationnel fourni et adapté dans le cadre de la SAE
Cartographie
- Leaflet
- OpenStreetMap
Gestion du code
- Git
- Développement collaboratif avec l'environnement de travail utilisé par l'équipe
Architecture du projet
La structure principale du projet est organisée de la manière suivante :
SAE_FIN_D_ANNEE-main/
│
├── assets/
│   ├── equipe.png
│   ├── logo_blanc.png
│   └── map.png
│
├── bdd/
│   ├── BddAdminClientUtils.php
│   ├── BddAdminLigneUtils.php
│   ├── BddAdminStatsUtils.php
│   ├── BddClientUtils.php
│   ├── BddConnexionUtils.php
│   ├── BddLigneUtils.php
│   ├── BddTrajetUtils.php
│   ├── BddUtils.php
│   ├── Inscription_utils.php
│   ├── LigneUtils.php
│   ├── reserverutils.php
│   ├── env.php
│   ├── ideeRequete.txt
│   └── testBdd.php
│
├── css/
│   └── style.css
│
├── includes/
│   ├── footer.php
│   ├── head.php
│   ├── jsIncludes.php
│   └── topbar.php
│
├── js/
│   └── script.js
│
├── admin_client.php
├── admin_dashboard.php
├── admin_modif_ligne.php
├── admin_modifier_client.php
├── admin_stats.php
├── action_supprimer_client.php
├── carte.php
├── connexion.php
├── conditions.php
├── deconnexion.php
├── equipe_devik.php
├── equipe_viking.php
├── horaires.php
├── index.php
├── inscription.php
├── lignes.php
├── mentions.php
├── profil.php
├── reserver.php
├── tarifs.php
├── template.php
├── trajet.php
├── vik.sql
└── README.md
Organisation générale
Le projet sépare principalement :
- les pages et contrôleurs PHP à la racine ;
- les fonctions d'accès et de traitement des données dans bdd/ ;
- les éléments communs de l'interface dans includes/ ;
- les feuilles de style dans css/ ;
- les scripts JavaScript dans js/ ;
- les ressources graphiques dans assets/.
Cette organisation permet de limiter la duplication du code et de séparer les responsabilités principales de l'application.
Base de données
Le projet utilise une base Oracle.
Le script vik.sql contient la définition de la base de données utilisée par l'application.
Les principales tables sont :
- VIK_DEPARTEMENT
- VIK_TYPE_CLIENT
- VIK_TARIF
- VIK_COMMUNE
- VIK_LIGNE
- VIK_CLIENT
- VIK_REDUCTION
- VIK_RESERVATION
- VIK_ETAPE
- VIK_NOEUD
La base permet notamment de représenter :
- les départements et communes ;
- les lignes de transport ;
- les arrêts et les étapes ;
- les horaires ;
- les clients ;
- les réservations ;
- les tarifs ;
- les réductions ;
- le système de points.
Le fichier vik.sql doit être exécuté sur une base Oracle accessible par l'application avant le lancement du site.
Installation
Prérequis
Pour exécuter le projet, il faut disposer de :
- un serveur web capable d'exécuter PHP ;
- PHP avec le support PDO ;
- le pilote Oracle nécessaire à PDO, notamment PDO_OCI ;
- un accès à une base Oracle ;
- un navigateur web récent ;
- Git, si le projet est récupéré depuis un dépôt.
Récupération du projet
Cloner le dépôt :
git clone <URL_DU_DEPOT>
Puis entrer dans le dossier :
cd SAE_FIN_D_ANNEE-main
Le projet doit ensuite être placé dans le répertoire servi par le serveur web.
Configuration de la connexion Oracle
La connexion à la base de données est centralisée dans :
bdd/env.php
Ce fichier contient les paramètres de connexion utilisés par les différentes pages PHP.
La configuration suit le principe suivant :
$db_usernameOracle = "UTILISATEUR";
$db_passwordOracle = "MOT_DE_PASSE";
$dbOracle = "oci:dbname=SERVEUR:PORT/SERVICE;charset=AL32UTF8";
Les identifiants réels ne doivent pas être publiés dans un dépôt public.
Avant de lancer l'application, il faut donc :
1. renseigner l'utilisateur Oracle ;
2. renseigner le mot de passe Oracle ;
3. vérifier le nom du serveur Oracle ;
4. vérifier le port ;
5. vérifier le service Oracle ;
6. vérifier que l'extension PDO Oracle est disponible.
Le projet utilise une connexion PDO avec gestion des exceptions.
Initialisation de la base
Le fichier suivant contient le script SQL de création de la base :
bdd/../vik.sql
Après création de la base, il faut vérifier que les tables nécessaires sont présentes et accessibles par l'utilisateur Oracle utilisé par l'application.
Lancement de l'application
Une fois PHP, Oracle et la connexion configurés, il suffit de démarrer le serveur web.
Avec le serveur PHP intégré, selon la configuration de l'environnement :
php -S localhost:8000
Puis ouvrir :
http://localhost:8000
Si l'application est installée sur un serveur universitaire ou un autre serveur PHP, l'URL dépendra de la configuration de ce serveur.
Utilisation
Parcours visiteur
Un parcours classique peut être réalisé ainsi :
Accueil
   |
   +-- Consulter les lignes
   |
   +-- Consulter les horaires
   |
   +-- Consulter les tarifs
   |
   +-- Consulter la carte
   |
   +-- Rechercher un trajet
             |
             +-- Choisir le trajet
             |
             +-- Réserver
                    |
                    +-- Confirmation
Parcours client
Connexion
   |
   +-- Profil
   |     |
   |     +-- Informations personnelles
   |     +-- Historique des réservations
   |
   +-- Recherche d'un trajet
   |
   +-- Réservation
   |
   +-- Déconnexion
Parcours administrateur
Espace administrateur
   |
   +-- Tableau de bord
   |
   +-- Gestion des clients
   |     |
   |     +-- Consultation
   |     +-- Modification
   |     +-- Suppression
   |
   +-- Gestion des lignes
   |     |
   |     +-- Modification
   |     +-- Gestion des arrêts
   |     +-- Gestion des horaires
   |
   +-- Statistiques
         |
         +-- Clients
         +-- Lignes
         +-- Réservations
         +-- Chiffre d'affaires
         +-- Points
         +-- Heures de pointe
         +-- Trajets populaires
Organisation du développement
Le développement a été réalisé dans une logique collaborative.
L'équipe s'est notamment appuyée sur :
- Git pour le versionnage ;
- un dépôt commun pour centraliser le projet ;
- des outils de collaboration dans l'environnement de développement ;
- une organisation des fonctionnalités en fonction des compétences disponibles ;
- une préparation préalable de la structure du projet et de l'interface.
La démarche de travail visait à maintenir une structure commune afin de réduire les conflits lors du développement simultané de plusieurs fonctionnalités.
Sécurité et bonnes pratiques
Plusieurs éléments doivent être pris en compte lors du déploiement du projet.
Identifiants de base de données
Les identifiants Oracle ne doivent pas être exposés publiquement.
Le fichier de configuration contenant les informations de connexion doit être protégé et, idéalement, exclu du dépôt lorsque le projet est déployé dans un environnement public.
Requêtes SQL
Les requêtes utilisant des données fournies par l'utilisateur doivent privilégier les requêtes préparées lorsque cela est nécessaire afin de limiter les risques d'injection SQL.
Sessions
Les pages utilisant l'authentification s'appuient sur les sessions PHP afin de conserver l'état de connexion de l'utilisateur.
Déploiement
Pour un déploiement réel, il serait nécessaire de renforcer notamment :
- la gestion des mots de passe ;
- la validation côté serveur ;
- la protection CSRF des formulaires sensibles ;
- la gestion des droits administrateur ;
- la configuration HTTPS ;
- la protection des fichiers de configuration ;
- la gestion centralisée des erreurs.
Contexte pédagogique
Ce projet correspond aux SAE 4, 5 et 6 du BUT Informatique.
La SAE avait notamment pour objectifs de réaliser et exploiter une base de données, d'interagir avec une application web, de prendre en compte la sécurité, de suivre une démarche de gestion de projet et d'organiser le travail d'une équipe informatique.
Le projet a donc été conçu comme une mise en situation professionnelle autour d'un client fictif, Viking Transport, et d'une équipe de développement fictive, DeviK.
Limites
Le projet constitue une réalisation académique et ne doit pas être considéré comme une plateforme de réservation de transport prête à être exploitée commercialement sans travaux complémentaires.
Avant une mise en production, il faudrait notamment approfondir :
- la sécurité applicative ;
- la gestion réelle des paiements ;
- la robustesse de l'authentification ;
- la gestion des mots de passe ;
- la protection des données personnelles ;
- la gestion des erreurs et des logs ;
- les tests automatisés ;
- la validation complète des entrées utilisateur ;
- la configuration de production du serveur et de la base de données.
Auteurs
Équipe DeviK
AHMADI Mohammad Elyas
CHAIGNON Nathan
CHUQUET Anaël
COLLET Léo
CONSTANTIN Thomas
GRENECHE Mathéo
GUILBERT Joan
PRENVEILLE Noé
Projet
Nom du projet : Viking Transport
Équipe : DeviK
Formation : BUT Informatique
Établissement : IUT de l'Université de Caen Normandie
Année universitaire : 2025-2026
Une requête, des solutions.
