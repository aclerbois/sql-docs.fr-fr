---
title: Importer et exporter des données en bloc à l’aide de l’utilitaire bcp (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], bcp utility
- bulk importing [SQL Server], bcp utility
- bcp utility [SQL Server], about bcp utility
ms.assetid: 73e949de-67a3-4c84-9735-7da1ad4ba34a
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4cef9b1c98ced19606f0e3d2650816a8214f526d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-and-export-bulk-data-by-using-the-bcp-utility-sql-server"></a>Importer et exporter des données en bloc à l'aide de l'utilitaire bcp (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Cette rubrique est une présentation générale de l'utilisation de l' [utilitaire bcp](../../tools/bcp-utility.md) pour exporter des données à partir de n'importe quel emplacement d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenant une instruction SELECT, vues partitionnées comprises.  
  
 L'utilitaire bcp (Bcp.exe) est un outil de ligne de commande qui fait appel à l'API BCP (Bulk Copy Program). L'utilitaire exécute les tâches suivantes :  
  
-   Exportations en bloc des données à partir d'une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un fichier de données.  
  
-   Exportations en bloc des données à partir d'une requête.  
  
-   Importations en bloc des données à partir d'un fichier de données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Génération des fichiers de format.  
  
 L’utilitaire bcp est accessible via la commande **bcp** . Pour utiliser la commande **bcp** afin d’importer des données en bloc, vous devez comprendre le schéma de la table et les types de données de ses colonnes, à moins que vous n’utilisiez un fichier de format pré-existant.  
  
 L'utilitaire bcp peut exporter des données à partir d'une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un fichier de données qui sera utilisé dans d'autres programmes. L'utilitaire permet également d'importer des données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un autre programme, généralement un autre système de gestion de base de données (SGBD). Les données sont d'abord exportées à partir du programme source dans un fichier de données, puis copiées, au cours d'une opération séparée, à partir du fichier de données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 La commande **bcp** fournit des commutateurs qui vous permettent de spécifier le type de données du fichier de données ainsi que d’autres informations. Si ces commutateurs ne sont pas spécifiés, une commande bcp demande des informations de mise en forme, comme le type de champs de données dans un fichier de données. La commande vous propose ensuite de créer un fichier de format contenant vos réponses interactives. Ce fichier est le plus souvent utile si vous avez besoin de flexibilité pour de futures opérations d'importation-exportation en bloc. Vous pouvez spécifier le fichier de format lors de l’exécution ultérieure de commandes **bcp** pour des fichiers de données équivalents. Pour plus d’informations, consultez [Spécifier des formats de données pour la compatibilité lors de l’utilisation de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
>**Remarque** L’utilitaire bcp est écrit à l’aide de la copie en bloc ODBC.
  
 Pour obtenir une description de la syntaxe de la commande **bcp** , consultez [bcp Utility](../../tools/bcp-utility.md).  
  
## <a name="examples"></a>Exemples  
|Les rubriques suivantes contiennent des exemples de l’utilisation de bcp : |
|---|
|[bcp Utility](../../tools/bcp-utility.md)<br /><br />Formats de données pour l'importation en bloc ou l'exportation en bloc (SQL Server)<br />&emsp;&#9679;&emsp;[Utiliser le format natif pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format caractère pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format natif Unicode pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format caractère Unicode pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[Spécifier des indicateurs de fin de champ et de fin de ligne (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l'importation en bloc (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[Conserver des valeurs d'identité lors de l'importation de données en bloc (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />Fichiers de format pour l’importation ou l’exportation de données (SQL Server)<br />&emsp;&#9679;&emsp;[Créer un fichier de format (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour importer des données en bloc (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour ignorer une colonne de table (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour ignorer un champ de données (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[Exemples d'importation et d'exportation en bloc de documents XML (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>                                                                                                                                                                                                                  </p>|

  
## <a name="more-examples-and-information"></a>Autres exemples et informations  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Clause SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-clause-transact-sql.md)   
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [Préparer l’importation de données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-to-bulk-import-data-sql-server.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
  
