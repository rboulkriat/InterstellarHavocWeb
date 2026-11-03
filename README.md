# Interstellar Havoc Web

Site web du jeu **Interstellar Havoc**, un tower defense spatial : placez des tours électriques sur des étoiles, repoussez les vagues d'ennemis et protégez votre base.

Ce projet a été réalisé dans le cadre d'une **SAÉ de développement web** (PHP / MySQL).

## Fonctionnalités

-  **Inscription / connexion** avec mots de passe hachés (`password_hash` / bcrypt)
-  **Profil et paramètres** du joueur (logo, e-mail, abonnement)
-  **Système d'amis** : demandes d'ami avec statut (*Attente*, *Accepté*, *Rejeté*)
-  **Page des règles** du jeu, classées par catégorie
-  **Historique des parties** : date, heure, durée et score
-  **Formulaire de contact** : les messages des joueurs sont enregistrés en base
-  **Protection CSRF** via un jeton de session

##  Le jeu en bref

### Règles principales

- Les ennemis ne se déplacent que sur le **chemin tracé**.
- Les tours ne peuvent être placées que sur une **étoile**.
- La boutique ne fait pas de crédit ; les objets achetés ne sont utilisables que sur le chemin.
- La base est détruite par les ennemis qui s'en approchent trop : à partir du **cinquième ennemi**, la partie est perdue.

### Les tours

Trois familles de tours, chacune déclinée en **3 niveaux** d'amélioration :

| Famille | Profil |
|---|---|
| **EdisonCoil** | Polyvalente, bonne cadence |
| **NikolaCoil** | Légère et peu coûteuse, portée courte |
| **OppenheimerCoil** | Lourde : dégâts et portée maximaux, prix élevé |

### Les ennemis

| Ennemi | Vie | Dégâts | Portée | Contourne les murs | Récompense |
|---|---|---|---|---|---|
| **Scavenger** | 100 | 10 | 40 | Oui | 100 |
| **Behemoth** | 150 | 20 | 50 | Non | 200 |
| **Balliste** | 60 | 5 | 30 | Oui | 50 |

##  Technologies

- **PHP** 8.x (architecture modulaire, orientée objet)
- **MySQL / MariaDB** (base `interstellarhavoc`)
- **HTML5 / CSS3 / JavaScript**
- Serveur local : **XAMPP**

## Structure du projet

```
InterstellarHavocWeb/
├── composants/            # Composants réutilisables (menu, pied de page, …)
├── css/                   # Feuilles de style
├── images/                # Ressources graphiques
├── js/                    # Scripts JavaScript
├── modules/               # Modules fonctionnels du site (ex. mod_parametre)
├── Connexion.php          # Connexion à la base de données
├── index.php              # Point d'entrée : session, jeton CSRF, composants
├── site.php               # Logique principale du site
├── template.php           # Gabarit de page
├── vue_generique.php      # Vue générique commune aux modules
└── interstellarhavoc.sql  # Script de création et de peuplement de la base
```

## Base de données

Le script `interstellarhavoc.sql` crée les tables suivantes :

| Table | Rôle |
|---|---|
| `joueur` | Comptes des joueurs (nom, logo, e-mail, mot de passe haché, abonnement) |
| `connexion` | Identifiants de connexion |
| `amis` | Relations d'amitié et leur statut |
| `partie` | Parties jouées (date, heure, score, durée, joueur, terrain) |
| `terrain` | Cartes de jeu (dimension, niveau, nombre de tours, départ / arrivée) |
| `tour` | Catalogue des tours (vie, dégâts, portée, cadence, prix, niveau) |
| `ennemi` | Catalogue des ennemis |
| `tour_partie` | Tours posées durant une partie (position, vie restante) |
| `ennemi_partie` | Ennemis présents durant une partie (position, vie restante) |
| `regles` | Règles du jeu affichées sur le site |
| `messages_joueurs` | Messages envoyés via le formulaire de contact |

## Installation

### Prérequis

- PHP 8.0 ou supérieur
- MySQL ou MariaDB
- Un serveur web local (Apache via XAMPP, WAMP, MAMP…)

### Étapes

1. **Cloner le dépôt** dans le dossier de votre serveur web (`htdocs` pour XAMPP) :

   ```bash
   git clone https://github.com/rboulkriat/InterstellarHavocWeb.git
   ```

2. **Créer la base de données** : ouvrez phpMyAdmin, créez une base nommée `interstellarhavoc`, puis importez le fichier `interstellarhavoc.sql`.

3. **Configurer la connexion** : ouvrez `Connexion.php` et adaptez l'hôte, le nom de la base, l'utilisateur et le mot de passe à votre environnement (par défaut sous XAMPP : `localhost`, `root`, mot de passe vide).

4. **Lancer le site** : démarrez Apache et MySQL, puis rendez-vous sur :

   ```
   http://localhost/InterstellarHavocWeb/
   ```

## Sécurité

- Mots de passe stockés avec un hachage **bcrypt** (jamais en clair)
- **Jeton CSRF** généré par session (`random_bytes`)
- Pensez à utiliser des **requêtes préparées** (PDO) pour toutes les interactions avec la base

 Les comptes présents dans `interstellarhavoc.sql` sont des données de test. Ne les conservez pas en production.

## Auteurs

Projet réalisé par **[rboulkriat](https://github.com/rboulkriat)** et son équipe dans le cadre de la SAÉ de développement web.
