# Projet de Fin d'Année - Gestion de Transport

[![CI](https://github.com/ElyasAhma4di/Projet-de-Fin-d-ann-e/actions/workflows/symfony.yml/badge.svg)](https://github.com/ElyasAhm4di/Projet-de-Fin-d-ann-e/actions)

Ceci est le dépôt de notre projet de fin d'année. L'objectif était de développer un site web de réseau de transport, avec une interface publique pour les usagers et un back-office pour l'administration.

## Ce que fait le projet

**Côté usager :**
- Inscription, connexion et gestion du profil client.
- Consultation de la carte, des lignes, des horaires et des tarifs.
- Système de réservation de trajets.

**Côté admin :**
- Dashboard global.
- Gestion des comptes clients (ajout, modification, suppression).
- Gestion des lignes et des itinéraires.
- Accès aux statistiques de réservation et d'utilisation.

## Stack technique

Nous avons travaillé en PHP natif (8+) sans framework pour consolider les bases.
- Front : HTML5, CSS3, JavaScript vanilla.
- Base de données : MySQL / MariaDB.
- CI/CD : GitHub Actions (workflow opérationnel).

L'architecture est structurée simplement : les vues principales sont à la racine, les modules récurrents (header, footer) sont rangés dans `/includes/`, et toute la logique métier et l'accès aux données se trouvent dans `/bdd/`.

## Lancer le projet en local

1. Clonez le dépôt dans le répertoire web de votre serveur local (htdocs, www, etc.) :
   ```bash
   git clone [https://github.com/VOTRE_NOM/Projet-de-Fin-d-ann-e.git](https://github.com/VOTRE_NOM/Projet-de-Fin-d-ann-e.git)
