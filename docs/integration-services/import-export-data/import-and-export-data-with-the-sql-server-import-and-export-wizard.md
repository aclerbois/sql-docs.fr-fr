---
title: Importer et exporter des données avec l’Assistant Importation et Exportation SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- exporting data
- mapping files [Integration Services]
- SQL Server Import and Export Wizard
- SSIS packages, creating
- packages [Integration Services], copying
- Integration Services packages, creating
- packages [Integration Services], creating
- SQL Server Integration Services packages, creating
- Import and Export Wizard
- copying data [Integration Services]
- importing data, SSIS packages
- sources [Integration Services], copying data
ms.assetid: c0e4d867-b2a9-4b2a-844b-2fe45be88f81
caps.latest.revision: 160
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d1478cd130bf6cea79d2a731bd214d13b402d393
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-and-export-data-with-the-sql-server-import-and-export-wizard"></a>Importer et exporter des données avec l’Assistant Importation et Exportation SQL Server

 > Pour accéder au contenu associé aux versions précédentes de SQL Server, consultez [Exécuter l’Assistant Importation et Exportation SQL Server](https://msdn.microsoft.com/library/ms140052(SQL.120).aspx).

 La fonction de l’Assistant Importation et Exportation[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est de copier simplement des données d’une source vers une destination. Cette présentation décrit les sources de données que l’Assistant peut utiliser en tant que sources et destinations, ainsi que les autorisations nécessaires à l’exécution de l’Assistant.

## <a name="get-the-wizard"></a>Se procurer l’Assistant
Si vous souhaitez exécuter l’Assistant, mais [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas installé sur votre ordinateur, vous pouvez installer l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en installant SSDT (SQL Server Data Tools). Pour plus d’informations, consultez [Télécharger SSDT (SQL Server Data Tools)](https://msdn.microsoft.com/library/mt204009.aspx).

## <a name="what-happens-when-i-run-the-wizard"></a>Que se passe-t-il lorsque j’exécute l’Assistant ?
-    **Se référer à la liste des étapes.** Pour obtenir une description des étapes de l’Assistant, consultez [Étapes de l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard.md). Il existe aussi une page de documentation propre à chaque page de l’Assistant.  
    \- ou \-
-   **Voir un exemple.** Pour avoir un rapide aperçu des différents écrans qui s’affichent dans une session normale, étudiez cet exemple facile tenant sur une seule page : [Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).  

##  <a name="wizardSources"></a> Quelles sources et quelles destinations puis-je utiliser ?  
 L’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut copier des données à destination et en provenance des sources de données répertoriées dans le tableau suivant. Pour vous connecter à certaines de ces sources de données, vous pouvez être amené à télécharger et installer des fichiers supplémentaires.
 
| Source de données | Dois-je télécharger des fichiers supplémentaires ? |
|-------------|-----------------------------------------|
|**Bases de données d’entreprise**<br/>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Oracle, DB2 et autres.|SQL Server ou SQL Server Data Tools (SSDT) installe les fichiers dont vous avez besoin pour vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En revanche, SSDT n’installe pas tous les fichiers qui vous sont nécessaires pour vous connecter à d’autres bases de données d’entreprise, telles que Oracle ou IBM DB2.<br/><br/>Pour vous connecter à une base de données d’entreprise, vous devez généralement disposer de deux choses :<br/><br/>1. **Logiciel client**. Si vous disposez déjà du logiciel client de votre système de base de données d’entreprise, vous avez en général ce qu’il vous faut pour établir une connexion. Si vous n’avez pas installé le logiciel client, contactez l’administrateur de base de données pour lui demander comment installer une copie sous licence.<br/><br/>2. **Pilotes ou fournisseurs**. Microsoft installe les pilotes et les fournisseurs nécessaires pour se connecter à Oracle. Pour la connexion à IBM DB2, procurez-vous le Fournisseur OLE DB Microsoft® pour DB2 v5.0 pour Microsoft SQL Server à partir de [Microsoft SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=52676).<br/><br/>Pour plus d’informations, consultez [Se connecter à une source de données SQL Server](connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md) ou [Se connecter à une source de données Oracle](connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md).|
|**Fichiers texte** (fichiers plats)|Aucun fichier supplémentaire n’est requis.<br/><br/>Pour plus d’informations, consultez [Se connecter à une source de données de fichiers plats](connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md).|
|**Fichiers Microsoft Excel et Microsoft Access**|Microsoft Office n’installe pas tous les fichiers nécessaires à l’établissement d’une connexion à des sources de données Excel et Access. Procédez au téléchargement suivant : [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920).<br/><br/>Pour plus d’informations, consultez [Se connecter à une source de données Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md) et [Se connecter à une source de données Access](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md).|
|**Sources de données Azure**<br/>Stockage d’objets blob Azure uniquement pour l’instant.|SQL Server Data Tools n’installe pas les fichiers dont vous avez besoin pour vous connecter à Stockage Blob Azure en tant que source de données. Obtenez le téléchargement suivant : [Microsoft SQL Server 2016 Integration Services Feature Pack pour Azure](https://www.microsoft.com/download/details.aspx?id=49492).<br/><br/>Pour plus d’informations, consultez [Se connecter à Stockage Blob Azure](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md).|
|**Bases de données open source**<br/>PostgreSQL, MySql et autres.|Pour vous connecter à ces sources de données, vous devez télécharger des fichiers supplémentaires.<br/><br/>- Pour **PostgreSQL**, consultez [Se connecter à une source de données PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md).<br/>- Pour **MySql**, consultez [Se connecter à une source de données MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md).|
|**Toute autre source de données pour laquelle un pilote ou un fournisseur est disponible**|Généralement, vous devez télécharger des fichiers supplémentaires pour vous connecter aux sources de données des types suivants.<br/><br/>- N’importe quelle source pour laquelle un **pilote ODBC** est disponible. Pour plus d’informations, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).<br/>- N’importe quelle source pour laquelle un **fournisseur de données .NET Framework** est disponible.<br/>- N’importe quelle source pour laquelle un **fournisseur OLE DB** est disponible.<br/><br/>Les composants tiers qui fournissent des fonctionnalités de source et de destination pour d’autres sources de données sont parfois commercialisés comme compléments pour SQL Server Integration Services (SSIS).|

## <a name="how-do-i-connect-to-my-data"></a>Comment me connecter à mes données ?
Pour plus d’informations sur la façon de se connecter à une source de données couramment utilisée, consultez une des pages suivantes :
-   [Se connecter à SQL Server](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Oracle](../../integration-services/import-export-data/connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter aux fichiers plats (fichiers texte)](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Access](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Stockage Blob Azure](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)
-   [Se connecter à ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)


Pour plus d’informations sur la façon de se connecter à une source de données qui n’est pas répertoriée ici, consultez [The Connection Strings Reference](https://www.connectionstrings.com/) (Références en matière de chaînes de connexion). Ce site tiers contient des exemples de chaînes de connexion et des renseignements complémentaires sur les fournisseurs de données et les informations de connexion dont ils ont besoin.

## <a name="what-permissions-do-i-need"></a>De quelles autorisations ai-je besoin ?  
 Pour mener à bien les étapes de l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez disposer au moins des autorisations suivantes. Si vous utilisez déjà vos source et destination de données, vous avez probablement les autorisations nécessaires.
  
|Des autorisations sont nécessaires pour effectuer les opérations suivantes|Si vous vous connectez à SQL Server, vous avez besoin de ces autorisations spécifiques |  
|-------------------------|----------------------------------------------------|  
|Se connecter aux bases de données sources et de destination ou aux partages de fichiers|Droits de connexion aux bases de données et au serveur|  
|Exporter ou lire les données d’une base de données ou d’un fichier source.|Autorisations SELECT pour les tables et les vues sources|  
|Importer ou écrire des données dans la base de données ou le fichier de destination.|Autorisations INSERT dans les tables de destination|  
|Créer la base de données ou le fichier de destination, le cas échéant|Autorisations CREATE DATABASE ou CREATE TABLE|  
|Enregistrer le package SSIS créé par l’Assistant, le cas échéant|Si vous souhaitez enregistrer le package dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]autorisations suffisantes pour enregistrer le package dans la base de données **msdb** .|  
  
## <a name="get-help-while-the-wizard-is-running"></a>Obtenir de l’aide pendant l’exécution de l’Assistant
> [!TIP]
> Pour afficher la documentation sur une page ou une boîte de dialogue déterminée de l’Assistant, appuyez sur la touche F1 à partir de cette page.   
  
##  <a name="wizardSSIS"></a> L’Assistant utilise SQL Server Integration Services (SSIS)  
 L’Assistant utilise SQL Server Integration Services (SSIS) pour copier les données. SSIS est un outil d’extraction, de transformation et de chargement (ETL) des données. Les pages de l’Assistant utilisent en partie le langage de SSIS.
  
 Dans SSIS, l’unité de base est le **package**. L’Assistant crée un package SSIS en mémoire à mesure que vous parcourez les pages de l’Assistant et spécifiez les options.    
  
Si vous avez installé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition ou une édition supérieure, vous pouvez éventuellement enregistrer le package SSIS. Vous pouvez ensuite réutiliser le package et l’étendre à l’aide du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] pour y ajouter des tâches, des transformations et une logique pilotée par les événements. L’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] constitue le moyen le plus simple de créer un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de base qui copie des données à partir d’une source vers une destination.

Pour plus d’informations, consultez [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md).

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Démarrez l'Assistant. Pour plus d’informations, consultez [Démarrer l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md).  

## <a name="see-also"></a>Voir aussi
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)  
[Mappage de type de données dans l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard.md)


