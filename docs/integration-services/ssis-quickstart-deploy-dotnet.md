---
title: Déployer un projet SSIS avec du code .NET (C#) | Microsoft Docs
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
ms.openlocfilehash: eaac43b601808460fad9714a98bb1f444df67546
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="deploy-an-ssis-project-with-c-code-in-a-net-app"></a>Déployer un projet SSIS avec du code C# dans une application .NET
Ce guide de démarrage rapide montre comment écrire du code C# pour se connecter à un serveur de base de données et déployer un projet SSIS.

Pour créer une application C#, vous pouvez utiliser Visual Studio, Visual Studio Code ou un autre outil de votre choix.

## <a name="prerequisites"></a>Conditions préalables requises

Avant de commencer, vérifiez que Visual Studio ou Visual Studio Code est installé sur votre ordinateur. Téléchargez l’édition Community gratuite de Visual Studio, ou le logiciel Visual Studio Code gratuit, à partir de la page de [Téléchargements Visual Studio](https://www.visualstudio.com/downloads/).

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
6. Examinez la chaîne de connexion **ADO.NET** complète. Votre code peut aussi utiliser un `SqlConnectionStringBuilder` pour recréer cette chaîne de connexion avec chaque valeur de paramètre que vous fournissez.

## <a name="create-a-new-visual-studio-project"></a>Créer un projet Visual Studio

1. Dans Visual Studio, choisissez **Fichier**, **Nouveau**, **Projet**. 
2. Dans la boîte de dialogue **Nouveau projet**, développez **Visual C#**.
3. Sélectionnez **Application console** et entrez *run_ssis_project* comme nom de projet.
4. Cliquez sur **OK** pour créer et ouvrir le nouveau projet dans Visual Studio.

## <a name="add-references"></a>Ajouter des références
1. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier **Références**, puis sélectionnez **Ajouter une référence**. La boîte de dialogue **Gestionnaire de références** s’ouvre.
2. Dans la boîte de dialogue **Gestionnaire de références**, développez **Assemblys** et sélectionnez **Extensions**.
3. Sélectionnez les deux références suivantes à ajouter :
    -   Microsoft.SqlServer.Management.Sdk.Sfc
    -   Microsoft.SqlServer.Smo
4. Cliquez sur le bouton **Parcourir** pour ajouter une référence à **Microsoft.SqlServer.Management.IntegrationServices**. (Cet assembly est installé uniquement dans le Global Assembly Cache (GAC).) La boîte de dialogue **Sélectionner les fichiers à référencer** s’ouvre.
5. Dans la boîte de dialogue **Sélectionner les fichiers à référencer**, accédez au dossier GAC qui contient l’assembly. En général, il s’agit du dossier `C:\Windows\assembly\GAC_MSIL\Microsoft.SqlServer.Management.IntegrationServices\14.0.0.0__89845dcd8080cc91`.
6. Sélectionnez l’assembly (autrement dit, le fichier .dll) dans le dossier et cliquez sur **Ajouter**.
7. Cliquez sur **OK** pour fermer la boîte de dialogue **Gestionnaire de références** et ajouter les trois références. Pour vérifier que les références y figurent, consultez la liste **Références** dans l’Explorateur de solutions.

## <a name="add-the-c-code"></a>Ajouter le code C# 
1. Ouvrez **Program.cs**.

2. Remplacez le contenu de **Program.cs** par le code suivant. Ajoutez les valeurs appropriées pour votre serveur, base de données, utilisateur et mot de passe.

> [!NOTE]
> L’exemple suivant utilise l’authentification Windows. Pour utiliser l’authentification SQL Server, remplacez l’argument `Integrated Security=SSPI;` par `User ID=<user name>;Password=<password>;`. Si vous vous connectez à un serveur Azure SQL Database, vous ne pouvez pas utiliser l’authentification Windows.

```csharp
using Microsoft.SqlServer.Management.IntegrationServices;
using System;
using System.Data.SqlClient;
using System.IO;

namespace deploy_ssis_project
{
    class Program
    {
        static void Main(string[] args)
        {
            // Variables
            string targetServerName = "localhost";
            string targetFolderName = "Project1Folder";
            string projectName = "Integration Services Project1";
            string projectFilePath = @"C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac";

            // Create a connection to the server
            string sqlConnectionString = "Data Source=" + targetServerName +
                ";Initial Catalog=master;Integrated Security=SSPI;";
            SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);

            // Create the Integration Services object
            IntegrationServices integrationServices = new IntegrationServices(sqlConnection);

            // Get the Integration Services catalog
            Catalog catalog = integrationServices.Catalogs["SSISDB"];

            // Create the target folder
            CatalogFolder folder = new CatalogFolder(catalog,
                targetFolderName, "Folder description");
            folder.Create();

            Console.WriteLine("Deploying " + projectName + " project.");

            byte[] projectFile = File.ReadAllBytes(projectFilePath);
            folder.DeployProject(projectName, projectFile);

            Console.WriteLine("Done.");
        }
    }
}
```

## <a name="run-the-code"></a>Exécuter le code

1. Pour exécuter l’application, appuyez sur **F5**.
2. Dans SSMS, vérifiez que le projet a été déployé.

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour déployer un package.
    - [Déployer un package SSIS avec SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [Déployer un package SSIS à partir de l’invite de commandes](./ssis-quickstart-deploy-cmdline.md)
    - [Déployer un package SSIS avec PowerShell](ssis-quickstart-deploy-powershell.md)
- Exécutez un package déployé. Pour exécuter un package, vous pouvez choisir parmi plusieurs outils et langages. Pour plus d’informations, consultez les articles suivants :
    - [Exécuter un package SSIS avec SSMS](./ssis-quickstart-run-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Exécuter un package SSIS à partir de l’invite de commandes](./ssis-quickstart-run-cmdline.md)
    - [Exécuter un package SSIS avec PowerShell](ssis-quickstart-run-powershell.md)
    - [Exécuter un package SSIS avec C#](./ssis-quickstart-run-dotnet.md) 
