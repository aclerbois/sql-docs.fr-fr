---
title: Utilisez SSMS pour gérer SQL Server sur Linux | Documents Microsoft
description: ''
author: rothja
ms.author: jroth
manager: craigg
ms.date: 05/21/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.technology: linux
ms.assetid: b2fcf858-21c3-462a-8d49-50c85647d092
ms.custom: sql-linux
ms.openlocfilehash: 2b6293e7c0d80eb1ebe02d6cd03f17626d793c05
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="use-sql-server-management-studio-on-windows-to-manage-sql-server-on-linux"></a>Utilisez SQL Server Management Studio sur Windows pour gérer SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article présente [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) et présente quelques tâches courantes. SSMS étant une application Windows, utilisez SSMS quand vous avez un ordinateur Windows qui peut se connecter à une instance SQL Server distante sur Linux.

> [!TIP]
> Si vous n’avez pas un ordinateur Windows pour exécuter SSMS sur, prendre en compte la nouvelle [SQL Server Operations Studio](../sql-operations-studio/index.md). Il fournit un outil graphique pour la gestion de SQL Server et s’exécute sur Linux et Windows.

[SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) fait partie d’une suite d’outils SQL des offres Microsoft gratuitement pour vos besoins de développement et de gestion. SSMS est un environnement intégré pour accéder à configurer, gérer, administrer et développer tous les composants de SQL Server. Il peut se connecter à SQL Server en cours d’exécution sur n’importe quelle plateforme localement, dans des conteneurs Docker et dans le cloud. Il se connecte également à la base de données SQL Azure et Azure SQL Data Warehouse. SSMS associe un groupe d’outils graphiques avec un nombre d’éditeurs de script performants pour permettre aux développeurs et aux administrateurs de tous les niveaux de compétence accès à SQL Server.

SSMS offre un large éventail de fonctionnalités de développement et de gestion pour SQL Server, notamment pour :

- Configurer, surveiller et administrer unique ou plusieurs instances de SQL Server
- déployer, surveiller et mettre à niveau les composants de la couche données tels que des entrepôts de données et les bases de données
- sauvegarde et restauration des bases de données
- générer et exécuter des scripts et des requêtes T-SQL et afficher les résultats
- générer des scripts T-SQL pour les objets de base de données
- afficher et modifier des données dans les bases de données
- Concevoir visuellement des requêtes T-SQL et les objets de base de données tels que les vues, les tables et procédures stockées

Consultez [What ' s SSMS ?](../ssms/sql-server-management-studio-ssms.md) pour plus d’informations sur SSMS.

## <a name="install-the-newest-version-of-sql-server-management-studio-ssms"></a>Installer la version la plus récente de SQL Server Management Studio (SSMS)

Lorsque vous travaillez avec SQL Server, vous devez toujours utiliser la dernière version de SQL Server Management Studio (SSMS). La dernière version de SSMS est continuellement mis à jour et optimisées et est actuellement compatible avec SQL Server 2017 Linux on. Pour télécharger et installer la version la plus récente, consultez [télécharger SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Pour rester à jour, la dernière version de SSMS vous invite lorsqu’il existe une nouvelle version disponible en téléchargement.

> [!NOTE]
> Avant de gérer Linux à l’aide de SSMS, passez en revue les [problèmes connus](sql-server-linux-release-notes.md) pour SSMS sur Linux.

## <a name="connect-to-sql-server-on-linux"></a>Se connecter à SQL Server sur Linux

Utilisez les étapes de base suivantes pour vous connecter :

1. Démarrez SSMS en tapant **Microsoft SQL Server Management Studio** dans la zone de recherche Windows, puis cliquez sur l'application de bureau.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png)

1. Dans la fenêtre **Se connecter au serveur**, entrez les informations suivantes (si SSMS est déjà en cours d'exécution, cliquez sur **Connecter > Moteur de base de données** pour ouvrir la fenêtre **Se connecter au serveur**) :

   | Paramètre |  Description |
   |-----|-----|
   | **Type de serveur** | La valeur par défaut est le moteur de base de données ; ne modifiez pas cette valeur. |
   | **Nom du serveur** | Entrez le nom de l’ordinateur Linux SQL Server cible ou son adresse IP. |
   | **Authentification** | Pour SQL Server 2017 sur Linux, utilisez **Authentification SQL Server**. |
   | **Connexion** | Entrez le nom d'un utilisateur ayant accès à une base de données sur le serveur (par exemple, le compte **AS** par défaut créé pendant la configuration).
 |
   | **Mot de passe** | Saisissez le mot de passe pour l'utilisateur spécifié (pour le compte **AS** que vous avez créé pendant la configuration). |

    ![SQL Server Management Studio : Se connecter au serveur de base de données SQL](./media/sql-server-linux-manage-ssms/connect.png)

1. Cliquez sur **Se connecter**.

    > [!TIP]
    > Si vous obtenez un échec de connexion, essayez d'abord de diagnostiquer le problème à partir du message d'erreur. Passez ensuite en revue les [recommandations en matière de résolution des problèmes de connexion](sql-server-linux-troubleshooting-guide.md#connection).
 
1. Après vous être connecté à votre serveur SQL, **l’Explorateur d’objets** s’ouvre et vous pouvez désormais accéder à votre base de données pour effectuer des tâches administratives ou interroger des données.

## <a name="run-transact-sql-queries"></a>Exécuter des requêtes Transact-SQL

Une fois que vous vous connectez à votre serveur, vous pouvez vous connecter à une base de données et exécuter des requêtes Transact-SQL. Requêtes Transact-SQL peuvent être utilisés pour presque toutes les tâches de base de données.

1. Dans **l’Explorateur d’objets**, accédez à la base de données cible sur le serveur. Par exemple, développez **bases de données système** pour travailler avec les **master** base de données.

1. Avec le bouton droit de la base de données, puis sélectionnez **nouvelle requête**.

1. Dans la fenêtre de requête, écrire une requête Transact-SQL pour sélectionner retournent les noms de toutes les bases de données sur votre serveur.

   ```sql
   SELECT [Name]
   FROM sys.Databases
   ```

   Si vous ne connaissez pas l’écriture de requêtes, consultez [l’écriture des instructions Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).

1. Cliquez sur le **Execute** bouton pour exécuter la requête et afficher les résultats.

   ![Réussite. Se connecter au serveur de base de données SQL : SQL Server Management Studio](./media/sql-server-linux-manage-ssms/execute-query.png)

Bien qu’il soit possible d’effectuer presque toute tâche de gestion des requêtes Transact-SQL, SSMS est un outil graphique qui rend plus facile à gérer SQL Server. Les sections suivantes fournissent des exemples d’utilisation de l’interface utilisateur graphique.

## <a name="create-and-manage-databases"></a>Créer et gérer des bases de données

Tout en étant connecté à la *master* base de données, vous pouvez créer des bases de données sur le serveur et modifier ou supprimer des bases de données existantes. Les étapes suivantes décrivent comment effectuer plusieurs tâches courantes de gestion de base de données au moyen de Management Studio. Pour effectuer ces tâches, vérifiez que vous êtes connecté à la *master* base de données avec la connexion du principal au niveau du serveur que vous avez créé lorsque vous configurez 2017 du serveur SQL sur Linux.

### <a name="create-a-new-database"></a>Créer une base de données

1. Démarrez SSMS, puis connectez-vous à votre serveur dans 2017 du serveur SQL sur Linux

2. Dans l’Explorateur d’objets, cliquez sur le *bases de données* dossier, puis cliquez sur * Nouvelle base de données... »

3. Dans le *nouvelle base de données* boîte de dialogue, entrez un nom pour votre nouvelle base de données, puis cliquez sur *OK*

La nouvelle base de données est correctement créé dans votre serveur. Si vous préférez créer une nouvelle base de données à l’aide de T-SQL, consultez [créer une base de données (SQL Server Transact-SQL)](../t-sql/statements/create-database-sql-server-transact-sql.md).

### <a name="drop-a-database"></a>Supprimer une base de données

1. Démarrez SSMS, puis connectez-vous à votre serveur dans 2017 du serveur SQL sur Linux

2. Dans l’Explorateur d’objets, développez le *bases de données* dossier pour afficher la liste de la base de données sur le serveur.

3. Dans l’Explorateur d’objets, avec le bouton droit sur la base de données que vous souhaitez supprimer, puis cliquez sur *supprimer*

4. Dans le *supprimer un objet* boîte de dialogue, vérifiez *fermer les connexions existantes* puis cliquez sur *OK*

La base de données est correctement supprimée de votre serveur. Si vous souhaitez supprimer une base de données à l’aide de T-SQL, consultez [supprimer une base de données (SQL Server Transact-SQL)](../t-sql/statements/drop-database-transact-sql.md).

## <a name="use-activity-monitor-to-see-information-about-sql-server-activity"></a>Moniteur d’activité permet d’afficher des informations sur l’activité de SQL Server

Le [moniteur d’activité](../relational-databases/performance-monitor/activity-monitor.md) outil est intégré dans SQL Server Management Studio (SSMS) et affiche des informations sur les processus SQL Server et la façon dont ces processus affectent l’instance actuelle de SQL Server.

1. Démarrez SSMS, puis connectez-vous à votre serveur dans 2017 du serveur SQL sur Linux

1. Dans l’Explorateur d’objets, cliquez sur le *server* nœud, puis cliquez sur *moniteur d’activité*

Moniteur d’activité affiche les volets extensibles et réductibles avec les informations suivantes :

- Vue d'ensemble
- Processus
- Attentes de ressources
- Fichier de données d’e/s
- Requêtes coûteuses récentes
- Requêtes coûteuses actives

Lorsqu’un volet est développé, le moniteur d’activité interroge l’instance pour plus d’informations. Lorsqu'un volet est réduit, toutes les activités d'interrogation cessent pour ce volet. Vous pouvez développer un ou plusieurs volets en même temps pour afficher différents types d’activité sur l’instance.

## <a name="see-also"></a>Voir aussi
- [Qu’est-ce que SSMS ?](../ssms/sql-server-management-studio-ssms.md)
- [Exporter et importer une base de données avec SSMS](sql-server-linux-migrate-ssms.md)
- [Didacticiel : SQL Server Management Studio](../ssms/tutorials/tutorial-sql-server-management-studio.md)
- [Didacticiel : écriture d'instructions Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Analyse des performances et surveillance de l'activité du serveur](../relational-databases/performance/server-performance-and-activity-monitoring.md)
