---
title: Outils SQL et des utilitaires pour SQL Server, base de données SQL Azure et Azure SQL Data Warehouse | Documents Microsoft
ms.custom: ''
ms.date: 02/22/2018
ms.prod: sql
ms.prod_service: sql-tools
ms.component: misc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: 0
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: be35a6b708e2f8a5430a796b466705f222d9748d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "33075286"
---
# <a name="sql-tools-and-utilities-for-sql-server-azure-sql-database-and-azure-sql-data-warehouse"></a>Outils SQL et des utilitaires pour SQL Server, base de données SQL Azure et l’entrepôt de données SQL Azure
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Pour gérer (requête, analyse, etc.) votre base de données que vous avez besoin d’un outil. Il existe plusieurs outils de base de données. Alors que vos bases de données peuvent être exécutés dans le cloud, sur Windows ou sur [Linux](../linux/sql-server-linux-overview.md), l’outil n’a pas besoin pour s’exécuter sur la même plateforme en tant que la base de données. 

Cet article fournit des informations sur les outils disponibles pour l’utilisation de vos bases de données SQL. 


## <a name="tools-to-run-queries-and-manage-databases"></a>Outils pour exécuter des requêtes et de gérer des bases de données  

| Outil | Description |
|:--|:--|
| [[!INCLUDE[name-sos](../includes/name-sos.md)]](../sql-operations-studio/download.md) | [!INCLUDE[name-sos](../includes/name-sos-short.md)] est un outil gratuit et léger, pour la gestion des bases de données partout où ils s’exécutent. Cette version préliminaire fournit des fonctionnalités de gestion de base de données, y compris un éditeur Transact-SQL étendue et personnalisable connaître l’état de fonctionnement de vos bases de données. **[!INCLUDE[name-sos](../includes/name-sos-short.md)] s’exécute sur Windows et Linux macOS**.|
| [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) | Utilisez SQL Server Management Studio (SSMS) pour les requêtes, concevoir et gérer votre SQL Server, base de données SQL Azure et Azure SQL Data Warehouse. **SSMS est exécuté sur Windows**.|
| [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md) | Activez Visual Studio dans un environnement de développement puissant pour SQL Server, base de données SQL Azure et Azure SQL Data Warehouse. **SSDT s’exécute sur Windows**.|
|[MSSQL-cli](mssql-cli.md)|MSSQL-cli est un outil de ligne de commande interactif pour l’exécution de requêtes SQL Server. **MSSQL-cli s’exécute sur Windows, Mac OS et Linux**|
| [Visual Studio Code](https://code.visualstudio.com/)| Après avoir installé Visual Studio Code, vous devez installer le [mssql extension](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) pour le développement de Microsoft SQL Server, base de données SQL Azure et SQL Data Warehouse. **Code Visual Studio s’exécute sur Windows et Linux macOS**.|

## <a name="which-tool-should-i-choose"></a>Quel outil dois-je choisir ?

- Vous souhaitez gérer une instance de SQL Server ou de la base de données, dans un éditeur non activable sur Windows, Linux ou Mac ? Choisissez [[!INCLUDE[name-sos](../includes/name-sos.md)]](../sql-operations-studio/download.md)
- Vous souhaitez gérer une instance de SQL Server ou de la base de données sur Windows avec prise en charge complète de l’interface graphique utilisateur ? Choisissez [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- Vous souhaitez créer ou mettre à jour le code de base de données, y compris la validation au moment de compilation, la refactorisation et le concepteur prend en charge sur Windows ? Choisissez [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- Vous souhaitez interroger le serveur SQL Server avec un outil de ligne de commande qui fonctionnalités IntelliSense, syntaxe haute-éclairage, et bien plus encore ? Choisissez [mssql-cli](mssql-cli.md)
- Vous souhaitez écrire des scripts T-SQL dans un éditeur non activable sur Windows, Linux ou Mac ? Choisissez [Visual Studio Code](https://code.visualstudio.com/) et [mssql extension](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="additional-tools"></a>Outils supplémentaires

| Outil | Description |
|:--|:--|
| [Gestionnaire de configuration](../tools/configuration-manager/sql-server-configuration-manager-help.md) | Utilisez le Gestionnaire de Configuration SQL Server pour configurer les services SQL Server et de configurer la connectivité réseau. Configuration Manager s’exécute sur Windows|
|[MSSQL-conf](../linux/sql-server-linux-configure-mssql-conf.md)|Mssql-conf permet de configurer SQL Server en cours d’exécution sur Linux.|
| [Assistant de migration SQL Server](../ssma/sql-server-migration-assistant.md) | Utilisez l’Assistant Migration SQL Server pour automatiser la migration de bases de données vers SQL Server à partir de Microsoft Access, DB2, MySQL, Oracle et Sybase.|
| [Distributed Replay](../tools/distributed-replay/install-distributed-replay-overview.md) | Utilisez la fonctionnalité de Distributed Replay pour vous aider à évaluer l’impact de futures mises à niveau de SQL Server. Utilisez également Distributed Replay pour aider à évaluer l’impact de matériel et de mises à niveau du système d’exploitation et de réglage de SQL Server. |
| [ssbdiagnose](../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md) | L’utilitaire ssbdiagnose signale des problèmes dans les conversations Service Broker ou la configuration des services de Service Broker. |


## <a name="command-line-utilities"></a>Utilitaires en ligne de commande

  Les utilitaires en ligne de commande vous permettent d’écrire des scripts d’opérations [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le tableau suivant contient la liste des utilitaires d'invite de commandes fournis avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
|**Utility**|**Description**|**Installé dans**|  
|-----------------|---------------------|----------------------|  
|[Utilitaire bcp](../tools/bcp-utility.md)|Utilisé pour copier des données entre une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et un fichier de données dans un format spécifié par l’utilisateur.|\<*lecteur*:>\Program Files\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[Utilitaire dta](../tools/dta/dta-utility.md)|Sert à analyser une charge de travail et à recommander des structures PDS (Physical Design Structures) permettant d'optimiser les performances du serveur pour cette charge de travail.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire dtexec](../integration-services/packages/dtexec-utility.md)|Sert à configurer et à exécuter un package [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . La version interface utilisateur de cet utilitaire d’invite de commandes se nomme **DTExecUI**et ouvre l’utilitaire d’exécution de package.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilitaire dtutil](../integration-services/dtutil-utility.md)|Utilisé pour gérer les packages SSIS.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Déployer des solutions de modèle avec l'utilitaire de déploiement](../analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility.md)|Sert à déployer les projets [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] dans des instances d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VShell\Common7\IDE|  
|[MSSQL-Générateur de script (version préliminaire publique)](https://blogs.technet.microsoft.com/dataplatforminsider/2017/05/17/try-new-sql-server-command-line-tools-to-generate-t-sql-scripts-and-monitor-dynamic-management-views/)|Utilisé pour générer des scripts de création et de T-SQL INSERT pour les objets de base de données de SQL Server, base de données SQL Azure et Azure SQL Data Warehouse.|Consultez notre [référentiel GitHub](https://github.com/Microsoft/sql-xplat-cli) pour le téléchargement et l’utilisation des informations.| 
|[Utilitaire osql](../tools/osql-utility.md)|Vous permet d'entrer des instructions, des procédures système et des fichiers de script [!INCLUDE[tsql](../includes/tsql-md.md)] à l'invite de commandes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire profiler](../tools/profiler-utility.md)|Sert à démarrer [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] à partir d'une invite de commandes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire RS.exe &#40;SSRS&#41;](../reporting-services/tools/rs-exe-utility-ssrs.md)|Sert à exécuter des scripts conçus pour gérer des serveurs de rapports [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire rsconfig &#40;SSRS&#41;](../reporting-services/tools/rsconfig-utility-ssrs.md)|Utilisé pour configurer une connexion de serveur de rapports.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire rskeymgmt &#40;SSRS&#41;](../reporting-services/tools/rskeymgmt-utility-ssrs.md)|Sert à gérer des clés de chiffrement sur un serveur de rapports.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqlagent90](../tools/sqlagent90-application.md)|Utilisé pour démarrer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent à partir d'une invite de commandes.|\<lecteur>:\Program Files\Microsoft SQL Server\\<*instance_name*>\MSSQL\Binn|  
|[sqlcmd Utility](../tools/sqlcmd-utility.md)|Vous permet d'entrer des instructions, des procédures système et des fichiers de script [!INCLUDE[tsql](../includes/tsql-md.md)] à l'invite de commandes.|\<*lecteur*:>\Program Files\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[Utilitaire SQLdiag](../tools/sqldiag-utility.md)|Sert à recueillir des informations de diagnostic pour le service de support technique [!INCLUDE[msCoName](../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqllogship](../tools/sqllogship-application.md)|Permet aux applications d'effectuer des opérations de restauration, de copie et de sauvegarde et les tâches de nettoyage associées pour une configuration de l'envoi de journaux sans effectuer de travaux de restauration, de copie et de sauvegarde.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire SqlLocalDB](../tools/sqllocaldb-utility.md)|Mode d'exécution de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] destiné aux développeurs de programme.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\|  
|[Utilitaire sqlmaint](../tools/sqlmaint-utility.md)|Sert à exécuter des plans de maintenance de bases de données créés dans des versions précédentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|\<lecteur>:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilitaire sqlps](../tools/sqlps-utility.md)|Sert à exécuter des commandes et des scripts PowerShell. Charge et inscrit le fournisseur PowerShell [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et les cmdlets.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqlservr](../tools/sqlservr-application.md)|Sert à démarrer et arrêter une instance de [!INCLUDE[ssDE](../includes/ssde-md.md)] à partir de l'invite de commandes pour le dépannage.|\<lecteur>:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilitaire Ssms](../tools/sql-server-management-studio/ssms-utility.md)|Sert à démarrer [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] à partir d'une invite de commandes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VSShell\Common7\IDE|  
|[Utilitaire tablediff](../tools/tablediff-utility.md)|Sert à comparer les données de deux tables pour détecter une non-convergence, ce qui s'avère utile lors du dépannage d'une topologie de réplication.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]COM|  

## <a name="sql-command-prompt-utilities-syntax-conventions"></a>Conventions régissant la syntaxe des utilitaires d’invite de commandes SQL  
  
|**Convention**|**Utilisé pour**|  
|--------------------|------------------|  
|MAJUSCULES|Instructions et termes utilisés au niveau du système d'exploitation.|  
|`monospace`|Exemples de commandes et de code de programmation.|  
|*italique*|Paramètres fournis par l'utilisateur.|  
|**gras**|Commandes, paramètres et autres éléments de la syntaxe qui doivent être tapés exactement de la manière indiquée.|  



