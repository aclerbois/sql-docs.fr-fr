---
title: Bien démarrer avec SQL Server 2017 sur Docker | Microsoft Docs
description: Ce démarrage rapide montre comment utiliser Docker pour exécuter l’image conteneur de SQL Server 2017. Ensuite, vous créez et interrogez une base de données avec sqlcmd.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 03/07/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.prod_service: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
ms.openlocfilehash: 6b28ac7d654d04f5e0998ecda31d16ec597f8d3d
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="quickstart-run-the-sql-server-2017-container-image-with-docker"></a>Démarrage rapide : Exécuter l’image de conteneur SQL Server 2017 avec Docker

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Dans ce démarrage rapide, vous utilisez Docker pour extraire et exécuter l’image conteneur de SQL Server 2017, [mssql-server-linux](https://hub.docker.com/r/microsoft/mssql-server-linux/). Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

Cette image est composée de SQL Server s’exécutant sur Linux basé sur Ubuntu 16.04. Elle peut être utilisée avec Docker Engine 1.8+ sur Linux ou sur Docker pour Mac/Windows.

> [!NOTE]
> Ce guide de démarrage rapide se concentre spécifiquement sur l’utilisation de l’image mssql-server-**linux**. L’image Windows n’est pas traitée, mais vous pouvez obtenir plus d’informations dans la [page du Hub Docker mssql-server-windows-developer](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

## <a id="requirements"></a> Configuration requise

- Moteur Docker 1.8 + sur n’importe quelle distribution de Linux prise en charge ou Docker pour Mac et Windows. Pour plus d’informations, consultez [Installer Docker](https://docs.docker.com/engine/installation/).
- Au moins 2 Go d’espace disque
- Au moins 2 Go de RAM
- [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

## <a name="pull-and-run-the-container-image"></a>Extraire et exécuter l’image conteneur

1. Extrayez l’image conteneur Linux de SQL Server 2017 à partir du Hub Docker.

   ```bash
   sudo docker pull microsoft/mssql-server-linux:2017-latest
   ```

   ```PowerShell
   docker pull microsoft/mssql-server-linux:2017-latest
   ```

   La commande précédente extrait la dernière image conteneur de SQL Server 2017. Si vous voulez extraire une image spécifique, ajoutez un signe deux-points et le nom de la balise (par exemple, `microsoft/mssql-server-linux:2017-GA`). Pour voir toutes les images disponibles, consultez [la page du hub Docker mssql-server-linux](https://hub.docker.com/r/microsoft/mssql-server-linux/tags/).
   
   Pour les commandes de l’interpréteur de commandes dans cet article, `sudo` est utilisé. Sur Mac OS, `sudo` ne soit requise. Sur Linux, si vous ne souhaitez pas utiliser `sudo` pour exécuter Docker, vous pouvez configurer un **docker** groupe et ajouter des utilisateurs à ce groupe. Pour plus d’informations, consultez [étapes de post-installation pour Linux](https://docs.docker.com/install/linux/linux-postinstall/).

1. Pour exécuter l’image conteneur avec Docker, vous pouvez utiliser la commande suivante à partir d’un interpréteur de commandes bash (Linux/macOS) ou d’une invite de commandes PowerShell avec élévation de privilèges.

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
      -p 1433:1433 --name sql1 \
      -d microsoft/mssql-server-linux:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
      -p 1433:1433 --name sql1 `
      -d microsoft/mssql-server-linux:2017-latest
   ```

   > [!NOTE]
   > Le mot de passe doit suivre la stratégie de mot de passe SQL Server par défaut, sinon le conteneur ne peut pas configurer SQL Server et s’arrête de fonctionner. Par défaut, le mot de passe doit avoir au moins 8 caractères appartenant à trois des quatre groupes suivants : lettres majuscules, lettres minuscules, chiffres de base 10 et symboles. Vous pouvez examiner le journal des erreurs en exécutant la commande [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).

   > [!NOTE]
   > Par défaut, cette commande crée un conteneur avec l’édition Développeur de SQL Server 2017. Le processus d’exécution des éditions de production dans des conteneurs est légèrement différent. Pour plus d’informations, consultez [Exécuter des images conteneur de production](sql-server-linux-configure-docker.md#production).

   Le tableau suivant décrit les paramètres de l’exemple `docker run` précédent :

   | Paramètre |  Description |
   |-----|-----|
   | **-e 'ACCEPT_EULA=Y'** |  Définissez la variable **ACCEPT_EULA** sur n’importe quelle valeur pour confirmer que vous acceptez le [Contrat de licence utilisateur final](http://go.microsoft.com/fwlink/?LinkId=746388). Paramètre obligatoire pour l’image de SQL Server. |
   | **-e ' SA_PASSWORD =\<YourStrong ! Passw0rd\>'** | Spécifiez votre propre mot de passe fort, qui doit avoir au moins 8 caractères et respecter les [exigences de mot de passe SQL Server](../relational-databases/security/password-policy.md). Paramètre obligatoire pour l’image de SQL Server. |
   | **p - 1433:1433** | Mappez un port TCP sur l’environnement hôte (première valeur) à un port TCP dans le conteneur (deuxième valeur). Dans cet exemple, SQL Server écoute sur TCP 1433 dans le conteneur, et ces informations sont exposées pour le port 1433, sur l’ordinateur hôte. |
   | **--name sql1** | Spécifiez un nom personnalisé pour le conteneur plutôt qu’un nom généré de manière aléatoire. Si vous exécutez plusieurs conteneurs, vous ne pouvez pas réutiliser le même nom. |
   | **microsoft/mssql-server-linux:2017-latest** | Image conteneur Linux de SQL Server 2017. |

1. Pour afficher vos conteneurs Docker, utilisez la commande `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

   Vous devez obtenir une sortie similaire à la capture d’écran suivante :

   ![Sortie de la commande Docker ps](./media/sql-server-linux-setup-docker/docker-ps-command.png)

1. Si la colonne **ÉTAT** affiche **En cours d’exécution**, SQL Server est en cours d’exécution dans le conteneur et écoute sur le port spécifié dans la colonne **PORTS**. Si la colonne **ÉTAT** pour votre conteneur SQL Server affiche **Quitté**, consultez la [section Résolution des problèmes dans le guide de configuration](sql-server-linux-configure-docker.md#troubleshooting).

Le paramètre `-h` (nom d’hôte) est également utile, mais il n’est pas utilisé dans ce didacticiel pour plus de simplicité. Il permet de changer le nom interne du conteneur en une valeur personnalisée. Il s’agit du nom retourné dans la requête Transact-SQL suivante :

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Pour identifier facilement le conteneur cible, définissez `-h` et `--name` sur la même valeur.

## <a name="change-the-sa-password"></a>Changer le mot de passe AS

[!INCLUDE [Change docker password](../includes/sql-server-linux-change-docker-password.md)]

## <a name="connect-to-sql-server"></a>Se connecter à SQL Server

La procédure suivante utilise l’outil en ligne de commande SQL Server, **sqlcmd**, dans le conteneur pour se connecter à SQL Server.

1. Utilisez la commande `docker exec -it` pour démarrer un interpréteur de commandes bash interactif dans votre conteneur en cours d’exécution. Dans l’exemple suivant, `sql1` est le nom spécifié par le paramètre `--name` quand vous avez créé le conteneur.

   ```bash
   sudo docker exec -it sql1 "bash"
   ```

   ```PowerShell
   docker exec -it sql1 "bash"
   ```

1. Une fois dans le conteneur, connectez-vous localement avec sqlcmd. Sqlcmd n’est pas dans le chemin par défaut, vous devez spécifier le chemin complet.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourNewStrong!Passw0rd>'
   ```

   > [!TIP]
   > Vous pouvez omettre le mot de passe sur la ligne de commande pour être invité à l’entrer.

1. Si l’opération réussit, vous devez accéder à une invite de commandes **sqlcmd** : `1>`.

## <a name="create-and-query-data"></a>Créer et interroger des données

Les sections suivantes vous guident lors de l’utilisation de **sqlcmd** et Transact-SQL pour créer une base de données, ajouter des données et exécuter une requête simple.

### <a name="create-a-new-database"></a>Créer une base de données

La procédure suivante crée une base de données nommée `TestDB`.

1. À partir de l’invite de commandes **sqlcmd**, collez la commande Transact-SQL suivante pour créer une base de données de test :

   ```sql
   CREATE DATABASE TestDB
   ```

1. Sur la ligne suivante, écrivez une requête pour retourner le nom de toutes les bases de données sur votre serveur :

   ```sql
   SELECT Name from sys.Databases
   ```

1. Les deux commandes précédentes n’ont pas été exécutées immédiatement. Vous devez taper `GO` sur une nouvelle ligne pour exécuter les commandes précédentes :

   ```sql
   GO
   ```

### <a name="insert-data"></a>Insérer des données

Créez ensuite une table, `Inventory`, et insérez deux nouvelles lignes.

1. À partir de l’invite de commandes **sqlcmd**, basculez le contexte vers la nouvelle base de données `TestDB` :

   ```sql
   USE TestDB
   ```

1. Créez une table nommée `Inventory` :

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

1. Insérez des données dans la nouvelle table :

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

1. Tapez `GO` pour exécuter les commandes précédentes :

   ```sql
   GO
   ```

### <a name="select-data"></a>Sélectionner les données

Exécutez maintenant une requête pour retourner des données de la table `Inventory`.

1. Dans l’invite de commandes **sqlcmd**, entrez une requête qui retourne les lignes de la table `Inventory` dont la quantité est supérieure à 152 :

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

1. Exécutez la commande :

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Quitter l’invite de commandes sqlcmd

1. Pour mettre fin à votre session **sqlcmd**, tapez `QUIT` :

   ```sql
   QUIT
   ```

1. Pour quitter l’invite de commandes interactive dans votre conteneur, tapez `exit`. Le conteneur continue de s’exécuter une fois que vous avez quitté l’interpréteur de commandes bash interactif.

## <a id="connectexternal"></a> Se connecter en dehors du conteneur

Vous pouvez aussi vous connecter à l’instance SQL Server sur votre machine Docker à partir de n’importe quel outil externe Linux, Windows ou macOS qui prend en charge les connexions SQL.

Les étapes suivantes utilisent **sqlcmd** en dehors de votre conteneur pour se connecter à SQL Server en cours d’exécution dans le conteneur. Ces étapes supposent que vous avez déjà installé les outils en ligne de commande SQL Server en dehors de votre conteneur. Les mêmes principes s’appliquent quand vous utilisez d’autres outils, mais le processus de connexion est propre à chaque outil.

1. Recherchez l’adresse IP de la machine qui héberge votre conteneur. Sur Linux, utilisez **ifconfig** ou **ip addr**. Sur Windows, utilisez **ipconfig**.

1. Exécutez sqlcmd en spécifiant l’adresse IP et le port mappé au port 1433 dans votre conteneur. Dans cet exemple, qui est le même port 1433, sur l’ordinateur hôte. Si vous avez spécifié un autre port mappé sur l’ordinateur hôte, vous devez l’utiliser ici.

   ```bash
   sqlcmd -S 10.3.2.4,1433 -U SA -P '<YourNewStrong!Passw0rd>'
   ```

   ```PowerShell
   sqlcmd -S 10.3.2.4,1433 -U SA -P "<YourNewStrong!Passw0rd>"
   ```

1. Exécutez les commandes Transact-SQL. Quand vous avez terminé, tapez `QUIT`.

Voici d’autres outils courants pour vous connecter à SQL Server :

- [Visual Studio Code](sql-server-linux-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) sur Windows](sql-server-linux-manage-ssms.md)
- [SQL Server Operations Studio (préversion)](../sql-operations-studio/what-is.md)
- [mssql-cli (préversion)](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/)

## <a name="remove-your-container"></a>Supprimer votre conteneur

Si vous voulez supprimer le conteneur SQL Server utilisé dans ce didacticiel, exécutez les commandes suivantes :

```bash
sudo docker stop sql1
sudo docker rm sql1
```

```PowerShell
docker stop sql1
docker rm sql1
```

> [!WARNING]
> L’arrêt et la suppression d’un conteneur supprime définitivement toutes les données SQL Server dans le conteneur. Si vous avez besoin de conserver vos données, [créez et copiez un fichier de sauvegarde en dehors du conteneur](tutorial-restore-backup-in-sql-server-container.md) ou utilisez une [technique de persistance de données de conteneur](sql-server-linux-configure-docker.md#persist).

## <a name="docker-demo"></a>Démonstration Docker

Une fois que vous avez essayé d’utiliser l’image conteneur de SQL Server pour Docker, vous pouvez être intéressé par des informations sur l’utilisation de Docker pour améliorer le développement et les tests. La vidéo suivante illustre l’utilisation de Docker dans une intégration continue et un scénario de déploiement.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T152/player]

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un didacticiel sur la restauration de fichiers de sauvegarde de base de données dans un conteneur, consultez [Restaurer une base de données SQL Server dans un conteneur Linux Docker](tutorial-restore-backup-in-sql-server-container.md). Pour explorer d’autres scénarios, comme l’exécution de plusieurs conteneurs, la persistance des données et la résolution des problèmes, consultez [Configurer des images conteneur de SQL Server 2017 sur Docker](sql-server-linux-configure-docker.md).

Consultez également le [dépôt GitHub mssql-docker](https://github.com/Microsoft/mssql-docker) pour obtenir des ressources, des commentaires et les problèmes connus.
