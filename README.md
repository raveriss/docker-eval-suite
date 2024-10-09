# Projet Inception - Validation des configurations Docker avec `make eval`

## Aperçu du projet

Ce projet est conçu comme un environnement pour tester et valider les configurations Docker, spécifiquement adapté au projet *Inception* de l'école 42. La commande clé, `make eval`, automatise la validation de plusieurs composants, tels que les Dockerfiles, le fichier `docker-compose.yml`, les variables d'environnement, et les configurations SSL/TLS, en s'assurant que le système respecte les bonnes pratiques de Docker.

## Description

La commande `make eval` est un outil puissant qui automatise la validation de votre environnement Docker. Elle vérifie que :
- Les fichiers requis sont présents (par exemple, `.env`, Dockerfiles).
- Les Dockerfiles respectent la structure correcte.
- Les certificats SSL et les ports sont correctement configurés (HTTPS uniquement sur le port 443).
- Aucune configuration interdite comme `network: host`, `--link` ou les boucles infinies (`tail -f`, `sleep infinity`) n'est présente.

En exécutant `make eval`, vous garantissez que votre environnement Docker respecte toutes les exigences nécessaires pour le projet *Inception*.

## Prérequis

Avant de lancer le projet, assurez-vous que votre environnement respecte les prérequis suivants :

- **Docker** : Version `27.0.3` ou ultérieure.
- **Docker Compose** : Version `v2.28.1` ou ultérieure.
- **GNU Make** : Version `4.3` ou ultérieure.
- Système d'exploitation : **Linux** (Ubuntu/Debian recommandé).

## Installation et Configuration

### Étape 1 : Cloner le dépôt

```bash
git clone https://github.com/votre-repo/inception.git
cd inception
```

### Étape 2 : Configurer le fichier .env
Le fichier `.env` contient les variables d'environnement nécessaires à votre configuration Docker. Remplissez-le correctement en ajoutant les variables suivantes :

```bash
# Nom de domaine pour le site WordPress
DOMAIN_NAME=student.42.fr

# Nom de la base de données SQL
SQL_DATABASE=sql_database

# Nom d'utilisateur pour la base de données SQL
SQL_USER=sql_user

# Mot de passe pour l'utilisateur de la base de données SQL
SQL_PASSWORD=sql_password

# Mot de passe root pour la base de données SQL
SQL_ROOT_PASSWORD=sql_root_password

# Titre du site WordPress
SITE_TITLE="Official_Site"

# Nom d'utilisateur pour l'administrateur WordPress
ADMIN_USER=firstuser

# Mot de passe pour l'administrateur WordPress
ADMIN_PASSWORD=firstuserpassword

# Adresse e-mail de l'administrateur WordPress
ADMIN_EMAIL=firstuser@exemple.com

# Nom d'un second utilisateur WordPress
SECOND_USER=seconduser

# Mot de passe pour le second utilisateur WordPress
SECOND_PASSWORD=seconduserpassword

# Confirmation du mot de passe pour le second utilisateur WordPress
SECOND_USER_PASSWORD=seconduserpassword

# Adresse e-mail du second utilisateur WordPress
SECOND_USER_EMAIL=seconduser@exemple.com
```
Assurez-vous de ne pas exposer d'informations sensibles dans votre fichier .env.

### Étape 3 : Valider la configuration
Lancez la commande suivante pour démarrer le processus de validation :

```bash
make eval
```
Cette commande va :

Vérifier la présence d'un fichier `.env` valide.
S'assurer que tous les fichiers nécessaires (ex : Dockerfiles, docker-compose.yml) sont présents.
Valider les paramètres de sécurité de Docker (ex : ports, certificats SSL/TLS).
### Utilisation de la commande `make eval`
#### Détails de la commande
La commande make eval effectue plusieurs vérifications importantes :

**Validation du fichier** `.env` :
S'assure que toutes les variables requises sont présentes.
Vérifie qu'aucune information d'identification critique n'est exposée dans le fichier `.env`.

**Validation des Dockerfiles** :
Vérifie que chaque service (ex : NGINX, WordPress, MariaDB) possède un Dockerfile correspondant.
S'assure que les Dockerfiles ne contiennent pas de configurations interdites comme des boucles infinies.

**Validation du fichier docker-compose.yml** :
Vérifie qu'aucune option interdite telle que network: host ou --link n'est utilisée.
S'assure qu'un réseau Docker personnalisé est configuré.

**Validation SSL/TLS et des ports** :
Confirme que NGINX fonctionne uniquement sur le port 443 avec un certificat SSL/TLS valide.
S'assure que HTTP (port 80) est désactivé et redirigé vers HTTPS.


### Exemple de sortie
Après avoir exécuté make eval, vous devriez obtenir une sortie similaire à ceci :

```plaintext
[OK] Aucun identifiant critique trouvé dans .env.
[OK] Tous les Dockerfiles requis sont présents et valides.
[OK] docker-compose.yml ne contient pas de configurations interdites.
[OK] Le certificat SSL/TLS est valide et NGINX fonctionne sur le port 443.
[OK] Le site WordPress est correctement configuré sans exposition via HTTP.
```
En cas de problème, vous verrez des messages d'erreur détaillant les configurations manquantes ou incorrectes :

```plaintext
[Erreur] Le fichier `.env` est manquant ou incomplet.
[Erreur] NGINX est accessible via des ports autres que 443.
[Erreur] Aucun réseau Docker nommé 'inception' n'a été trouvé.
```
### Erreurs fréquentes et solutions
Voici quelques problèmes courants que vous pourriez rencontrer lors de l'exécution de make eval :

1. Fichier `.env` manquant
Message d'erreur :

```plaintext
[Erreur] `.env` est manquant dans srcs.
```
Solution : Assurez-vous de créer un fichier `.env` dans le répertoire srcs avec toutes les variables requises.

2. Dockerfile invalide
Message d'erreur :

```plaintext
[Erreur] Le Dockerfile pour nginx est manquant ou vide.
```
Solution : Vérifiez que chaque répertoire de service (ex : nginx, wordpress, mariadb) contient un Dockerfile valide et complet.

3. SSL/TLS non configuré
Message d'erreur :

```plaintext
[Erreur] Aucun certificat SSL/TLS détecté.
```
Solution : Assurez-vous que votre service NGINX est configuré avec un certificat SSL/TLS et fonctionne sur le port 443.

4. Configurations interdites dans docker-compose.yml
Message d'erreur :

```plaintext
[Erreur] docker-compose.yml contient 'network: host' ou '--link'.
```
Solution : Supprimez les configurations interdites dans votre fichier docker-compose.yml et assurez-vous d'utiliser un réseau Docker personnalisé.

### Contribution
Si vous souhaitez contribuer au projet, vous pouvez modifier le Makefile pour ajouter de nouveaux tests ou configurations. Par exemple, vous pouvez ajouter des vérifications pour de nouveaux services ou implémenter des validations de sécurité supplémentaires.

Pour modifier les tests, ouvrez le Makefile et éditez la cible eval. Les contributions sont les bienvenues, en particulier de la part des étudiants de l'école 42 !
