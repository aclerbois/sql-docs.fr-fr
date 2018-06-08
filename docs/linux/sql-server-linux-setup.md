---
title: Guide d’installation pour 2017 du serveur SQL sur Linux | Documents Microsoft
description: Installer, mettre à jour et désinstaller SQL Server sur Linux. Cet article traite des scénarios en ligne, hors connexion et sans assistance.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 04/06/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: bbf781d365174042f9358fd1e78a26d916f81f99
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Aide à l’installation de SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cette rubrique explique comment installer, mettre à jour et désinstaller SQL Server 2017 sur Linux.

> [!TIP]
> Ce guide coves plusieurs scénarios de déploiement. Si vous cherchez seulement pour obtenir des instructions d’installation pas à pas, passez à un des Démarrages rapides suivants :
> - [Démarrage rapide RHEL](quickstart-install-connect-red-hat.md)
> - [Démarrage rapide SLES](quickstart-install-connect-suse.md)
> - [Démarrage rapide d’Ubuntu](quickstart-install-connect-ubuntu.md)
> - [Démarrage rapide de docker](quickstart-install-connect-docker.md)

Pour obtenir des réponses aux questions fréquemment posées, consultez le [SQL Server sur le Forum aux questions sur Linux](../linux/sql-server-linux-faq.md).

## <a id="supportedplatforms"></a> Plateformes prises en charge

SQL Server 2017 est pris en charge sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) et Ubuntu. Il est également disponible sous une image Docker qui peut s’exécuter sur le moteur Docker sur Linux ou Docker pour Windows/Mac.

| Plateforme | Versions prises en charge | Obtenir
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3 ou 7.4 | [Obtenir RHEL 7.4](http://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 SP2 | [Obtenir SLES v12 SP2](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [Obtenir Ubuntu 16.04](http://www.ubuntu.com/download/server)
| **Moteur docker** | 1.8+ | [Obtenir Docker](http://www.docker.com/products/overview)

Microsoft prend également en charge le déploiement et la gestion des conteneurs de SQL Server à l’aide de OpenShift et Kubernetes.

> [!NOTE]
> SQL Server est testé et pris en charge sous Linux pour les distributions précédemment répertoriées. Si vous choisissez d’installer SQL Server sur un système d’exploitation non pris en charge, passez en revue les **prennent en charge de la stratégie** section de la [politique de support technique pour Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) pour comprendre la prise en charge implications.

## <a id="system"></a> Configuration système requise

SQL Server 2017 a les prérequis suivants pour Linux :

|||
|-----|-----|
| **Mémoire** | 2 Go |
| **Système de fichiers** | **XFS** ou **EXT4** (autres systèmes de fichiers, tel que **BTRFS**, non pris en charge) |
| **Espace disque** | 6 GO |
| **Vitesse du processeur** | 2 GHz |
| **Cœurs de processeur** | 2 cœurs |
| **Type de processeur** | x64 compatibles uniquement |

Si vous utilisez les partages distants **NFS (Network File System)** en production, notez les exigences de prise en charge suivantes :

- Utiliser la version NFS **4.2 ou ultérieure**. Les versions antérieures de NFS ne gèrent pas les fonctionnalités requises, telles que fallocate et la création de fichier sparse, courantes avec les systèmes de fichiers modernes.
- Positionnez uniquement les répertoires **/var/opt/mssql** sur le montage NFS. Les autres fichiers, tels que les fichiers binaires du système SQL Server, ne sont pas pris en charge.
- Assurez-vous que les clients NFS utilisent l’option 'nolock' lorsque qu'ils montent le partage distant.

## <a id="repositories"></a> Configurer des référentiels de code source

Lorsque vous installez ou mettez à niveau de SQL Server, vous obtenez la dernière version de SQL Server 2017 à partir de votre référentiel de Microsoft. Les Démarrages rapides utilisent le **mise à jour Cumulative (CU)** référentiel. Mais vous pouvez configurer à la place la **GDR** référentiel. Pour plus d’informations sur les référentiels et comment les configurer, consultez [configurer des référentiels pour SQL Server sur Linux](sql-server-linux-change-repo.md).

> [!IMPORTANT]
> Si vous avez installé précédemment la version RC de SQL Server 2017 de CTP, vous devez supprimer le référentiel d’aperçu et inscrire une disponibilité générale un. Pour plus d’informations, consultez [configurer des référentiels pour SQL Server sur Linux](sql-server-linux-change-repo.md).

## <a id="platforms"></a> Installation de SQL Server

Vous pouvez installer SQL Server sur Linux à partir de la ligne de commande. Pour obtenir des instructions, consultez les Démarrages rapides suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-docker.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%2fsql%2flinux%2ftoc.json)

## <a id="upgrade"></a> Mettre à jour de SQL Server

Pour mettre à jour le package **mssql-serveur** vers la dernière version, utilisez une des commandes suivantes en fonction de votre plateforme :

| Plateforme | Commandes de mise à jour de package |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

Ces commandes téléchargent le package le plus récent et remplacent les fichiers binaires situés sous `/opt/mssql/`. Les bases de données généré par l’utilisateur et les bases de données système ne sont pas affectées par cette opération.

## <a id="rollback"></a> Restauration SQL Server

Pour restaurer ou rétrograder SQL Server vers une version précédente, procédez comme suit :

1. Identifiez le numéro de version pour le package de SQL Server que vous souhaitez rétrograder. Pour obtenir la liste de numéros de versions de package, consultez les [notes de publication](sql-server-linux-release-notes.md).

1. Passez à une version antérieure de SQL Server. Dans les commandes suivantes, remplacez `<version_number>` par le numéro de version SQL Server que vous avez identifié à l’étape 1.

   | Plateforme | Commandes de mise à jour de package |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> Il est uniquement pris en charge de mettre à niveau vers une version avec la même version majeure, telle que SQL Server 2017.

## <a id="versioncheck"></a> Vérifiez la version installée de SQL Server

Pour vérifier votre version actuelle et l’édition de SQL Server sur Linux, utilisez la procédure suivante :

1. Si ce n'est déjà fait, installez les [les outils de ligne de SQL Server](sql-server-linux-setup-tools.md).

1. Utilisez **sqlcmd** pour exécuter une commande Transact-SQL qui affiche la version de SQL Server et l’édition.

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a id="uninstall"></a> Désinstaller SQL Server

Pour supprimer le package **mssql-server** sous Linux, utilisez une des commandes suivantes en fonction de votre plateforme :

| Plateforme | Commandes de suppression de package |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

La suppression du package ne supprime pas les fichiers de base de données. Si vous souhaitez supprimer les fichiers de base de données, utilisez la commande suivante :

```bash
sudo rm -rf /var/opt/mssql/
```

## <a id="unattended"></a> Installation sans assistance

Vous pouvez effectuer une installation sans assistance de la manière suivante :

- Suivez les étapes initiales dans les [Démarrages rapides](#platforms) pour inscrire les référentiels et installez SQL Server.
- Lorsque vous exécutez `mssql-conf setup`, définissez les [variables d’environnement](sql-server-linux-configure-environment-variables.md)et utilisez l'option `-n` (sans invite) .

L’exemple suivant configure l’édition Developer de SQL Server avec la variable d'environnement **MSSQL_PID**. Elle accepte également le CLUF (**ACCEPT_EULA**) et définit le mot de passe SA (**MSSQL_SA_PASSWORD**). Le `-n` effectue une installation sans invite où les valeurs de configuration sont extraites des variables d’environnement.

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

Vous pouvez également créer un script qui exécute d’autres actions. Par exemple, vous pouvez installer d’autres packages SQL Server.

Pour un exemple de script plus détaillé, consultez les exemples suivants :

- [Red Hat script d’installation sans assistance](sample-unattended-install-redhat.md)
- [SUSE script d’installation sans assistance](sample-unattended-install-suse.md)
- [Ubuntu script d’installation sans assistance](sample-unattended-install-ubuntu.md)

## <a id="offline"></a> Installation hors connexion

Si l’ordinateur Linux n’a pas accès aux référentiels en ligne utilisés dans les [Démarrages rapides](#platforms), vous pouvez télécharger directement les fichiers de package. Ces packages se trouvent dans le référentiel Microsoft, [ https://packages.microsoft.com ](https://packages.microsoft.com).

> [!TIP]
> Si vous avez installé avec succès avec les étapes décrites dans le démarrage rapide, il est inutile télécharger ou installer manuellement l’ou les packages SQL Server. Cette section concerne uniquement le scénario hors connexion.

1. **Téléchargez le package de moteur de base de données pour votre plateforme**. Recherchez les liens de téléchargement de package dans la section des détails du package dans les [Notes de publication](sql-server-linux-release-notes.md).

1. **Déplacez le package téléchargé sur votre ordinateur Linux**. Si vous avez utilisé un autre ordinateur pour télécharger les packages, déplacez les packages vers l’ordinateur Linux avec la commande **scp**.

1. **Installer le package de moteur de base de données**. Utilisez une des commandes suivantes en fonction de votre plateforme. Remplacez le nom du fichier de package dans cet exemple par le nom exact que vous avez téléchargé.

   | Plateforme | Commande d’installation de package |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > Vous pouvez également installer les packages RPM (RHEL et SLES) avec la commande `rpm -ivh` commande, mais les commandes du tableau précédent  installent automatiquement les dépendances si elles sont disponibles à partir de référentiels approuvés.

1. **Résoudre les dépendances manquantes** : vous pouvez avoir des dépendances manquantes à ce stade. Si ce n’est pas le cas, vous pouvez ignorer cette étape. Sur Ubuntu, si vous avez accès à des référentiels approuvés contenant ces dépendances, la solution la plus simple consiste à utiliser la commande `apt-get -f install`. Cette commande termine également l’installation de SQL Server. Pour examiner les dépendances manuellement, utilisez les commandes suivantes :

   | Plateforme | Commande de dépendances de liste |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   Après avoir résolu les dépendances manquantes, essayez de réinstaller le package mssql-server.

1. **Terminer l’installation de SQL Server**. Utiliser **mssql-conf** pour terminer l’installation de SQL Server :

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="optional-sql-server-features"></a>Fonctionnalités facultatives de SQL Server

Après l’installation, vous pouvez également installer ou activer les fonctionnalités facultatives de SQL Server.

- [Outils de ligne de commande de SQL Server](sql-server-linux-setup-tools.md)
- [SQL Server Agent](sql-server-linux-setup-sql-agent.md)
- [Recherche de texte intégral SQL Server](sql-server-linux-setup-full-text-search.md)
- [SQL Server Integration Services (Ubuntu)](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> Pour obtenir des réponses aux questions fréquemment posées, consultez le [SQL Server sur le Forum aux questions sur Linux](sql-server-linux-faq.md).
