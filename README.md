# Viking Transport - Application web de réservation et d'administration

Projet réalisé par le **Groupe 3 - agence DeviK** dans le cadre de la SAÉ S2.04 / S2.05 / S2.06 du BUT Informatique (IUT Grand Ouest Normandie, année 2025-2026).

> DeviK - « Une requête, des solutions. »

---

## Sommaire

1. [Contexte](#1-contexte)
2. [Équipe](#2-équipe)
3. [Fonctionnalités](#3-fonctionnalités)
4. [Règles de gestion](#4-règles-de-gestion)
5. [Architecture technique](#5-architecture-technique)
6. [Modèle de données](#6-modèle-de-données)
7. [Algorithmes de recherche d'itinéraires](#7-algorithmes-de-recherche-ditinéraires)
8. [Installation et configuration](#8-installation-et-configuration)
9. [Déploiement sur le serveur de l'IUT](#9-déploiement-sur-le-serveur-de-liut)
10. [Organisation du travail](#10-organisation-du-travail)
11. [Limites connues et pistes d'amélioration](#11-limites-connues-et-pistes-damélioration)

---

## 1. Contexte

Viking Transport est un réseau de cars normands qui regroupe les transports régionaux non urbains. Chaque client peut réserver un voyage entre deux communes, en empruntant une ou plusieurs lignes.

Le sujet, rédigé par E. Porcq, demande de concevoir et d'exploiter une base de données, puis de développer en équipe une application web (HTML, PHP, CSS, JavaScript) répondant à un cahier des charges. Le projet se déroule sur trois jours consécutifs en fin de semestre, selon une démarche agile : choix de fonctionnalités, développement, démonstrations au client, rétrospectives avec le coach.

| Élément | Détail |
|---|---|
| Ressources concernées | S2.04 Exploitation d'une base de données, S2.05 Gestion d'un projet, S2.06 Organisation d'un travail d'équipe |
| Client fictif | Viking Transport |
| Agence | DeviK (Groupe 3) |
| Périmètre | 38 lignes (19 lignes, sens A et B), 81 communes, 9 départements normands et limitrophes |
| Livrables | Application web, base de données, démonstration évaluée par l'équipe de clients |

---

## 2. Équipe

Le groupe est composé de huit étudiants. Aucun rôle exclusif n'a été figé : les fonctionnalités ont été réparties selon les préférences et les compétences de chacun, et tous les membres ont participé au développement.

| Membre | Rôle principal |
|---|---|
| AHMADI Mohammad Elyas | Développement PHP et conception générale de l'application |
| CHAIGNON Nathan | Codéveloppement des modules et de l'architecture back-end |
| CHUQUET Anaël | Conception de l'architecture logicielle et intégration front-end |
| COLLET Léo | Identité visuelle et design graphique de l'interface |
| CONSTANTIN Thomas | Définition de l'architecture globale et structures back-end |
| GRENECHE Mathéo | Développement des vues front-end et maquettes fonctionnelles |
| GUILBERT Joan | Développement PHP, conception de modules, requêtes SQL complexes |
| PRENVEILLE Noé | Modélisation, conception du système d'information, structuration de la base de données |

Encadrement : E. Porcq (rédacteur du sujet, organisateur et tuteur).

---

## 3. Fonctionnalités

Le backlog compte 22 user stories réparties en 5 lots, traitées par ordre de priorité.

### Lot 1 - Client non inscrit

| Priorité | Fonctionnalité | Pages principales |
|---|---|---|
| 1 | Visualiser la liste des lignes | `lignes.php` |
| 2 | Visualiser les horaires d'une ligne | `horaires.php` |
| 3 | Réserver un voyage sur une ligne | `reserver.php` |
| 4 | Réserver un voyage sur une partie d'une ligne | `reserver.php` |
| 5 | Réserver un voyage sur plusieurs lignes | `reserver.php` |
| 6 | Réserver un voyage multi-lignes et connaître le tarif | `reserver.php`, `tarifs.php` |

### Lot 2 - Client fidélisé

| Priorité | Fonctionnalité | Pages principales |
|---|---|---|
| 7 | Se connecter à l'application | `connexion.php`, `deconnexion.php` |
| 8 | Créer un compte client | `inscription.php` |
| 9 | Consulter et modifier son compte, ses trajets et ses points | `profil.php` |
| 11 | Gagner des points de fidélité en réservant | `reserver.php` |
| 12 | Utiliser des points de fidélité pour réduire le prix | `reserver.php` |

### Lot 3 - Administrateur

| Priorité | Fonctionnalité | Pages principales |
|---|---|---|
| 13 | Visualiser les comptes clients et leurs réservations | `admin_client.php`, `admin_modifier_client.php` |
| 14 | Modifier ou supprimer un compte client | `admin_modifier_client.php`, `action_supprimer_client.php` |
| 15 | Identifier les comptes inactifs | `admin_client.php` |
| 16 | Consulter des statistiques | `admin_stats.php` |

### Lot 4 - Recherche d'itinéraires (client fidélisé)

| Priorité | Fonctionnalité | Pages principales |
|---|---|---|
| 17 | Trouver les trajets possibles entre deux communes | `trajet.php` |
| 18 | Trouver les trajets avec les horaires | `trajet.php` |
| 19 | Trouver le trajet le plus rapide | `trajet.php` |
| 20 | Trouver le trajet le plus court | `trajet.php` |
| 21 | Classer les trajets par distance ou par durée | `trajet.php` |

### Lot 5 - Administration du réseau

| Priorité | Fonctionnalité | Pages principales |
|---|---|---|
| 22 | Modifier les lignes et les horaires (ajout, modification, suppression d'arrêts et d'horaires) | `admin_modif_ligne.php` |

### Pages complémentaires

| Page | Rôle |
|---|---|
| `index.php` | Page d'accueil |
| `carte.php` | Carte interactive du réseau et sélection de trajets |
| `admin_dashboard.php` | Tableau de bord d'administration |
| `equipe_devik.php`, `equipe_viking.php` | Présentation de l'agence et du client |
| `conditions.php`, `mentions.php` | Conditions générales et mentions légales |

Les statistiques administrateur couvrent : meilleurs clients, lignes les plus utilisées, réservations par période, chiffre d'affaires total et par ligne, clients ayant le plus de points, heures de pointe et trajets les plus populaires.

---

## 4. Règles de gestion

### Tarification

Le prix de base d'un voyage dépend de la distance totale parcourue, selon 13 tranches (de 5 EUR pour 0 à 10 km jusqu'à 90 EUR pour 301 à 500 km), stockées dans la table `vik_tarif`. Le prix s'applique au voyage complet, quel que soit le nombre d'étapes.

### Niveaux de fidélité

Le niveau d'un client dépend de son nombre total de points cumulés et lui donne une réduction permanente sur le prix de ses réservations.

| Niveau | Seuil de points | Prix payé |
|---|---|---|
| Nouveau | 10 | 95 % |
| Poussin | 400 | 90 % |
| Junior | 3 000 | 80 % |
| Argent | 10 000 | 65 % |
| Or | 50 000 | 50 % |

### Points de fidélité

- Un voyage rapporte 1 point pour 10 km, uniquement des points entiers.
- Les points gagnés ne sont utilisables qu'à partir du voyage suivant.
- Les points peuvent être échangés contre une réduction en euros, selon la table `vik_reduction` :

| Points | Réduction |
|---|---|
| 100 | 1 EUR |
| 500 | 7 EUR |
| 1 000 | 15 EUR |

- L'application calcule la combinaison de paliers la plus avantageuse et ne consomme que le nombre de points nécessaire lorsque la réduction dépasse le prix du billet.

### Cycle de vie des comptes

| Situation | Conséquence |
|---|---|
| Plus d'un an sans connexion | Compte signalé comme inactif dans l'interface d'administration |
| Plus de deux ans sans connexion | Compte signalé comme à supprimer |
| Client non inscrit | Réservations rattachées au client numéro 0, sans point |

### Distinction entre étape et nœud

- Un **nœud** est un passage d'un car à un arrêt d'une commune, à une heure donnée, avec la distance et la durée jusqu'au nœud suivant. Il y a autant de nœuds que de passages de cars.
- Une **étape** est une portion de voyage effectuée par un client sur une ligne ; elle peut comporter plusieurs nœuds.
- Chaque commune ne possède qu'un seul arrêt.

---

## 5. Architecture technique

### Stack

| Couche | Technologies |
|---|---|
| Serveur | PHP 8 natif, sans framework, Apache |
| Accès aux données | PDO, requêtes préparées |
| Base de données | Oracle (driver PDO OCI) |
| Interface | HTML5, CSS3, JavaScript, Bootstrap 5.3 et Bootstrap Icons |
| Versionnement | Git (forge de l'université), branches par fonctionnalité |

### Arborescence

```
.
├── index.php, lignes.php, horaires.php, tarifs.php, carte.php
├── reserver.php, trajet.php
├── connexion.php, deconnexion.php, inscription.php, profil.php
├── admin_dashboard.php, admin_client.php, admin_modifier_client.php,
│   action_supprimer_client.php, admin_stats.php, admin_modif_ligne.php
├── equipe_devik.php, equipe_viking.php, conditions.php, mentions.php
├── includes/            Gabarits communs : head, topbar, footer, scripts
├── bdd/                 Accès aux données et logique métier
│   ├── env.php                  Paramètres de connexion
│   ├── BddConnexionUtils.php    Ouverture de connexion et exécution de requêtes
│   ├── BddClientUtils.php       Authentification, historique, informations client
│   ├── Inscription_utils.php    Création de compte
│   ├── BddLigneUtils.php        Lignes, arrêts, horaires, trajet complet
│   ├── BddTrajetUtils.php       Requêtes de trajets et nœuds
│   ├── reserverutils.php        Distance par segment, réservation multi-segments, points
│   ├── BddAdminClientUtils.php  Droits administrateur, gestion des clients
│   ├── BddAdminLigneUtils.php   Modification des horaires et des lignes
│   └── BddAdminStatsUtils.php   Requêtes statistiques
├── assets/, css/, js/   Ressources statiques
└── vik.sql              Script de création et de jeu d'essai de la base
```

### Principes de conception

- Séparation entre les vues (racine du projet) et la logique d'accès aux données (`bdd/`).
- Gabarits partagés (en-tête, barre de navigation, pied de page) pour garantir une interface cohérente.
- Contrôle d'accès administrateur sur chaque page d'administration, fondé sur la table `vik_administrateur`.
- Transactions SQL pour les opérations multi-tables : enregistrement d'une réservation avec ses étapes, suppression d'un client avec ses réservations.
- Échappement des données affichées avec `htmlspecialchars` dans les vues principales.

---

## 6. Modèle de données

### Tables

| Table | Description |
|---|---|
| `vik_departement` | Départements (numéro, nom) |
| `vik_commune` | Communes (code INSEE, département, nom, population) |
| `vik_ligne` | Lignes : numéro, commune de départ et commune terminus |
| `vik_noeud` | Passages des cars : ligne, arrêt, arrêt suivant, heure, distance et durée jusqu'au suivant |
| `vik_type_client` | Niveaux de fidélité : seuil de points et pourcentage de prix |
| `vik_client` | Clients : identité, coordonnées, mot de passe, points en cours, points cumulés, dernière connexion |
| `vik_administrateur` | Adresses de courriel des clients disposant des droits d'administration |
| `vik_tarif` | Tranches de distance et prix |
| `vik_reduction` | Paliers de points et réductions en euros |
| `vik_reservation` | Réservations : client, tranche tarifaire, date, points gagnés, prix total |
| `vik_etape` | Étapes de chaque réservation : ligne, communes de départ et d'arrivée, distance, heure |

### Relations principales

- Une commune appartient à un département ; un client habite un département et possède un niveau.
- Une ligne relie deux communes (départ et terminus) et se compose de nœuds ordonnés.
- Une réservation appartient à un client (le client 0 représente les non-inscrits), applique une tranche tarifaire et se compose d'une ou plusieurs étapes.
- Une étape se rattache à une réservation, à une ligne et à deux communes.

### Note sur le script `vik.sql`

Le fichier `vik.sql` fourni dans le dépôt est le script Oracle du sujet. Il ne définit pas la colonne `CLI_MDP` de `vik_client` ni la table `vik_administrateur`, que l'application utilise, et la table `vik_noeud` y est présente uniquement dans un bloc de commentaire, sans données d'horaires. Ces éléments doivent être présents dans le schéma pour que l'authentification, l'espace administrateur, les horaires et la recherche d'itinéraires fonctionnent.

---

## 7. Algorithmes de recherche d'itinéraires

La page `trajet.php` construit un graphe orienté pondéré à partir de la table `vik_noeud` : les sommets sont les communes, les arcs sont les liaisons entre arrêts successifs, pondérés par la distance et par la durée.

| Besoin | Méthode |
|---|---|
| Trajet le plus court | Algorithme de Dijkstra, critère distance |
| Trajet le plus rapide | Algorithme de Dijkstra, critère durée |
| Trajets alternatifs classés | Recherche des k plus courts chemins par exclusion successive d'arcs, k = 5 par critère |
| Horaires réels | Pour chaque changement de ligne, recherche du premier passage à l'arrêt à une heure supérieure ou égale à l'heure souhaitée, puis cumul des durées d'étape |
| Regroupement par ligne | Les arcs consécutifs d'une même ligne sont regroupés en segments pour l'affichage |

Les trajets pour lesquels aucun horaire n'est disponible après l'heure demandée sont écartés. Les résultats sont triés par distance ou par durée réelle et limités à dix propositions.

Lors de la réservation, la distance de chaque segment est calculée par un algorithme de Dijkstra sur les nœuds de la ligne concernée, puis le tarif est déterminé à partir de la distance totale du voyage.

---

## 8. Installation et configuration

### Prérequis

- PHP 8 ou supérieur avec l'extension `pdo_oci` (client Oracle Instant Client).
- Serveur web Apache.
- Accès à une base Oracle contenant le schéma décrit en section 6.

### Étapes

1. Cloner le dépôt dans le répertoire servi par Apache :
   ```bash
   git clone https://github.com/ElyasAhm4di/SAE_FIN_D_ANNEE.git
   ```
2. Créer le schéma et charger les données à partir de `vik.sql` (SQL Developer ou SQL*Plus), en ajoutant les éléments mentionnés en section 6.
3. Renseigner les paramètres de connexion dans `bdd/env.php` :
   ```php
   $db_usernameOracle = "identifiant";
   $db_passwordOracle = "mot_de_passe";
   $dbOracle = "oci:dbname=hote:1521/service;charset=AL32UTF8";
   ```
4. Ouvrir le site dans le navigateur et tester la connexion à la base depuis `bdd/testBdd.php`.
5. Pour obtenir les droits d'administration, ajouter l'adresse de courriel du compte concerné dans `vik_administrateur`.

Les identifiants réels ne doivent pas être versionnés : utiliser un fichier local exclu par `.gitignore`.

---

## 9. Déploiement sur le serveur de l'IUT

L'application est déployée sur l'environnement PHP et Oracle de l'IUT.

| Élément | Valeur |
|---|---|
| Site du groupe | `https://dev-agile3.users.info.unicaen.fr/` |
| Transfert de fichiers | SFTP (FileZilla), authentification par fichier de clé |
| Hôte | `dev-agile3.users.info.unicaen.fr` |
| Dossier de dépôt | `dev` |
| Compte Oracle | `agile_3` |

Recommandation du support technique : fermer la connexion PDO (`$conn = null;`) sur chaque page dès qu'elle n'est plus utilisée.

---

## 10. Organisation du travail

### Préparation

Une réunion préparatoire, tenue le 18 mai 2026 à l'IUT, a permis de définir l'identité de l'agence (nom et logo, conçus de manière collective), de dresser le bilan des compétences, et de fixer les éléments à préparer avant le début de la SAÉ :

- dépôt Git sur la forge de l'université ;
- structure de base du back-end ;
- en-tête et pied de page réutilisables, avec une maquette prête dès le premier jour ;
- révision de la structure de la base de données et conformité de l'application au schéma UML ;
- utilisation du framework Bootstrap pour gagner en efficacité.

### Méthode

- Démarche agile : démonstrations régulières au client, rétrospectives animées par le coach, mêlée quotidienne en début de journée.
- Priorités du backlog respectées (22 user stories en 5 lots).
- Travail en binôme avec les extensions de synchronisation de VS Code, et un dépôt Git organisé en branches par fonctionnalité (connexion, réservation, statistiques, administration clients, modification des horaires, etc.) fusionnées dans une branche `stable`.

### Usage de l'intelligence artificielle

Conformément au sujet, l'IA générative est limitée à un rôle d'assistance : compréhension d'erreurs SQL ou PHP, aide au débogage, résolution de problèmes de versionnage. Elle n'a pas vocation à générer des fonctionnalités complètes.

### Documents de projet

Maquettes de l'interface, modèle logique de données (MLD), backlog, compte rendu de la réunion de préparation et documentation technique du serveur PHP-Oracle.

---

## 11. Limites connues et pistes d'amélioration

| Sujet | Constat | Amélioration proposée |
|---|---|---|
| Mots de passe | Stockés et comparés en clair dans `vik_client` | Hachage avec `password_hash` et `password_verify` |
| Identifiants de base | Présents dans `bdd/env.php` versionné | Fichier de configuration local exclu du dépôt |
| Injection SQL | `ListeHorairesLigne` concatène un paramètre dans la requête | Requête préparée |
| Portabilité | Requêtes spécifiques à Oracle (`TO_CHAR`, `SYSDATE`, `FETCH FIRST`, `NVL`) | Couche d'abstraction ou portage vers MySQL / MariaDB |
| Documentation | README précédent évoquant MySQL et un workflow CI absent du dépôt | Aligner la documentation sur l'existant (cette version) |
| Campagnes de promotion | Fonctionnalité optionnelle du sujet non implémentée | Ajout d'un module de promotions pour l'administrateur |
| Historique des points | Consultation détaillée de l'utilisation des points non fournie | Table d'historique des mouvements de points |

---

## Crédits

Projet pédagogique réalisé par le Groupe 3 (agence DeviK) : AHMADI Mohammad Elyas, CHAIGNON Nathan, CHUQUET Anaël, COLLET Léo, CONSTANTIN Thomas, GRENECHE Mathéo, GUILBERT Joan, PRENVEILLE Noé.

Sujet et encadrement : E. Porcq, IUT Grand Ouest Normandie.
