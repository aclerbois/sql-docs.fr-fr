---
title: Choisir une destination (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.chooseadestination.f1
ms.assetid: 1898be15-3e69-42d3-8ecb-3733c9f6c8e3
caps.latest.revision: 104
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 88d04a720ccd3e9f2255b029479c5dc8b73c9506
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="choose-a-destination-sql-server-import-and-export-wizard"></a>Choisir une destination (Assistant Importation et Exportation SQL Server)
 Une fois que vous avez fourni les informations relatives à la source de vos données et indiqué comment s’y connecter, l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche **Choisir une destination**. Dans cette page, vous fournissez les informations relatives à la destination de vos données et à la façon de s’y connecter.
  
Pour plus d’informations sur les destinations de données que vous pouvez utiliser, consultez [Quelles sources de données et destinations puis-je utiliser ?](import-and-export-data-with-the-sql-server-import-and-export-wizard.md#wizardSources). 

## <a name="screen-shot-of-the-destination-page"></a>Capture d’écran de la page Destination
La capture d’écran suivante montre la première partie de la page **Choisir une destination** de l’Assistant. Le reste de la page présente un nombre variable d’options qui dépendent de la destination de données que vous choisissez ici.

![Choisir une destination](../../integration-services/import-export-data/media/choose-destination.png)

## <a name="choose-a-destination"></a>Choisir une destination
 **Destination**  
 Spécifiez la destination en sélectionnant le fournisseur de données qui peut importer les données dans la destination.
 
-   **Le fournisseur de données dont vous avez besoin est généralement repérable par son nom**, car celui-ci comprend habituellement le nom de la destination : par exemple, Destination *Fichier plat*, Microsoft *Excel*, Microsoft *Access*, Fournisseur de données .Net Framework pour *SqlServer*, Fournisseur de données .Net Framework pour *Oracle*.

-   **Si vous avez un pilote ODBC pour votre destination**, sélectionnez le fournisseur de données .NET Framework pour ODBC. Entrez ensuite les informations propres au pilote. Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des destinations. Le fournisseur de données .NET Framework pour ODBC agit comme un wrapper autour du pilote ODBC. Pour plus d’informations, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

-   **Plusieurs fournisseurs peuvent être disponibles pour votre destination.** En général, vous pouvez sélectionner n’importe quel fournisseur qui fonctionne avec votre destination. Par exemple, pour vous connecter à Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez utiliser le fournisseur de données .NET Framework pour SQL Server ou le pilote ODBC de SQL Server. (D’autres fournisseurs sont également toujours présents dans la liste, mais ils ne sont plus pris en charge.) 

## <a name="my-destination-isnt-in-the-list"></a>Ma destination n’est pas dans la liste
-   **Vous pouvez être amené à télécharger le fournisseur de données** à partir de Microsoft ou d’un tiers. La liste des fournisseurs de données disponibles affichée dans la liste **Destination** comprend uniquement les fournisseurs installés sur votre ordinateur. Pour plus d’informations sur les destinations que vous pouvez utiliser, consultez [Quelles sources de données et quelles destinations puis-je utiliser ?](import-and-export-data-with-the-sql-server-import-and-export-wizard.md#wizardSources)

-   **Avez-vous un pilote ODBC pour votre destination ?** Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des destinations. Si vous avez un pilote ODBC pour votre destination, sélectionnez le fournisseur de données .NET Framework pour ODBC. Entrez ensuite les informations propres au pilote. Le fournisseur de données .NET Framework pour ODBC agit comme un wrapper autour du pilote ODBC. Pour plus d’informations, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

-   **Fournisseurs 32 bits et 64 bits.** Si vous exécutez l’Assistant 64 bits, vous ne voyez pas les destinations pour lesquelles seul un fournisseur 32 bits est installé, et vice versa.

> [!NOTE]
> Pour utiliser la version 64 bits de l’Assistant Importation et Exportation SQL Server, vous devez installer SQL Server. SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) sont des applications 32 bits qui installent uniquement des fichiers 32 bits, y compris la version 32 bits de l’Assistant.

## <a name="after-you-choose-a-destination"></a>Après avoir choisi une destination
Après avoir choisi une destination, le reste de la page **Choisir une destination** présente un nombre variable d’options qui dépendent du fournisseur de données choisi.

Pour se connecter à une destination couramment utilisée, consultez une de ces pages.
-   [Se connecter à SQL Server](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Oracle](../../integration-services/import-export-data/connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter aux fichiers plats (fichiers texte)](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Access](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Stockage Blob Azure](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)
-   [Se connecter à PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)

Pour plus d’informations sur la façon de se connecter à une destination qui n’est pas répertoriée ici, consultez [The Connection Strings Reference](https://www.connectionstrings.com/). Ce site tiers contient des exemples de chaînes de connexion et des renseignements complémentaires sur les fournisseurs de données et les informations de connexion dont ils ont besoin.

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Une fois que vous avez fourni les informations relatives à la destination de vos données et à la façon de s’y connecter, la page suivante est **Spécifier la copie ou l’interrogation de table**. Dans cette page, vous indiquez si vous voulez copier la totalité d’une table ou uniquement certaines lignes. Pour plus d’informations, consultez [Spécifier la copie ou l’interrogation de table](../../integration-services/import-export-data/specify-table-copy-or-query-sql-server-import-and-export-wizard.md).  

## <a name="see-also"></a>Voir aussi
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)


