---
title: Prise en main 2017 du serveur SQL sur Ubuntu | Documents Microsoft
description: Ce démarrage rapide montre comment installer SQL Server 2017 sur Ubuntu puis créer et interroger une base de données avec sqlcmd.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 02/22/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: ebe7da1e1024cefc14c52d0a02e0517b764c8d07
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-ubuntu"></a>Démarrage rapide : Installer SQL Server et créer une base de données sur Ubuntu

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce guide de démarrage rapide, vous d’abord installez SQL Server 2017 sur Ubuntu 16.04. Puis vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

> [!TIP]
> Ce didacticiel nécessite une saisie de la part de l’utilisateur et une connexion Internet. Si vous êtes intéressé par les procédures d'installation [sans assistance](sql-server-linux-setup.md#unattended) ou [hors connexion](sql-server-linux-setup.md#offline), consultez [aide à l’installation de SQL Server sur Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Configuration requise

Vous devez disposer d’un ordinateur Ubuntu 16.04 avec **au moins 2 Go** de mémoire.

Pour installer Ubuntu sur votre propre ordinateur, accédez à [ http://www.ubuntu.com/download/server ](http://www.ubuntu.com/download/server). Vous pouvez également créer des machines virtuelles de Ubuntu dans Azure. Consultez [créer et gérer les machines virtuelles Linux avec l’interface CLI Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> À ce stade, le [sous-système Windows pour Linux](https://msdn.microsoft.com/commandline/wsl/about) pour Windows 10 n’est pas pris en charge comme cible d’installation.

Pour les autres exigences système, consultez [configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

## <a id="install"></a>Installation de SQL Server

Pour configurer SQL Server sur Ubuntu, exécutez les commandes suivantes dans un terminal pour installer le package **mssql-serveur**.

> [!IMPORTANT]
> Si vous avez déjà installé une version CTP ou RC de SQL Server 2017, vous devez d’abord supprimer l’ancien référentiel avant d’inscrire un des référentiels de la disponibilité générale (GA). Pour plus d’informations, consultez [passer du référentiel de version préliminaire au référentiel de disponibilité générale](sql-server-linux-change-repo.md).

1. Importer les clés GPG référentiel public :

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Enregistrer le référentiel Microsoft SQL Server Ubuntu :

   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

   > [!NOTE]
   > Il s’agit du référentiel de mise à jour cumulative (CU). Pour plus d’informations sur les options de votre référentiel et leurs différences, consultez [modifier les référentiels sources](sql-server-linux-change-repo.md).

1. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

1. Après la fin de l’installation du package, exécutez le programme d’installation **mssql-conf** et suivez les invites pour définir le mot de passe SA et choisir votre édition.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Si vous testez SQL Server 2017 dans ce didacticiel, les éditions suivantes sont concédées librement sous licence : Evaluation, Developer et Express.

   > [!NOTE]
   > Veillez à spécifier un mot de passe fort pour le compte d’administrateur système (longueur minimale de 8 caractères, incluant des majuscules et des minuscules, des chiffres et/ou des caractères spéciaux).

1. Une fois la configuration terminée, vérifiez que le service est en cours d’exécution :

   ```bash
   systemctl status mssql-server
   ```

1. Si vous envisagez de vous connecter à distance, vous devrez peut-être également ouvrir le port TCP du serveur SQL (1433 par défaut) sur votre pare-feu.

À ce stade, SQL Server est en cours d’exécution sur votre ordinateur Ubuntu et est prêt à être utilisé !

## <a id="tools"></a>Installer les outils de ligne de commande SQL Server

Pour créer une base de données, vous devez vous connecter avec un outil qui peut exécuter des instructions Transact-SQL sur le serveur SQL Server. Les étapes suivantes permettent d'installer les outils de ligne de commande de SQL Server : [sqlcmd](../tools/sqlcmd-utility.md) et [bcp](../tools/bcp-utility.md).

Utilisez les étapes suivantes pour installer le package **mssql-tools** sur Ubuntu.  

1. Importez les clés publiques GPG de référentiel.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Inscrire le référentiel Microsoft Ubuntu.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Mettre à jour la liste des sources et exécutez la commande d’installation avec le kit du développeur unixODBC.

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Pour mettre à jour vers la dernière version de **mssql-tools** exécutez les commandes suivantes :
   >    ```bash
   >   sudo apt-get update 
   >   sudo apt-get install mssql-tools 
   >   ```

1. **Facultatif**: ajoutez `/opt/mssql-tools/bin/` à votre variable d'environnement de **chemin d’accès** dans un interpréteur de commandes bash.

   Pour rendre **sqlcmd et bcp** accessible à partir de l’interpréteur de commandes pour les sessions de connexion, modifiez votre **chemin d’accès** dans le fichier **~/.bash_profile**  avec la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Pour rendre **sqlcmd/bcp** accessible à partir de l’interface de l’interpréteur de commandes pour les sessions interactives/de non-connexion, modifiez le **chemin d’accès** dans le fichier **~/.bashrc** avec la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

> [!TIP]
> **SQLCMD** est un outil unique pour se connecter à SQL Server afin d'exécuter des requêtes et d'effectuer des tâches de gestion et de développement. D'autres outils incluent :
>
> * [SQL Server Operations Studio (préversion)](../sql-operations-studio/what-is.md)
> * [SQL Server Management Studio](sql-server-linux-manage-ssms.md)
> * [Visual Studio Code](sql-server-linux-develop-use-vscode.md).
> * [mssql-cli (préversion)](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/)

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
