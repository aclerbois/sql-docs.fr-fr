---
Title: 'Tutorial: Connect to and query a SQL Server instance by using SQL Server Management Studio'
description: Tutoriel pour se connecter à une instance SQL Server en utilisant SQL Server Management Studio et en exécutant des requêtes T-SQL de base.
keywords: SQL Server, SSMS, SQL Server Management Studio
author: MashaMSFT
ms.author: mathoma
ms.date: 03/13/2018
ms.topic: tutorial
ms.suite: sql
ms.prod_service: sql-tools
ms.reviewer: sstein
manager: craigg
ms.prod: sql
ms.technology: ssms
ms.openlocfilehash: 5ccc024b8589efa95af2503a8ea5bdba0c47147b
ms.sourcegitcommit: 38f8824abb6760a9dc6953f10a6c91f97fa48432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-connect-to-and-query-a-sql-server-instance-by-using-sql-server-management-studio"></a>Tutoriel : Se connecter à une instance SQL Server et l’interroger en utilisant SQL Server Management Studio
Ce tutoriel vous apprend à utiliser SSMS (SQL Server Management Studio) pour vous connecter à votre instance SQL Server et exécuter des commandes T-SQL (Transact-SQL) de base. L’article explique comment effectuer les opérations suivantes :

> [!div class="checklist"]  
> * Se connecter à une instance de SQL Server    
> * Créer une base de données ("TutorialDB")    
> * Créer une table ("Customers") dans votre nouvelle base de données   
> * Insérer des lignes dans votre nouvelle table 
> * Interroger la nouvelle table et afficher les résultats    
> * Utiliser la table de fenêtre de requête pour vérifier les propriétés de votre connexion 
> * Changer le serveur auquel votre fenêtre de requête est connectée

## <a name="prerequisites"></a>Conditions préalables requises
Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio et d’un accès à une instance SQL Server. 

- Installez [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

Si vous n’avez pas accès à une instance SQL Server, sélectionnez votre plateforme parmi les liens suivants. Si vous choisissez l’authentification SQL, utilisez vos informations d’identification de connexion SQL Server.
- **Windows** : [Téléchargez SQL Server 2017 Édition Développeur](https://www.microsoft.com/sql-server/sql-server-downloads).
- **macOS** : [Téléchargez SQL Server 2017 sur Docker](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker).


## <a name="connect-to-a-sql-server-instance"></a>Se connecter à une instance de SQL Server

1. Démarrez SQL Server Management Studio. La première fois que vous exécutez SSMS, la fenêtre **Se connecter au serveur** s’ouvre. Si elle ne s’ouvre pas, vous pouvez l’ouvrir manuellement en sélectionnant **Explorateur d’objets** > **Se connecter** > **Moteur de base de données**.

    ![Lien Se connecter dans l’Explorateur d’objets](media/connect-query-sql-server/connectobjexp.png)

2. Dans la fenêtre **Se connecter au serveur**, procédez comme suit : 

    - Pour **Type de serveur**, sélectionnez **Moteur de base de données** (généralement l’option par défaut).
    - Pour **Nom du serveur**, entrez le nom de votre instance SQL Server. (Cet article utilise le nom d’instance SQL2016ST sur le nom d’hôte NODE5 [NODE5\SQL2016ST].) Si vous ne savez pas comment déterminer le nom de votre instance SQL Server, consultez [Conseils et astuces supplémentaires sur l’utilisation de SSMS](ssms-tricks.md#determine-sql-server-name).  

    ![Champ « Nom du serveur » avec la possibilité d’utiliser une instance SQL Server](media/connect-query-sql-server/connection2.png)

    - Pour **Authentification**, sélectionnez **Authentification Windows**. Cet article utilise l’authentification Windows, mais la connexion SQL Server est également prise en charge. Si vous sélectionnez **Connexion SQL**, vous êtes invité à fournir un nom d’utilisateur et un mot de passe. Pour plus d’informations sur les types d’authentification, consultez [Se connecter au serveur (moteur de base de données)](https://docs.microsoft.com/sql/ssms/f1-help/connect-to-server-database-engine).

    Vous pouvez également modifier d’autres options de connexion en sélectionnant **Options**. Exemples d’options de connexion : la base de données à laquelle vous êtes connecté, la valeur du délai d’expiration de la connexion et le protocole réseau. Cet article utilise les valeurs par défaut pour toutes les options. 

3. Une fois que vous avez renseigné tous les champs, sélectionnez **Se connecter**. 

### <a name="examples-of-successful-connections"></a>Exemples de connexions réussies
Pour vérifier que votre connexion au serveur SQL Server a réussi, développez et explorez les objets dans l’**Explorateur d’objets**. Ces objets varient en fonction du type de serveur auquel vous êtes connecté. 

- Connexion à un serveur SQL Server local (dans ce cas, NODE5\SQL2016ST) : ![Connexion à un serveur local](media/connect-query-sql-server/connect-on-prem.png)

- Connexion à SQL Azure DB (dans ce cas, msftestserver.database.windows.net : ![Connexion à une base de données SQL Azure DB](media/connect-query-sql-server/connect-sql-azure.png)

  >[!NOTE]
  > Dans ce tutoriel, vous avez utilisé précédemment l’*authentification Windows* pour vous connecter à votre serveur SQL Server local, mais cette méthode n’est pas prise en charge par SQL Azure DB. De ce fait, c’est l’authentification SQL qui est utilisée dans cette image pour établir une connexion à la base de données SQL Azure DB. Pour plus d’informations, consultez [Authentification locale SQL](../../relational-databases/security/choose-an-authentication-mode.md) et [Authentification SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview#control-access). 

## <a name="create-a-database"></a>Créer une base de données
Créez une base de données appelée TutorialDB en procédant comme suit : 

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre instance de serveur et sélectionnez **Nouvelle requête** :

   ![Lien Nouvelle requête](media/connect-query-sql-server/newquery.png)
   
2. Dans la fenêtre de la requête, collez l’extrait de code T-SQL suivant : 
   ```sql
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```
2. Pour exécuter la requête, sélectionnez **Exécuter** (ou sélectionnez F5 sur votre clavier). 

   ![Commande Exécuter](media/connect-query-sql-server/execute.png)
  
    Une fois que la requête est terminée, la nouvelle base de données TutorialDB s’affiche dans la liste des bases de données de l’Explorateur d’objets. Si elle n’apparaît pas, cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Actualiser**.  


## <a name="create-a-table-in-the-new-database"></a>Créer une table dans la nouvelle base de données
Dans cette section, vous allez créer une table dans la base de données TutorialDB que vous venez de créer. Étant donné que l’éditeur de requête est toujours dans le contexte de la base de données *master*, changez le contexte de connexion avec la base de données *TutorialDB* en procédant comme suit : 

1. Dans la liste déroulante des bases de données, sélectionnez la base de données que vous voulez, comme ici : 

   ![Changer la base de données](media/connect-query-sql-server/changedb.png)

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, sélectionnez-le et sélectionnez **Exécuter** (ou sélectionnez F5 sur votre clavier).  
   Vous pouvez remplacer le texte existant dans la fenêtre de requête ou l’ajouter à la fin. Pour exécuter tous les éléments de la fenêtre de requête, sélectionnez **Exécuter**. Pour exécuter une partie du texte, sélectionnez-la et sélectionnez **Exécuter**.  
  
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

Une fois que la requête est terminée, la nouvelle table Customers s’affiche dans la liste des tables de l’Explorateur d’objets. Si la table ne s’affiche pas, cliquez avec le bouton droit sur le nœud **TutorialDB** > **Tables** dans l’Explorateur d’objets et sélectionnez **Actualiser**.

## <a name="insert-rows-into-the-new-table"></a>Insérer des lignes dans la nouvelle table
Insérez des lignes dans la table Customers que vous avez créée précédemment. Pour ce faire, collez l’extrait de code T-SQL suivant dans la fenêtre de requête et sélectionnez **Exécuter** : 


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

## <a name="query-the-table-and-view-the-results"></a>Interroger la table et afficher les résultats
Les résultats d’une requête sont visibles sous la fenêtre du texte de requête. Pour interroger la table Customers et afficher les lignes qui ont été précédemment insérées, procédez comme suit :  

1. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, puis sélectionnez **Exécuter** : 

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Les résultats de la requête sont affichés sous la zone dans laquelle le texte a été entré : 

   ![Liste des résultats](media/connect-query-sql-server/queryresults.png)

2. Modifiez la présentation des résultats en sélectionnant l’une des options suivantes :

     ![Trois options d’affichage des résultats de requête](media/connect-query-sql-server/results.png)

    - Le bouton du milieu affiche les résultats en **Mode grille**, qui est l’option par défaut. 
    - Le premier bouton affiche les résultats en **Mode texte**, comme illustré dans l’image de la section suivante.
    - Le troisième bouton vous permet d’enregistrer les résultats dans un fichier dont l’extension est .rpt par défaut.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Vérifier les propriétés de votre connexion en utilisant la table de la fenêtre de requête
Des informations sur les propriétés de connexion figurent sous les résultats de votre requête. Après avoir exécuté la requête citée plus haut dans l’étape précédente, vérifiez les propriétés de connexion en bas de la fenêtre de requête.

- Vous pouvez identifier le serveur et la base de données auxquels vous êtes connecté, ainsi que le nom d’utilisateur avec lequel vous êtes connecté.
- Vous pouvez également voir la durée de la requête et le nombre de lignes qui sont retournées par la requête exécutée précédemment.

    ![Propriétés de connexion](media/connect-query-sql-server/connectionproperties.png)
    
    Dans cette image, notez que les résultats sont affichés en **Mode texte**. 

## <a name="change-the-server-that-the-query-window-is-connected-to"></a>Changer le serveur auquel la fenêtre de requête est connectée
Vous pouvez changer le serveur auquel votre fenêtre de requête actuelle est connectée en procédant comme suit :

1. Cliquez avec le bouton droit dans la fenêtre de requête, puis sélectionnez **Connexion** > **Changer la connexion**. La fenêtre **Se connecter au serveur** s’ouvre de nouveau.
2. Changez le serveur auquel votre requête est connectée. 
 
   ![Commande Changer la connexion](media/connect-query-sql-server/changeconnection.png)

    > [!NOTE]
    > Cette action change uniquement le serveur auquel la fenêtre de requête est connectée, pas le serveur auquel l’Explorateur d’objets est connecté. 



