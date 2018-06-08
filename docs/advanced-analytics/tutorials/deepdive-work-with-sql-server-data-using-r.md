---
title: Travailler avec des données de SQL Server à l’aide de R (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e57e94d1d7856bfc9082c1a73a13a5c0a620b5ed
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="work-with-sql-server-data-using-r-sql-and-r-deep-dive"></a>Travailler avec des données SQL Server à l’aide de R (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Dans cette leçon, vous définissez l’environnement et ajoutez les données que vous avez besoin pour vos modèles d’apprentissage et exécutez certaines des résumés rapides des données. Dans le cadre du processus, vous devez effectuer ces tâches :
  
- Créer une base de données pour stocker les données de formation et d’évaluation de deux modèles R.
  
- Créer un compte (utilisateur Windows ou connexion SQL) à utiliser pour la communication entre votre station de travail et l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
- Créer des sources de données en R pour utiliser des objets de base de données et des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
- Utiliser la source de données R pour charger des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
- Utiliser R pour obtenir une liste de variables et modifier les métadonnées de la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
- Créer un contexte de calcul pour permettre l’exécution à distance de code R.
  
- (Facultatif) Activer le suivi sur le contexte de calcul à distance.
  
## <a name="create-the-database-and-user"></a>Créer la base de données utilisateur

Pour cette procédure pas à pas, créez une nouvelle base de données [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]et ajouter une connexion SQL avec des autorisations pour écrire et lire des données et pour exécuter des scripts R.

> [!NOTE]
> Si vous lisez uniquement des données, le compte qui exécute des scripts R requiert des autorisations SELECT (**db_datareader** rôle) sur la base de données spécifié. Toutefois, dans ce didacticiel, vous devez disposer des privilèges d’administrateur DDL pour préparer la base de données et créer des tables pour enregistrer les résultats du calcul de score.
> 
> En outre, si vous n’êtes pas le propriétaire de la base de données, vous avez besoin de l’autorisation EXECUTE ANY EXTERNAL SCRIPT, afin d’exécuter des scripts R.

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez l’instance où [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] est activé, cliquez avec le bouton droit sur **Bases de données**, puis sélectionnez **Nouvelle base de données**.
  
2. Tapez un nom pour la nouvelle base de données. Vous pouvez utiliser n’importe quel nom ; pensez simplement à modifier tous les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] et les scripts R dans cette procédure pas à pas en conséquence.
  
    > [!TIP]
    > Pour afficher le nom de la base de données mis à jour, cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Actualiser** .
  
3. Cliquez sur **Nouvelle requête**, et définissez le contexte de base de données sur la base de données master.
  
4. Dans la fenêtre **Nouvelle requête** , exécutez les commandes suivantes pour créer les comptes d’utilisateurs et les assigner à la base de données utilisée pour ce didacticiel. Le cas échéant, modifiez le nom de la base de données.
  
**Utilisateur Windows**
  
```SQL
 -- Create server user based on Windows account
USE master
GO
CREATE LOGIN [<DOMAIN>\<user_name>] FROM WINDOWS WITH DEFAULT_DATABASE=[DeepDive]

 --Add the new user to tutorial database
USE [DeepDive]
GO
CREATE USER [<user_name>] FOR LOGIN [<DOMAIN>\<user_name>] WITH DEFAULT_SCHEMA=[db_datareader]
```

**Connexion SQL**

```SQL
-- Create new SQL login
USE master
GO
CREATE LOGIN DDUser01 WITH PASSWORD='<type password here>', CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF;

-- Add the new SQL login to tutorial database
USE [DeepDive]
GO
CREATE USER [DDUser01] FOR LOGIN [DDUser01] WITH DEFAULT_SCHEMA=[db_datareader]
```

5. Pour vérifier que l’utilisateur a bien été créé, sélectionnez la nouvelle base de données, développez **Sécurité**, puis développez **Utilisateurs**.

## <a name="troubleshooting"></a>Dépannage

Cette section répertorie quelques problèmes courants que vous pouvez rencontrer au cours de la configuration de la base de données.

- **Comment puis-je vérifier la connectivité de la base de données et vérifier les requêtes SQL ?**
  
    Avant d’exécuter le code R à l’aide du serveur, vous voudrez peut-être vérifier que la base de données est accessible à partir de votre environnement de développement R. L’ [Explorateur de serveurs de Visual Studio](https://msdn.microsoft.com/library/x603htbk.aspx) et [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) sont des outils gratuits qui ont une connectivité de base de données et des fonctionnalités de gestion puissantes.
  
    Si vous ne souhaitez pas installer des outils de gestion de base de données supplémentaires, vous pouvez créer une connexion de test à l’instance de SQL Server à l’aide de [l’Administrateur de sources de données ODBC](https://msdn.microsoft.com/library/ms714024.aspx) dans le Panneau de configuration. Si la base de données est correctement configurée et que vous entrez les nom d’utilisateur et mot de passe corrects, vous devriez pouvoir voir la base de données que vous venez de créer et la sélectionner comme base de données par défaut.
  
    Si vous ne pouvez pas vous connecter à la base de données, vérifiez que les connexions à distance sont activées pour le serveur et que le protocole Canaux nommés a été activé. Conseils de dépannage supplémentaires sont fournies dans cet article : [problèmes de connexion pour le moteur de base de données SQL Server](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine).
  
- **Pourquoi le nom de ma table est-il précédé de « datareader » ?**
  
    Lorsque vous spécifiez le schéma par défaut pour cet utilisateur en tant que **db_datareader**, toutes les tables et autres nouveaux objets créés par cet utilisateur sont précédées du *schéma* nom. Un schéma ressemble à un dossier que vous pouvez ajouter à une base de données pour organiser des objets. Le schéma définit également les privilèges d’un utilisateur dans la base de données.
  
    Lorsque le schéma est associé à un nom d’utilisateur particulier, l’utilisateur est la _propriétaire du schéma_. Quand vous créez un objet, vous le créez toujours dans votre propre schéma, sauf si vous demandez spécifiquement qu’il soit créé dans un autre schéma.
  
    Par exemple, si vous créez une table avec le nom `*`TestData`, and your default schema is **db\_datareader**, the table is created with the name `< nom_base_de_données > .db_datareader. TestData ».
  
    Pour cette raison, une base de données peut contenir plusieurs tables portant le même nom, tant que les tables appartiennent à des schémas différents.
   
    Si vous recherchez une table et que vous ne spécifiez pas un schéma, le serveur de base de données recherche un schéma dont vous êtes propriétaire. Ainsi, vous n’avez pas besoin de spécifier le nom de schéma pour accéder à des tables dans un schéma associé à votre compte de connexion.
  
- **Je ne dispose pas de privilèges DDL. Puis-je quand même suivre le didacticiel ?**
  
    Oui. Toutefois, vous devez demander à quelqu’un de précharger les données dans les tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et ignorer les sections qui nécessitent la création de tables. Les fonctions qui requièrent des privilèges DDL sont appelées dans le didacticiel autant que possible.

    En outre, demandez à votre administrateur de vous accorder l’autorisation EXECUTE ANY EXTERNAL SCRIPT. Il est nécessaire pour l’exécution du script R, si à distance ou à l’aide de `sp_execute_external_script`.

## <a name="next-step"></a>Étape suivante

[Créer des objets de données SQL Server à l’aide de RxSqlServerData](../../advanced-analytics/tutorials/deepdive-create-sql-server-data-objects-using-rxsqlserverdata.md)

## <a name="overview"></a>Vue d'ensemble

[Immersion dans la science des données : utilisation des packages RevoScaleR](../../advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)



