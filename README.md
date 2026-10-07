# Viking Transport

Application web de réservation et d'administration pour un réseau de cars normands (fictif). Projet du **Groupe 3, agence DeviK**, réalisé en trois jours dans le cadre des SAÉ S2.04, S2.05 et S2.06 du BUT Informatique (IUT Grand Ouest Normandie, 2025-2026).

> DeviK : « Une requête, des solutions. »

**Sommaire** : [Le projet](#le-projet) · [L'équipe](#léquipe) · [Ce que fait le site](#ce-que-fait-le-site) · [Règles de gestion](#règles-de-gestion) · [Architecture](#architecture) · [Base de données](#base-de-données) · [Recherche d'itinéraires](#recherche-ditinéraires) · [Installation](#installation) · [Façon de travailler](#façon-de-travailler) · [Limites connues](#limites-connues)

---

## Le projet

Viking Transport regroupe les transports régionaux non urbains de Normandie : 19 lignes (38 avec les deux sens), 81 communes, 9 départements normands et limitrophes. Un client choisit deux communes et réserve un voyage, sur une ou plusieurs lignes.

Le sujet, rédigé par E. Porcq, demandait de concevoir et d'exploiter une base de données, puis de développer en équipe une application web en HTML, PHP, CSS et JavaScript. Le tout se déroule en méthode agile : choix de fonctionnalités, développement, démonstrations à l'équipe cliente, rétrospectives avec le coach. Le rendu est évalué sur l'application, la base et la démonstration.

## L'équipe

Huit étudiants. Aucun rôle n'a été figé : les fonctionnalités ont été réparties selon les envies et les compétences de chacun, et tout le monde a écrit du code. Les rôles ci-dessous indiquent seulement où chacun a mis le plus d'énergie.

- **AHMADI Mohammad Elyas** : développement PHP et conception générale de l'application
- **CHAIGNON Nathan** : modules et architecture back-end
- **CHUQUET Anaël** : architecture logicielle et intégration front-end
- **COLLET Léo** : identité visuelle et design de l'interface
- **CONSTANTIN Thomas** : architecture globale et structures back-end
- **GRENECHE Mathéo** : vues front-end et maquettes fonctionnelles
- **GUILBERT Joan** : développement PHP, modules, requêtes SQL complexes
- **PRENVEILLE Noé** : modélisation, système d'information, structure de la base

Encadrement : E. Porcq, auteur du sujet, organisateur et tuteur.

## Ce que fait le site

Le backlog comptait 22 user stories, traitées par priorité et regroupées en cinq lots.

**Un visiteur non inscrit** consulte les lignes (`lignes.php`) et leurs horaires (`horaires.php`), puis réserve un voyage (`reserver.php`) : sur une ligne entière, sur une partie seulement, ou sur plusieurs lignes à la suite. Le tarif s'affiche avant de valider (`tarifs.php`).

**Un client inscrit** peut créer son compte (`inscription.php`), se connecter (`connexion.php`) et gérer son profil, ses trajets et ses points (`profil.php`). Chaque réservation rapporte des points de fidélité, qu'il peut dépenser pour baisser le prix d'un voyage suivant. Il a aussi accès à la recherche d'itinéraires (`trajet.php`) : trajets possibles entre deux communes, avec horaires, du plus rapide ou du plus court, classés par distance ou par durée.

**Un administrateur** voit les comptes clients et leurs réservations (`admin_client.php`), les modifie ou les supprime (`admin_modifier_client.php`, `action_supprimer_client.php`) et repère les comptes inactifs. Il consulte des statistiques (`admin_stats.php`) : meilleurs clients, lignes les plus utilisées, réservations par période, chiffre d'affaires total et par ligne, clients avec le plus de points, heures de pointe, trajets les plus populaires. Il peut enfin modifier le réseau lui-même : ajouter, modifier ou supprimer des arrêts et des horaires (`admin_modif_ligne.php`). Le tableau de bord est `admin_dashboard.php`.

Autour de ça, quelques pages d'appoint : `index.php` (accueil), `carte.php` (carte interactive du réseau et choix de trajets), `equipe_devik.php` et `equipe_viking.php` (présentation de l'agence et du client), `conditions.php` et `mentions.php`.

## Règles de gestion

**Tarif.** Le prix de base dépend de la distance totale du voyage, selon 13 tranches stockées dans `vik_tarif` : de 5 € pour 0 à 10 km jusqu'à 90 € pour 301 à 500 km. Il s'applique au voyage complet, quel que soit le nombre d'étapes.

**Niveaux de fidélité.** Le niveau d'un client dépend de ses points cumulés et lui donne une réduction permanente :

| Niveau | Points cumulés | Prix payé |
|---|---:|---:|
| Nouveau | 10 | 95 % |
| Poussin | 400 | 90 % |
| Junior | 3 000 | 80 % |
| Argent | 10 000 | 65 % |
| Or | 50 000 | 50 % |

**Points.** Un voyage rapporte 1 point par tranche de 10 km, en points entiers. Les points gagnés ne servent qu'à partir du voyage suivant. On les échange contre une réduction en euros (table `vik_reduction`) : 100 points valent 1 €, 500 points 7 €, 1 000 points 15 €. L'application cherche la combinaison de paliers la plus avantageuse et, si la réduction dépasse le prix du billet, ne consomme que les points nécessaires.

**Comptes.** Un compte sans connexion depuis plus d'un an est signalé comme inactif dans l'interface d'administration, et à supprimer au-delà de deux ans. Les réservations d'un visiteur non inscrit sont rattachées au client numéro 0 et ne rapportent aucun point.

**Nœud et étape.** Deux mots à ne pas confondre. Un *nœud* est un passage de car à un arrêt, à une heure donnée, avec la distance et la durée jusqu'au nœud suivant : il y en a autant que de passages. Une *étape* est la portion de voyage qu'un client fait sur une ligne, et elle peut couvrir plusieurs nœuds. Chaque commune n'a qu'un seul arrêt.

## Architecture

Du PHP 8 natif, sans framework, servi par Apache. L'accès aux données passe par PDO avec des requêtes préparées, sur une base Oracle (driver PDO OCI). L'interface est en HTML5, CSS3, JavaScript, Bootstrap 5.3 et Bootstrap Icons. Le code est versionné avec Git, une branche par fonctionnalité.

Les pages sont à la racine du projet. Les chemins (`./includes/…`, `./bdd/…`) sont relatifs à cette racine, c'est pourquoi l'arborescence reste à plat.

```
.
├── index.php, lignes.php, horaires.php, tarifs.php, carte.php
├── reserver.php, trajet.php
├── connexion.php, deconnexion.php, inscription.php, profil.php
├── admin_dashboard.php, admin_client.php, admin_modifier_client.php,
│   action_supprimer_client.php, admin_stats.php, admin_modif_ligne.php
├── equipe_devik.php, equipe_viking.php, conditions.php, mentions.php
├── template.php          Squelette de page à copier pour en créer une nouvelle
├── includes/             Morceaux communs : head, topbar, footer, scripts
├── bdd/                  Accès aux données et logique métier
│   ├── env.php                   Paramètres de connexion
│   ├── BddConnexionUtils.php     Ouverture de la connexion, exécution des requêtes
│   ├── BddUtils.php              Point d'entrée unique : charge les modules ci-dessous
│   ├── BddClientUtils.php        Authentification, historique, infos client
│   ├── Inscription_utils.php     Création de compte
│   ├── BddLigneUtils.php         Lignes, arrêts, horaires, trajet complet
│   ├── LigneUtils.php            Petits utilitaires sur les communes
│   ├── BddTrajetUtils.php        Requêtes de trajets et de nœuds
│   ├── reserverutils.php         Distance par segment, réservation multi-segments, points
│   ├── BddAdminClientUtils.php   Droits administrateur, gestion des clients
│   ├── BddAdminLigneUtils.php    Modification des lignes et des horaires
│   ├── BddAdminStatsUtils.php    Requêtes statistiques
│   ├── testBdd.php               Test de connexion à la base
│   └── ideeRequete.txt           Brouillon de requêtes (notes de travail)
├── assets/, css/, js/    Images, feuille de style, script
└── vik.sql               Création de la base et jeu d'essai
```

Quelques principes tenus d'un bout à l'autre :

- les vues restent à la racine, la logique d'accès aux données est dans `bdd/` ;
- l'en-tête, la barre de navigation et le pied de page sont partagés, pour une interface cohérente ;
- chaque page d'administration vérifie les droits de l'utilisateur dans la table `vik_administrateur` ;
- les opérations qui touchent plusieurs tables sont des transactions SQL : enregistrer une réservation avec ses étapes, supprimer un client avec ses réservations ;
- les données affichées passent par `htmlspecialchars` dans les vues principales.

## Base de données

Onze tables :

- **Géographie** : `vik_departement` (numéro, nom) et `vik_commune` (code INSEE, département, nom, population).
- **Réseau** : `vik_ligne` (numéro, commune de départ et terminus) et `vik_noeud` (ligne, arrêt, arrêt suivant, heure de passage, distance et durée jusqu'au suivant).
- **Clients** : `vik_client` (identité, coordonnées, mot de passe, points en cours, points cumulés, dernière connexion), `vik_type_client` (niveaux de fidélité) et `vik_administrateur` (adresses des clients qui ont les droits d'administration).
- **Tarification** : `vik_tarif` (tranches de distance et prix) et `vik_reduction` (paliers de points et réductions en euros).
- **Réservations** : `vik_reservation` (client, tranche tarifaire, date, points gagnés, prix total) et `vik_etape` (ligne, communes de départ et d'arrivée, distance, heure).

Une commune appartient à un département. Une ligne relie deux communes et se compose de nœuds ordonnés. Une réservation appartient à un client (le 0 pour les non-inscrits), applique une tranche tarifaire et se découpe en une ou plusieurs étapes.

**À savoir sur `vik.sql`.** C'est le script Oracle fourni avec le sujet, et il est incomplet pour l'application : il ne définit ni la colonne `CLI_MDP` de `vik_client` ni la table `vik_administrateur`, et `vik_noeud` n'y figure que dans un bloc de commentaire, sans aucun horaire. Sans ces éléments, l'authentification, l'espace administrateur, les horaires et la recherche d'itinéraires ne fonctionnent pas. Il faut les ajouter au schéma à la main.

## Recherche d'itinéraires

`trajet.php` construit un graphe orienté à partir de `vik_noeud` : les communes sont les sommets, les liaisons entre arrêts successifs sont les arcs, pondérés par la distance et par la durée.

- Le trajet le plus court et le plus rapide viennent d'un algorithme de **Dijkstra**, avec la distance ou la durée comme poids.
- Les alternatives viennent d'une recherche des **k plus courts chemins** par exclusion successive d'arcs (k = 5 par critère).
- Pour les horaires réels, chaque changement de ligne cherche le premier passage à l'arrêt à une heure supérieure ou égale à l'heure souhaitée, puis cumule les durées d'étape. Les trajets sans horaire après l'heure demandée sont écartés.
- Les arcs consécutifs d'une même ligne sont regroupés en segments pour l'affichage.

Les résultats sont triés par distance ou par durée réelle, dans la limite de dix propositions. À la réservation, la distance de chaque segment est calculée par un Dijkstra sur les nœuds de la ligne concernée, et le tarif découle de la distance totale du voyage.

## Installation

Il faut PHP 8 ou plus avec l'extension `pdo_oci` (donc Oracle Instant Client), un serveur Apache, et l'accès à une base Oracle contenant le schéma ci-dessus.

1. Cloner le dépôt dans le dossier servi par Apache :
   ```bash
   git clone https://github.com/ElyasAhm4di/SAE_FIN_D_ANNEE.git
   ```
2. Créer le schéma et charger les données depuis `vik.sql` (SQL Developer ou SQL*Plus), en y ajoutant les éléments manquants décrits plus haut.
3. Renseigner la connexion dans `bdd/env.php` :
   ```php
   $db_usernameOracle = "identifiant";
   $db_passwordOracle = "mot_de_passe";
   $dbOracle = "oci:dbname=hote:1521/service;charset=AL32UTF8";
   ```
4. Ouvrir le site, et vérifier la connexion à la base avec `bdd/testBdd.php`.
5. Pour donner les droits d'administration à un compte, ajouter son adresse de courriel dans `vik_administrateur`.

Ne versionnez jamais vos vrais identifiants : gardez `bdd/env.php` en local, hors du dépôt (voir [Limites connues](#limites-connues)).

Le site du groupe est hébergé sur le serveur de l'IUT : <https://dev-agile3.users.info.unicaen.fr/>. Conseil du support technique : fermer la connexion PDO (`$conn = null;`) dans chaque page dès qu'elle ne sert plus.

## Façon de travailler

Une réunion préparatoire, le 18 mai 2026 à l'IUT, a servi à choisir le nom et le logo de l'agence ensemble, à faire le point sur les compétences de chacun, et à lister ce qu'il fallait avoir prêt avant le début de la SAÉ : le dépôt Git sur la forge de l'université, la structure de base du back-end, un en-tête et un pied de page réutilisables avec une maquette dès le premier jour, une base de données revue et conforme au schéma UML, et Bootstrap pour aller plus vite.

Pendant les trois jours : mêlée quotidienne au début de chaque journée, démonstrations régulières au client, rétrospectives avec le coach, backlog suivi dans l'ordre des priorités. Le travail se faisait souvent en binôme avec les extensions de partage de VS Code, sur des branches par fonctionnalité (connexion, réservation, statistiques, administration des clients, modification des horaires…) fusionnées dans une branche `stable`.

L'IA générative n'a servi que d'assistant, comme le sujet l'impose : comprendre une erreur SQL ou PHP, aider au débogage, démêler un souci de versionnage. Elle n'a pas écrit de fonctionnalités entières.

Documents produits : maquettes, modèle logique de données, backlog, compte rendu de la réunion de préparation, documentation technique du serveur PHP-Oracle.

## Limites connues

Ce qui n'est pas satisfaisant aujourd'hui, du plus grave au plus mineur :

1. **Identifiants de base versionnés.** `bdd/env.php` est dans le dépôt, avec ses identifiants. Les supprimer du fichier ne suffit pas, ils restent dans l'historique Git : il faut changer le mot de passe côté base, puis sortir le fichier du dépôt et fournir à la place un exemple (`env.example.php`).
2. **Mots de passe en clair.** Ils sont stockés et comparés tels quels dans `vik_client`. La correction passe par `password_hash` et `password_verify`.
3. **Injection SQL.** `ListeHorairesLigne` concatène un paramètre dans la requête. À remplacer par une requête préparée.
4. **Dépendance à Oracle.** Les requêtes utilisent `TO_CHAR`, `SYSDATE`, `FETCH FIRST` et `NVL`. Passer à MySQL ou MariaDB demande une couche d'abstraction ou un portage.
5. **Fonctions du sujet non faites.** Les campagnes de promotion (optionnelles) ne sont pas implémentées, et l'historique détaillé de l'usage des points non plus. Il faudrait un module de promotions pour l'administrateur et une table de mouvements de points.

---

Projet pédagogique du Groupe 3 (agence DeviK). Sujet et encadrement : E. Porcq, IUT Grand Ouest Normandie.
