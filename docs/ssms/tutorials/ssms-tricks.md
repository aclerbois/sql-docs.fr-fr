---
Title: 'Tutorial: Additional tips and tricks for using SQL Server Management Studio'
description: 'Tutoriel indiquant des conseils et astuces supplémentaires pour utiliser SSMS. '
keywords: SQL Server, SSMS, SQL Server Management Studio
author: MashaMSFT
ms.author: mathoma
ms.date: 03/13/2018
ms.topic: Tutorial
ms.suite: sql
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
manager: craigg
helpviewer_keywords:
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
ms.openlocfilehash: 80d50132c4e2b38ecda9d24b3c0f4c09b93ca4e6
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="tutorial-additional-tips-and-tricks-for-using-ssms"></a>Tutoriel : Conseils et astuces supplémentaires pour utiliser SSMS
Ce tutoriel vous propose des astuces supplémentaires pour utiliser SQL Server Management Studio (SSMS). Cet article vous montre comment : 

> [!div class="checklist"]
> * Ajouter/supprimer des marques de commentaire dans le texte Transact-SQL (T-SQL)
> * Mettre en retrait du texte
> * Filtrer des objets dans l’Explorateur d’objets
> * Accéder à votre journal des erreurs SQL Server
> * Rechercher le nom de votre instance SQL Server

## <a name="prerequisites"></a>Conditions préalables requises
Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio, de l’accès à un serveur SQL et d’une base de données AdventureWorks. 

- Installez [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez un [exemple de base de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Pour savoir comment restaurer une base de données dans SSMS, consultez [Restauration d’une base de données](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). 


## <a name="commentuncomment-your-t-sql-code"></a>Ajouter/supprimer des marques de commentaire dans votre code T-SQL
Vous pouvez ajouter et supprimer des marques de commentaire dans des parties de votre texte à l’aide du bouton **Commenter** de la barre d’outils. Le texte qui est commenté n’est pas exécuté. 

1. Ouvrez SQL Server Management Studio. 
2. Connectez-vous à votre serveur SQL.
3. Ouvrez une fenêtre Nouvelle requête. 
4. Collez le code T-SQL suivant dans la fenêtre de texte : 

  ```sql
    USE master
    GO

    -- Drop the database if it already exists
    IF  EXISTS (
        SELECT name 
            FROM sys.databases 
            WHERE name = N'TutorialDB'
            )

    DROP DATABASE TutorialDB
    GO

    CREATE DATABASE TutorialDB
    GO

    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
 ``` 


5. Mettez en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Commenter** dans la barre d’outils : 

    ![Bouton Commenter](media/ssms-tricks/comment.png)
6. Sélectionnez **Exécuter** pour exécuter la partie de texte sans marque de commentaire. 
7. Mettez en surbrillance tous les éléments à l’exception de la commande **ALTER DATABASE**, puis sélectionnez le bouton **Commenter** :

    ![Tout commenter](media/ssms-tricks/commenteverything.png)

8. Mettez en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Supprimer les marques de commentaire** pour supprimer les marques de commentaire :

    ![Supprimer les marques de commentaire dans le texte](media/ssms-tricks/uncomment.png)
    
9. Sélectionnez **Exécuter** pour exécuter la partie de texte sans marque de commentaire. 

## <a name="indent-your-text"></a>Mettre en retrait du texte
Vous pouvez utiliser les boutons de mise en retrait de la barre d’outils pour augmenter ou réduire le retrait du texte. 

1. Ouvrez une fenêtre Nouvelle requête. 
2. Collez le code T-SQL suivant dans la fenêtre de texte : 

  ```sql
    USE master
    GO

    -- Drop the database if it already exists
    IF  EXISTS (
        SELECT name 
            FROM sys.databases 
            WHERE name = N'TutorialDB'
            )

    DROP DATABASE TutorialDB
    GO

    CREATE DATABASE TutorialDB
    GO

    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
 ``` 
 
3. Mettez en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Augmenter le retrait** dans la barre d’outils pour déplacer ce texte vers l’avant :

    ![Augmenter le retrait](media/ssms-tricks/increaseindent.png)

4. Mettez à nouveau en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Réduire le retrait** pour déplacer ce texte vers l’arrière.

    ![Réduire le retrait](media/ssms-tricks/decreaseindent.png)


## <a name="filter-objects-in-object-explorer"></a>Filtrer des objets dans l’Explorateur d’objets
Vous pouvez filtrer les objets pour faciliter la recherche d’un objet spécifique dans les bases de données qui en contiennent beaucoup. Cette section décrit comment filtrer les tables, mais vous pouvez utiliser les étapes suivantes pour n’importe quel autre nœud dans l’Explorateur d’objets :

1. Connectez-vous à votre serveur SQL.
2. Développez **Bases de données** > **AdventureWorks** > **Tables**. Toutes les tables de la base de données s’affichent.
5. Cliquez avec le bouton droit sur **Tables**, puis sélectionnez **Filtrer** > **Paramètres de filtre**:

    ![Paramètres du filtre](media/ssms-tricks/filtersettings.png)

6. Dans la fenêtre **Paramètres de filtre**, vous pouvez modifier les paramètres de filtre suivants :
    - Filtrer par nom : 
   
      ![Filtrer par nom](media/ssms-tricks/filterbyname.png)

    - Filtrer par schéma : 
    
      ![Filtrer par schéma](media/ssms-tricks/filterbyschema.png)

7. Pour effacer le filtre, cliquez avec le bouton droit sur **Tables**, puis sélectionnez **Supprimer le filtre**.

    ![Supprimer le filtre](media/ssms-tricks/removefilter.png)
    


## <a name="access-your-sql-server-error-log"></a>Accéder à votre journal des erreurs SQL Server
Le journal des erreurs est un fichier qui contient les détails de ce qui se produit dans votre instance SQL Server. Vous pouvez parcourir et interroger le journal des erreurs dans SSMS. Le journal des erreurs est un fichier .log qui se trouve sur votre disque.

### <a name="open-the-error-log-in-ssms"></a>Ouvrir le journal des erreurs dans SSMS
1. Connectez-vous à votre serveur SQL.  
2. Développez **Gestion** > **Journaux SQL Server**. 
4. Cliquez avec le bouton droit sur le journal des erreurs **Actuel**, puis sélectionnez **Voir le journal SQL Server** :

    ![Voir le journal des erreurs dans SSMS](media/ssms-tricks/viewerrorloginssms.png)

### <a name="query-the-error-log-in-ssms"></a>Interroger le journal des erreurs dans SSMS
1. Connectez-vous à votre serveur SQL.
2. Ouvrez une fenêtre Nouvelle requête.
3. Collez le code T-SQL suivant dans la fenêtre de votre requête :

 ```sql
   sp_readerrorlog 0,1,'Server process ID' 
  ``` 

4. Remplacez le texte entre guillemets simples par le texte à rechercher.
5. Exécutez la requête et passez en revue les résultats :
   
    ![Interroger le journal des erreurs](media/ssms-tricks/queryerrorlog.png)


### <a name="find-the-error-log-location-if-youre-connected-to-sql-server"></a>Rechercher l’emplacement du journal des erreurs si vous êtes connecté à SQL Server
1. Connectez-vous à votre serveur SQL.
2. Ouvrez une fenêtre Nouvelle requête.
3. Collez le code T-SQL suivant dans la fenêtre de votre requête, puis sélectionnez **Exécuter** :

 ```sql
    SELECT SERVERPROPERTY('ErrorLogFileName') AS 'Error log file location'  
  ``` 

4. Les résultats indiquent l’emplacement du journal des erreurs dans le système de fichiers : 

    ![Rechercher le journal des erreurs par requête](media/ssms-tricks/finderrorlogquery.png)

### <a name="find-the-error-log-location-if-you-cant-connect-to-sql-server"></a>Rechercher l’emplacement du journal des erreurs si vous ne pouvez pas vous connecter à SQL Server
1. Ouvrez le Gestionnaire de configuration SQL Server. 
2. Développez **Services**.
3. Cliquez avec le bouton droit sur votre instance SQL Server, puis sélectionnez **Propriétés** :

    ![Propriétés de serveur du Gestionnaire de configuration](media/ssms-tricks/serverproperties.PNG)

4. Sélectionnez l’onglet **Paramètres de démarrage**.
5. Dans la zone **Paramètres existants**, le chemin après le « -e » correspond à l’emplacement du journal des erreurs : 
    
    ![Journal des erreurs](media/ssms-tricks/errorlog.png)
    
    Plusieurs fichiers errorlog.* se trouvent à cet emplacement. Le nom de fichier qui se termine par *.log est le fichier du journal des erreurs actuel. Les noms de fichiers qui se terminent par des chiffres sont les fichiers journaux précédents. Un journal est créé chaque fois que le serveur SQL redémarre.

6. Ouvrez le fichier errorlog.log dans le Bloc-notes. 

## <a name="determine-sql-server-name"></a>Rechercher le nom de votre serveur SQL
Vous avez plusieurs options pour rechercher le nom de votre serveur SQL avant et après vous y être connecté.  

### <a name="before-you-connect-to-sql-server"></a>Avant de vous connecter à SQL Server
1. Suivez les étapes pour localiser le [journal des erreurs SQL Server sur le disque](#finding-your-error-log-if-you-cannot-connect-to-sql). 
2. Ouvrez le fichier errorlog.log dans le Bloc-notes.  
3. Recherchez le texte *Server name is*.
    
    Ce qui figure entre guillemets simples est le nom de l’instance SQL Server à laquelle vous allez vous connecter :

    ![Rechercher le nom de serveur dans le journal des erreurs](media/ssms-tricks/servernameinlog.png)
    
    Le format du nom est HOSTNAME\INSTANCENAME. Si vous voyez uniquement le nom d’hôte, vous avez installé l’instance par défaut et son nom est MSSQLSERVER. Quand vous vous connectez à une instance par défaut, il vous suffit d’entrer le nom d’hôte pour vous connecter à votre serveur SQL.  

### <a name="when-youre-connected-to-sql-server"></a>Une fois que vous êtes connecté à SQL Server 
Une fois que vous êtes connecté à SQL Server, le nom du serveur est disponible à trois emplacements : 

1. Le nom du serveur est indiqué dans l’Explorateur d’objets :

    ![Nom de l’instance SQL Server dans l’Explorateur d’objets](media/ssms-tricks/nameinobjectexplorer.png)
2. Le nom du serveur est indiqué dans la fenêtre de requête :

    ![Nom de l’instance SQL Server dans la fenêtre de requête](media/ssms-tricks/nameinquerywindow.png)
3. Le nom du serveur est indiqué dans **Propriétés**.
    - Dans le menu **Affichage**, sélectionnez **Fenêtre Propriétés** :

      ![Nom de l’instance SQL Server dans la fenêtre Propriétés](media/ssms-tricks/nameinproperties.png)

### <a name="if-youre-connected-to-an-alias-or-availability-group-listener"></a>Si vous êtes connecté à un alias ou un écouteur de groupe de disponibilité 
Quand vous êtes connecté à un alias ou un écouteur de groupe de disponibilité, ces informations sont indiquées dans l’Explorateur d’objets et la fenêtre Propriétés. Dans ce cas, le nom SQL Server n’est peut-être pas visible et doit être interrogé : 

1. Connectez-vous à votre serveur SQL.
2. Ouvrez une fenêtre Nouvelle requête.
3. Collez le code T-SQL suivant dans la fenêtre : 

  ```sql
   select @@Servername 
 ``` 
4. Consultez les résultats de la requête pour identifier le nom de l’instance SQL Server à laquelle vous êtes connecté : 
    
    ![Interroger le nom du serveur SQL](media/ssms-tricks/queryservername.png)


