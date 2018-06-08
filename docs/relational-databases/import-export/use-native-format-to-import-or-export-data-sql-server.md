---
title: Utiliser le format natif pour importer ou exporter des données (SQL Server) | Microsoft Docs
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
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 90f15cbee03a155745366ed1a29d101752406ae3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>Utiliser le format natif pour importer ou exporter des données (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Il est recommandé d'utiliser le format natif lorsque vous transférez des données en bloc entre plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen d'un fichier de données qui ne contient pas de caractères appartenant à un jeu de caractères étendus/codés sur deux octets (DBCS).  

> [!NOTE]
>  Pour transférer des données en bloc entre plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen d'un fichier de données qui contient des caractères appartenant à un jeu de caractères étendus/codés sur deux octets, utilisez de préférence le format natif Unicode. Pour plus d’informations, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).

Le format natif préserve les types de données native d'une base de données. Il est destiné aux transferts de données à haute vitesse entre des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous utilisez un fichier de format, les tables source et cible ne doivent pas nécessairement être identiques. Le transfert de données se déroule en deux étapes :  
  
1.  exportation en bloc des données d'une table source vers un fichier de données ;  
  
2.  importation en bloc des données d'un fichier de données vers la table cible.  
  
L'utilisation d'un format natif entre tables identiques permet de gagner du temps et de l'espace puisque les conversions superflues des types de données à partir de et vers le format caractère sont éliminées. Pour atteindre la vitesse de transfert optimale, quelques contrôles sont effectués au niveau du formatage des données. Pour empêcher que des problèmes ne surviennent au niveau des données chargées, tenez compte des restrictions suivantes.  

|Dans cette rubrique :|
|---|
|[Restrictions](#restrictions)|
|[Traitement des données au format natif par l'utilitaire bcp](#considerations)|
|[Options de commande pour le format natif](#command_options)|
|[Exemples de conditions de test](#etc)<br /><br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de format non XML](#nonxml_format_file)|
|[Exemples](#examples)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format natif pour exporter des données](#bcp_native_export)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format natif pour importer des données sans un fichier de format](#bcp_native_import)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format natif pour importer des données avec un fichier de format non XML](#bcp_native_import_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format natif sans un fichier de format](#bulk_native)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format natif avec un fichier de format non XML](#bulk_native_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET et du format natif avec un fichier de format non XML](#openrowset_native_fmt)|
|[Tâches associées](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|

## Restrictions<a name="restrictions"></a>  
Pour importer des données au format natif, veillez à ce que les conditions suivantes soient réunies :  
  
-   Le fichier de données doit être au format natif.  
  
-   Soit la table cible doit être compatible avec le fichier de données (qui doit présenter le nombre voulu de colonnes, avec les mêmes types de données, longueur de colonne, état NULL, etc.), soit vous devez utiliser un fichier de format pour mapper chacun des champs sur sa colonne correspondante.  
  
    > [!NOTE]
    >  Si vous importez des données d'un fichier qui ne concorde pas avec la table cible, l'importation peut réussir mais les valeurs de données insérées dans la table cible peuvent être incorrectes. En effet, les données du fichier sont interprétées en se basant sur le format de la table cible. Par conséquent, tout défaut de correspondance entraîne l'insertion de valeurs incorrectes. Cependant, en aucun cas une telle non-correspondance peut-elle provoquer des incohérences logiques ou physiques dans la base de données.  
  
     Pour plus d’informations sur l’utilisation de fichiers de format, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Une importation réussie n'endommage pas la table cible.  
  
## Traitement des données au format natif par l'utilitaire bcp<a name="considerations"></a>
 Cette section comporte des remarques spécifiques liées à la manière dont l'utilitaire **bcp** exporte et importe les données au format natif.  
  
-   Données non-caractère  
  
     [L’utilitaire bcp](../../tools/bcp-utility.md) emploie le format de données binaires interne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour écrire des données qui ne sont pas de type caractère d’une table vers un fichier de données.  
  
-   Données[char](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)   
  
     Au début de chaque champ [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) , [bcp](../../tools/bcp-utility.md) ajoute la longueur du préfixe.  
  
    > [!IMPORTANT]
    >  En mode natif, [l’utilitaire bcp](../../tools/bcp-utility.md) convertit par défaut les caractères de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en caractères OEM avant de les copier dans un fichier de données. [L’utilitaire bcp](../../tools/bcp-utility.md) convertit les caractères d’un fichier de données en caractères ANSI avant de les importer en bloc dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Au cours de ces conversions, il est possible que des données caractères étendus soient perdues. Dans le cas de caractères étendus, vous devez soit utiliser le format natif Unicode, soit spécifier une page de codes.
  
-   Données[sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)   
  
     Si des données [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) sont stockées en tant que SQLVARIANT dans un fichier de données au format natif, elles conservent l’ensemble de leurs caractéristiques. Les métadonnées qui enregistrent le type de données de chaque valeur de données sont stockées en même temps que celle-ci. Les métadonnées sont employées pour recréer la valeur de données avec le même type de données dans une colonne [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) de destination.  
  
     Si le type de données de la colonne de destination n’est pas [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), chaque valeur de données est convertie dans le type de données de la colonne de destination, suivant les règles normales de la conversion implicite de données. Si une erreur survient pendant la conversion des données, le traitement actif est annulé. Toute valeur [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) transférée entre des colonnes [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) peut faire l’objet de problèmes de conversion des pages de codes.  
  
     Pour plus d’informations sur la conversion de données, consultez [Conversion de types de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## Options de commande pour le format natif<a name="command_options"></a>  
Vous pouvez importer des données au format natif dans une table à l’aide de l’utilitaire [bcp](../../tools/bcp-utility.md) ou des instructions [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Si vous utilisez une commande [bcp](../../tools/bcp-utility.md) ou une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), vous pouvez spécifier le format de données dans l’instruction.  Pour une instruction [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), vous devez spécifier le format de données dans un fichier de format.  

Le format natif est pris en charge par les options de commande suivantes :  

|Command|Option|Description|  
|-------------|------------|-----------------|  
|bcp|**-n**|Force l’utilitaire bcp à utiliser les types de données natifs des données.*|  
|BULK INSERT|DATAFILETYPE **='native'**|Utilise les types de données native ou widenative des données. Notez que DATAFILETYPE n'est pas nécessaire si un fichier de format spécifie les types de données.|  
|OPENROWSET|Néant|Doit utiliser un fichier de format.|

  
 \*Pour charger des données au format natif (**-n**) dans un format compatible avec des versions antérieures de clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez le commutateur **-V** . Pour plus d’informations, consultez [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
> [!NOTE]
>  Vous pouvez également spécifier le formatage par champ dans un fichier de format. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  

## Exemples de conditions de test<a name="etc"></a>  
Les exemples de cette rubrique sont fondés sur la table et le fichier de format définis ci-dessous.

### **Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données test, une table nommée `myNative` et remplit la table avec des valeurs initiales.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### **Exemple de fichier de format non XML**<a name="nonxml_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myNative.fmt`basé sur le schéma de `myNative`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  De plus, pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de type caractère et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T -n 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## Exemples<a name="examples"></a>
Les exemples ci-dessous utilisent la base de données et les fichiers de format créés ci-dessus.

### **Utilisation de bcp et du format natif pour exporter des données**<a name="bcp_native_export"></a>
Commutateur **-n** et commande **OUT** .  Remarque : Le fichier de données créé dans cet exemple est utilisé dans tous les exemples suivants.  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### **Utilisation de bcp et du format natif pour importer des données sans un fichier de format**<a name="bcp_native_import"></a>
Commutateur **-n** et commande **IN** .  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### **Utilisation de bcp et du format natif pour importer des données avec un fichier de format non XML**<a name="bcp_native_import_fmt"></a>
Commutateurs **-n** et **-f** switches et **IN** commet.  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### **Utilisation de BULK INSERT et du format natif sans un fichier de format**<a name="bulk_native"></a>
Argument**DATAFILETYPE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### **Utilisation de BULK INSERT et du format natif avec un fichier de format non XML**<a name="bulk_native_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### **Utilisation d’OPENROWSET et du format natif avec un fichier de format non XML**<a name="openrowset_native_fmt"></a>
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## Tâches associées<a name="RelatedTasks"></a>
Pour utiliser des formats de données pour l'importation ou l'exportation en bloc 
  
-   [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  
