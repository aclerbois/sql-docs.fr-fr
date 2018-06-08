---
title: Configurer des référentiels pour SQL Server sur Linux | Documents Microsoft
description: Vérifier et configurer les référentiels sources pour 2017 du serveur SQL sur Linux. Le référentiel source a une incidence sur la version de SQL Server qui est appliquée pendant l’installation et mise à niveau.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 02/14/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 63cb2f56852a668bc48aa85cd31a72a2b583463b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-repositories-for-installing-and-upgrading-sql-server-on-linux"></a>Configurez des référentiels pour l’installation et la mise à niveau de SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article décrit comment configurer le référentiel correct pour les mises à niveau et les installations de SQL Server 2017 sur Linux.

> [!IMPORTANT]
> Si vous avez installé précédemment la version RC de SQL Server 2017 de CTP, vous devez utiliser les étapes de cet article pour inscrire un dépôt de la disponibilité générale (GA) et mettre à niveau ou réinstaller. Les versions préliminaires de SQL Server 2017 ne sont pas prises en charge et va expirer.

## <a id="repositories"></a>Référentiels

Lorsque vous installez SQL Server sur Linux, vous devez configurer un référentiel de Microsoft. Ce référentiel est utilisé pour acquérir le package du moteur de base de données, **mssql-serveur**et les packages SQL Server. Il existe actuellement trois référentiels principales :

| Référentiel | Nom |  Description |
|---|---|---|
| **Aperçu** | **mssql-server** | Référentiel d’aperçu pour les versions RC et CTP de SQL Server. Ce référentiel n’est pas pris en charge pour SQL Server 2017. |
| **CU** | **mssql-server-2017** | Référentiel de SQL Server 2017 Cumulative Update (CU). |
| **GDR** | **mssql-server-2017-gdr** | Référentiel de SQL Server 2017 GDR uniquement les mises à jour critiques. |

## <a id="cuversusgdr"></a> Mise à jour cumulative et GDR

Il est important de noter qu’il existe deux principaux types de référentiels pour chaque point de distribution :

- **Les mises à jour cumulative (CU)**: référentiel de la mise à jour Cumulative (CU) contient des packages pour la version de SQL Server de base et tous les correctifs de bogues ou améliorations apportées depuis cette version. Mises à jour cumulatives sont spécifiques à une version release, telles que SQL Server 2017. Ils sont publiés sur une cadence régulière.

- **GDR**: référentiel du GDR contient des packages pour la base version de SQL Server et uniquement les correctifs critiques et les mises à jour de sécurité depuis cette version. Ces mises à jour sont également ajoutés à la prochaine version CU.

Chaque version de CU et correctif logiciel grand public contient le package SQL Server complète et toutes les mises à jour précédentes pour ce référentiel. Mise à jour à partir d’une version GDR vers une version CU prend en charge la modification de votre référentiel configuré pour SQL Server. Vous pouvez également [rétrograder](sql-server-linux-setup.md#rollback) à n’importe quelle version dans votre version principale (ex : 2017).

> [!NOTE]
> Vous pouvez mettre à jour à partir d’une version GDR CU libérer à tout moment en modifiant les référentiels. Mise à jour à partir d’une CU version à une version de correctif logiciel grand public n’est pas pris en charge. 

## <a id="configure"></a> Configurer un référentiel

Les sections suivantes décrivent comment vérifier et configurer un référentiel pour les plateformes prises en charge suivantes :

- [Red Hat Enterprise Server](#rhel)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#sles)

## <a id="rhel"></a> Configurez des référentiels RHEL
Utilisez les étapes suivantes pour configurer des référentiels sur Red Hat Enterprise Server (RHEL).

### <a name="check-for-previously-configured-repositories-rhel"></a>Recherchez les référentiels configurées précédemment (RHEL)
Commencez par vérifier si vous avez déjà enregistré un référentiel SQL Server.

1. Afficher les fichiers dans le **/etc/yum.repos.d** répertoire avec la commande suivante :

   ```bash
   sudo ls /etc/yum.repos.d
   ```

2. Recherchez le fichier qui configure le répertoire de SQL Server, tels que **mssql-server.repo**.

3. Imprimer le contenu du fichier.

   ```bash
   sudo cat /etc/yum.repos.d/mssql-server.repo
   ```

4. La propriété **name** est le réertoire configuré. Vous pouvez l’identifier la table dans le [référentiels](#repositories) section de cet article.

### <a name="remove-old-repository-rhel"></a>Supprimer l’ancien référentiel (RHEL)
Si nécessaire, supprimez l’ancien référentiel avec la commande suivante.

```bash
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Cette commande suppose que le fichier identifié dans la section précédente a été nommé **mssql-server.repo**.

### <a name="configure-new-repository-rhel"></a>Configurer le nouveau référentiel (RHEL)
Configurez le nouveau référentiel à utiliser pour les mises à niveau et les installations de SQL Server. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

| Référentiel | Command |
|---|---|
| **CU** | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo` |
| **GDR** | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017-gdr.repo` |

## <a id="sles"></a> Configurez des référentiels SLES
Utilisez les étapes suivantes pour configurer des référentiels SLES.

### <a name="check-for-previously-configured-repositories-sles"></a>Recherchez les référentiels configurées précédemment (SLES)
Commencez par vérifier si vous avez déjà enregistré un référentiel SQL Server.

1. Utilisez **zypper informations** pour obtenir des informations sur un référentiel préconfiguré.

   ```bash
   sudo zypper info mssql-server
   ```

2. Le **référentiel** propriété est le référentiel. Vous pouvez l’identifier la table dans le [référentiels](#repositories) section de cet article.

### <a name="remove-old-repository-sles"></a>Supprimer l’ancien référentiel (SLES)
Si nécessaire, supprimez l’ancien espace de stockage. Utilisez une des commandes suivantes en fonction du type de référentiel précédemment configuré.

| Référentiel | Commande pour supprimer |
|---|---|
| **Aperçu** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server'` |
| **CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017'` |
| **GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017-gdr'`|

### <a name="configure-new-repository-sles"></a>Configurer le nouveau référentiel (SLES)
Configurez le nouveau référentiel à utiliser pour les mises à niveau et les installations de SQL Server. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

| Référentiel | Command |
|---|---|
| **CU** | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo` |
| **GDR** | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017-gdr.repo` |

## <a id="ubuntu"></a> Configurez des référentiels Ubuntu
Utilisez les étapes suivantes pour configurer des référentiels sur Ubuntu.

### <a name="check-for-previously-configured-repositories-ubuntu"></a>Recherchez les référentiels configurées précédemment (Ubuntu)
Commencez par vérifier si vous avez déjà enregistré un référentiel SQL Server.

1. Afficher le contenu de la **/etc/apt/sources.list** fichier.

   ```bash
   sudo cat /etc/apt/sources.list
   ```

2. Examinez l’URL du package pour mssql-serveur. Vous pouvez l’identifier la table dans le [référentiels](#repositories) section de cet article.

### <a name="remove-old-repository-ubuntu"></a>Supprimer l’ancien référentiel (Ubuntu)
Si nécessaire, supprimez l’ancien espace de stockage. Utilisez une des commandes suivantes en fonction du type de référentiel précédemment configuré.

| Référentiel | Commande pour supprimer |
|---|---|
| **Aperçu** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server xenial main'` 
| **CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017 xenial main'` | 
| **GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr xenial main'` |

### <a name="configure-new-repository-ubuntu"></a>Configurer le nouveau référentiel (Ubuntu)
Configurez le nouveau référentiel à utiliser pour les mises à niveau et les installations de SQL Server.

1. Importez les clés publiques GPG de référentiel.

   ```bash
   sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

   | Référentiel | Command |
   |---|---|
   | **CU** | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"` |
   | **GDR** | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017-gdr.list)"` |

3. Exécutez **apt-get mise à jour**.

   ```bash
   sudo apt-get update
   ```

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré le référentiel approprié, vous pouvez passer à [installer](sql-server-linux-setup.md#platforms) ou [mettre à jour](sql-server-linux-setup.md#upgrade) SQL Server et les liés les packages à partir du référentiel de nouveau.

> [!IMPORTANT]
> À ce stade, si vous choisissez d’utiliser un des articles de l’installation, tel que le [Démarrages rapides](sql-server-linux-setup.md#platforms), souvenez-vous que vous avez déjà configuré le référentiel cible. Ne répétez pas cette étape dans les didacticiels. Cela est particulièrement vrai si vous configurez le référentiel GDR, étant donné que les Démarrages rapides utilisent le référentiel CU.

Pour plus d’informations sur l’installation de SQL Server 2017 sur Linux, consultez [aide à l’Installation de SQL Server sur Linux](sql-server-linux-setup.md).
