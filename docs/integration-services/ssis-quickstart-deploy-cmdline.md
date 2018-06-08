---
title: Déployer un projet SSIS à partir de l’invite de commandes | Microsoft Docs
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5f1bd8e0279da97974a4a1b420c7eea592931c6a
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="deploy-an-ssis-project-from-the-command-prompt-with-isdeploymentwizardexe"></a>Déployer un projet SSIS à partir de l’invite de commandes avec ISDeploymentWizard.exe
Ce guide de démarrage rapide montre comment déployer un projet SSIS à partir de l’invite de commandes en exécutant l’Assistant Déploiement d’Integration Services, `ISDeploymentWizard.exe`.

Pour plus d’informations sur l’Assistant Déploiement d’Integration Services, consultez [Assistant Déploiement d’Integration Services](packages/deploy-integration-services-ssis-projects-and-packages.md#integration-services-deployment-wizard).

## <a name="prerequisites"></a>Conditions préalables requises

La validation décrite dans cet article pour le déploiement sur Azure SQL Database nécessite SQL Server Data Tools (SSDT) version 17.4 ou ultérieure. Pour obtenir la dernière version de SSDT, consultez [Télécharger SSDT (SQL Server Data Tools)](../ssdt/download-sql-server-data-tools-ssdt.md).

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

## <a name="authentication-methods-in-the-deployment-wizard"></a>Méthodes d’authentification dans l’Assistant Déploiement

Si vous déployez sur un serveur SQL Server avec l’Assistant Déploiement, vous devez utiliser l’authentification Windows. Vous ne pouvez pas utiliser l’authentification SQL Server.

Si vous déployez sur un serveur Azure SQL Database, vous devez utiliser l’authentification SQL Server ou l’authentification Azure Active Directory. Vous ne pouvez pas utiliser l’authentification Windows.

## <a name="start-the-integration-services-deployment-wizard"></a>Démarrer l’Assistant Déploiement d’Integration Services
1. Ouvrez une fenêtre d'invite de commandes.

2. Exécutez `ISDeploymentWizard.exe`. L’Assistant Déploiement d’Integration Services s’ouvre.

    Si le dossier qui contient `ISDeploymentWizard.exe` ne figure pas dans votre variable d’environnement `path`, vous devrez peut-être utiliser la commande `cd` pour basculer vers son répertoire. Pour SQL Server 2017, ce dossier est généralement `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn`.

## <a name="deploy-a-project-with-the-wizard"></a>Déployer un projet avec l’Assistant
1. Dans la page **Introduction** de l’Assistant, examinez l’introduction. Cliquez sur **Suivant** pour ouvrir la page **Sélectionner la source**.

2. Dans la page **Sélectionner la source**, sélectionnez le projet SSIS existant à déployer.
    -   Pour déployer un fichier de déploiement de projet que vous avez créé en générant un projet dans l’environnement de développement, sélectionnez **Fichier de déploiement de projet** et entrez le chemin du fichier .ispac.
    -   Pour déployer un projet déjà déployé dans une base de données du catalogue SSIS, sélectionnez **Catalogue Integration Services** et entrez le nom du serveur et le chemin du projet dans le catalogue.
    Cliquez sur **Suivant** pour afficher la page **Sélectionner la destination** .
  
3.  Dans la page **Sélectionner la destination**, sélectionnez la destination du projet.
    -   Entrez le nom complet du serveur. Si le serveur cible est un serveur Azure SQL Database, le nom est au format suivant : `<server_name>.database.windows.net`.
    -   Fournissez les informations d’authentification et sélectionnez **Se connecter**. Consultez [Méthodes d’authentification dans l’Assistant Déploiement](#wizard_auth) dans cet article.
    -   Sélectionnez ensuite **Parcourir** pour sélectionner le dossier cible dans SSISDB.
    -   Ensuite, sélectionnez **Suivant** pour ouvrir la page **Vérifier**. (Le bouton **Suivant** est activé seulement si vous sélectionnez **Se connecter**.)

4.  Dans la page **Vérifier**, vérifiez les paramètres que vous avez sélectionnés.
    -   Vous pouvez modifier vos sélections en cliquant sur **Précédent**ou en cliquant sur l'une des étapes dans le volet gauche.
    -   Cliquez sur **Déployer** pour démarrer le processus de déploiement.

5.  Si vous déployez sur un serveur Azure SQL Database, la page **Valider** s’ouvre et inspecte les packages du projet à la recherche de problèmes connus susceptibles d’impacter leur exécution dans Azure-SSIS Integration Runtime. Pour plus d’informations, consultez [Valider des packages SSIS déployés sur Azure](lift-shift/ssis-azure-validate-packages.md).

6.  Une fois le processus de déploiement effectué, la page **Résultats** s’affiche. Cette page indique la réussite ou l’échec de chaque action.
    -   Si l’action a échoué, cliquez sur **Échec** dans la colonne **Résultat** pour afficher une explication de l’erreur.
    -   Si vous le souhaitez, cliquez sur **Enregistrer le rapport** pour enregistrer les résultats dans un fichier XML.
    -   Cliquez sur **Fermer** pour quitter l’Assistant.

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour déployer un package.
    - [Déployer un package SSIS avec SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [Déployer un package SSIS avec PowerShell](ssis-quickstart-deploy-powershell.md)
    - [Déployer un package SSIS avec C#](./ssis-quickstart-deploy-dotnet.md) 
- Exécutez un package déployé. Pour exécuter un package, vous pouvez choisir parmi plusieurs outils et langages. Pour plus d’informations, consultez les articles suivants :
    - [Exécuter un package SSIS avec SSMS](./ssis-quickstart-run-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Exécuter un package SSIS à partir de l’invite de commandes](./ssis-quickstart-run-cmdline.md)
    - [Exécuter un package SSIS avec PowerShell](ssis-quickstart-run-powershell.md)
    - [Exécuter un package SSIS avec C#](./ssis-quickstart-run-dotnet.md) 
