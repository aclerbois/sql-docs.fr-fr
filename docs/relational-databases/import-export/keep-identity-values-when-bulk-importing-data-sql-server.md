---
title: Conserver des valeurs d’identité lors de l’importation de données en bloc (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server], bulk imports
- data formats [SQL Server], identity values
- bulk importing [SQL Server], identity values
ms.assetid: 45894a3f-2d8a-4edd-9568-afa7d0d3061f
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 38dec1be5097af3b73888da95c52b83b06781022
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="keep-identity-values-when-bulk-importing-data-sql-server"></a>Conserver des valeurs d'identité lors de l'importation de données en bloc (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Les fichiers de données contenant des valeurs d’identité peuvent être importés en bloc dans une instance de Microsoft SQL Server.  Par défaut, les valeurs de la colonne d'identité du fichier de données importé sont ignorées et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte automatiquement des valeurs uniques.  Ces valeurs uniques reposent sur les valeurs de départ et d'incrément spécifiées lors de la création de la table.

Si les fichiers de données ne contiennent pas de valeurs pour la colonne d'identificateur de la table, vous devez utiliser un fichier de format pour préciser si la colonne d'identificateur de la table doit être ignorée lors de l'importation de données.  Consultez [Utiliser un fichier de format pour ignorer une colonne de table (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md) pour plus d’informations.

|Contour|
|---|
|[Conserver les valeurs d’identité](#keep_identity)<br />[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de données](#sample_data_file)<br />&emsp;&#9679;&emsp;[Exemple de fichier de format non XML](#nonxml_format_file)<br />[Exemples](#examples)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et conservation des valeurs d’identité sans fichier de format](#bcp_identity)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et conservation des valeurs d’identité avec un fichier de format non XML](#bcp_identity_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et de valeurs d’identité générées sans fichier de format](#bcp_default)<br />&emsp;&#9679;&emsp;[Utilisation de la commande bcp et de valeurs d’identité générées avec un fichier de format non XML](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et conservation des valeurs d’identité sans fichier de format](#bulk_identity)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et conservation des valeurs d’identité avec un fichier de format non XML](#bulk_identity_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et de valeurs d’identité générées sans fichier de format](#bulk_default)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et de valeurs d’identité générées avec un fichier de format non XML](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET et conservation des valeurs d’identité avec un fichier de format non XML](#openrowset_identity_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET et de valeurs d’identité générées avec un fichier de format non XML](#openrowset_default_fmt)<br /><p>                                                                                                                                                                                                                  </p>|

## Conserver les valeurs d’identité <a name="keep_identity"></a>  
Pour empêcher [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'assigner des valeurs d'identité lors de l'importation en bloc de lignes de données dans une table, utilisez le qualificateur de commande de conservation d'identité approprié.  Lorsque vous spécifiez un qualificateur de conservation d'identité, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les valeurs d'identité du fichier de données.  Ces qualificateurs sont les suivants :

|Command|Qualificateur de conservation d'identité|Type de qualificateur|  
|-------------|------------------------------|--------------------|  
|bcp|-E|Commutateur|  
|BULK INSERT|KEEPIDENTITY|Argument|  
|INSERT ... SELECT * FROM OPENROWSET(BULK...)|KEEPIDENTITY|Indicateur de table|  
   
 Pour plus d’informations, consultez [bcp Utility](../../tools/bcp-utility.md), [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md), [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md), [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md), [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md) et [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  

> [!NOTE]
>  Pour créer un numéro à incrémentation automatique qui peut être utilisé dans plusieurs tables ou être appelé par des applications sans faire référence à une table, consultez [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md).
 
## Exemples de conditions de test<a name="etc"></a>  
Les exemples de cette rubrique sont fondés sur la table, le fichier de données et le fichier de format définis ci-dessous.

### **Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données de test et une table nommée `myIdentity`.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myIdentity ( 
   PersonID smallint IDENTITY(1,1) NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date
   );
```
 
### **Exemple de fichier de données**<a name="sample_data_file"></a>
À l’aide du Bloc-notes, créez un fichier vide `D:\BCP\myIdentity.bcp` et insérez les données ci-dessous.  
```
3,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
1,Stella,Rosenhain,1992-03-02
4,Miller,Dylan,1954-01-05
```
  
Vous pouvez aussi exécuter le script PowerShell suivant pour créer et remplir le fichier de données :
```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'myIdentity.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '3,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '1,Stella,Rosenhain,1992-03-02';
Add-Content -Path $bcpFile -Value '4,Miller,Dylan,1954-01-05';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```

### **Exemple de fichier de format non XML**<a name="nonxml_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myIdentity.fmt`basé sur le schéma de `myIdentity`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  Pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de caractère, **t** est utilisé pour spécifier une virgule comme [délimiteur de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :
  
```
bcp TestDatabase.dbo.myIdentity format nul -c -f D:\BCP\myIdentity.fmt -t, -T

REM Review file
Notepad D:\BCP\myIdentity.fmt
```
 
> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## Exemples<a name="examples"></a>
Les exemples ci-dessous utilisent la base de données, le fichier de données et les fichiers de format créés ci-dessus.
  
### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et conservation des valeurs d’identité sans fichier de format**<a name="bcp_identity"></a>
Commutateur **-E** .  À partir d'une invite de commandes, entrez la commande suivante :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t, -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```

### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et conservation des valeurs d’identité avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bcp_identity_fmt"></a>
Commutateurs **-E** et **-f** .  À partir d'une invite de commandes, entrez la commande suivante :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
 
### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et de valeurs d’identité générées sans fichier de format**<a name="bcp_default"></a>
Utilisation des valeurs par défaut.  À partir d'une invite de commandes, entrez la commande suivante :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t,

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### **Utilisation de la commande [bcp](../../tools/bcp-utility.md) et de valeurs d’identité générées avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bcp_default_fmt"></a>
Utilisation des valeurs par défaut et du commutateur **-f** .  À partir d'une invite de commandes, entrez la commande suivante :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et conservation des valeurs d’identité sans fichier de format**<a name="bulk_identity"></a>
Argument**KEEPIDENTITY** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
    FROM 'D:\BCP\myIdentity.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et conservation des valeurs d’identité avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bulk_identity_fmt"></a>
Arguments**KEEPIDENTITY** et **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt',
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et de valeurs d’identité générées sans fichier de format**<a name="bulk_default"></a>
Utilisation des valeurs par défaut.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### **Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et de valeurs d’identité générées avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="bulk_default_fmt"></a>
Utilisation des valeurs par défaut et de l’argument **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### **Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et conservation des valeurs d’identité avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="openrowset_identity_fmt"></a>
Indicateur de table**KEEPIDENTITY** et argument **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
WITH (KEEPIDENTITY) 
(PersonID, FirstName, LastName, BirthDate)
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
 
### **Utilisation d’ [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et de valeurs d’identité générées avec un [fichier de format non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)**<a name="openrowset_default_fmt"></a>
Utilisation des valeurs par défaut et de l’argument **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
(FirstName, LastName, BirthDate)
    SELECT FirstName, LastName, BirthDate
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
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
  
1.  [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
2.  [Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
3.  [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
[Fichiers de format pour l'importation ou l'exportation de données (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
