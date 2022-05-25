---
title: Configurer ownCloud avec Object Storage
excerpt: Configurer ownCloud avec Object Storage
slug: configure_owncloud_with_object_storage
section: Object Storage Standard (Swift)
order: 170
---

**Dernière mise à jour le 22/05/2022**

## Objectif

[ownCloud](https://owncloud.org/) est une application de stockage et de gestion de fichiers en ligne.
Cette solution offre plusieurs fonctionnalités, dont la synchronisation entre plusieurs appareils. Vous pouvez également ajouter du stockage externe comme l'Object Storage d'OpenStack.

**Ce guide explique comment configurer votre ownCloud avec Object Storage.**


## Prérequis

- Le fichier OpenRC, obtenu depuis [l'espace client OVHcloud](https://docs.ovh.com/fr/public-cloud/charger-les-variables-denvironnement-openstack/) ou [Horizon](https://docs.ovh.com/fr/public-cloud/presentation-dhorizon/)
- [Espacede stockage](https://docs.ovh.com/fr/storage/pcs/creation-de-conteneur/) dédié à ownCloud


## Instructions

### Installation

Vous devez d'abord installer ownCloud :

```bash
root@instance:~$ apt install owncloud
```

> [!primary]
>
> Assurez-vous que le référentiel que vous utilisez contient la dernière version de ownCloud. 
>

Pour fonctionner, OwnCloud doit disposer d'une base de données MySQL. Si vous n’en avez pas, installez-le en exécutant la commande suivante :

```bash
root@instance:~$ apt install mysql-server
```

### Configuration

Pour configurer la base de données qui sera utilisée par ownCloud, connectez-vous à votre serveur MySQL avec le mot de passe root défini lors de l'installation du serveur :


```bash
root@instance:~$ mysql -u root -p
```

À ce stade, vous pouvez créer un nouvel utilisateur et une base de données dédiée à ownCloud :

```sql
***** Create user *****
mysql> CREATE USER 'owncloud'@'localhost' IDENTIFIED BY 'P@ssw0rd';

***** Create database *****
mysql> CREATE DATABASE `owncloud` ;

***** Grant all privileges on "ownCloud" to the "owncloud" database *****
mysql> GRANT ALL PRIVILEGES ON `owncloud` . * TO 'owncloud'@'localhost';
```

Connectez-vous à ownCloud sur votre navigateur en saisissant : `http://serverIP/owncloud` :

![ownCloud](images/img_3325.jpg){.thumbnail}

Dans cette interface :

- Créer un compte administrateur
- Renseignez le répertoire de données (facultatif, si vous souhaitez simplement utiliser l'Object Storage, vous pouvez laisser celui par défaut).
- Entrez les informations d'identification de votre base.


Après avoir validé l'opération, vous pouvez accéder à votre interface OwnCloud et activer l'application qui vous permet d'ajouter un support de stockage externe.

Pour ce faire, cliquez sur `File`{.action} en haut à gauche et sélectionnez `Applications`{.action} :

![ownCloud](images/img_3327.jpg){.thumbnail}

Activez ensuite l'application `External storage support`{.action} à partir du menu Applications `Disabled`.

![ownCloud](images/img_3328.jpg){.thumbnail}

Ceci fait, configurez cette application en cliquant sur votre identifiant en haut à droite et en sélectionnant `Admin`{.action} :

![ownCloud](images/img_3326.jpg){.thumbnail}

Dans le menu `External storage`, sélectionnez `Add storage`{.action} et `OpenStack Object Storage`{.action} :

![ownCloud](images/img_3329.jpg){.thumbnail}

Saisissez les informations de votre fichier OpenRC :

- Votre identifiant Horizon qui correspond au champ "OS_USERNAME" du fichier OpenRC
- Le nom de votre container que vous avez créé précédemment pour ownCloud
- La région dans laquelle se trouve votre conteneur : "OS_REGION_NAME"
- Le nom du tenant, correspondant au champ "OS_TENANT_NAME"
- Votre mot de passe Horizon
- Le nom du service correspondant à "Swift"
- L'adresse du point de terminaison, correspondant au champ "OS_AUTH_URL" ou "https://auth.cloud.ovh.net/v2.0"


La "API key" et le "Maximum waiting time" sont optionnels.

> [!primary]
>
> Le conteneur que vous avez créé doit être entièrement dédié à ownCloud car l'application utilisera des métadonnées.
>
> Une fois que vous avez entré toutes les informations et vérifié qu'elles sont correctes, la pastille rouge en regard du nom de votre dossier devient verte et est disponible dans la section `Stockage` externe de votre page d'accueil :
>


![ownCloud](images/img_3330.jpg){.thumbnail}


## Aller plus loin
 
Échangez avec notre communauté d'utilisateurs sur <https://community.ovh.com/>.
