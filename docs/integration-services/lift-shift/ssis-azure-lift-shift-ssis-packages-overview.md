---
title: Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud | Microsoft Docs
ms.date: 05/22/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.component: lift-shift
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e3c8b39ac59f3ac6bddd985de12602f498e1ed97
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="lift-and-shift-sql-server-integration-services-workloads-to-the-cloud"></a>Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud
Vous pouvez maintenant déplacer vos packages et charges de travail SQL Server Integration Services (SSIS) vers le cloud Azure.
-   Stockez et gérez les projets et packages SSIS dans la base de données de catalogues SSIS (SSISDB) sur Azure SQL Database ou SQL Database Managed Instance (préversion).
-   Exécutez des packages dans une instance d’Azure-SSIS Integration Runtime, composant d’Azure Data Factory.
-   Utilisez des outils familiers comme SQL Server Management Studio (SSMS) pour les tâches courantes.

## <a name="benefits"></a>Avantages
Le déplacement de vos charges de travail SSIS locales vers Azure présente les avantages potentiels suivants :
-   **Réduisez les coûts opérationnels** et la charge de gestion de l’infrastructure quand vous exécutez SSIS localement ou sur des machines virtuelles Azure.
-   **Augmentez la haute disponibilité** pour pouvoir spécifier plusieurs nœuds par cluster, et étendez les fonctionnalités de haute disponibilité d’Azure et d’Azure SQL Database.
-   **Augmentez la scalabilité** pour pouvoir spécifier plusieurs cœurs par nœud (scale up) et plusieurs nœuds par cluster (scale out).

## <a name="architecture-overview"></a>Vue d’ensemble de l’architecture
Le tableau suivant met en évidence les différences entre une instance SSIS locale et une instance SSIS sur Azure. La différence la plus importante est la séparation du stockage et de l’exécution. Azure Data Factory héberge le moteur de runtime des packages SSIS sur Azure. Le moteur d’exécution est appelé Azure-SSIS Integration Runtime (Azure-SSIS IR). Pour plus d’informations, consultez [Azure-SSIS Runtime Integration](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime).

| Stockage | Runtime | Extensibilité |
|---|---|---|
| Local (SQL Server) | Runtime SSIS hébergé par SQL Server | SSIS Scale Out (dans SQL Server 2017 et versions ultérieures)<br/><br/>Solutions personnalisées (dans les versions antérieures de SQL Server) |
| Dans Azure (SQL Database ou SQL Database Managed Instance (préversion)) | Azure-SSIS Integration Runtime, composant d’Azure Data Factory | Options de mise à l’échelle pour Azure-SSIS Integration Runtime |
| | | |

Il vous suffit de provisionner Azure-SSIS IR une seule fois. Vous pouvez ensuite utiliser des outils familiers tels que SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) pour déployer, configurer, exécuter, surveiller, planifier et gérer les packages.

## <a name="version-support"></a>Prise en charge de version

Vous pouvez déployer n’importe quel package créé avec l’une des versions de SSIS sur Azure. Quand vous déployez un package sur Azure sans erreur de validation, le package est automatiquement mis à niveau avec le dernier format de package. Le package est donc toujours mis à niveau avec la dernière version de SSIS.

Le processus de déploiement valide chaque package pour vérifier qu’il peut s’exécuter dans Azure SSIS Integration Runtime. Pour plus d’informations, consultez [Valider des packages SSIS déployés sur Azure](ssis-azure-validate-packages.md).

## <a name="prerequisites"></a>Conditions préalables requises

Pour déployer des packages SSIS sur Azure, vous devez avoir l’une des versions suivantes de SQL Server Data Tools (SSDT) :
-   Pour Visual Studio 2017, version 15.3 ou ultérieure.
-   Pour Visual Studio 2015, version 17.2 ou version ultérieure.

Pour plus d’informations sur les prérequis d’Azure-SSIS Integration Runtime, consultez [Déployer des packages SQL Server Integration Services sur Azure - Prérequis](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure#prerequisites).

> [!NOTE]
> Pendant cette préversion publique, Azure-SSIS Integration Runtime n’est pas encore disponible dans toutes les régions. Pour plus d’informations sur les régions prises en charge, consultez [Produits disponibles par région - Microsoft Azure](https://azure.microsoft.com/regions/services/).

## <a name="provision-ssis-on-azure"></a>Provisionner SSIS sur Azure

Avant de pouvoir déployer et exécuter des packages SSIS dans Azure, vous devez provisionner la base de données de catalogues SSIS (SSISDB) et Azure SSIS Integration Runtime. Suivez les étapes de provisionnement dans cet article : [Déployer des packages SQL Server Integration Services sur Azure](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure).

Quand vous provisionnez Azure-SSIS IR, vous pouvez effectuer un scale up et un scale out en spécifiant des valeurs pour les options suivantes :
-   La taille du nœud (notamment le nombre de cœurs) et le nombre de nœuds du cluster.
-   L’instance existante d’Azure SQL Database pour héberger la base de données de catalogues SSIS (SSISDB), et le niveau de service de la base de données.
-   Le nombre maximal d’exécutions parallèles par nœud.

Pour plus d’informations sur les performances, consultez [Configurer Azure-SSIS Integration Runtime pour de hautes performances](https://docs.microsoft.com/azure/data-factory/configure-azure-ssis-integration-runtime-performance).

## <a name="design-packages"></a>Concevoir des packages

Vous continuez à **concevoir et générer des packages** localement dans SSDT, ou dans Visual Studio avec SSDT installé.

### <a name="connect-to-data-sources"></a>Se connecter aux sources de données

Pour plus d’informations sur la connexion à des sources de données locales à partir du cloud avec **l’authentification Windows**, consultez [Se connecter à des sources de données locales et des partages de fichiers Azure avec l’authentification Windows](ssis-azure-connect-with-windows-auth.md).

Pour plus d’informations sur la connexion à des fichiers et partages de fichiers, consultez [Stocker et récupérer des fichiers sur des partages de fichiers locaux et dans Azure avec SSIS](ssis-azure-files-file-shares.md).

### <a name="available-ssis-components"></a>Composants SSIS disponibles

Quand vous provisionnez une instance de SQL Database pour héberger SSISDB, le Feature Pack Azure pour SSIS et le composant redistribuable Access sont également installés. Ces composants fournissent une connectivité à diverses sources de données **Azure** et aux fichiers **Excel et Access**, en plus des sources de données prises en charge par les composants intégrés.

Vous pouvez également installer des composants supplémentaires. Pour plus d’informations, consultez [Installation personnalisée pour le runtime d’intégration SSIS Azure](/azure/articles/data-factory/how-to-configure-azure-ssis-ir-custom-setup.md).

Si vous êtes éditeur de logiciels, vous pouvez mettre à jour l’installation de vos composants sous licence pour les rendre disponibles sur Azure. Pour plus d’informations, consultez [Développer des composants personnalisés payants ou sous licence pour le runtime d’intégration Azure-SSIS](https://docs.microsoft.com/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components).

### <a name="transaction-support"></a>Prise en charge des transactions

Avec SQL Server en local et sur des machines virtuelles Azure, vous pouvez utiliser des transactions Microsoft Distributed Transaction Coordinator (MSDTC). Pour configurer MSDTC sur chaque nœud d’Azure-SSIS IR, utilisez la fonctionnalité d’installation personnalisée. Pour plus d’informations, consultez [Installation personnalisée pour le runtime d’intégration SSIS Azure](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Avec Azure SQL Database, vous pouvez utiliser uniquement des transactions élastiques. Pour plus d’informations, consultez [Transactions distribuées entre les bases de données cloud](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview).

## <a name="deploy-and-run-packages"></a>Déployer et exécuter des packages

**Modèle de déploiement**. Vous devez utiliser le **modèle de déploiement de projet**, et non le modèle de déploiement de package, quand vous déployez des projets dans SSISDB sur Azure.

**Options de déploiement et d’exécution**. Pour déployer des projets et exécuter des packages sur Azure, vous pouvez utiliser plusieurs outils et options de script de votre choix :
-   SQL Server Management Studio (SSMS)
-   Transact-SQL (à partir de SSMS, Visual Studio Code ou tout autre outil)
-   Un outil en ligne de commande
-   PowerShell
-   C# et le modèle objet de gestion SSIS

**Se connecter à SSISDB**. Le **nom de l’instance SQL Database** qui héberge SSISDB devient la première partie du nom en quatre parties à utiliser quand vous déployez et exécutez des packages à partir de SSDT et SSMS, au format suivant : `<sql_database_name>.database.windows.net`. Pour plus d’informations sur la connexion à la base de données de catalogues SSIS dans Azure, consultez [Se connecter à la base de données de catalogues SSISDB sur Azure](ssis-azure-connect-to-catalog-database.md).

Pour commencer, consultez [Déployer, exécuter et surveiller un package SSIS sur Azure](ssis-azure-deploy-run-monitor-tutorial.md).

## <a name="monitor-packages"></a>Surveiller les packages
Pour surveiller les packages en cours d’exécution dans SSMS, vous pouvez utiliser les outils de création de rapports suivants dans SSMS.
-   Cliquez avec le bouton droit sur **SSISDB**, puis sélectionnez **Opérations actives** pour ouvrir la boîte de dialogue **Opérations actives**.
-   Sélectionnez un package dans l’Explorateur d’objets, cliquez avec le bouton droit, sélectionnez **Rapports**, **Rapports standard**, puis **Toutes les exécutions**.

Pour surveiller Azure-SSIS Integration Runtime, consultez [Surveiller le runtime d’intégration Azure-SSIS](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

## <a name="schedule-packages"></a>Planifier les packages
Pour planifier l’exécution des packages stockés dans Azure SQL Database, vous pouvez utiliser divers outils. Pour plus d’informations, consultez [Planifier l’exécution d’un package SSIS sur Azure](ssis-azure-schedule-packages.md).

## <a name="next-steps"></a>Étapes suivantes
Pour bien démarrer avec les charges de travail SSIS sur Azure, consultez les articles suivants :
-   [Déployer des packages SQL Server Integration Services sur Azure](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)
-   [Déployer, exécuter et surveiller un package SSIS sur Azure](ssis-azure-deploy-run-monitor-tutorial.md)
