---
title: Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données (SQL Server) | Microsoft Docs
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
- mapping columns to fields during import [SQL Server]
- format files [SQL Server], mapping columns to fields
ms.assetid: e7ee4f7e-24c4-4eb7-84d2-41e57ccc1ef1
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 438aeab5917f8c89d86c281b86705cfc3d6ad351
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server"></a>Utiliser un fichier de format pour mapper les colonnes d'une table aux champs d'un fichier de données (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Il se peut que les champs d'un fichier de données ne soient pas dans le même ordre que les colonnes correspondantes présentes dans la table. Cette rubrique présente les fichiers de format XML et non-XML ayant été modifiés de sorte à accepter un fichier de données dont les champs sont organisés dans un ordre différent de celui des colonnes de la table correspondante. Le fichier de format modifié permet de mapper les champs de données sur les colonnes correspondantes de la table.  Veuillez consulter [Créer un fichier de format (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) pour plus d’informations.

|Contour|
|---|
|[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de données](#sample_data_file)<br />[Création des fichiers de format](#create_format_file)<br />&emsp;&#9679;&emsp;[Création d’un fichier de format non-XML](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[Modification du fichier de format non-XML](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[Création d’un fichier de format XML](#xml_format_file)<br />&emsp;&#9679;&emsp;[Modification du fichier de format XML](#modify_xml_format_file)<br />[Importation de données avec un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données](#import_data)<br />&#9679;&emsp;&emsp;[Utilisation d’un fichier de format bcp et non-XML](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et d’un fichier de format XML](#bcp_xml)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et d’un fichier de format non-XML](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et d’un fichier de format XML](#bulk_xml)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et d’un fichier de format non-XML](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et d’un fichier de format XML](#openrowset_xml)|

> [!NOTE]  
>  Vous pouvez utiliser un fichier de format non-XML ou XML pour importer en bloc un fichier de données dans la table à l’aide d’une commande [bcp utility](../../tools/bcp-utility.md), d’une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou d’une instruction INSERT... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Pour plus d’informations, consultez [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  

## Exemples de conditions de test<a name="etc"></a>  
Les fichiers de format modifiés pris en exemple dans cette rubrique sont fondés sur la table et le fichier de données définis ci-dessous.

### Exemple de table<a name="sample_table"></a>
Le script ci-dessous crée une base de données de test et une table nommée `myRemap`.  Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myRemap
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   Gender char(1)
   );
```

### Fichier de données d’exemple<a name="sample_data_file"></a>
Les données ci-dessous présentent `FirstName` et `LastName` dans l’ordre inverse, comme présenté dans la table `myRemap`.  À l’aide du Bloc-notes, créez un fichier vide `D:\BCP\myRemap.bcp` et insérez les données suivantes :
```
1,Grosse,Anthony,M
2,Fatnowna,Alica,F
3,Rosenhain,Stella,F
```

## Création des fichiers de format<a name="create_format_file"></a>
Pour effectuer l’importation en bloc de données de `myRemap.bcp` dans la table `myRemap` , le fichier de format doit effectuer les tâches suivantes :
* mapper le premier champ des données à la première colonne, `PersonID`;
* mapper le second champ des données à la troisième colonne, `LastName`;
* mapper le troisième champ des données à la deuxième colonne, `FirstName`;
* mapper le quatrième champ des données à la quatrième colonne, `Gender`.

La méthode la plus simple pour créer le fichier de format consiste à utiliser [bcp utility](../../tools/bcp-utility.md).  Tout d’abord, créez un fichier de format de base à partir de la table existante.  Ensuite, modifiez le fichier de format de base afin qu’il reflète le fichier de données réel.

### Création d’un fichier de format non-XML<a name="nonxml_format_file"></a>
Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées. La commande suivante utilise l’ [utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non-xml `myRemap.fmt`basé sur le schéma de `myRemap`.  En outre, le qualificateur `c` est utilisé pour sépcifier les données de caractère, `t,` est utilisé pour spécifier une virgule comme délimiteur de champ, et `T` est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :
```
bcp TestDatabase.dbo.myRemap format nul -c -f D:\BCP\myRemap.fmt -t, -T
```
### Modification du fichier de format non-XML <a name="modify_nonxml_format_file"></a>
Consultez [Structure des fichiers de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) pour la terminologie.  Ouvrez `D:\BCP\myRemap.fmt` dans Bloc-notes et effectuez les modifications suivantes :
1.  Réorganisez l’ordre des lignes du fichier de format, afin que les lignes soient dans le même ordre que les données de `myRemap.bcp`.
2.  Assurez-vous que les valeurs d’ordre du champ de fichier hôte soient séquentielles.
3.  Assurez-vous qu’un retour chariot est inséré après la dernière ligne du fichier de format.

Comparez les modifications :     
**Avant**
```
13.0
4
1       SQLCHAR 0       7       ","      1     PersonID               ""
2       SQLCHAR 0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR 0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
**After**
```
13.0
4
1       SQLCHAR 0       7       ","      1     PersonID               ""
2       SQLCHAR 0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR 0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
Le fichier de format modifié reflète à présent les éléments suivants :
* Le premier champ de données figurant dans `myRemap.bcp` est mappé à la première colonne ; ` myRemap.. PersonID`
* Le second champ de données figurant dans `myRemap.bcp` est mappé à la troisième colonne ; `myRemap.. LastName`
* Le troisième champ de données figurant dans `myRemap.bcp` est mappé à la deuxième colonne ; `myRemap.. FirstName`
* Le quatrième champ de données figurant dans `myRemap.bcp` est mappé à la quatrième colonne ; ` myRemap.. Gender`

### Création d’un fichier de format XML <a name="xml_format_file"></a>  
Veuillez consulter [Fichiers de format XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise l’ [utilitaire bcp](../../tools/bcp-utility.md) pour créer un fichier de format xml `myRemap.xml`basé sur le schéma de `myRemap`.  En outre, le qualificateur `c` est utilisé pour sépcifier les données de caractère, `t,` est utilisé pour spécifier une virgule comme délimiteur de champ, et `T` est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  Le qualificateur `x` doit être utilisé pour générer un fichier de format XML.  À partir d'une invite de commandes, entrez la commande suivante :
```
bcp TestDatabase.dbo.myRemap format nul -c -x -f D:\BCP\myRemap.xml -t, -T
```
### Modification du fichier de format XML <a name="modify_xml_format_file"></a>
Consultez [Syntaxe de schéma pour les fichiers de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) pour la terminologie.  Ouvrez `D:\BCP\myRemap.xml` dans Bloc-notes et effectuez les modifications suivantes :
1. L’ordre dans lequel les éléments \<FIELD> sont déclarés dans le format de fichier est l’ordre dans lequel ces champs s’affichent dans le fichier de données, par conséquent, inversez l’ordre pour les éléments \<FIELD> avec les attributs d’ID 2 et 3.
2. Vérifiez que les valeurs de l’attribut \<FIELD> ID sont séquentielles.
3. L’ordre des éléments \<COLUMN> dans l’élément \<ROW> définit l’ordre dans lequel ils sont renvoyés par l’opération en bloc.  Le fichier de format XML assigne à chaque élément \<COLUMN> un nom local qui n’a aucune relation avec la colonne dans la table cible d’une opération d’importation en bloc.  L’ordre des éléments \<COLUMN> est indépendant de l’ordre des éléments \<FIELD> dans une définition \<RECORD>.  Chaque élément \<COLUMN> correspond à un élément \<FIELD> (dont l’ID est spécifié dans l’attribut SOURCE de l’élément \<COLUMN>).  Par conséquent, les valeurs de \<COLUMN> SOURCE sont ls seuls attributs qui nécessitent une révision.  Inversez l’ordre pour les attributs 2 et 3 de \<COLUMN> SOURCE.

Comparez les modifications :  
**Avant**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
**After**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="2" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
Le fichier de format modifié reflète à présent les éléments suivants :
* FIELD 1, qui correspond à COLUMN 1, est mappé à la première colonne de la table `myRemap.. PersonID`
* FIELD 2, qui correspond à COLUMN 2, est mappé de nouveau à la troisième colonne de la table `myRemap.. LastName`
* FIELD 3, qui correspond à COLUMN 3, est mappé de nouveau à la seconde colonne de la table `myRemap.. FirstName`
* FIELD 4, qui correspond à COLUMN 4, est mappé à la quatrième colonne de la table `myRemap.. Gender`


## Importation de données avec un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données<a name="import_data"></a>
Les exemples ci-dessous utilisent la base de données, le fichier de données et les fichiers de format créés ci-dessus.

### Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
À partir d'une invite de commandes, entrez la commande suivante :
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.fmt -T
```

### Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
À partir d'une invite de commandes, entrez la commande suivante :
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.xml -T
```

### Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### [Utilisation d’OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
Exécutez l’instruction Transact-SQL suivant dans Microsoft SQL Server Management Studio (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```


  
## <a name="see-also"></a> Voir aussi  
[Utilitaire bcp](../../tools/bcp-utility.md)   
 [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
  
