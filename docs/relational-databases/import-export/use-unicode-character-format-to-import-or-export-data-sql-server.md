---
title: Utiliser le format caractère Unicode pour importer ou exporter des données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], Unicode character
- Unicode [SQL Server], bulk importing and exporting
ms.assetid: 74342a11-c1c0-4746-b482-7f3537744a70
caps.latest.revision: 37
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ecc827d1b784ad81907443c42f80d69b6f7f05f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-unicode-character-format-to-import-or-export-data-sql-server"></a>Utiliser le format caractère Unicode pour importer ou exporter des données (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Le format caractère Unicode est recommandé pour le transfert en bloc de données entre plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide d'un fichier de données qui contient des caractères étendus ou DBCS. Le format de données caractère Unicode permet d'exporter des données depuis un serveur à l'aide d'une page de codes différente de celle utilisée par le client qui effectue l'opération. Dans ces cas, l'utilisation du format caractère Unicode présente les avantages suivants :  
  
* Si les données sources et de destination sont de types de données Unicode, l'utilisation du format caractère Unicode conserve toutes les données caractère.  
  
* Si les données sources et de destination ne sont pas de types de données Unicode, l’utilisation du format caractère Unicode réduit au minimum la perte de caractères étendus dans les données sources qui ne peuvent pas être représentées sur la destination.

|Dans cette rubrique :|
|---|
|[Considérations relatives à l’utilisation du format caractère Unicode](#considerations)|
|[Considérations spéciales relatives à l’utilisation du format caractère Unicode, de bcp et d’un fichier de format](#special_considerations)|
|[Options de commande du format caractère Unicode](#command_options)|
|[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de format non XML](#nonxml_format_file)|
|[Exemples](#examples)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format caractère Unicode pour exporter des données](#bcp_widechar_export)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format caractère Unicode pour importer des données sans un fichier de format](#bcp_widechar_import)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format caractère Unicode pour importer des données avec un fichier de format non XML](#bcp_widechar_import_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format caractère Unicode sans un fichier de format](#bulk_widechar)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format caractère Unicode avec un fichier de format non XML](#bulk_widechar_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET et du format caractère Unicode avec un fichier de format non XML](#openrowset_widechar_fmt)|
|[Tâches associées](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
 
## Considérations relatives à l’utilisation du format caractère Unicode<a name="considerations"></a>
Lors de l’utilisation du format caractère Unicode, tenez compte des points suivants :  

* Par défaut, [l’utilitaire bcp](../../tools/bcp-utility.md) sépare les champs de données de caractères par le caractère de tabulation et termine les enregistrements par un caractère de nouvelle ligne.  Pour plus d’informations sur la spécification des terminateurs de remplacement, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

* Les données [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) stockées dans un fichier de données au format caractère Unicode fonctionnent comme dans un fichier de données au format caractère, à la différence que les données sont stockées en tant que données [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) et non pas [char](../../t-sql/data-types/char-and-varchar-transact-sql.md). Pour plus d’informations sur le format caractère, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  

## Considérations spéciales relatives à l’utilisation du format caractère Unicode, de bcp et d’un fichier de format<a name="special_considerations"></a>
Les fichiers de données de format caractère Unicode respectent les conventions pour les fichiers Unicode.  Les deux premiers octets du fichier sont des nombres hexadécimaux (0xFFFE).  Ces octets sont utilisés comme marques d’ordre d’octet, précisant si l’octet de poids fort est enregistré en premier ou en dernier dans le fichier.  [L’utilitaire bcp](../../tools/bcp-utility.md) peut mal interpréter les marques d’ordre d’octet et provoquer l’échec d’une partie du processus d’importation ; vous pouvez recevoir un message d’erreur similaire à ce qui suit :
```
Starting copy...
SQLState = 22005, NativeError = 0
Error = [Microsoft][ODBC Driver 13 for SQL Server]Invalid character value for cast specification
```

Les marques d’ordre d’octet peuvent être mal interprétées dans les conditions suivantes :
* [L’utilitaire bcp](../../tools/bcp-utility.md) est utilisé et le commutateur **-w** est utilisé pour indiquer le caractère Unicode

* Un fichier de format est utilisé

* Le premier champ dans le fichier de données n’est pas de type caractère

Déterminez si l’une des solutions suivantes peut être disponible pour votre situation *spécifique* :
* N’utilisez pas de fichier de format.  Un exemple de cette solution de contournement est fourni ci-dessous ; consultez [Utilisation de bcp et du format caractère Unicode pour importer des données sans un fichier de format](#bcp_widechar_import).

* Utilisez le commutateur **-c** à la place de **-w**.

* Réexportez les données à l’aide d’un format natif.

* Utilisez [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  Des exemples de ces solutions de contournement sont fournis ci-dessous ; consultez [Utilisation de BULK INSERT et du format caractère Unicode avec un fichier de format non XML](#bulk_widechar_fmt) et [Utilisation d’OPENROWSET et du format caractère Unicode avec un fichier de format non XML](#openrowset_widechar_fmt).
 
* Insérez manuellement le premier enregistrement dans la table de destination, puis utilisez le commutateur **-F 2** pour que l’importation démarre sur le deuxième enregistrement.

* Insérez manuellement le premier enregistrement factice dans le fichier de données, puis utilisez le commutateur **-F 2** pour que l’importation démarre sur le deuxième enregistrement.  Un exemple de cette solution de contournement est fourni ci-dessous ; consultez [Utilisation de bcp et du format caractère Unicode pour importer des données avec un fichier de format non XML](#bcp_widechar_import_fmt).

* Utilisez une table de préproduction où la première colonne est un type de données caractère.

* Réexportez les données et modifiez l’ordre des champs de données afin que le premier champ de données soit de type caractère.  Utilisez ensuite un fichier de format pour remapper le champ de données à l’ordre réel de la table.  Pour obtenir un exemple, consultez [Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md).
  
## Options de commande du format caractère Unicode<a name="command_options"></a>  
Vous pouvez importer des données au format caractère Unicode dans une table à l’aide de la commande [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Si vous utilisez une commande [bcp](../../tools/bcp-utility.md) ou une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), vous pouvez spécifier le format de données dans l’instruction.  Pour une instruction [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), vous devez spécifier le format de données dans un fichier de format.  
  
Le format caractère Unicode est pris en charge par les options de commande suivantes :  
  
|Command|Option|Description|  
|-------------|------------|-----------------|  
|bcp|**-w**|Utilise le format caractère Unicode.|  
|BULK INSERT|DATAFILETYPE **='widechar'**|Utilise le format caractère Unicode lors de l'importation de données en bloc.|  
|OPENROWSET|Néant|Doit utiliser un fichier de format.|
  
> [!NOTE]
>  Vous pouvez également spécifier le formatage par champ dans un fichier de format. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## Exemples de conditions de test<a name="etc"></a>  
Les exemples de cette rubrique sont fondés sur la table et le fichier de format définis ci-dessous.

### **Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données test, une table nommée `myWidechar` et remplit la table avec des valeurs initiales.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidechar ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidechar
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidechar;
```

### **Exemple de fichier de format non XML**<a name="nonxml_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myWidechar.fmt`basé sur le schéma de `myWidechar`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  De plus, pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de type caractère et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d’une invite de commandes, entrez les commandes suivantes :

```
bcp TestDatabase.dbo.myWidechar format nul -f D:\BCP\myWidechar.fmt -T -w

REM Review file
Notepad D:\BCP\myWidechar.fmt
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`


## Exemples<a name="examples"></a>
Les exemples ci-dessous utilisent la base de données et les fichiers de format créés ci-dessus.

### **Utilisation de bcp et du format caractère Unicode pour exporter des données**<a name="bcp_widechar_export"></a>
Commutateur **-w** et commande **OUT** .  Remarque : Le fichier de données créé dans cet exemple est utilisé dans tous les exemples suivants.  À partir d’une invite de commandes, entrez les commandes suivantes :
```
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w

REM Review results
NOTEPAD D:\BCP\myWidechar.bcp
```

### **Utilisation de bcp et du format caractère Unicode pour importer des données sans un fichier de format**<a name="bcp_widechar_import"></a>
Commutateur **-w** et commande **IN** .  À partir d’une invite de commandes, entrez les commandes suivantes :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Import data
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -T -w

REM Review results is SSMS
```

### **Utilisation de bcp et du format caractère Unicode pour importer des données avec un fichier de format non XML**<a name="bcp_widechar_import_fmt"></a>
Commutateurs **-w** et **-f** switches et **IN** commet.  Vous devez utiliser une solution de contournement dans la mesure où cet exemple implique bcp, un fichier de format, le caractère Unicode, et le premier champ de données dans le fichier de données n’est pas de type caractère.  Consultez [Considérations spéciales relatives à l’utilisation du format caractère Unicode, de bcp et d’un fichier de format](#special_considerations)ci-dessus.  Le fichier de données `myWidechar.bcp` est modifié par l’ajout d’un enregistrement supplémentaire « factice » qui est ensuite ignoré avec le commutateur `-F 2` .

À l’invite de commandes, entrez les commandes suivantes et suivez la procédure de modification :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Open data file
Notepad D:\BCP\myWidechar.bcp
REM Copy first record and then paste as new first record.  This additional record is the "dummy" record.
REM Close file.

REM Import data instructing bcp to skip dummy record with the -F 2 switch.
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -f D:\BCP\myWidechar.fmt -T -F 2

REM Review results is SSMS

REM Return data file to original state for usage in other examples
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w
```
  
### **Utilisation de BULK INSERT et du format caractère Unicode sans un fichier de format**<a name="bulk_widechar"></a>
Argument**DATAFILETYPE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
    FROM 'D:\BCP\myWidechar.bcp'
    WITH (
        DATAFILETYPE = 'widechar'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### **Utilisation de BULK INSERT et du format caractère Unicode avec un fichier de format non XML**<a name="bulk_widechar_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
   FROM 'D:\BCP\myWidechar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidechar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### **Utilisation d’OPENROWSET et du format caractère Unicode avec un fichier de format non XML**<a name="openrowset_widechar_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar;  -- for testing
INSERT INTO TestDatabase.dbo.myWidechar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidechar.bcp', 
        FORMATFILE = 'D:\BCP\myWidechar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
 
  
## Tâches associées<a name="RelatedTasks"></a>
Pour utiliser des formats de données pour l'importation ou l'exportation en bloc  
-   [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  
