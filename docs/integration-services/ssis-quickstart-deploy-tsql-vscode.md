---
title: Déployer un projet SSIS avec Transact-SQL (VS Code) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.component: quick-start
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b4611b711b9f220af26a7f629480fa9f7b4c071c
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="deploy-an-ssis-project-from-visual-studio-code-with-transact-sql"></a>Déployer un projet SSIS à partir de Visual Studio Code avec Transact-SQL
Ce guide de démarrage rapide montre comment utiliser Visual Studio Code pour se connecter à la base de données du catalogue SSIS, puis utiliser des instructions Transact-SQL pour déployer un projet SSIS dans le catalogue SSIS.

Visual Studio Code est un éditeur de code pour Windows, Mac OS et Linux qui prend en charge les extensions, notamment l’extension `mssql` pour la connexion à Microsoft SQL Server, Azure SQL Database ou Azure SQL Data Warehouse. Pour plus d’informations sur VS Code, consultez [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Conditions préalables requises

Avant de commencer, vérifiez que vous avez installé la dernière version de Visual Studio Code et chargé l’extension `mssql`. Pour télécharger ces outils, consultez les pages suivantes :
-   [Télécharger Visual Studio Code](https://code.visualstudio.com/Download)
-   [Extension mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>Plateformes prises en charge

Vous pouvez utiliser les informations de ce guide de démarrage rapide pour déployer un projet SSIS sur les plateformes suivantes :

-   SQL Server sur Windows.

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour déployer un package SSIS sur Azure SQL Database : La procédure stockée `catalog.deploy_project` attend le chemin du fichier `.ispac` dans le système de fichiers local. Pour plus d’informations sur le déploiement et l’exécution de packages dans Azure, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour déployer un package SSIS sur SQL Server sur Linux : Pour plus d’informations sur l’exécution de packages sur Linux, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="set-language-mode-to-sql-in-vs-code"></a>Définir SQL comme mode de langage dans VS Code

Pour activer les commandes `mssql` et T-SQL IntelliSense, définissez **SQL** comme mode de langage dans Visual Studio Code.

1. Ouvrez Visual Studio Code, puis ouvrez une nouvelle fenêtre. 

2. Cliquez sur **Texte brut** en bas à droite de la barre d’état.
 
3. Dans le menu déroulant **Sélectionner le mode de langage** qui s’affiche, sélectionnez ou entrez **SQL**, puis appuyez sur **Entrée** pour définir SQL comme mode de langage. 

## <a name="connect-to-the-ssis-catalog-database"></a>Se connecter à la base de données du catalogue SSIS

Utilisez Visual Studio Code pour établir une connexion au catalogue SSIS.

1. Dans VS Code, appuyez sur **Ctrl+Maj+P** (ou **F1**) pour ouvrir la palette de commandes.

2. Tapez **sqlcon** et appuyez sur **Entrée**.

3. Appuyez sur **Entrée** pour sélectionner **Créer un profil de connexion**. Cette étape crée un profil de connexion pour votre instance de SQL Server.

4. Suivez les invites afin de spécifier les propriétés de connexion pour le nouveau profil de connexion. Après avoir spécifié chaque valeur, appuyez sur **Entrée** pour continuer. 

   | Paramètre       | Valeur suggérée | En savoir plus |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nom du serveur** | Nom complet du serveur |  |
   | **Nom de la base de données** | **SSISDB** | Nom de la base de données avec laquelle établir une connexion. |
   | **Authentification** | Connexion SQL | |
   | **User name** | Compte Administrateur du serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe (connexion SQL)** | Mot de passe du compte Administrateur de votre serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |
   | **Enregistrer le mot de passe** | Oui ou Non | Si vous ne souhaitez pas entrer le mot de passe à chaque fois, sélectionnez Oui. |
   | **Entrez un nom pour ce profil** | Nom d’un profil, tel que **mySSISServer** | L’enregistrement du nom de profil permet d’accélérer les connexions suivantes. | 

5. Appuyez sur la touche **Échap** pour fermer le message d’information qui vous signale que le profil a été créé et connecté.

6. Vérifiez votre connexion dans la barre d’état.

## <a name="run-the-t-sql-code"></a>Exécuter le code T-SQL
Exécutez le code Transact-SQL suivant pour déployer un projet SSIS.

1. Dans la fenêtre **Éditeur**, entrez la requête suivante dans la fenêtre de requête vide.

2. Mettez à jour les valeurs de paramètres dans la procédure stockée `catalog.deploy_project` pour votre système.

3. Appuyez sur **Ctrl+Maj+E** pour exécuter le code et déployer le projet.

```sql
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary = (SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
    @project_name = '<project_name',
    @Project_Stream = @ProjectBinary,
    @operation_id = @operation_id out
```

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour déployer un package.
    - [Déployer un package SSIS avec SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Déployer un package SSIS à partir de l’invite de commandes](./ssis-quickstart-deploy-cmdline.md)
    - [Déployer un package SSIS avec PowerShell](ssis-quickstart-deploy-powershell.md)
    - [Déployer un package SSIS avec C#](./ssis-quickstart-deploy-dotnet.md) 
- Exécutez un package déployé. Pour exécuter un package, vous pouvez choisir parmi plusieurs outils et langages. Pour plus d’informations, consultez les articles suivants :
    - [Exécuter un package SSIS avec SSMS](./ssis-quickstart-run-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Exécuter un package SSIS à partir de l’invite de commandes](./ssis-quickstart-run-cmdline.md)
    - [Exécuter un package SSIS avec PowerShell](ssis-quickstart-run-powershell.md)
    - [Exécuter un package SSIS avec C#](./ssis-quickstart-run-dotnet.md) 
