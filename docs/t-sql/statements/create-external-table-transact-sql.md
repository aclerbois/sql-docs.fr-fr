---
title: CREATE EXTERNAL TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 5/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE_EXTERNAL_TABLE
- CREATE EXTERNAL TABLE
- PolyBase, T-SQL
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, table create
- PolyBase, external table
ms.assetid: 6a6fd8fe-73f5-4639-9908-2279031abdec
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0ea81621b94490c267b6d7c9f3e010bd22279610
ms.sourcegitcommit: 0cc2cb281e467a13a76174e0d9afbdcf4ccddc29
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2018
---
# <a name="create-external-table-transact-sql"></a>CREATE EXTERNAL TABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Crée une table externe pour PolyBase ou des requêtes de base de données élastique. Selon le scénario, la syntaxe diffère considérablement. Une table externe créée pour PolyBase ne peut pas être utilisée pour les requêtes de base de données élastique.  De même, une table externe créée pour les requêtes de base de données élastique ne peut pas être utilisée pour PolyBase, etc. 
  
> [!NOTE]  
>  PolyBase est pris en charge uniquement sur SQL Server 2016 (ou ultérieur), Azure SQL Data Warehouse et Parallel Data Warehouse. Les requêtes de base de données élastique sont prises en charge uniquement sur Azure SQL Database version 12 ou ultérieure.  


- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les tables externes pour accéder aux données stockées dans un cluster Hadoop ou dans le stockage Blob Azure. Peut également être utilisé afin de créer une table externe pour une [requête de base de données élastique](https://azure.microsoft.com/documentation/articles/sql-database-elastic-query-overview/).  
  
 Utilisez une table externe pour :  
  
-   Interroger des données Hadoop ou Azure Blob Storage avec des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
-   Importer des données Hadoop ou Azure Blob Storage, et les stocker dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Créer une table externe à utiliser avec une requête de  
     base de données élastique  
     
- Importer des données Azure Data Lake Store et les stocker dans Azure SQL Data Warehouse
  
 Voir aussi [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md) et [DROP EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-table-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  

```  
-- Syntax for SQL Server 
  
-- Create a new external table  
CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name   
    ( <column_definition> [ ,...n ] )  
    WITH (   
        LOCATION = 'folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage  
    | REJECT_VALUE = reject_value  
    | REJECT_SAMPLE_VALUE = reject_sample_value  
  
}  
  
-- Create a table for use with Elastic Database query  
CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name   
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,   
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```  

```  
-- Syntax for Azure SQL Database
  
-- Create a table for use with Elastic Database query  
CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name   
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,   
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```  


```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
-- Create a new external table in SQL Server PDW  
CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name   
    ( <column_definition> [ ,...n ] )  
    WITH (   
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
    | REJECTED_ROW_LOCATION = '\REJECT_Directory'
  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* . [ schema_name ] . | schema_name. ] *table_name*  
 Nom (composé d’une à trois parties) de la table à créer. Pour une table externe, seules les métadonnées de la table sont stockées dans SQL, avec des statistiques succinctes sur le fichier et ou le dossier référencé dans Hadoop ou Azure Blob Storage. Aucune donnée n’est déplacée ni stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 \<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE autorise une ou plusieurs définitions de colonne. CREATE EXTERNAL TABLE et CREATE TABLE utilisent la même syntaxe pour définir une colonne. Il existe toutefois une exception à cela : vous ne pouvez pas utiliser DEFAULT CONSTRAINT sur des tables externes. Pour plus d’informations sur les définitions de colonne et leurs types de données, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) et [CREATE TABLE dans Azure SQL Database](http://msdn.microsoft.com/library/d53c529a-1d5f-417f-9a77-64ccc6eddca1).  
  
 Les définitions de colonne, y compris les types de données et le nombre de colonnes, doivent correspondre aux données des fichiers externes. En cas de non correspondance, les lignes du fichier sont rejetées lors de l’interrogation des données.  
  
 Pour les tables externes qui référencent des fichiers provenant de sources de données externes, les définitions de colonne et de type doivent correspondre exactement au schéma du fichier externe. Lorsque vous définissez des types de données qui référencent des données stockées dans Hadoop/Hive, utilisez les mappages suivants entre les types de données SQL et Hive, et castez le type en un type de données SQL lorsque vous sélectionnez des données. Les types incluent toutes les versions de Hive, sauf indication contraire.

> [!NOTE]  
>  SQL Server ne prend pas en charge la valeur de données _infinie_ Hive lors des conversions. Si vous l’utilisez, PolyBase échoue et émet une erreur de conversion de type de données.


|Type de données SQL|Type de données .NET|Type de données Hive|Type de données Hadoop/Java|Commentaires|  
|-------------------|--------------------|--------------------|----------------------------|--------------|  
|TINYINT|Byte|TINYINT|ByteWritable|Pour les nombres non signés.|  
|SMALLINT|Int16|SMALLINT|ShortWritable||  
|INT|Int32|INT|IntWritable||  
|BIGINT|Int64|BIGINT|LongWritable||  
|bit|Booléen|boolean|BooleanWritable||  
|FLOAT|Double|double|DoubleWritable||  
|REAL|Unique|FLOAT|FloatWritable||  
|money|Décimal|double|DoubleWritable||  
|SMALLMONEY|Décimal|double|DoubleWritable||  
|NCHAR|String<br /><br /> Char[]|chaîne|texte||  
|NVARCHAR|String<br /><br /> Char[]|chaîne|Texte||  
|char|String<br /><br /> Char[]|chaîne|Texte||  
|varchar|String<br /><br /> Char[]|chaîne|Texte||  
|binary|Byte[]|binary|BytesWritable|S’applique à Hive 0.8 et versions ultérieures|  
|varbinary|Byte[]|binary|BytesWritable|S’applique à Hive 0.8 et versions ultérieures|  
|Date|DateTime|TIMESTAMP|TimestampWritable||  
|smalldatetime|DateTime|TIMESTAMP|TimestampWritable||  
|datetime2|DateTime|TIMESTAMP|TimestampWritable||  
|DATETIME|DateTime|TIMESTAMP|TimestampWritable||  
|time|TimeSpan|TIMESTAMP|TimestampWritable||  
|Décimal|Décimal|Décimal|BigDecimalWritable|S’applique à Hive 0.11 et versions ultérieures|  
  
 LOCATION =  '*folder_or_filepath*'  
 Spécifie le dossier, ou le chemin et le nom du fichier, où se trouvent les données Hadoop ou Azure Blob Storage. L’emplacement commence au dossier racine. Le dossier racine est l’emplacement de données qui est spécifié dans la source de données externe.  


Dans SQL Server, l’instruction CREATE EXTERNAL TABLE crée le chemin et le dossier s’ils n’existent pas déjà. Vous pouvez ensuite utiliser INSERT INTO pour exporter les données d’une table SQL Server locale dans la source de données externe. Pour plus d’informations, consultez [Requêtes PolyBase](/sql/relational-databases/polybase/polybase-queries). 

Dans SQL Data Warehouse et Analytics Platform System, l’instruction [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crée le chemin et le dossier s’ils n’existent pas. Dans ces deux produits, CREATE EXTERNAL TABLE ne crée pas le chemin ni le dossier.

  
 Si vous spécifiez LOCATION comme étant un dossier, une requête PolyBase qui sélectionne des données dans la table externe récupère les fichiers dans le dossier et dans tous ses sous-dossiers. Tout comme Hadoop, PolyBase ne retourne pas le contenu des dossiers masqués. Il ne retourne pas non plus les fichiers dont le nom commence par un trait de soulignement (_) ou un point (.).  
  
 Dans cet exemple, si LOCATION='/webdata/', une requête PolyBase retourne des lignes à partir de mydata.txt et mydata2.txt.  Il ne retourne pas les données de mydata3.txt, car il se trouve dans un sous-dossier d’un dossier masqué. Il ne retourne pas non plus _hidden.txt, car il s’agit d’un fichier masqué.  
  
 ![Données récursives pour tables externes](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Données récursives pour tables externes")  
  
 Pour modifier la valeur par défaut et uniquement lire les données du dossier racine, définissez l’attribut \<polybase.recursive.traversal> sur 'false' dans le fichier de configuration core-site.xml. Ce fichier se trouve sous `<SqlBinRoot>\Polybase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Par exemple, `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.  
  
 DATA_SOURCE = *external_data_source_name*  
 Spécifie le nom de la source de données externe qui contient l’emplacement des données externes. Cet emplacement est soit Hadoop, soit Azure Blob Storage. Pour créer une source de données externe, utilisez [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
 FILE_FORMAT = *external_file_format_name*  
 Spécifie le nom de l’objet de format de fichier externe qui stocke le type de fichier et la méthode de compression des données externes. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).  
  
 Options REJECT  
 Vous pouvez spécifier les paramètres REJECT qui déterminent la façon dont PolyBase traite les enregistrements *incorrects* qu’il récupère à partir de la source de données externe. Un enregistrement de données est considéré comme « incorrect » si les types de données ou le nombre de colonnes ne correspondent pas aux définitions de colonne de la table externe.  
  
 Si vous ne spécifiez pas ou ne modifiez pas les valeurs REJECT, PolyBase utilise les valeurs par défaut. Ces informations sur les paramètres REJECT sont stockées en tant que métadonnées supplémentaires lorsque vous créez une table externe avec l’instruction CREATE EXTERNAL TABLE.   Lorsqu’une prochaine instruction SELECT ou SELECT INTO SELECT sélectionne des données dans la table externe, PolyBase utilise les options REJECT pour déterminer le nombre ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête. . La requête retourne les résultats (partiels) jusqu’à ce que le seuil de rejet soit dépassé. Ensuite, elle échoue avec le message d’erreur correspondant.  
  
 REJECT_TYPE = **value** | percentage  
 Précise si l’option REJECT_VALUE est spécifiée comme une valeur littérale ou un pourcentage.  
  
 valeur  
 REJECT_VALUE est une valeur littérale, et non un pourcentage. La requête PolyBase échoue lorsque le nombre de lignes rejetées dépasse la valeur *reject_value*.  
  
 Par exemple, si REJECT_VALUE = 5 et REJECT_TYPE = value, la requête PolyBase SELECT échoue après le rejet de 5 lignes.  
  
 percentage  
 REJECT_VALUE est un pourcentage, et non une valeur littérale. Une requête PolyBase échoue lorsque le *pourcentage* de lignes ayant échoué dépasse la valeur *reject_value*. Le pourcentage de lignes ayant échoué est calculé à intervalles.  
  
 REJECT_VALUE = *reject_value*  
 Spécifie la valeur ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête.  
  
 Pour REJECT_TYPE = value, *reject_value* doit être un entier compris entre 0 et 2 147 483 647.  
  
 Pour REJECT_TYPE = percentage, *reject_value* doit être une valeur float comprise entre 0 et 100.  
  
 REJECT_SAMPLE_VALUE = *reject_sample_value*  
 Cet attribut est nécessaire lorsque vous spécifiez REJECT_TYPE = percentage. Il détermine le nombre de lignes à tenter de récupérer avant que PolyBase ne recalcule le pourcentage de lignes rejetées.  
  
 Le paramètre *reject_sample_value* doit être un entier compris entre 0 et 2 147 483 647.  
  
 Par exemple, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcule le pourcentage de lignes ayant échoué après avoir tenté d’importer 1000 lignes à partir du fichier de données externe. Si le pourcentage de lignes ayant échoué est inférieur à la valeur de *reject_value*, PolyBase tente de récupérer 1000 autres lignes. Il continue de recalculer le pourcentage de lignes ayant échoué après avoir tenté d’importer chacune des 1000 lignes supplémentaires.  
  
> [!NOTE]  
>  Étant donné que PolyBase calcule le pourcentage de lignes ayant échoué à intervalles, le pourcentage de lignes ayant échoué peut dépasser la valeur de *reject_value*.  
  

Exemple :  
  
 Cet exemple montre comment les trois options REJECT interagissent les unes avec les autres. Par exemple, si REJECT_TYPE = percentage, REJECT_VALUE = 30 et REJECT_SAMPLE_VALUE = 100, le scénario suivant peut se produire :  
  
-   PolyBase tente de récupérer les 100 premières lignes : la récupération échoue pour 25 d’entre elles, et réussit pour les 75 autres.  
  
-   Le pourcentage de lignes ayant échoué qui est obtenu est de 25 %, ce qui est inférieur à la valeur de rejet de 30 %. Par conséquent, PolyBase va continuer de récupérer les données à partir de la source de données externe.  
  
-   PolyBase tente de charger les 100 lignes suivantes. Cette fois-ci, le chargement réussit pour 25 d’entre elles, et échoue pour les 75 autres.  
  
-   Le pourcentage de lignes ayant échoué est recalculé et on obtient 50 %. Le pourcentage de lignes ayant échoué a donc dépassé la valeur de rejet de 30 %.  
  
-   La requête PolyBase échoue après le rejet de 50 % des 200 premières lignes qu’elle a tenté de retourner. Notez que les lignes correspondantes sont retournées avant que la requête PolyBase ne détecte que le seuil de rejet a été dépassé.  
  
REJECTED_ROW_LOCATION = *Emplacement de répertoire*
  
  Spécifie le répertoire dans la Source de données externe dans lequel les lignes rejetées et le fichier d’erreur correspondant doivent être écrits.
Si le chemin spécifié n’existe pas, PolyBase en crée un en votre nom. Un répertoire enfant est créé sous le nom « _rejectedrows ». Le caractère «_   » garantit que le répertoire est placé dans une séquence d’échappement pour le traitement d’autres données, sauf s’il est explicitement nommé dans le paramètre d’emplacement (location). Dans ce répertoire figure un dossier qui est créé d’après l’heure de soumission du chargement dans le format YearMonthDay - HourMinuteSecond (par exemple, 20180330-173205). Dans ce dossier, deux types de fichiers sont écrits : le fichier _reason (raison) et le fichier de données. 

Les fichiers de raison et les fichiers de données ont tous deux le queryID associé à l’instruction CTAS. Comme les données et la raison se trouvent dans des fichiers distincts, les fichiers correspondants ont un suffixe analogue. 
  
 Options de table externe partitionnée  
 Spécifie la source de données externe (source de données non SQL Server) et une méthode de distribution pour la [requête de base de données élastique](https://azure.microsoft.com/documentation/articles/sql-database-elastic-query-overview/).  
  
 DATA_SOURCE  
 Source de données externe, comme les données stockées dans un système de fichiers Hadoop, un stockage Blob Azure ou un [gestionnaire de cartes de partitions](https://azure.microsoft.com/documentation/articles/sql-database-elastic-scale-shard-map-management/).  
  
 SCHEMA_NAME  
 La clause SCHEMA_NAME offre la possibilité de mapper la définition de table externe sur une table d’un autre schéma dans la base de données distante. Cela permet de lever l’ambiguïté pour les schémas qui existent à la fois sur des bases de données locales et des bases de données distantes.  
  
 OBJECT_NAME  
 La clause OBJECT_NAME offre la possibilité de mapper la définition de table externe sur une table portant un nom différent dans la base de données distante. Cela permet de lever l’ambiguïté pour les noms d’objets qui existent à la fois sur des bases de données locales et des bases de données distantes.  
  
 DISTRIBUTION  
 Facultatif. Cette option est obligatoire uniquement pour les bases de données de type SHARD_MAP_MANAGER. Ce paramètre contrôle si une table est traitée comme une table partitionnée ou une table répliquée. Avec les tables **SHARDED** (*nom de colonne*), les données des différentes tables ne se chevauchent pas. **REPLICATED** spécifie que les tables contiennent les mêmes données sur chaque partition. **ROUND_ROBIN** indique qu’une méthode spécifique à l’application est utilisée pour distribuer les données.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite les autorisations utilisateur suivantes :  
  
-   **CREATE TABLE**  
  
-   **ALTER ANY SCHEMA**  
  
-   **ALTER ANY EXTERNAL DATA SOURCE**  
  
-   **ALTER ANY EXTERNAL FILE FORMAT**  

-   **CONTROL DATABASE**
  
 Notez que la connexion qui crée la source de données externe doit être autorisée à lire et à écrire dans la source de données externe, qui est située dans Hadoop ou Azure Blob Storage.  


 > [!IMPORTANT]  

>  L’autorisation ALTER ANY EXTERNAL DATA SOURCE accorde à n’importe quel principal la possibilité de créer et de modifier tout objet de source de données externe. Par conséquent, elle permet également d’accéder à toutes les informations d’identification délimitées à la base de données. Cette autorisation doit être considérée comme fournissant des privilèges très élevés, et doit donc être accordée uniquement aux principaux de confiance du système.

## <a name="error-handling"></a>Gestion des erreurs  
 Lors de l’exécution de l’instruction CREATE EXTERNAL TABLE, PolyBase tente de se connecter à la source de données externe. Si la tentative de connexion échoue, l’instruction échoue et la table externe n’est pas créée. L’échec de la commande peut prendre plusieurs minutes, car PolyBase tente plusieurs connexions avant que la requête n’échoue.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Dans les scénarios de requête ad hoc (c’est-à-dire avec SELECT FROM EXTERNAL TABLE), PolyBase stocke les lignes extraites de la source de données externe dans une table temporaire. Une fois la requête terminée, PolyBase supprime la table temporaire. Aucune donnée permanente n’est stockée dans les tables SQL.  
  
 En revanche, dans le scénario d’importation (c’est-à-dire avec SELECT INTO FROM EXTERNAL TABLE), PolyBase stocke les lignes extraites de la source de données externe en tant que données permanentes dans la table SQL. La nouvelle table est créée lors de l’exécution de la requête, au moment où Polybase récupère les données externes.  
  
 PolyBase peut envoyer (push) une partie du calcul des requêtes vers Hadoop pour améliorer les performances des requêtes. C’est ce que l’on appelle la poussée de prédicats. Pour ce faire, spécifiez l’option d’emplacement du gestionnaire de ressources Hadoop dans [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
 Vous pouvez créer de nombreuses tables externes qui référencent les mêmes sources de données externes ou des sources différentes.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Dans la version CTP2, la fonctionnalité d’exportation (c’est-à-dire le stockage définitif de données SQL dans la source de données externe) n’est pas prise en charge. Cette fonctionnalité sera disponible dans la version CTP3.  
  
 Étant donné que les données d’une table externe se trouvent en dehors de l’application, elles ne sont pas sous le contrôle de PolyBase, et peuvent donc être modifiées ou supprimées à tout moment par un processus externe. Pour cette raison, il n’est pas garanti que les résultats d’une requête exécutée sur une table externe soient déterministes. La même requête peut retourner des résultats différents chaque fois qu’elle est exécutée sur une table externe. De même, une requête peut échouer si des données externes sont supprimées ou déplacées.  
  
 Vous pouvez créer plusieurs tables externes qui référencent chacune des sources de données externes différentes. Toutefois, si vous exécutez plusieurs requêtes simultanées sur des sources de données Hadoop différentes, chaque source Hadoop doit utiliser le même paramètre de configuration de serveur pour la connectivité Hadoop. Par exemple, vous ne pouvez pas exécuter simultanément une requête sur un cluster Hadoop Cloudera et sur un cluster Hadoop Hortonworks, puisqu’ils utilisent des paramètres de configuration différents. Pour connaître les paramètres de configuration et les combinaisons prises en charge, consultez [Configuration de la connectivité PolyBase &#40;Transact-SQL&#41;](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).  
  
 Seules les instructions DDL suivantes sont autorisées avec les tables externes :  
  
-   CREATE TABLE et DROP TABLE  
  
-   CREATE STATISTICS et DROP STATISTICS  
Remarque : L’utilisation de CREATE STATISTICS et DROP STATISTICS sur les tables externes n’est pas prise en charge dans Azure SQL Database. 
  
-   CREATE VIEW et DROP VIEW  
  
 Les constructions et les opérations suivantes ne sont pas prises en charge :  
  
-   La contrainte DEFAULT sur les colonnes de table externe  
  
-   Les opérations DML delete, insert et update  
  
 Limitations des requêtes :  
  
 PolyBase peut consommer un maximum de 33 000 fichiers par dossier lors de l’exécution simultanée de 32 requêtes PolyBase. Ce nombre maximal inclut les fichiers et les sous-dossiers de chaque dossier HDFS. Si le degré de concurrence est inférieur à 32, un utilisateur peut exécuter des requêtes PolyBase sur des dossiers dans HDFS contenant plus de 33 000 fichiers. Il est recommandé de raccourcir au maximum les chemins de fichiers externes et de ne pas utiliser plus de 30 000 fichiers par dossier HDFS. Lorsque trop de fichiers sont référencés, une exception d’insuffisance de mémoire Java Virtual Machine (JVM) peut être levée.  

Largeur limite des tables : Dans SQL Server 2016, PolyBase a une limite de largeur de ligne de 32 Ko, basée sur la taille maximale d’une ligne valide par définition de table. Si la somme du schéma de colonne est supérieure à 32 Ko, PolyBase ne peut pas interroger les données. 

Dans SQL Data Warehouse, cette limite a été relevée à 1 Mo.


## <a name="locking"></a>Verrouillage  
 Verrou partagé sur l’objet SCHEMARESOLUTION.  
  
## <a name="security"></a>Sécurité  
 Les fichiers de données d’une table externe sont stockés dans Hadoop ou Azure Blob Storage. Ces fichiers de données sont créés et gérés par vos propres processus. Il vous incombe de gérer la sécurité des données externes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-create-an-external-table-with-data-in-text-delimited-format"></a>A. Créer une table externe avec des données au format texte délimité  
 Cet exemple montre toutes les étapes nécessaires à la création d’une table externe dont les données sont des fichiers de texte délimité. Il définit la source de données externe *mydatasource* et le format de fichier externe *myfileformat*. Ces objets de niveau base de données sont ensuite référencés dans l’instruction CREATE EXTERNAL TABLE. Pour plus d’informations, consultez [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md) et [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).  
  
```  
  
CREATE EXTERNAL DATA SOURCE mydatasource  
WITH (  
    TYPE = HADOOP,  
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'  
)  
  
CREATE EXTERNAL FILE FORMAT myfileformat  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,   
    FORMAT_OPTIONS (FIELD_TERMINATOR ='|')  
);  
  
CREATE EXTERNAL TABLE ClickStream (   
    url varchar(50),  
    event_date date,  
    user_IP varchar(50)  
)  
WITH (  
        LOCATION='/webdata/employee.tbl',  
        DATA_SOURCE = mydatasource,  
        FILE_FORMAT = myfileformat  
    )  
;  
  
```  
  
### <a name="b-create-an-external-table-with-data-in-rcfile-format"></a>B. Créer une table externe avec des données au format RCFile  
 Cet exemple montre toutes les étapes nécessaires à la création d’une table externe contenant des données au format RCFile. Il définit la source de données externe *mydatasource_rc* et le format de fichier externe *myfileformat_rc*. Ces objets de niveau base de données sont ensuite référencés dans l’instruction CREATE EXTERNAL TABLE. Pour plus d’informations, consultez [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md) et [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).  
  
```  
  
CREATE EXTERNAL DATA SOURCE mydatasource_rc  
WITH (  
    TYPE = HADOOP,  
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'  
)  
  
CREATE EXTERNAL FILE FORMAT myfileformat_rc  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'  
)  
;  
  
CREATE EXTERNAL TABLE ClickStream_rc (   
    url varchar(50),  
    event_date date,  
    user_ip varchar(50)  
)  
WITH (  
        LOCATION='/webdata/employee_rc.tbl',  
        DATA_SOURCE = mydatasource_rc,  
        FILE_FORMAT = myfileformat_rc  
    )  
;  
  
```  
  
### <a name="c-create-an-external-table-with-data-in-orc-format"></a>C. Créer une table externe avec des données au format ORC  
 Cet exemple montre toutes les étapes nécessaires à la création d’une table externe contenant des données au format ORC. Il définit la source de données externe mydatasource_orc et le format de fichier externe myfileformat_orc. Ces objets de niveau base de données sont ensuite référencés dans l’instruction CREATE EXTERNAL TABLE. Pour plus d’informations, consultez [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md) et [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).  
  
```  
  
CREATE EXTERNAL DATA SOURCE mydatasource_orc  
WITH (  
    TYPE = HADOOP,  
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'  
)  
  
CREATE EXTERNAL FILE FORMAT myfileformat_orc  
WITH (  
    FORMAT = ORC,  
    COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
)  
;  
  
CREATE EXTERNAL TABLE ClickStream_orc (   
    url varchar(50),  
    event_date date,  
    user_ip varchar(50)  
)  
WITH (  
        LOCATION='/webdata/',  
        DATA_SOURCE = mydatasource_orc,  
        FILE_FORMAT = myfileformat_orc  
    )  
;  
  
```  
  
### <a name="d-querying-hadoop-data"></a>D. Interrogation de données Hadoop  
 Clickstream est une table externe qui se connecte au fichier texte délimité employee.tbl dans un cluster Hadoop. La requête suivante ressemble à une requête exécutée sur une table standard. Toutefois, cette requête récupère les données dans Hadoop, puis calcule les résultats.  
  
```  
SELECT TOP 10 (url) FROM ClickStream WHERE user_ip = 'xxx.xxx.xxx.xxx'  
;  
```  
  
### <a name="e-join-hadoop-data-with-sql-data"></a>E. Joindre des données Hadoop à des données SQL  
 Cette requête ressemble à une requête JOIN standard exécutée sur deux tables SQL. La différence est que PolyBase récupère les données Clickstream dans Hadoop, puis les joint à la table UrlDescription. L’une des tables est une table externe, et l’autre est une table SQL standard.  
  
```  
SELECT url.description  
FROM ClickStream cs  
JOIN UrlDescription url ON cs.url = url.name  
WHERE cs.url = 'msdn.microsoft.com'  
;  
```  
  
### <a name="f-import-data-from-hadoop-into-a-sql-table"></a>F. Importer des données Hadoop dans une table SQL  
 Cet exemple crée la table ms_user SQL qui stocke de façon permanente le résultat d’une jointure entre la table SQL standard *user* et la table externe *ClickStream*.  
  
```  
SELECT DISTINCT user.FirstName, user.LastName  
INTO ms_user  
FROM user INNER JOIN (  
    SELECT * FROM ClickStream WHERE cs.url = 'www.microsoft.com'  
    ) AS ms_user  
ON user.user_ip = ms.user_ip  
;  
  
```  
  
### <a name="g-create-an-external-table-for-a-sharded-data-source"></a>G. Créer une table externe pour une source de données partitionnée  
 Cet exemple remappe une vue de gestion dynamique à distance vers une table externe à l’aide des clauses SCHEMA_NAME et OBJECT_NAME.  
  
```  
CREATE EXTERNAL TABLE [dbo].[all_dm_exec_requests]([session_id] smallint NOT NULL,  
  [request_id] int NOT NULL,  
  [start_time] datetime NOT NULL,   
  [status] nvarchar(30) NOT NULL,  
  [command] nvarchar(32) NOT NULL,  
  [sql_handle] varbinary(64),  
  [statement_start_offset] int,  
  [statement_end_offset] int,  
  [cpu_time] int NOT NULL)  
WITH  
(  
  DATA_SOURCE = MyExtSrc,  
  SCHEMA_NAME = 'sys',  
  OBJECT_NAME = 'dm_exec_requests',  
  DISTRIBUTION=ROUND_ROBIN  
);   
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-importing-data-from-adls-into-azure-includessdwincludesssdw-mdmd"></a>H. Importation de données ADLS dans Azure [!INCLUDE[ssDW](../../includes/ssdw-md.md)]  
 
  
```  

-- These values come from your Azure Active Directory Application used to authenticate to ADLS
CREATE DATABASE SCOPED CREDENTIAL ADLUser 
WITH IDENTITY = '<clientID>@\<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;


CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = 'adl://pbasetr.azuredatalakestore.net'
)



CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH ( FORMATTYPE = DELIMITEDTEXT 
     , FORMATOPTIONS ( FIELDTERMINATOR = '|' 
                     , STRINGDELIMITER = '' 
                     , DATEFORMAT = 'yyyy-MM-dd HH:mm:ss.fff' 
                     , USETYPE_DEFAULT = FALSE 
                     ) 
    )


CREATE EXTERNAL TABLE [dbo].[DimProductexternal] 
( [ProductKey] [int] NOT NULL, 
  [ProductLabel] nvarchar NULL, 
  [ProductName] nvarchar NULL ) 
WITH ( LOCATION='/DimProduct/' , 
       DATA_SOURCE = AzureDataLakeStore , 
       FILE_FORMAT = TextFileFormat , 
       REJECT_TYPE = VALUE ,
       REJECT_VALUE = 0 ) ;


CREATE TABLE [dbo].[DimProduct] 
WITH (DISTRIBUTION = HASH([ProductKey] ) ) 
AS SELECT * FROM 
[dbo].[DimProduct_external] ; 
     
```  
  
### <a name="i-join-external-tables"></a>I. Joindre des tables externes  
  
```  
SELECT url.description  
FROM ClickStream cs  
JOIN UrlDescription url ON cs.url = url.name  
WHERE cs.url = 'msdn.microsoft.com'  
;  
```  
  
### <a name="j-join-hdfs-data-with-pdw-data"></a>J. Joindre des données HDFS et des données PDW  
  
```  
SELECT cs.user_ip FROM ClickStream cs  
JOIN User u ON cs.user_ip = u.user_ip  
WHERE cs.url = 'www.microsoft.com'  
;  
  
```  
  
### <a name="k-import-row-data-from-hdfs-into-a-distributed-pdw-table"></a>K. Importer des données de lignes HDFS dans une table PDW distribuée  
  
```  
CREATE TABLE ClickStream_PDW  
WITH ( DISTRIBUTION = HASH (url) )  
AS SELECT url, event_date, user_ip FROM ClickStream  
;  
```  
  
### <a name="l-import-row-data-from-hdfs-into-a-replicated-pdw-table"></a>L. Importer des données de lignes HDFS dans une table PDW répliquée  
  
```  
CREATE TABLE ClickStream_PDW  
WITH ( DISTRIBUTION = REPLICATE )  
AS SELECT url, event_date, user_ip   
FROM ClickStream  
;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Exemples de requêtes de métadonnées courantes (SQL Server PDW)](http://msdn.microsoft.com/en-us/733fc99b-b9f6-4a29-b085-a1bd4f09f2ed)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE AS SELECT &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)  
  
  



