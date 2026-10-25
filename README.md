# Volkane — préproduction du back-office

Ce dossier prépare un CMS Directus indépendant avec une base PostgreSQL Railway. Le Data Studio de Directus fournit la connexion et le tableau de bord. Le site V24/V25 n'est pas encore relié à l'API : les changements dans ce CMS ne se reflètent pas encore sur le site. Garder le site public hors de cette première mise en ligne.

## Déploiement Railway

1. Crée un dépôt GitHub privé `volkane-preproduction` et pousse le contenu de ce dossier à sa racine.
2. Dans Railway, crée un projet `Volkane — Préproduction`, ajoute un service PostgreSQL, puis un service depuis le dépôt privé. Le `Dockerfile` lance Directus.
3. Dans les variables du service Directus, renseigne `KEY` et `SECRET` avec deux chaînes aléatoires longues et différentes. Crée `ADMIN_EMAIL` et un mot de passe unique. Ne les mets jamais dans GitHub.
4. Ajoute les variables de base de données du service PostgreSQL en utilisant les références proposées par Railway : `DB_CLIENT=pg`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USER`, `DB_PASSWORD`. Lie-les aux variables du service PostgreSQL. Garde la base en réseau privé.
5. Lance le déploiement. Dans Networking, génère un domaine Railway temporaire et reporte-le dans `PUBLIC_URL`, puis redéploie.
6. Connecte-toi au Data Studio avec le compte administrateur. Désactive l’inscription publique. Dans Settings → Access Control → Administrator, impose le MFA. Crée ensuite des comptes nominatifs et des rôles au besoin. Ne partage pas le compte admin.
7. Pour importer le contenu, installe Node.js LTS si nécessaire. Exporte le JSON complet depuis le back-office de démonstration le plus récent, place-le sous `content.json` dans ce dossier, puis crée un fichier `.env` local contenant `DIRECTUS_URL`, `ADMIN_EMAIL` et `ADMIN_PASSWORD`. Ces deux fichiers sont ignorés par Git. Ouvre un terminal dans ce dossier et lance `node scripts/import-content.mjs content.json`. L’import s’arrête si une collection contient déjà des données.
8. Vérifie que les collections sont privées pour le rôle Public, que seuls les comptes connectés peuvent accéder au Data Studio, que le MFA fonctionne et que les données restent présentes après un redéploiement.

Le premier déploiement initialise l’administrateur à partir de `ADMIN_EMAIL` et `ADMIN_PASSWORD`. Après confirmation de la connexion, supprime `ADMIN_PASSWORD` des variables Railway. Les secrets `KEY` et `SECRET` doivent rester configurés. Active les sauvegardes PostgreSQL avant d’y saisir des données importantes.

## Limites de cette étape

Le CMS conserve les contenus dans PostgreSQL et possède son propre tableau de bord protégé. Il n’active pas le back-office `#/admin` déjà intégré au HTML et il ne publie pas encore les modifications sur le site. La prochaine étape de développement consiste à connecter les pages à l’API, définir les droits de publication et tester le flux brouillon → publié. Les demandes de contact, le mail, les images, le RGPD et la sécurité complète du site demandent aussi une intégration dédiée.
