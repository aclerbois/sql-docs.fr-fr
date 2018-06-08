---
title: Déployer un projet SSIS avec PowerShell | Microsoft Docs
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
ms.openlocfilehash: f245553e318ccdba4f8f5d212e5c4c92ec5564ca
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="deploy-an-ssis-project-with-powershell"></a>Déployer un projet SSIS avec PowerShell
Ce guide de démarrage rapide montre comment utiliser un script PowerShell pour se connecter à un serveur de base de données et déployer un projet SSIS dans le catalogue SSIS.

## <a name="prerequisites"></a>Conditions préalables requises

Un serveur Azure SQL Database écoute sur le port 1433. Si vous essayez de vous connecter à un serveur Azure SQL Database en étant derrière un pare-feu d’entreprise, ce port doit être ouvert dans le pare-feu d’entreprise pour que vous puissiez vous connecter.

## <a name="supported-platforms"></a>Plateformes prises en charge

Vous pouvez utiliser les informations de ce guide de démarrage rapide pour déployer un projet SSIS sur les plateformes suivantes :

-   SQL Server sur Windows.

-   Azure SQL Database. Pour plus d’informations sur le déploiement et l’exécution de packages dans Azure, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour déployer un package SSIS sur SQL Server sur Linux. Pour plus d’informations sur l’exécution de packages sur Linux, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Pour Azure SQL Database, obtenir les informations de connexion

Pour déployer le projet sur Azure SQL Database, obtenez les informations de connexion dont vous avez besoin pour vous connecter à la base de données du catalogue SSIS (SSISDB). Vous avez besoin des informations de connexion et du nom de serveur complet dans les procédures qui suivent.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Sélectionnez **Bases de données SQL** dans le menu de gauche, puis sélectionnez la base de données SSISDB dans la page **Bases de données SQL**. 
3. Dans la page **Vue d’ensemble** de votre base de données, notez le nom complet du serveur. Pour voir l’option **Cliquer pour copier**, pointez sur le nom du serveur. 
4. Si vous oubliez vos informations de connexion de serveur Azure SQL Database, accédez à la page du serveur SQL Database pour afficher le nom d’administrateur de serveur. Vous pouvez réinitialiser le mot de passe si nécessaire.
5. Cliquez sur **Afficher les chaînes de connexion de la base de données**.
6. Examinez la chaîne de connexion **ADO.NET** complète.

## <a name="powershell-script"></a>Script PowerShell
Entrez les valeurs appropriées pour les variables en haut du script suivant, puis exécutez le script pour déployer le projet SSIS.

> [!NOTE]
> L’exemple suivant utilise l’authentification Windows pour effectuer un déploiement sur un serveur SQL local. Pour utiliser l’authentification SQL Server, remplacez l’argument `Integrated Security=SSPI;` par `User ID=<user name>;Password=<password>;`. Si vous vous connectez à un serveur Azure SQL Database, vous ne pouvez pas utiliser l’authentification Windows.

```powershell
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectFilePath = "C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac"
$ProjectName = "Integration Services Project1"

# Load the IntegrationServices assembly
$loadStatus = [System.Reflection.Assembly]::Load("Microsoft.SQLServer.Management.IntegrationServices, "+
    "Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91, processorArchitecture=MSIL")

# Create a connection to the server
$sqlConnectionString = `
    "Data Source=" + $TargetServerName + ";Initial Catalog=master;Integrated Security=SSPI;"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $SSISNamespace".IntegrationServices" $sqlConnection

# Get the Integration Services catalog
$catalog = $integrationServices.Catalogs["SSISDB"]

# Create the target folder
$folder = New-Object $SSISNamespace".CatalogFolder" ($catalog, $TargetFolderName,
    "Folder description")
$folder.Create()

Write-Host "Deploying " $ProjectName " project ..."

# Read the project file and deploy it
[byte[]] $projectFile = [System.IO.File]::ReadAllBytes($ProjectFilePath)
$folder.DeployProject($ProjectName, $projectFile)

Write-Host "Done."
```

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour déployer un package.
    - [Déployer un package SSIS avec SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [Déployer un package SSIS à partir de l’invite de commandes](./ssis-quickstart-deploy-cmdline.md)
    - [Déployer un package SSIS avec C#](./ssis-quickstart-deploy-dotnet.md) 
- Exécutez un package déployé. Pour exécuter un package, vous pouvez choisir parmi plusieurs outils et langages. Pour plus d’informations, consultez les articles suivants :
    - [Exécuter un package SSIS avec SSMS](./ssis-quickstart-run-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Exécuter un package SSIS à partir de l’invite de commandes](./ssis-quickstart-run-cmdline.md)
    - [Exécuter un package SSIS avec PowerShell](ssis-quickstart-run-powershell.md)
    - [Exécuter un package SSIS avec C#](./ssis-quickstart-run-dotnet.md) 
