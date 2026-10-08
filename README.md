# Viking Transport

Application web de réservation et d'administration pour un réseau de cars normands (fictif). Projet du **Groupe 3, agence DeviK** · SAÉ S2.04, S2.05, S2.06 · BUT Informatique, IUT Grand Ouest Normandie · 2025-2026

Périmètre : 19 lignes (38 avec les deux sens), 81 communes, 9 départements. Projet de 3 jours en méthode agile, sujet de E. Porcq.

**Stack** : PHP 8 natif (sans framework), Apache, PDO avec requêtes préparées, Oracle (driver PDO OCI), HTML/CSS/JavaScript, Bootstrap 5.3.

## Équipe

AHMADI Mohammad Elyas (développement PHP, conception générale), CHAIGNON Nathan, CHUQUET Anaël, COLLET Léo, CONSTANTIN Thomas, GRENECHE Mathéo, GUILBERT Joan, PRENVEILLE Noé. Encadrement : E. Porcq.

Les fonctionnalités ont été réparties selon les compétences de chacun, et tous les membres ont développé.

## Fonctionnalités

22 user stories en 5 lots.

| Profil | Fonctionnalités | Pages |
|---|---|---|
| Visiteur | Lignes, horaires, réservation sur une ligne, une partie de ligne ou plusieurs lignes, tarif | `lignes.php`, `horaires.php`, `reserver.php`, `tarifs.php` |
| Client inscrit | Inscription, connexion, profil, trajets, points de fidélité (gain et utilisation) | `inscription.php`, `connexion.php`, `profil.php` |
| Client inscrit | Recherche d'itinéraires : trajets possibles, avec horaires, le plus court, le plus rapide, classement | `trajet.php` |
| Administrateur | Comptes clients (consultation, modification, suppression, comptes inactifs) | `admin_client.php`, `admin_modifier_client.php`, `action_supprimer_client.php` |
| Administrateur | Statistiques : meilleurs clients, lignes les plus utilisées, réservations par période, chiffre d'affaires, heures de pointe, trajets populaires | `admin_stats.php` |
| Administrateur | Modification des lignes et des horaires | `admin_modif_ligne.php` |

Autres pages : `index.php`, `carte.php` (carte interactive), `admin_dashboard.php`, `equipe_devik.php`, `equipe_viking.php`, `conditions.php`, `mentions.php`.

## Règles de gestion

**Tarif.** Prix de base selon la distance totale, en 13 tranches (table `vik_tarif`) : de 5 € (0 à 10 km) à 90 € (301 à 500 km).

**Fidélité.** 1 point pour 10 km, en points entiers, utilisables à partir du voyage suivant. Le niveau dépend des points cumulés et donne une réduction permanente :

| Niveau | Points | Prix payé |
|---|---:|---:|
| Nouveau | 10 | 95 % |
| Poussin | 400 | 90 % |
| Junior | 3 000 | 80 % |
| Argent | 10 000 | 65 % |
| Or | 50 000 | 50 % |

Les points s'échangent contre une réduction (`vik_reduction`) : 100 points = 1 €, 500 = 7 €, 1 000 = 15 €. L'application choisit la combinaison la plus avantageuse et ne consomme que les points nécessaires.

**Comptes.** Sans connexion depuis plus d'un an : inactif ; plus de deux ans : à supprimer. Les réservations d'un visiteur sont rattachées au client 0, sans point.

**Nœud et étape.** Un nœud est un passage de car à un arrêt, à une heure donnée, avec distance et durée jusqu'au nœud suivant. Une étape est la portion de voyage d'un client sur une ligne, sur un ou plusieurs nœuds. Chaque commune a un seul arrêt.

## Architecture

Les pages sont à la racine ; les chemins (`./includes/…`, `./bdd/…`) sont relatifs à cette racine.

```
├── *.php                     Pages (voir Fonctionnalités)
├── template.php              Squelette pour créer une page
├── includes/                 head, topbar, footer, scripts
├── bdd/
│   ├── env.php               Paramètres de connexion
│   ├── BddConnexionUtils.php Connexion et exécution des requêtes
│   ├── BddUtils.php          Point d'entrée : charge les modules
│   ├── BddClientUtils.php, Inscription_utils.php   Clients, authentification, inscription
│   ├── BddLigneUtils.php, LigneUtils.php, BddTrajetUtils.php   Lignes, horaires, trajets, nœuds
│   ├── reserverutils.php     Distance par segment, réservation, points
│   ├── BddAdminClientUtils.php, BddAdminLigneUtils.php, BddAdminStatsUtils.php   Administration
│   ├── testBdd.php           Test de connexion
│   └── ideeRequete.txt       Brouillon de requêtes
├── assets/, css/, js/        Ressources statiques
└── vik.sql                   Création de la base et jeu d'essai
```

Principes : vues séparées de l'accès aux données (`bdd/`) ; en-tête, navigation et pied de page partagés ; droits administrateur vérifiés sur chaque page admin (table `vik_administrateur`) ; transactions SQL pour les opérations multi-tables (réservation et ses étapes, suppression d'un client et de ses réservations) ; `htmlspecialchars` sur les données affichées dans les vues principales.

## Base de données

| Domaine | Tables |
|---|---|
| Géographie | `vik_departement`, `vik_commune` |
| Réseau | `vik_ligne`, `vik_noeud` |
| Clients | `vik_client`, `vik_type_client`, `vik_administrateur` |
| Tarification | `vik_tarif`, `vik_reduction` |
| Réservations | `vik_reservation`, `vik_etape` |

> **`vik.sql` est incomplet** pour l'application : il ne définit ni la colonne `CLI_MDP` de `vik_client`, ni la table `vik_administrateur`, et `vik_noeud` y figure seulement en commentaire, sans horaires. Sans ces éléments, l'authentification, l'espace administrateur, les horaires et les itinéraires ne fonctionnent pas : il faut les ajouter au schéma.

## Recherche d'itinéraires

`trajet.php` construit un graphe orienté depuis `vik_noeud` (communes = sommets, liaisons entre arrêts successifs = arcs pondérés par la distance et la durée).

- Plus court et plus rapide : algorithme de Dijkstra (poids distance ou durée).
- Alternatives : k plus courts chemins par exclusion successive d'arcs, k = 5 par critère.
- Horaires : à chaque changement de ligne, premier passage à une heure supérieure ou égale à l'heure souhaitée ; les trajets sans horaire sont écartés.
- Résultats triés par distance ou durée, limités à 10.

À la réservation, la distance de chaque segment est calculée par Dijkstra sur les nœuds de la ligne, et le tarif découle de la distance totale.

## Installation

Prérequis : PHP 8+ avec `pdo_oci` (Oracle Instant Client), Apache, une base Oracle avec le schéma ci-dessus.

1. `git clone https://github.com/ElyasAhm4di/SAE_FIN_D_ANNEE.git` dans le dossier servi par Apache.
2. Créer le schéma depuis `vik.sql`, en ajoutant les éléments manquants.
3. Renseigner `bdd/env.php` :
   ```php
   $db_usernameOracle = "identifiant";
   $db_passwordOracle = "mot_de_passe";
   $dbOracle = "oci:dbname=hote:1521/service;charset=AL32UTF8";
   ```
4. Vérifier la connexion avec `bdd/testBdd.php`.
5. Pour les droits d'administration, ajouter l'adresse de courriel du compte dans `vik_administrateur`.

Ne versionnez pas vos identifiants réels (voir Limites connues). Le site est hébergé sur le serveur de l'IUT : <https://dev-agile3.users.info.unicaen.fr/>.

## Organisation du travail

Réunion préparatoire le 18 mai 2026 : nom et logo de l'agence, bilan des compétences, préparatifs (dépôt Git, structure back-end, en-tête et pied de page réutilisables avec maquette, base de données conforme à l'UML, Bootstrap).

Méthode agile : mêlée quotidienne, démonstrations au client, rétrospectives avec le coach. Une branche Git par fonctionnalité (connexion, réservation, statistiques, administration…), fusionnée dans `stable`. Travail souvent en binôme (partage de session VS Code).

## Usage de l'IA

Des outils d'IA générative ont servi d'assistant : compréhension d'erreurs SQL ou PHP, débogage, problèmes de versionnage, mise en forme de la documentation. La conception et la logique du projet ont été réalisées par l'équipe, conformément au sujet qui limite l'IA à un rôle d'aide.

## Limites connues

1. **Identifiants versionnés** : `bdd/env.php` est dans le dépôt. Les supprimer ne suffit pas (historique Git) : changer le mot de passe, sortir le fichier du dépôt, fournir un `env.example.php`.
2. **Mots de passe en clair** dans `vik_client` : passer à `password_hash` et `password_verify`.
3. **Injection SQL** : `ListeHorairesLigne` concatène un paramètre dans la requête ; utiliser une requête préparée.
4. **Dépendance à Oracle** : `TO_CHAR`, `SYSDATE`, `FETCH FIRST`, `NVL`. Un portage MySQL ou MariaDB demande une couche d'abstraction.
5. **Non implémenté** : campagnes de promotion (optionnel) et historique détaillé de l'usage des points.
