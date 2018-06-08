---
title: Utiliser le format caractère pour importer ou exporter des données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], character
- character formats [SQL Server]
ms.assetid: d925e66a-1a73-43cd-bc06-1cbdf8174a4d
caps.latest.revision: 42
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 994dbd67b155d791cc7d4ee25dc8525c471a5f11
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-character-format-to-import-or-export-data-sql-server"></a>Utiliser le format caractère pour importer ou exporter des données (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Le format caractère est recommandé pour l'exportation en bloc de données dans un fichier texte qui doit être utilisé dans un autre programme, ou pour l'importation en bloc de données à partir d'un fichier texte généré par un autre programme.  

Le format caractère utilise le format de données de caractères pour toutes les colonnes. L'enregistrement d'informations au format caractère est utile lorsque les données sont exploitées par un autre programme, par exemple un tableur, ou lorsque les données doivent être copiées dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'une base de données issue d'un autre éditeur comme Oracle.  
  
> [!NOTE]
>  Lors du transfert en bloc de données entre des instances de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si le fichier de données contient des données caractères Unicode mais aucun caractère étendu ni jeu de caractères codés sur deux octets (DBCS), utilisez le format de caractères Unicode. Pour plus d’informations, consultez [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).
  
|Dans cette rubrique :|
|---|
|[Considérations relatives à l'utilisation du format caractère](#considerations)|
|[Options de commande pour le format caractère](#command_options)|
|[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de format non XML](#nonxml_format_file)<br />|
|[Exemples](#examples)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format caractère pour exporter des données](#bcp_char_export)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format caractère pour importer des données sans un fichier de format](#bcp_char_import)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format caractère pour importer des données avec un fichier de format non XML](#bcp_char_import_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format caractère sans un fichier de format](#bulk_char)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format caractère avec un fichier de format non XML](#bulk_char_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET et du format caractère avec un fichier de format non XML](#openrowset_char_fmt)|
|[Tâches associées](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|


  
## Considérations relatives à l'utilisation du format caractère<a name="considerations"></a>
Lors de l'utilisation du format caractère, tenez compte des points suivants :  
  
-   Par défaut, [l’utilitaire bcp](../../tools/bcp-utility.md) sépare les champs de données de caractères par le caractère de tabulation et termine les enregistrements par un caractère de nouvelle ligne.  Pour plus d’informations sur la spécification des terminateurs de remplacement, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
-   Par défaut, avant l'exportation ou l'importation en bloc de données en mode caractère, les conversions suivantes sont réalisées :  
  
    |Sens de l'opération en bloc|Conversion|  
    |---------------------------------|----------------|  
    |Exporter|Convertit les données en représentation caractère. Si la demande est faite explicitement, les données sont converties vers la page de codes demandée pour les colonnes de caractères. Si aucune page de codes n'est spécifiée, les données de caractères sont converties à l'aide de la page de codes OEM de l'ordinateur client.|  
    |Importer|Convertit les données de caractères en représentation native si nécessaire, puis traduit les données de caractères de la page de codes du client vers la page de codes des colonnes cibles.|  
  
-   Pour éviter la perte de caractères étendus pendant la conversion, utilisez le format de caractères Unicode ou spécifiez une page de codes.  
  
-   Les données [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) stockées dans un fichier au format caractère sont enregistrées sans métadonnées. Chaque valeur de données est convertie au format [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) suivant les règles de conversion implicite des données. Importées dans une colonne [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) , les données sont importées au format [char](../../t-sql/data-types/char-and-varchar-transact-sql.md). Importées dans une colonne dont le type de données est différent de [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), les données sont converties à partir du format [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) à l’aide de la conversion implicite. Pour plus d’informations sur la conversion de données, consultez [Conversion de types de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
-   [L’utilitaire bcp](../../tools/bcp-utility.md) exporte les valeurs de type [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) sous forme de fichiers de données au format caractère, avec quatre chiffres après le séparateur décimal et sans symboles de groupement de chiffres comme les espaces de séparation. Par exemple, une colonne [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) contenant la valeur 1 234 567,123456 est exportée en bloc dans un fichier de données en tant que chaîne de caractères 1234567,1235.  
  
## Options de commande pour le format caractère<a name="command_options"></a>  
Vous pouvez importer des données au format caractère dans une table à l’aide de l’utilitaire [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Si vous utilisez une commande [bcp](../../tools/bcp-utility.md) ou une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), vous pouvez spécifier le format de données dans l’instruction.  Pour une instruction [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), vous devez spécifier le format de données dans un fichier de format.  
  
Le format caractère est pris en charge par les options de commande suivantes :  
  
|Command|Option|Description|  
|-------------|------------|-----------------|  
|bcp|**-c**|Force l’utilitaire bcp à utiliser les données de type caractère.*|  
|BULK INSERT|DATAFILETYPE **='char'**|Utilise le format caractère lors de l'importation en bloc des données.|  
|OPENROWSET|Néant|Doit utiliser un fichier de format.|
  
 \*Pour charger les données caractères (**-c**) dans un format compatible avec les versions antérieures des clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez le commutateur **-V** . Pour plus d’informations, consultez [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
   
> [!NOTE]
>  Vous pouvez également spécifier le formatage par champ dans un fichier de format. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).

## Exemples de conditions de test<a name="etc"></a>  
Les exemples de cette rubrique sont fondés sur la table et le fichier de format définis ci-dessous.

### **Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données test, une table nommée `myChar` et remplit la table avec des valeurs initiales.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myChar ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myChar
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myChar;
```

### **Exemple de fichier de format non XML**<a name="nonxml_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myChar.fmt`basé sur le schéma de `myChar`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  De plus, pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de type caractère et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp TestDatabase.dbo.myChar format nul -f D:\BCP\myChar.fmt -T -c 

REM Review file
Notepad D:\BCP\myChar.fmt
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## Exemples<a name="examples"></a>
Les exemples ci-dessous utilisent la base de données et les fichiers de format créés ci-dessus.

### **Utilisation de bcp et du format caractère pour exporter des données**<a name="bcp_char_export"></a>
Commutateur **-c** et commande **OUT** .  Remarque : Le fichier de données créé dans cet exemple est utilisé dans tous les exemples suivants.  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp TestDatabase.dbo.myChar OUT D:\BCP\myChar.bcp -T -c

REM Review results
NOTEPAD D:\BCP\myChar.bcp
```

### **Utilisation de bcp et du format caractère pour importer des données sans un fichier de format**<a name="bcp_char_import"></a>
Commutateur **-c** et commande **IN** .  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -T -c

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### **Utilisation de bcp et du format caractère pour importer des données avec un fichier de format non XML**<a name="bcp_char_import_fmt"></a>
Commutateurs **-c** et **-f** switches et **IN** commet.  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -f D:\BCP\myChar.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### **Utilisation de BULK INSERT et du format caractère sans un fichier de format**<a name="bulk_char"></a>
Argument**DATAFILETYPE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
    FROM 'D:\BCP\myChar.bcp'
    WITH (
        DATAFILETYPE = 'Char'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### **Utilisation de BULK INSERT et du format caractère avec un fichier de format non XML**<a name="bulk_char_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
   FROM 'D:\BCP\myChar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myChar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### **Utilisation d’OPENROWSET et du format caractère avec un fichier de format non XML**<a name="openrowset_char_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar;  -- for testing
INSERT INTO TestDatabase.dbo.myChar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myChar.bcp', 
        FORMATFILE = 'D:\BCP\myChar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```
  
## Tâches associées<a name="RelatedTasks"></a>  
Pour utiliser des formats de données pour l'importation ou l'exportation en bloc 
  
-   [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utiliser le format natif pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
  
