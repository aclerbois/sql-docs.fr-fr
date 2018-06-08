---
title: Utiliser un fichier de format pour ignorer un champ de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], skipping data fields
- skipping data fields when importing
ms.assetid: 6a76517e-983b-47a1-8f02-661b99859a8b
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5ddb64e049a94f16b04985aeb5e6240940befbd2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-a-format-file-to-skip-a-data-field-sql-server"></a>Utiliser un fichier de format pour ignorer un champ de données (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Le nombre de champs contenus dans un fichier de données peut être supérieur au nombre de colonnes dans la table. Cette rubrique explique comment modifier les fichiers de format non-XML et XML en mappant les colonnes de la table avec les champs de données correspondants et en ignorant les champs supplémentaires afin de prendre en charge un fichier de données contenant davantage de champs.  Veuillez consulter [Créer un fichier de format (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) pour plus d’informations.

|Contour|
|---|
|[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de données](#sample_data_file)<br />[Création des fichiers de format](#create_format_file)<br />&emsp;&#9679;&emsp;[Création d’un fichier de format non-XML](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[Modification d’un fichier de format non-XML](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[Création d’un fichier de format XML](#xml_format_file)<br />&emsp;&#9679;&emsp;[Modification d’un fichier de format XML](#modify_xml_format_file)<br />[Importation de données avec un fichier de format pour ignorer un champ de données](#import_data)<br />&#9679;&emsp;&emsp;[Utilisation d’un fichier de format bcp et non-XML](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et d’un fichier de format XML](#bcp_xml)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et d’un fichier de format non-XML](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et d’un fichier de format XML](#bulk_xml)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et d’un fichier de format non-XML](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et d’un fichier de format XML](#openrowset_xml)<p>                                                                                                                                                                                                                  </p>|
  
> [!NOTE]
>  Vous pouvez utiliser un fichier de format non-XML ou XML pour importer en bloc un fichier de données dans la table à l’aide d’une commande [bcp utility](../../tools/bcp-utility.md), d’une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou d’une instruction INSERT... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Pour plus d’informations, consultez [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

## Exemples de conditions de test<a name="etc"></a>  
Les fichiers de format modifiés pris en exemple dans cette rubrique sont fondés sur la table et le fichier de données définis ci-dessous.
  
### Exemple de table<a name="sample_table"></a>
Le script ci-dessous crée une base de données de test et une table nommée `myTestSkipField`.  Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
 
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myTestSkipField
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30)
   );
```
  
### Fichier de données d’exemple<a name="sample_data_file"></a>
Créez un fichier vide `D:\BCP\myTestSkipField.bcp` et insérez les données suivantes : 
```
1,SkipMe,Anthony,Grosse
2,SkipMe,Alica,Fatnowna
3,SkipMe,Stella,Rosenhain
```
  
## Création des fichiers de format<a name="create_format_file"></a>
Pour effectuer l’importation en bloc de données de `myTestSkipField.bcp` dans la table `myTestSkipField` , le fichier de format doit effectuer les tâches suivantes :
* mapper le premier champ des données à la première colonne, `PersonID`;
* ignorer le deuxième champ des données ;
* mapper le troisième champ des données à la deuxième colonne, `FirstName`;
* mapper le quatrième champ des données à la troisième colonne, `LastName`.  

La méthode la plus simple pour créer le fichier de format consiste à utiliser [bcp utility](../../tools/bcp-utility.md).  Tout d’abord, créez un fichier de format de base à partir de la table existante.  Ensuite, modifiez le fichier de format de base afin qu’il reflète le fichier de données réel.
  
### <a name="nonxml_format_file"></a>Création d’un fichier de format non-XML 
Veuillez consulter [Fichiers de format non-XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées. La commande suivante utilise [bcp utility](../../tools/bcp-utility.md) pour générer un fichier de format non-xml `myTestSkipField.fmt`basé sur le schéma de `myTestSkipField`.  En outre, le qualificateur `c` est utilisé pour sépcifier les données de caractère, `t,` est utilisé pour spécifier une virgule comme délimiteur de champ, et `T` est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :

```
bcp TestDatabase.dbo.myTestSkipField format nul -c -f D:\BCP\myTestSkipField.fmt -t, -T
```

### Modification du fichier de format non-XML <a name="modify_nonxml_format_file"></a>
Consultez [Structure des fichiers de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) pour la terminologie. Ouvrez `D:\BCP\myTestSkipField.fmt` dans Bloc-notes et effectuez les modifications suivantes :
1) Copiez l’intégralité de la ligne format-file pour `FirstName` et collez-la directement après `FirstName` sur la ligne suivante.
2) Incrémentez de 1 la valeur d’ordre des champs du fichier hôte pour la nouvelle ligne et toutes les lignes suivantes.
3) Augmentez la valeur du nombre de colonnes pour refléter le nombre réel de champs figurant dans le fichier de données.
3) Modifiez l’ordre des colonnes du serveur en les faisant passer de `2` à `0` pour la deuxième ligne format-file. 

Comparez les modifications apportées :  
**Avant**
```
13.0
3
1       SQLCHAR 0       7       ","      1     PersonID     ""
2       SQLCHAR 0       25      ","      2     FirstName    SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       30      "\r\n"   3     LastName     SQL_Latin1_General_CP1_CI_AS
```
**After**
```
13.0
4
1       SQLCHAR 0       7       ","      1     PersonID     ""
2       SQLCHAR 0       25      ","      0     FirstName    SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       25      ","      2     FirstName    SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR 0       50      "\r\n"   3     LastName     SQL_Latin1_General_CP1_CI_AS

```

Le fichier de format modifié reflète à présent les éléments suivants :
* 4 champs de données
* Le premier champ de données figurant dans `myTestSkipField.bcp` est mappé à la première colonne ; ` myTestSkipField.. PersonID`
* Le deuxième champ de données dans `myTestSkipField.bcp` n’est mappé à aucune colonne.
* Le troisième champ de données figurant dans `myTestSkipField.bcp` est mappé à la deuxième colonne ; `myTestSkipField.. FirstName`
* Le quatrième champ de données figurant dans `myTestSkipField.bcp` est mappé à la troisième colonne ; `myTestSkipField.. LastName`

### Création d’un fichier de format XML <a name="xml_format_file"></a>  
Veuillez consulter [Fichiers de format XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [bcp utility](../../tools/bcp-utility.md) pour créer un fichier de format xml `myTestSkipField.xml`basé sur le schéma de `myTestSkipField`.  En outre, le qualificateur `c` est utilisé pour sépcifier les données de caractère, `t,` est utilisé pour spécifier une virgule comme délimiteur de champ, et `T` est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  Le qualificateur `x` doit être utilisé pour générer un fichier de format XML.  À partir d'une invite de commandes, entrez la commande suivante :

```
bcp TestDatabase.dbo.myTestSkipField format nul -c -x -f D:\BCP\myTestSkipField.xml -t, -T
```

### Modification du fichier de format XML <a name="modify_xml_format_file"></a>
Consultez [Syntaxe de schéma pour les fichiers de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) pour la terminologie.  Ouvrez `D:\BCP\myTestSkipField.xml` dans Bloc-notes et effectuez les modifications suivantes :
1) Copiez la totalité du deuxième champ et collez-la directement après le deuxième champ sur la ligne suivante.
2) Incrémentez la valeur « FIELD ID » de 1 pour le nouveau champ et pour chaque champ suivant.
3) Incrémentez la valeur de « COLUMN SOURCE » de 1 pour `FirstName`, et `LastName` afin de refléter le mappage modifié.

Comparez les modifications apportées :  
**Avant**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
 </ROW>
</BCPFORMAT>
```

**After**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="4" NAME="LastName" xsi:type="SQLVARYCHAR"/>
 </ROW>
</BCPFORMAT>
```

Le fichier de format modifié reflète à présent les éléments suivants :
* 4 champs de données
* FIELD 1 qui correspond à COLUMN 1 est mappé à la première colonne de la table. `myTestSkipField.. PersonID`
* FIELD 2 ne correspond à aucune COLUMN et n’est, par conséquent, mappé à aucune colonne de la table.
* FIELD 3 qui correspond à COLUMN 3 est mappé à la seconde colonne de la table. `myTestSkipField.. FirstName`
* FIELD 4 qui correspond à COLUMN 4 est mappé à la troisième colonne de la table. `myTestSkipField.. LastName`

## Importation de données avec un fichier de format pour ignorer un champ de données<a name="import_data"></a>
Les exemples ci-dessous utilisent la base de données, le fichier de données et les fichiers de format créés ci-dessus.

### Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
À partir d'une invite de commandes, entrez la commande suivante :
```
bcp TestDatabase.dbo.myTestSkipField IN D:\BCP\myTestSkipField.bcp -f D:\BCP\myTestSkipField.fmt -T
```

### Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
À partir d'une invite de commandes, entrez la commande suivante :
```
bcp TestDatabase.dbo.myTestSkipField IN D:\BCP\myTestSkipField.bcp -f D:\BCP\myTestSkipField.xml -T
```

### Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
BULK INSERT dbo.myTestSkipField   
   FROM 'D:\BCP\myTestSkipField.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myTestSkipField.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
BULK INSERT dbo.myTestSkipField   
   FROM 'D:\BCP\myTestSkipField.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myTestSkipField.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myTestSkipField;
INSERT INTO dbo.myTestSkipField
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myTestSkipField.bcp',
        FORMATFILE = 'D:\BCP\myTestSkipField.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### [Utilisation d’OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
INSERT INTO dbo.myTestSkipField 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myTestSkipField.bcp',
        FORMATFILE = 'D:\BCP\myTestSkipField.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

  
## <a name="see-also"></a> Voir aussi  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
  
