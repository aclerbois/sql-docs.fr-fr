---
title: Choisir une source de données (Assistant Importation et Exportation SQL Server) | Microsoft Docs
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
- sql13.dts.impexpwizard.chooseadatasource.f1
ms.assetid: ebf28a62-dfc1-4b39-9db5-df1919e5fccb
caps.latest.revision: 124
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: df46499e89a9a4482ab8440049fd1dea0e608c4a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="choose-a-data-source-sql-server-import-and-export-wizard"></a>Choisir une source de données (Assistant Importation et Exportation SQL Server)
  Après la page d’accueil, l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche **Choisir une source de données**. Dans cette page, vous indiquez la source de vos données et la façon de s’y connecter.
  
Pour plus d’informations sur les sources de données que vous pouvez utiliser, consultez [Quelles sont les sources et destinations de données que je peux utiliser ?](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md#wizardSources)

## <a name="screen-shot-of-the-choose-a-data-source-page"></a>Capture d’écran de la page Choisir une source de données 
La capture d’écran suivante montre la première partie de la page **Choisir une source de données** de l’Assistant. Le reste de la page présente un nombre variable d’options qui dépendent de la source de données que vous choisissez ici.

![Choisir la source](../../integration-services/import-export-data/media/choose-source.png)

## <a name="choose-a-data-source"></a>Choisir une source de données
 **Source de données**  
Spécifiez la source de données en sélectionnant le fournisseur de données qui peut se connecter à la source.

-   **Le fournisseur de données dont vous avez besoin est généralement repérable par son nom**, car celui-ci comprend habituellement le nom de la source de données : par exemple, Source *Fichier plat*, Microsoft *Excel*, Microsoft *Access*, Fournisseur de données .Net Framework pour *SqlServer*, Fournisseur de données .Net Framework pour *Oracle*.

-   **Si vous avez un pilote ODBC pour votre source de données**, sélectionnez le fournisseur de données .NET Framework pour ODBC. Entrez ensuite les informations propres au pilote. Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des sources de données. Le fournisseur de données .NET Framework pour ODBC agit comme un wrapper autour du pilote ODBC. Pour plus d’informations, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

-   **Plusieurs fournisseurs peuvent être disponibles pour votre source de données.** En général, vous pouvez sélectionner n’importe quel fournisseur qui fonctionne avec votre source. Par exemple, pour vous connecter à Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez utiliser le fournisseur de données .NET Framework pour SQL Server ou le pilote ODBC de SQL Server. (D’autres fournisseurs sont également toujours présents dans la liste, mais ne sont plus pris en charge.) 

## <a name="my-data-source-isnt-in-the-list"></a>Ma source de données n’est pas dans la liste
-   **Vous pouvez être amené à télécharger le fournisseur de données** à partir de Microsoft ou d’un tiers. La liste des fournisseurs de données disponibles affichée dans la liste **Source de données** comprend uniquement les fournisseurs installés sur votre ordinateur. Pour plus d’informations sur les sources de données que vous pouvez utiliser, consultez [Quelles sont les sources et destinations de données que je peux utiliser ?](import-and-export-data-with-the-sql-server-import-and-export-wizard.md#wizardSources)

-   **Avez-vous un pilote ODBC pour votre source de données ?** Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des sources de données. Si vous disposez d’un pilote ODBC pour votre source de données, sélectionnez le fournisseur de données .NET Framework pour ODBC. Entrez ensuite les informations propres au pilote. Le fournisseur de données .NET Framework pour ODBC agit comme un wrapper autour du pilote ODBC. Pour plus d’informations, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

-   **Fournisseurs 32 bits et 64 bits.** Si vous exécutez l’Assistant 64 bits, vous ne voyez pas les sources de données pour lesquelles seul un fournisseur 32 bits est installé, et vice versa.

> [!NOTE]
> Pour utiliser la version 64 bits de l’Assistant Importation et Exportation SQL Server, vous devez installer SQL Server. SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) sont des applications 32 bits qui installent uniquement des fichiers 32 bits, y compris la version 32 bits de l’Assistant.

## <a name="after-you-choose-a-data-source"></a>Une fois que vous avez choisi une source de données
Une fois que vous avez choisi une source de données, le reste de la page **Choisir une source de données** présente un nombre variable d’options qui dépendent du fournisseur de données que vous sélectionnez.

Pour se connecter à une source de données couramment utilisée, consultez une de ces pages.
-   [Se connecter à SQL Server](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Oracle](../../integration-services/import-export-data/connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter aux fichiers plats (fichiers texte)](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Access](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à Stockage Blob Azure](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)
-   [Se connecter à PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)
-   [Se connecter à MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)

Pour plus d’informations sur la façon de se connecter à une source de données qui n’est pas répertoriée ici, consultez [The Connection Strings Reference](https://www.connectionstrings.com/) (Références en matière de chaînes de connexion). Ce site tiers contient des exemples de chaînes de connexion et des renseignements complémentaires sur les fournisseurs de données et les informations de connexion dont ils ont besoin.

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Une fois que vous avez fourni les informations relatives à la source de vos données et indiqué comment s’y connecter, la page **Choisir une destination**s’affiche. Dans cette page, vous fournissez les informations relatives à la destination de vos données et à la façon de s’y connecter. Pour plus d’informations, consultez [Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md).
 
## <a name="see-also"></a>Voir aussi
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)


