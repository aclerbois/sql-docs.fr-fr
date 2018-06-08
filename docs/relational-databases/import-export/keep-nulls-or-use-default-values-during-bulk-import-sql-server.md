---
title: Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc (SQL Server) | Microsoft Docs
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
- bulk importing [SQL Server], null values
- bulk importing [SQL Server], default values
- data formats [SQL Server], null values
- bulk rowset providers [SQL Server]
- bcp utility [SQL Server], null values
- BULK INSERT statement
- default values
- OPENROWSET function, bulk importing
- data formats [SQL Server], default values
ms.assetid: 6b91d762-337b-4345-a159-88abb3e64a81
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3d753ac39e0cb5f0eb19ab5c0ed146a80f743ab2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="keep-nulls-or-use-default-values-during-bulk-import-sql-server"></a>Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l'importation en bloc (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Par défaut, quand des données sont importées dans une table, la commande [bcp](../../tools/bcp-utility.md) et l’instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) inspectent toutes les valeurs par défaut définies pour les colonnes de la table.  Par exemple, si un fichier de données contient un champ NULL, la valeur par défaut de la colonne est chargée à la place.  La commande [bcp](../../tools/bcp-utility.md) et l’instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) vous permettent de spécifier que les valeurs NULL doivent être conservées.

À l'opposé, une instruction INSERT standard conserve la valeur NULL au lieu d'insérer une valeur par défaut. L'instruction INSERT ... L’instruction SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) présente le même comportement de base que l’instruction INSERT standard, mais elle prend également en charge un [indicateur de table](../../t-sql/queries/hints-transact-sql-table.md) pour l’insertion des valeurs par défaut.

|Contour|
|---|
|[Conservation des valeurs Null](#keep_nulls)<br />[Utilisation des valeurs par défaut avec l’instruction INSERT ... SELECT * FROM OPENROWSET(BULK...)](#keep_default)<br />[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de données](#sample_data_file)<br />&emsp;&#9679;&emsp;[Exemple de fichier de format non XML](#nonxml_format_file)<br />[Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l'importation en bloc](#import_data)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et conservation des valeurs Null sans fichier de format](#bcp_null)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et conservation des valeurs Null avec un fichier de format non XML](#bcp_null_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et utilisation des valeurs par défaut sans fichier de format](#bcp_default)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et utilisation des valeurs par défaut avec un fichier de format non XML](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et conservation des valeurs Null sans fichier de format](#bulk_null)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et conservation des valeurs Null avec un fichier de format non XML](#bulk_null_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et utilisation des valeurs par défaut sans fichier de format](#bulk_default)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et utilisation des valeurs par défaut avec un fichier de format non XML](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et conservation des valeurs Null avec un fichier de format non XML](#openrowset__null_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET(BULK...) et utilisation des valeurs par défaut avec un fichier de format non XML](#openrowset__default_fmt)

## Conservation des valeurs Null<a name="keep_nulls"></a>  
Les qualificateurs suivants spécifient qu'un champ vide du fichier de données conserve sa valeur NULL lors de l'importation en bloc, au lieu d'hériter d'une valeur par défaut (s'il y en a une) pour les colonnes de table.  Pour [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md), par défaut, les colonnes qui ne sont pas spécifiées dans l’opération de chargement en bloc sont définies avec la valeur Null.
  
|Command|Qualificateur|Type de qualificateur|  
|-------------|---------------|--------------------|  
|bcp|-k|Commutateur|  
|BULK INSERT|KEEPNULLS**\***|Argument|  
|INSERT ... SELECT * FROM OPENROWSET(BULK...)|Néant|Néant|  
  
**\*** Dans le cas de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), si des valeurs par défaut ne sont pas disponibles, la colonne de table doit être définie de manière à autoriser les valeurs Null. 
  
> [!NOTE]
> Ces qualificateurs désactivent le contrôle des définitions DEFAULT sur une table par ces commandes d'importation en bloc.  Toutefois, pour toute instruction INSERT concurrente, des définitions DEFAULT sont attendues.
 
## Utilisation des valeurs par défaut avec l’instruction INSERT ... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)<a name="keep_default"></a>  
Pour un champ vide du fichier de données, vous pouvez spécifier que la colonne de table correspondante utilise sa valeur par défaut (s’il y en a une).  Pour utiliser les valeurs par défaut, utilisez l’indicateur de table [KEEPDEFAULTS](../../t-sql/queries/hints-transact-sql-table.md).
 
> [!NOTE]
>  Pour plus d’informations, consultez [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md), [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md), [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md) et [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)

## Exemples de conditions de test<a name="etc"></a>  
Les exemples de cette rubrique sont fondés sur la table, le fichier de données et le fichier de format définis ci-dessous.

### **Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données de test et une table nommée `myNulls`.  La quatrième colonne de table, `Kids`, a une valeur par défaut.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNulls ( 
   PersonID smallint not null,
   FirstName varchar(25),
   LastName varchar(30),
   Kids varchar(13) DEFAULT 'Default Value',
   BirthDate date
   );
```

### **Exemple de fichier de données**<a name="sample_data_file"></a>
À l’aide du Bloc-notes, créez un fichier vide `D:\BCP\myNulls.bcp` et insérez les données ci-dessous.  Notez qu’il n’existe aucune valeur dans le troisième enregistrement, quatrième colonne.

```
1,Anthony,Grosse,Yes,1980-02-23
2,Alica,Fatnowna,No,1963-11-14
3,Stella,Rosenhain,,1992-03-02
```

Vous pouvez aussi exécuter le script PowerShell suivant pour créer et remplir le fichier de données :

```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'MyNulls.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,Yes,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,No,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```
  
### **Exemple de fichier de format non XML**<a name="nonxml_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise l’ [utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myNulls.fmt`basé sur le schéma de `myNulls`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  Pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de caractère, **t** est utilisé pour spécifier une virgule comme [délimiteur de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp TestDatabase.dbo.myNulls format nul -c -f D:\BCP\myNulls.fmt -t, -T

REM Review file
Notepad D:\BCP\myNulls.fmt
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

 Pour plus d’informations sur la création de fichiers de format, consultez [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
## Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l'importation en bloc<a name="import_data"></a>
Les exemples ci-dessous utilisent la base de données, le fichier de données et les fichiers de format créés ci-dessus.

### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et conservation des valeurs Null sans fichier de format**<a name="bcp_null"></a>

Commutateur **-k** .  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et conservation des valeurs Null avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bcp_null_fmt"></a>
Commutateurs **-k** et **-f** . À partir d'une invite de commandes, entrez la commande suivante :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et utilisation des valeurs par défaut sans fichier de format**<a name="bcp_default"></a>
À partir d'une invite de commandes, entrez la commande suivante :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et utilisation des valeurs par défaut avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bcp_default_fmt"></a>
Commutateur **-f** .  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et conservation des valeurs Null sans fichier de format**<a name="bulk_null"></a>
Argument**KEEPNULLS** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
USE TestDatabase;
GO
TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
    FROM 'D:\BCP\myNulls.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et conservation des valeurs Null avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bulk_null_fmt"></a>
Arguments**KEEPNULLS** et **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt',
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et utilisation des valeurs par défaut sans fichier de format**<a name="bulk_default"></a>
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et utilisation des valeurs par défaut avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bulk_default_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### **Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et conservation des valeurs Null avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="openrowset__null_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### **Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et utilisation des valeurs par défaut avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="openrowset__default_fmt"></a>
Indicateur de table**KEEPDEFAULTS** et argument **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
WITH (KEEPDEFAULTS) 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Conserver des valeurs d’identité lors de l’importation de données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)  
  
-   [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)  
  
 **Pour utiliser un fichier de format**  
  
-   [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
-   [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
 **Pour utiliser des formats de données pour l'importation ou l'exportation en bloc**  
  
-   [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
 **Pour spécifier des formats de données pour la compatibilité lors de l'utilisation de bcp**  
  
-   [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
-   [Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
  
  
