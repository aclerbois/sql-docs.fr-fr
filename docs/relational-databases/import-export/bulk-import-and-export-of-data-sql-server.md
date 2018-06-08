---
title: Importation et exportation en bloc de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- exporting data
- bulk importing [SQL Server], about bulk importing
- data [SQL Server], bulk export and import
- transferring data
- importing data, (See also bulk importing [SQL Server])
- copying data [SQL Server]
- bulk exporting [SQL Server]
- importing data, bulk import
- copying data [SQL Server], bulk export and import
- exporting data,(See also bulk exporting [SQL Server])
- bulk exporting [SQL Server], about bulk exporting
- bulk importing [SQL Server]
- importing data
ms.assetid: 19049021-c048-44a2-b38d-186d9f9e4a65
caps.latest.revision: 61
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e6ed82f416bdc4e94b779a98671d7382749e626b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bulk-import-and-export-of-data-sql-server"></a>Importation et exportation en bloc de données (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge l’exportation en bloc de données (*données en bloc*) à partir d’une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l’importation en bloc de données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une vue non partitionnée. 
  
-   L'*exportation en bloc* consiste à copier des données d'une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers un fichier de données.

-   Le terme*importation en bloc* fait référence au chargement de données d’un fichier de données vers une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par exemple, vous pouvez exporter des données d'une application [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel vers un fichier de données, puis importer en bloc ces données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
 
##  <a name="MethodsForBuliIE"></a> Méthodes pour l’importation et l’exportation de données en bloc  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge l'exportation en bloc de données à partir d'une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l'importation en bloc de données dans une table ou une vue non partitionnée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les méthodes de base disponibles sont les suivantes.  
 
  
|Méthode|Description|Importe les données|Exporte les données|  
|------------|-----------------|------------------|------------------|  
|[bcp (utilitaire)](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)|Utilitaire en ligne de commande (Bcp.exe) qui exporte et importe en bloc des données et génère des fichiers de format.|Oui|Oui|  
|[BULK INSERT, instruction](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)|Instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui importe des données directement d'un fichier de données dans une table de base de données ou une vue non partitionnée.|Oui|non|  
|instruction [INSERT ... instruction SELECT * FROM OPENROWSET(BULK...)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)|Instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui utilise le fournisseur d'ensembles de lignes OPENROWSET pour importer en bloc des données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en spécifiant la fonction OPENROWSET(BULK…) afin de sélectionner des données dans une instruction INSERT.|Oui|non| 
|[Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)|L’Assistant crée des packages simples qui importent et exportent des données dans plusieurs formats de données courants (bases de données, feuilles de calcul, fichiers texte, etc.).|Oui|Oui|  
  
> [!IMPORTANT]
> Les fichiers de valeurs séparées par des virgules (CSV, Comma-Separated Value) ne sont pas pris en charge par les opérations d'importation en bloc SQL Server. Toutefois, dans certains cas, un fichier CSV peut être utilisé comme fichier de données pour une importation en bloc de données dans SQL Server. Notez que la marque de fin de champ d'un fichier CSV n'est pas nécessairement une virgule. Pour plus d’informations, consultez [Préparer des données en vue d’une exportation ou d’une importation en bloc (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

> [!NOTE]
> Seul l’utilitaire bcp est pris en charge par Azure SQL Database et Azure SQL DW pour l’importation et l’exportation des fichiers délimités.
  
##  <a name="FFs"></a> Fichiers de format  
 L’[utilitaire bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) gèrent tous l’utilisation d’un *fichier de format* spécialisé qui stocke les informations de format de chaque champ dans un fichier de données. Un fichier de format peut également contenir des informations sur la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondante. Le fichier de format peut être utilisé pour fournir toutes les informations de format nécessaires pour exporter en bloc des données à partir d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et pour importer en bloc des données vers une instance du même type.  
  
 Les fichiers de format procurent une souplesse qui permet d'une part l'interprétation des données tel qu'elles existent dans le fichier de données au moment de l'importation, et d'autre part le formatage des données dans le fichier de données au moment de l'exportation. Cette souplesse vous dispense d'écrire un code spécial en vue d'interpréter ou de reformater les données en fonction des exigences spécifiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de l'application externe. Ainsi, si les données exportées en bloc doivent être chargées dans une application nécessitant des valeurs séparées par une virgule, vous pouvez utiliser un fichier de format pour insérer des virgules comme terminateurs de champ dans les données exportées.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge deux genres de fichiers de format : XML et non-XML.  
  
 L’ [utilitaire bcp](../../tools/bcp-utility.md) est le seul outil capable de générer un fichier de format. Pour plus d’informations, consultez [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md). Pour plus d’informations sur les fichiers de format, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
> [!NOTE]
> Si, à l'occasion d'une opération d'exportation ou d'importation en bloc, aucun fichier de format n'est fourni, vous pouvez choisir de remplacer le formatage par défaut via la ligne de commande.

|Rubriques connexes|
|---|
|[Préparer des données en vue d’une exportation ou d’une importation en bloc (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)|
|[Formats de données pour l'importation en bloc ou l'exportation en bloc (SQL Server)](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format natif pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format caractère pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format natif Unicode pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format caractère Unicode pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)|
|[Spécifier des formats de données pour la compatibilité lors de l'utilisation de bcp (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[Spécifier le type de stockage de fichiers à l’aide de bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[Spécifier la longueur des champs au moyen de bcp (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[Spécifier des indicateurs de fin de champ et de fin de ligne (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)|
|[Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l'importation en bloc (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)|
|[Conserver des valeurs d'identité lors de l'importation de données en bloc (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)|
|[Fichiers de format pour l'importation ou l'exportation de données (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Créer un fichier de format (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour importer des données en bloc (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour ignorer une colonne de table (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour ignorer un champ de données (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)|
 
  
## <a name="more-information"></a>Informations complémentaires  
 [Conditions requises pour une journalisation minimale dans l'importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)   
 [Exemples d’importation et d’exportation en bloc de documents XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)   
 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)   
 [Copier des bases de données sur d'autres serveurs](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [Exécution du chargement en masse de données XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)   
 [Exécution d'opérations de copie en bloc](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)   

  
  
