---
title: Utiliser un fichier de format pour importer des données en bloc (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], format files
- format files [SQL Server], importing data using
ms.assetid: 2956df78-833f-45fa-8a10-41d6522562b9
caps.latest.revision: 45
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f7d4f213bf9ff30304dd45f8c47071998c9a85f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-a-format-file-to-bulk-import-data-sql-server"></a>Utiliser un fichier de format pour importer des données en bloc (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Cette rubrique illustre l'utilisation d'un fichier de format dans les importations en bloc.  Un fichier de format met en relation les champs du fichier de données avec les colonnes de la table.  Veuillez consulter [Créer un fichier de format (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) pour plus d’informations.

|Contour|
|---|
|[Avant de commencer](#begin)<br />[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de données](#sample_data_file)<br />[Création des fichiers de format](#create_format_file)<br />&emsp;&#9679;&emsp;[Création d’un fichier de format non-XML](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[Création d’un fichier de format XML](#xml_format_file)<br />[Utilisation d'un fichier de format pour importer des données en bloc](#import_data)<br />&#9679;&emsp;&emsp;[Utilisation d’un fichier de format bcp et non-XML](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et d’un fichier de format XML](#bcp_xml)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et d’un fichier de format non-XML](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et d’un fichier de format XML](#bulk_xml)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et d’un fichier de format non-XML](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et d’un fichier de format XML](#openrowset_xml)<p>                                                                                                                                                                                                                  </p>|
  
## Avant de commencer<a name="begin"></a>
* Pour qu’un fichier de format fonctionne avec un fichier de données de caractères Unicode, tous les champs d’entrée doivent être des chaînes de texte Unicode (autrement dit, des chaînes Unicode de taille fixe ou terminées par un caractère).
* Pour exporter ou importer en bloc des données [SQLXML](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md) , utilisez l’un des types de données ci-dessous dans votre fichier de format.
  * SQLCHAR ou SQLVARYCHAR (les données sont envoyées dans la page de codes du client ou dans la page de codes impliquée par le classement)
  * SQLNCHAR ou SQLNVARCHAR (les données sont envoyées au format Unicode)
  * SQLBINARY ou SQLVARYBIN (les données sont envoyées sans être converties).
* Azure SQL Database et Azure SQL Data Warehouse prennent uniquement en charge [bcp](../../tools/bcp-utility.md).  Pour plus d’informations, consultez :
  * [Chargement de données dans Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-overview-load/)
  * [Charger des données SQL Server dans Azure SQL Data Warehouse (fichiers plats)](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-from-sql-server-with-bcp/)
  * [Migration de vos données](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-migrate-data/)

## Exemples de conditions de test<a name="etc"></a>  
Les fichiers de format pris en exemple dans cette rubrique sont fondés sur la table et le fichier de données définis ci-dessous.

### **Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données de test et une table nommée `myFirstImport`.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.MyFirstImport (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   BirthDate Date
   );
```

### **Fichier de données d’exemple**<a name="sample_data_file"></a>
À l’aide du Bloc-notes, créez un fichier vide `D:\BCP\myFirstImport.bcp` et insérez les données suivantes :
```
1,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
3,Stella,Rosenhain,1992-03-02
```

Vous pouvez aussi exécuter le script PowerShell suivant pour créer et remplir le fichier de données :
```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = Join-Path -Path $dir -ChildPath 'MyFirstImport.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```

## Création des fichiers de format<a name="create_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.

### **Création d’un fichier de format non-XML**<a name="nonxml_format_file"></a>
Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myFirstImport.fmt`basé sur le schéma de `myFirstImport`.  Pour utiliser une commande bcp pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  Pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de caractère, **t** est utilisé pour spécifier une virgule comme [délimiteur de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -f D:\BCP\myFirstImport.fmt -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.fmt
```

Votre fichier de format non XML `D:\BCP\myFirstImport.fmt` doit se présenter comme suit :
```
13.0
4
1       SQLCHAR             0       7       ","      1     PersonID               ""
2       SQLCHAR             0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR             0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR             0       11      "\r\n"   4     BirthDate              ""
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

### **Création d’un fichier de format XML**<a name="xml_format_file"></a>  
Veuillez consulter [Fichiers de format XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise l’ [utilitaire bcp](../../tools/bcp-utility.md) pour créer un fichier de format xml `myFirstImport.xml`basé sur le schéma de `myFirstImport`. Pour utiliser une commande bcp pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option de format nécessite toujours l’option **-f** . De plus, pour créer un fichier de format XML, vous devez spécifier l’option **-x** .  Pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de caractère, **t** est utilisé pour spécifier une virgule comme [délimiteur de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :
```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -x -f D:\BCP\myFirstImport.xml -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.xml
```

Votre fichier de format XML `D:\BCP\myFirstImport.xml` doit se présenter comme suit :
```xml
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="11"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="4" NAME="BirthDate" xsi:type="SQLDATE"/>
 </ROW>
</BCPFORMAT>
```

## Utilisation d'un fichier de format pour importer des données en bloc<a name="import_data"></a>
Les exemples ci-dessous utilisent la base de données, le fichier de données et les fichiers de format créés ci-dessus.

### **Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bcp_nonxml"></a>
À partir d'une invite de commandes, entrez la commande suivante :
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport"
```


### **Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)**<a name="bcp_xml"></a>
À partir d'une invite de commandes, entrez la commande suivante :
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.xml -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport;"
```


### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bulk_nonxml"></a>
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)**<a name="bulk_xml"></a>
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### **Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="openrowset_nonxml"></a>    
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### **[Utilisation d’OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)**<a name="openrowset_xml"></a>
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```
  
## <a name="more-examples"></a>Exemples supplémentaires  
 [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
 [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
 [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
 [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  [Fichiers de format pour l'importation ou l'exportation de données (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
  
