---
title: 'Démarrage rapide : Se connecter et interroger un entrepôt de données SQL Azure à l’aide des opérations de SQL Studio (version préliminaire) | Documents Microsoft'
description: Ce démarrage rapide montre comment utiliser les opérations de SQL Studio (version préliminaire) pour vous connecter à une base de données SQL et exécuter une requête
ms.custom: tools|sos
ms.date: 03/08/2018
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: quickstart
author: yualan
ms.author: alayu
manager: craigg
ms.openlocfilehash: 2f0ad75a6be9cb3ad1404a0406601fbef68b0303
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="quickstart-use-includename-sosincludesname-sos-shortmd-to-connect-and-query-data-in-azure-sql-data-warehouse"></a>Démarrage rapide : Utiliser [!INCLUDE[name-sos](../includes/name-sos-short.md)] pour vous connecter et interroger des données dans l’entrepôt de données SQL Azure

Ce démarrage rapide montre comment utiliser [!INCLUDE[name-sos](../includes/name-sos-short.md)] pour vous connecter à l’entrepôt de données SQL Azure, puis utiliser les instructions Transact-SQL pour créer, insérer et sélectionner des données. 

## <a name="prerequisites"></a>Configuration requise
Pour effectuer ce démarrage rapide, vous devez [!INCLUDE[name-sos](../includes/name-sos-short.md)]et un entrepôt de données SQL Azure.

- [Installer [!INCLUDE[name-sos](../includes/name-sos-short.md)] ](download.md).

Si vous n’avez pas encore un entrepôt de données SQL, consultez [créer un entrepôt de données SQL](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

N’oubliez pas le nom du serveur, les informations d’identification de connexion !


## <a name="connect-to-your-data-warehouse"></a>Se connecter à votre entrepôt de données

Utilisez [!INCLUDE[name-sos](../includes/name-sos-short.md)] pour établir une connexion à votre serveur Azure SQL Data Warehouse.

1. La première fois que vous exécutez [!INCLUDE[name-sos](../includes/name-sos-short.md)] le **connexion** page doit s’ouvrir. Si vous ne voyez pas le **connexion** , cliquez sur **ajouter une connexion**, ou **nouvelle connexion** icône dans le **serveurs** encadré :
   
   ![Nouvelle icône de connexion](media/quickstart-sql-dw/new-connection-icon.png)

2. Cet article utilise *connexion SQL*, mais *l’authentification Windows* est également pris en charge. Renseignez les champs comme suit à l’aide du nom du serveur, le nom d’utilisateur et le mot de passe pour *votre* serveur SQL Azure :

   | Paramètre       | Valeur suggérée |  Description |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nom du serveur** | Nom complet du serveur | Le nom doit être semblable à celui-ci : **sqldwsample.database.windows.net** |
   | **Authentification** | Connexion SQL| L’authentification SQL est utilisée dans ce didacticiel. |
   | **Nom d'utilisateur** | Compte Administrateur du serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe (connexion SQL)** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |
   | **Enregistrer le mot de passe** | Oui ou Non | Sélectionnez Oui si vous ne souhaitez pas entrer le mot de passe chaque fois. |
   | **Nom de la base de données** | *Laissez vide* | Nom de la base de données avec laquelle établir une connexion. |
   | **Groupe de serveurs** | Sélectionnez <Default> | Si vous avez créé un groupe de serveurs, vous pouvez définir pour un groupe de serveurs spécifiques. | 

   ![Nouvelle icône de connexion](media/quickstart-sql-dw/new-connection-screen.png) 

3. Si votre serveur n’a pas une règle de pare-feu autorisant Studio des opérations SQL pour se connecter, le **créer une nouvelle règle de pare-feu** s’ouvre. Remplissez le formulaire pour créer une règle de pare-feu. Pour plus d’informations, consultez [des règles de pare-feu](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

   ![Nouvelle règle de pare-feu](media/quickstart-sql-dw/firewall.png)  

4. Après vous être connecté votre serveur s’ouvre dans le *serveurs* barre latérale.

## <a name="create-the-tutorial-data-warehouse"></a>Créer l’entrepôt de données du didacticiel
1. Cliquez avec le bouton droit sur votre serveur, dans l’Explorateur d’objets et sélectionnez **nouvelle requête.**

1. Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```


## <a name="create-a-table"></a>Créer une table

L’éditeur de requête est toujours connecté à la *master* base de données, mais souhaitez créer une table dans le *TutorialDB* base de données. 

1. Modifier le contexte de connexion au **TutorialDB**:

   ![Changer de contexte](media/quickstart-sql-database/change-context.png)


1. Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   > [!NOTE]
   > Vous pouvez l’ajouter à ou remplacer la requête précédente dans l’éditeur. Notez qu’un clic sur **exécuter** exécute uniquement la requête sélectionnée. Si rien n’est sélectionné, cliquez sur **exécuter** exécute toutes les requêtes dans l’éditeur.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```


## <a name="insert-rows"></a>Insérer des lignes

1. Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```


## <a name="view-the-result"></a>Afficher le résultat
1. Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

1. Les résultats de la requête sont affichés :

   ![Sélectionnez les résultats](media/quickstart-sql-dw/select-results.png)


## <a name="clean-up-resources"></a>Nettoyer les ressources

Autres articles de cette collection s’appuient sur ce démarrage rapide. Si vous envisagez de continuer en cas de travailler avec les Démarrages rapides suivants, ne pas nettoyer les ressources créées dans ce démarrage rapide. Si vous n’envisagez pas de continuer, utilisez les étapes suivantes pour supprimer les ressources créées par ce démarrage rapide dans le portail Azure.
Nettoyer les ressources en supprimant les groupes de ressources que vous n’avez plus besoin. Pour plus d’informations, consultez [nettoyer les ressources](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal#clean-up-resources).


## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous êtes connecté à un entrepôt de données SQL Azure et que vous avez exécuté une requête, essayez le [didacticiel de l’éditeur de Code](tutorial-sql-editor.md).