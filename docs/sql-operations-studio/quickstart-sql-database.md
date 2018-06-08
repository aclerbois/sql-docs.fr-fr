---
title: 'Démarrage rapide : Se connecter et interroger une base de données SQL Azure à l’aide des opérations de SQL Studio (version préliminaire) | Documents Microsoft'
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
ms.openlocfilehash: c72e6d5b8e3e2770300e6b890b076bf77617849b
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="quickstart-use-includename-sosincludesname-sos-shortmd-to-connect-and-query-azure-sql-database"></a>Démarrage rapide : Utiliser [!INCLUDE[name-sos](../includes/name-sos-short.md)] pour vous connecter et interroger la base de données SQL Azure

Ce démarrage rapide montre comment utiliser *[!INCLUDE[name-sos](../includes/name-sos-short.md)]* pour vous connecter à une base de données SQL Azure, puis utilisez les instructions Transact-SQL (T-SQL) pour créer le *TutorialDB* utilisé dans [!INCLUDE[name-sos](../includes/name-sos-short.md)] didacticiels.

## <a name="prerequisites"></a>Configuration requise

Pour effectuer ce démarrage rapide, vous devez [!INCLUDE[name-sos](../includes/name-sos-short.md)]et un serveur SQL Azure.

- [Installer [!INCLUDE[name-sos](../includes/name-sos-short.md)] ](download.md).

Si vous n’avez pas encore un serveur SQL Azure, effectuez une des suivant Démarrages rapides des base de données SQL Azure (n’oubliez pas le nom du serveur et les informations d’identification de connexion !) :

- [Création de base de données - portail](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)
- [Création de base de données - CLI](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-cli)
- [Création de base de données - PowerShell](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-powershell)


## <a name="connect-to-your-azure-sql-database-server"></a>Se connecter à votre serveur de base de données SQL Azure

Utilisez [!INCLUDE[name-sos](../includes/name-sos-short.md)] pour établir une connexion à votre serveur de base de données SQL Azure.

1. La première fois que vous exécutez [!INCLUDE[name-sos](../includes/name-sos-short.md)] le **connexion** page doit s’ouvrir. Si vous ne voyez pas le **connexion** , cliquez sur **ajouter une connexion**, ou **nouvelle connexion** icône dans le **serveurs** encadré :
   
   ![Nouvelle icône de connexion](media/quickstart-sql-database/new-connection-icon.png)

2. Cet article utilise *connexion SQL*, mais *l’authentification Windows* est également pris en charge. Renseignez les champs comme suit à l’aide du nom du serveur, le nom d’utilisateur et le mot de passe pour *votre* serveur SQL Azure :

   | Paramètre       | Valeur suggérée |  Description |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nom du serveur** | Nom complet du serveur | Le nom doit être semblable à celui-ci : **nomserveur.basededonnées.Windows.NET** |
   | **Authentification** | Connexion SQL| L’authentification SQL est utilisée dans ce didacticiel. |
   | **Nom d'utilisateur** | Compte Administrateur du serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe (connexion SQL)** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |
   | **Enregistrer le mot de passe** | Oui ou Non | Sélectionnez Oui si vous ne souhaitez pas entrer le mot de passe chaque fois. |
   | **Nom de la base de données** | *Laissez vide* | Le nom de la base de données que vous souhaitez vous connecter. |
   | **Groupe de serveurs** | Sélectionnez <Default> | Si vous avez créé un groupe de serveurs, vous pouvez définir pour un groupe de serveurs spécifiques. | 

   ![Nouvelle icône de connexion](media/quickstart-sql-database/new-connection-screen.png)  

3. Si votre serveur n’a pas une règle de pare-feu autorisant Studio des opérations SQL pour se connecter, le **créer une nouvelle règle de pare-feu** s’ouvre. Remplissez le formulaire pour créer une règle de pare-feu. Pour plus d’informations, consultez [des règles de pare-feu](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

   ![Nouvelle règle de pare-feu](media/quickstart-sql-database/firewall.png)  

4. Après vous être connecté votre serveur s’ouvre dans le *serveurs* barre latérale.

## <a name="create-the-tutorial-database"></a>Créer la base de données didacticiel

Les sections suivantes créent le *TutorialDB* base de données qui est utilisé dans plusieurs [!INCLUDE[name-sos](../includes/name-sos-short.md)] didacticiels.

1. Cliquez avec le bouton droit sur votre serveur SQL Azure dans la barre latérale de serveurs et sélectionnez **nouvelle requête.**

1. Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   ```sql
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
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
      CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```


## <a name="insert-rows"></a>Insérer des lignes

- Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```


## <a name="view-the-result"></a>Afficher le résultat
1. Collez l’extrait de code suivant dans l’éditeur de requête, cliquez sur **exécuter**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

1. Les résultats de la requête sont affichés :

   ![Sélectionnez les résultats](media/quickstart-sql-database/select-results.png)


## <a name="clean-up-resources"></a>Nettoyer les ressources

Autres articles de cette collection s’appuient sur ce démarrage rapide. Si vous envisagez de continuer en cas de travailler avec les Démarrages rapides suivants, ne pas nettoyer les ressources créées dans ce démarrage rapide. Si vous n’envisagez pas de continuer, utilisez les étapes suivantes pour supprimer les ressources créées par ce démarrage rapide dans le portail Azure.
Nettoyer les ressources en supprimant les groupes de ressources que vous n’avez plus besoin. Pour plus d’informations, consultez [nettoyer les ressources](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal#clean-up-resources).

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous êtes connecté à une base de données SQL Azure et que vous avez exécuté une requête, essayez le [didacticiel de l’éditeur de Code](tutorial-sql-editor.md).
