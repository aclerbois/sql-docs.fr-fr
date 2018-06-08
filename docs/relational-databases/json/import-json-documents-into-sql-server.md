---
title: Importer des documents JSON dans SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: json
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- dbe-json
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 0e908ec0-7173-4cd2-8f48-2700757b53a5
caps.latest.revision: 5
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 726b1d8bf70204912f9ebbf4c2352dcc0dc55fc7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-json-documents-into-sql-server"></a>Importer des documents JSON dans SQL Server
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Cette rubrique décrit comment importer des fichiers JSON dans SQL Server. Il existe un grand nombre de documents JSON stockés dans des fichiers. Les applications journalisent des informations dans les fichiers JSON, les capteurs génèrent des informations stockées dans les fichiers JSON, etc. Il est important de pouvoir lire les données JSON stockées dans des fichiers, charger les données dans SQL Server et les analyser.

## <a name="import-a-json-document-into-a-single-column"></a>Importer un document JSON dans une seule colonne
**OPENROWSET(BULK)** est une fonction table qui peut lire des données à partir de n’importe quel fichier sur le réseau ou le lecteur local, si SQL Server dispose d’un accès en lecture à cet emplacement. Elle retourne une table avec une colonne unique où figure le contenu du fichier. Vous pouvez utiliser différentes options avec la fonction OPENROWSET(BULK), comme des séparateurs. Mais dans le cas le plus simple, vous pouvez charger tout le contenu d’un fichier en tant que valeur de texte, (Cette valeur unique élevée est appelée objet volumineux à caractère unique, ou SINGLE_CLOB.) 

Voici un exemple de fonction **OPENROWSET(BULK)** qui lit le contenu d’un fichier JSON et le retourne à l’utilisateur en tant que valeur unique :

```sql
SELECT BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j
```

OPENJSON(BULK) lit le contenu du fichier et le retourne dans `BulkColumn`.

Vous pouvez également charger le contenu du fichier dans une variable locale ou dans une table, comme indiqué dans l’exemple suivant :

```sql
-- Load file contents into a variable
SELECT @json = BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j

-- Load file contents into a table 
SELECT BulkColumn
 INTO #temp 
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j
```

Après avoir chargé le contenu du fichier JSON, vous pouvez enregistrer le texte JSON dans une table.

## <a name="import-multiple-json-documents"></a>Importer plusieurs documents JSON
Vous pouvez adopter la même approche pour charger un ensemble de fichiers JSON depuis le système de fichiers vers une variable locale. On part du principe que les fichiers se nomment `book<index>.json`.
  
```sql
DECLARE @i INT = 1
DECLARE @json AS NVARCHAR(MAX)

WHILE(@i < 10)
BEGIN
    SET @file = 'C:\JSON\Books\book' + cast(@i AS VARCHAR(5)) + '.json';
    SELECT @json = BulkColumn FROM OPENROWSET (BULK (@file), SINGLE_CLOB) AS j
    SELECT * FROM OPENJSON(@json) AS json
    -- Optionally, save the JSON text in a table.
    SET @i = @i + 1 ;
END
```

## <a name="import-json-documents-from-azure-file-storage"></a>Importer des documents JSON à partir du Stockage de fichiers Azure
Vous pouvez également utiliser OPENROWSET(BULK) comme indiqué ci-dessus pour lire les fichiers JSON à partir d’autres emplacements accessibles à SQL Server. Par exemple, le Stockage de fichiers Azure prend en charge le protocole SMB. Ainsi, vous pouvez mapper un lecteur virtuel local au partage de Stockage de fichiers Azure à l’aide de la procédure suivante :
1.  Créez un compte de stockage de fichier (par exemple, `mystorage`), un partage de fichiers (par exemple, `sharejson`) et un dossier dans le Stockage de fichiers Azure à l’aide du portail Azure ou d’Azure PowerShell.
2.  Chargez des fichiers JSON vers le partage de stockage de fichiers.
3.  Créez sur votre ordinateur une règle de pare-feu sortante dans le Pare-feu Windows qui autorise l’utilisation du port 445. Notez qu’il est possible que votre fournisseur de services Internet bloque ce port. Si vous obtenez une erreur DNS (erreur 53) à l’étape suivante, cela signifie que vous n’avez pas ouvert le port 445 ou que votre fournisseur de services Internet le bloque.
4. Montez le partage Stockage Fichier Azure en tant que lecteur local (par exemple `T:`).

    Voici la syntaxe de commande :

    ```dos
    net use [drive letter] \\[storage name].file.core.windows.net\[share name] /u:[storage account name] [storage account access key]
    ```

    Voici un exemple qui affecte la lettre de lecteur local `T:` au partage Stockage Fichier Azure :

    ```dos
    net use t: \\mystorage.file.core.windows.net\sharejson /u:myaccount hb5qy6eXLqIdBj0LvGMHdrTiygkjhHDvWjUZg3Gu7bubKLg==
    ```

    Vous trouverez la clé de compte de stockage et la clé d’accès au compte de stockage primaire ou secondaire dans la section Clés des Paramètres dans le portail Azure.

5.  Vous pouvez maintenant accéder à vos fichiers JSON à partir du partage Stockage Fichier Azure à l’aide du lecteur mappé, comme indiqué dans l’exemple suivant :

    ```sql
    SELECT book.* FROM
     OPENROWSET(BULK N't:\books\books.json', SINGLE_CLOB) AS json
     CROSS APPLY OPENJSON(BulkColumn)
     WITH( id nvarchar(100), name nvarchar(100), price float,
        pages_i int, author nvarchar(100)) AS book
    ```

Pour plus d’informations sur le Stockage de fichiers Azure, consultez [Stockage de fichiers](https://azure.microsoft.com/services/storage/files/).

## <a name="import-json-documents-from-azure-blob-storage"></a>Importer des documents JSON à partir de Stockage Blob Azure

Vous pouvez charger des fichiers directement dans Azure SQL Database à partir de Stockage Blob Azure avec la commande T-SQL BULK INSERT ou la fonction OPENROWSET.

Commencez par créer une source de données externe, comme illustré dans l’exemple suivant.

```sql
CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
 WITH ( TYPE = BLOB_STORAGE,
        LOCATION = 'https://myazureblobstorage.blob.core.windows.net',
        CREDENTIAL= MyAzureBlobStorageCredential);
```

Ensuite, exécutez une commande BULK INSERT avec l’option DATA_SOURCE.

```sql
BULK INSERT Product
FROM 'data/product.dat'
WITH ( DATA_SOURCE = 'MyAzureBlobStorage');
```

Pour plus d’informations et pour obtenir un exemple d’utilisation d’OPENROWSET, consultez [Chargement de fichiers de Stockage Blob Azure vers Azure SQL Database](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/02/23/loading-files-from-azure-blob-storage-into-azure-sql-database/).

## <a name="parse-json-documents-into-rows-and-columns"></a>Analyser des documents JSON dans des lignes et des colonnes
Au lieu de lire un fichier JSON entier en tant que valeur unique, vous pouvez l’analyser et retourner les livres figurant dans le fichier, ainsi que leurs propriétés, dans des lignes et des colonnes. L’exemple suivant utilise un fichier JSON provenant de [ce site](https://github.com/tamingtext/book/blob/master/apache-solr/example/exampledocs/books.json) et contenant une liste de livres.

### <a name="example-1"></a>Exemple 1
Dans l’exemple le plus simple, vous pouvez charger la liste entière à partir du fichier. 

```sql
SELECT value
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
```

### <a name="example-2"></a>Exemple 2
OPENROWSET lit une valeur de texte unique à partir du fichier, la retourne sous la forme d’un BulkColumn, et la transmet à la fonction OPENJSON. OPENJSON itère au sein du tableau d’objets JSON dans le tableau BulkColumn et retourne un livre sur chaque ligne au format JSON :

```json
{"id":"978-0641723445", "cat":["book","hardcover"], "name":"The Lightning Thief", … }
{"id":"978-1423103349", "cat":["book","paperback"], "name":"The Sea of Monsters", … }
{"id":"978-1857995879", "cat":["book","paperback"], "name":"Sophie’s World : The Greek", … } 
{"id":"978-1933988177", "cat":["book","paperback"], "name":"Lucene in Action, Second", … }
```

### <a name="example-3"></a>Exemple 3
La fonction OPENJSON peut analyser le contenu JSON et le transformer en tableau ou en jeu de résultats. L’exemple suivant charge le contenu, analyse le JSON chargé et retourne les cinq champs sous forme de colonnes :

```sql
SELECT book.*
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
 WITH( id nvarchar(100), name nvarchar(100), price float,
 pages_i int, author nvarchar(100)) AS book
```

Dans cet exemple, OPENROWSET(BULK) lit le contenu du fichier et transmet ce contenu à la fonction OPENJSON avec un schéma défini pour la sortie. OPENJSON établit une correspondance avec les propriétés dans les objets JSON en utilisant des noms de colonnes. Par exemple, la propriété `price` est retournée en tant que colonne `price` et convertie vers le type de données float. Voici les résultats :

|Id|Nom   |price|pages_i|Author
|---|---|---|---|---|
978-0641723445|The Lightning Thief|12.5|384|Rick Riordan| 
978-1423103349|The Sea of Monsters|6.49|304|Rick Riordan| 
978-1857995879|Sophie’s World : The Greek Philosophers|3.07|64|Jostein Gaarder| 
978-1933988177|Lucene in Action, Second Edition|30.5|475|Michael McCandless|
||||||

Vous pouvez maintenant retourner cette à l’utilisateur, ou charger les données dans une autre table.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>En savoir plus sur JSON dans SQL Server et Azure SQL Database  
  
### <a name="microsoft-blog-posts"></a>Billets de blog Microsoft  
  
Pour accéder à un grand nombre de solutions spécifiques, de cas d’usage et de recommandations, consultez ces [billets de blog](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/) sur la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database.  

### <a name="microsoft-videos"></a>Vidéos Microsoft

Pour obtenir une présentation visuelle de la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database, consultez les vidéos suivantes :

-   [SQL Server 2016 et prise en charge de JSON](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Utilisation de JSON dans SQL Server 2016 et Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON : un pont entre les mondes relationnel et NoSQL](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a> Voir aussi
[Conversion de données JSON en lignes et colonnes à l’aide d’OPENJSON](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)

