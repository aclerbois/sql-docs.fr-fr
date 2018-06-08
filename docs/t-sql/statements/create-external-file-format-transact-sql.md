---
title: CREATE EXTERNAL FILE FORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 2/20/2018
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL FILE FORMAT
- CREATE_EXTERNAL_FILE_FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, file format
- PolyBase, external file format
ms.assetid: abd5ec8c-1a0e-4d38-a374-8ce3401bc60c
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f478c5f06ff846d313625dc0792b33708a9ca358
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-external-file-format-transact-sql"></a>CREATE EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]

  Crée un objet de format de fichier externe qui définit des données externes stockées dans Hadoop, dans le Stockage Blob Azure ou dans Azure Data Lake Store. La création d’un format de fichier externe est un prérequis à la création d’une table externe. En créant un format de fichier externe, vous spécifiez la disposition des données référencées par une table externe.  
  
 PolyBase prend en charge les formats de fichier suivants :
  
-   Texte délimité  
  
-   Hive RCFile  
  
-   Hive ORC
  
-   Parquet  
  
Pour créer une table externe, consultez [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md).
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe
  
```
-- Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = PARQUET  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);  
  
--Create an external file format for ORC files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = ORC  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.DefaultCodec'      }  
    ]);  
  
--Create an external file format for RCFILE.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = {  
        'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'  
      | 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'  
    }  
    [ , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec' ]);  
  
--Create an external file format for DELIMITED TEXT files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT  
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    [ , DATA_COMPRESSION = {  
           'org.apache.hadoop.io.compress.GzipCodec'  
         | 'org.apache.hadoop.io.compress.DefaultCodec'  
        }  
     ]);  
  
<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter 
    | First_Row = integer -- ONLY AVAILABLE SQL DW
    | DATE_FORMAT = datetime_format  
    | USE_TYPE_DEFAULT = { TRUE | FALSE } 
    | Encoding = {'UTF8' | 'UTF16'} 
}  
```  
  
## <a name="arguments"></a>Arguments  
 *file_format_name*  
 Spécifie un nom pour le format de fichier externe.
  
 FORMAT_TYPE = [ PARQUET | ORC | RCFILE | PARQUET] Spécifie le format des données externes.
  
   -   PARQUET Spécifie un format Parquet.
  
   -   ORC  
   Spécifie un format ORC. Cette option nécessite Hive version 0.11 ou version ultérieure dans le cluster Hadoop externe. Dans Hadoop, le format de fichier ORC offre une meilleure compression et de meilleures performances que le format de fichier RCFILE.

   -   RCFILE (en association avec SERDE_METHOD = *SERDE_method*) Spécifie un format de fichier RcFile. Cette option nécessite que vous spécifiiez un sérialiseur Hive et la méthode de désérialiseur (SerDe). Cette exigence est la même si vous utilisez Hive/HiveQL dans Hadoop pour interroger les fichiers RC. Notez que la méthode SerDe respecte la casse.

   Exemples de spécification RCFile avec les deux méthodes SerDe prises en charge par PolyBase :

    -   FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'

    -   FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'

   -   DELIMITEDTEXT spécifie un format de texte avec des délimiteurs de colonne, également appelés « marques de fin de champ ».
  
 FIELD_TERMINATOR = *field_terminator*  
S’applique uniquement aux fichiers texte délimité. La marque de fin de champ spécifie un ou plusieurs caractères qui indiquent la fin de chaque champ (colonne) dans le fichier texte délimité. La valeur par défaut est le caractère ꞌ|ꞌ. Pour une prise en charge garantie, nous vous recommandons d’utiliser un ou plusieurs caractères ASCII.
  
  
 Exemples :  
  
-   FIELD_TERMINATOR = '|'  
  
-   FIELD_TERMINATOR = ' '  
  
-   FIELD_TERMINATOR = ꞌ\tꞌ  
  
-   FIELD_TERMINATOR = '~|~'  
  
 STRING_DELIMITER = *string_delimiter*  
Spécifie la marque de fin de champ pour les données de type chaîne du fichier texte délimité. Le délimiteur de chaîne est constitué d’un ou plusieurs caractères, et est entouré de guillemets simples. La valeur par défaut est la chaîne vide "". Pour une prise en charge garantie, nous vous recommandons d’utiliser un ou plusieurs caractères ASCII.
 
  
 Exemples :  

-   STRING_DELIMITER = '"'

-   STRING_DELIMITER = '0 x 22'-- valeur hexadécimale de guillemets doubles
  
-   STRING_DELIMITER = '*'  
  
-   STRING_DELIMITER = ꞌ,ꞌ  
  
-   STRING_DELIMITER = '0x7E0x7E'--deux tildes (par exemple, ~~)
 
 FIRST_ROW = *First_row_int*  
Spécifie le numéro de la ligne qui est lue en premier dans tous les fichiers lors d’un chargement PolyBase. Ce paramètre peut prendre les valeurs allant de 1 à 15. Si la valeur est définie sur 2, la première ligne de chaque fichier (ligne d’en-tête) est ignorée lorsque les données sont chargées. Les lignes sont ignorées en présence de marques de fin de ligne (/r/n, /r, /n). Lorsque cette option est utilisée pour l’exportation, les lignes sont ajoutées aux données pour garantir que le fichier pourra être lu sans perte de données. Si la valeur est définie sur > 2, la première ligne exportée est celle des noms de colonnes de la table externe.

 DATE\_FORMAT = *datetime_format*  
Spécifie un format personnalisé pour toutes les données de date et d’heure qui peuvent apparaître dans un fichier texte délimité. Si le fichier source utilise des formats DateHeure par défaut, cette option n’est pas nécessaire. Un seul format DateHeure personnalisé est autorisé par fichier. Vous ne pouvez pas spécifier plusieurs formats DateHeure personnalisés dans un même fichier. Toutefois, vous pouvez utiliser plusieurs formats DateHeure si chacun d’eux est le format par défaut de son type de données dans la définition de la table externe.

PolyBase utilise uniquement le format de date personnalisé pour l’importation des données. Il n’utilise pas le format personnalisé pour l’écriture de données dans un fichier externe.

 Lorsque DATE_FORMAT n’est pas spécifié ou est une chaîne vide, PolyBase utilise les formats par défaut suivants :
  
-   DateTime : 'yyyy-MM-dd HH:mm:ss'  
  
-   SmallDateTime : 'yyyy-MM-dd HH:mm'  
  
-   Date : 'yyyy-MM-dd'  
  
-   DateTime2 : 'yyyy-MM-dd HH:mm:ss'  
  
-   DateTimeOffset : 'yyyy-MM-dd HH:mm:ss'  
  
-   Time : 'HH:mm:ss'  
  
Des **exemples de formats de date** se trouvent dans le tableau suivant :
  
Remarques sur ce tableau :  
  
-   Les années, les mois et les jours peuvent avoir de nombreux formats et ordres différents. Le tableau présente uniquement le format **ymd**. Les mois peuvent avoir un ou deux chiffres, ou trois caractères. Les jours peuvent avoir un ou deux chiffres. Les années peuvent comporter deux ou quatre chiffres.
  
-   Les millisecondes (fffffff) ne sont pas obligatoires.
  
-   La mention du matin ou de l’après-midi (tt) n’est pas obligatoire. Par défaut, l’heure du matin (AM) est utilisée.
  
|Type de date| Exemple|Description|  
|---------------|-------------|-----------------|  
|DateTime|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 24 heures, de 00 à 59 minutes, de 00 à 59 secondes, ainsi que 3 chiffres pour les millisecondes.|  
|DateTime|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffftt'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 12 heures, de 00 à 59 minutes, de 00 à 59 secondes, 3 chiffres pour les millisecondes, ainsi que la mention AM, am, PM ou pm. |  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd HH:mm'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 23 heures et de 00 à 59 minutes.|  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd hh:mmtt'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 11 heures, de 00 à 59 minutes, pas de secondes, et la mention AM, am, PM ou pm.|  
|Date|DATE_FORMAT =  'yyyy-MM-dd'|L’année, le mois et le jour. Aucun élément d’heure n’est inclus.|  
|Date|DATE_FORMAT = 'yyyy-MMM-dd'|L’année, le mois et le jour. Lorsque le mois est spécifié avec 3 M, la valeur d’entrée est l’une des chaînes suivantes : Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov ou Dec.|  
|datetime2|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 23 heures, de 00 à 59 minutes, de 00 à 59 secondes, ainsi que 7 chiffres pour les millisecondes.|  
|datetime2|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 11 heures, de 00 à 59 minutes, de 00 à 59 secondes, 7 chiffres pour les millisecondes, ainsi que la mention AM, am, PM ou pm.|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff zzz'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 23 heures, de 00 à 59 minutes, de 00 à 59 secondes, 7 chiffres pour les millisecondes, ainsi que le décalage des fuseaux horaires que vous placez dans le fichier ainsi : `{+&#124;-}HH:ss`. Par exemple, étant donné que l’heure de Los Angeles, sans l’heure d’été, est en retard de 8 heures par rapport à l’heure UTC, la valeur -08:00, dans le fichier d’entrée va spécifier le fuseau horaire de Los Angeles.|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt zzz'|Outre l’année, le mois et le jour, ce format de date inclut de 00 à 11 heures, de 00 à 59 minutes, de 00 à 59 secondes, 7 chiffres pour les millisecondes, la mention AM, am, PM ou pm, et le décalage des fuseaux horaires. Voir la description à la ligne précédente.|  
|Time|DATE_FORMAT = 'HH:mm:ss'|Il n’existe aucune valeur de date, seulement de 00 à 23 heures, de 00 à 59 minutes et de 00 à 59 secondes.|  
  
 Voici tous les formats de date pris en charge :
  
|DATETIME|smalldatetime|Date|datetime2|datetimeoffset|  
|--------------|-------------------|----------|---------------|--------------------|  
|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fff]|[M[M]]M-[d]d-[yy]yy HH:mm[:00]|[M[M]]M-[d]d-[yy]yy|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff]|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm[:00][tt]||[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fff]|[M[M]]M-[yy]yy-[d]d HH:mm[:00]|[M[M]]M-[yy]yy-[d]d|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff]|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm[:00][tt]||[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fff]|[yy]yy-[M[M]]M-[d]d HH:mm[:00]|[yy]yy-[M[M]]M-[d]d|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm[:00][tt]||[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fff]|[yy]yy-[d]d-[M[M]]M HH:mm[:00]|[yy]yy-[d]d-[M[M]]M|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm[:00][tt]||[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fff]|[d]d-[M[M]]M-[yy]yy HH:mm[:00]|[d]d-[M[M]]M-[yy]yy|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff]|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm[:00][tt]||[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fff]|[d]d-[yy]yy-[M[M]]M HH:mm[:00]|[d]d-[yy]yy-[M[M]]M|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm[:00][tt]||[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
  
 Détails :  
  
-   Pour séparer le mois, du jour et de l’année, vous pouvez utiliser les caractères « – », « / » ou « . ». Par souci de simplicité, le tableau utilise uniquement le séparateur « – ».
  
-   Pour spécifier le mois sous forme de texte, utilisez les trois caractères (ou plus). Les mois constitués d’un ou deux caractères sont interprétés comme des nombres.
  
-   Pour séparer les valeurs d’heure, utilisez le symbole « : ».
  
-   Les lettres entre crochets sont facultatives.
  
-   Les lettres « tt » correspondent aux mentions [AM|PM|am|pm]. AM est la valeur par défaut. Lorsque « tt » est spécifié, la valeur d’heure (hh) doit être comprise entre 0 et 12.
  
-   Les lettres « zzz » désignent le décalage par rapport au fuseau horaire du système, au format {+|-}HH:ss].
  
 USE_TYPE_DEFAULT = { TRUE | **FALSE** }  
 Spécifie comment gérer les valeurs manquantes dans les fichiers texte délimité lorsque PolyBase récupère les données à partir du fichier texte.
  
 TRUE  
 Lors de la récupération des données à partir du fichier texte, stockez chaque valeur manquante à l’aide de la valeur par défaut du type de données de la colonne correspondante dans la définition de la table externe. Par exemple, remplacez une valeur manquante par :  
  
-   0 si la colonne est définie comme une colonne numérique.
  
-   Une chaîne vide «» si la colonne est une colonne de chaîne.
  
-   1900-01-01 si la colonne est une colonne de date.
  
 FALSE  
 Stockez toutes les valeurs manquantes en tant que valeurs NULL. Toutes les valeurs NULL qui sont stockées à l’aide du mot NULL dans le fichier texte délimité sont importées en tant que chaînes NULL.
  
   Encodage = {'UTF8' | 'UTF16'}  
 Dans Azure SQL Data Warehouse, PolyBase peut lire les fichiers texte délimité aux formats UTF8 et UTF16-LE. Dans SQL Server et PDW, PolyBase ne prend pas en charge la lecture des fichiers UTF16.
  
 DATA_COMPRESSION = *data_compression_method*  
 Spécifie la méthode de compression appliquée aux données externes. Lorsque DATA_COMPRESSION n’est pas spécifié, par défaut, les données ne sont pas compressées.
 Pour fonctionner correctement, les fichiers compressés avec Gzip doivent avoir l’extension de fichier « .gz ».
 
 Le type de format DELIMITEDTEXT prend en charge les méthodes de compression suivantes :
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'

 Le type de format RCFILE prend en charge la méthode de compression suivante :
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
 Le type de format ORC prend en charge les méthodes de compression suivantes :
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
  
 Le type de format PARQUET prend en charge les méthodes de compression suivantes :
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
  
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation ALTER ANY EXTERNAL FILE FORMAT.
  
## <a name="general-remarks"></a>Remarques d'ordre général
 Le format de fichier externe est limité à la base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Il est limité au serveur dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 Les options de format sont toutes facultatives et s’appliquent uniquement aux fichiers texte délimité.  
  
 Lorsque les données sont stockées dans l’un des formats compressés, PolyBase décompresse d’abord les données avant de retourner les enregistrements de données.
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions
  
 Dans les fichiers texte délimité, le séparateur de lignes doit être pris en charge par LineRecordReader de Hadoop. Il doit donc être « \r », « \n » ou « \r\n ». Ces délimiteurs ne sont pas configurables par l’utilisateur.
  
 Les combinaisons de méthodes SerDe prises en charge par RCFile et les méthodes de compression de données prises en charge sont répertoriées plus haut dans cet article. Toutes les combinaisons ne sont pas prises en charge.
  
 Le nombre maximal de requêtes PolyBase simultanées est de 32. Lorsque 32 requêtes sont exécutées simultanément, chaque requête peut lire un maximum de 33 000 fichiers à partir de l’emplacement de fichier externe. Le dossier racine et chaque sous-dossier comptent également comme un fichier. Si le degré de concurrence est inférieur à 32, l’emplacement de fichier externe peut contenir plus de 33 000 fichiers.
  
 En raison de la limitation concernant le nombre de fichiers de la table externe, nous vous recommandons de stocker moins de 30 000 fichiers à la racine et dans les sous-dossiers de l’emplacement de fichier externe. En outre, nous vous recommandons de limiter au maximum le nombre de sous-dossiers dans le répertoire racine. Lorsque trop de fichiers sont référencés, une exception d’insuffisance de mémoire Java Virtual Machine peut être levée.
  
  En cas d’exportation de données vers Hadoop ou vers le Stockage Blob Azure via PolyBase, seules les données sont exportées, et non les noms de colonnes (métadonnées), comme le définit la commande CREATE EXTERNAL TABLE.

## <a name="locking"></a>Verrouillage  
 Prend un verrou partagé sur l’objet EXTERNAL FILE FORMAT.
  
## <a name="performance"></a>Performances
 L’utilisation de fichiers compressés présente l’inconvénient de devoir transférer moins de données entre la source de données externe et SQL Server, tout en augmentant l’utilisation du processeur pour compresser et décompresser les données.
  
 Les fichiers texte compressés avec GZip ne sont pas fractionnables. Pour améliorer les performances des fichiers texte compressés avec Gzip, nous vous recommandons de générer plusieurs fichiers et de les stocker dans le même répertoire de la source de données externe. Cette structure de fichiers permet à PolyBase de lire et de décompresser les données plus rapidement à l’aide de plusieurs processus de lecture et de décompression. Le nombre idéal de fichiers compressés correspond au nombre maximal de processus de lecteur de données par nœud de calcul. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], le nombre maximal de processus de lecteur de données est de 8 par nœud dans la version actuelle. Dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], le nombre maximal de processus de lecteur de données par nœud varie selon les objectifs du contrat. Pour plus d’informations, consultez [Azure SQL Data Warehouse loading patterns and strategies](https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies/).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-create-a-delimitedtext-external-file-format"></a>A. Créer un format de fichier externe DELIMITEDTEXT  
 Cet exemple crée un format de fichier externe nommé *textdelimited1* pour un fichier texte délimité. Les options répertoriées pour FORMAT\_OPTIONS spécifient que les champs du fichier doivent être séparés par le caractère « | ». Le fichier texte est également compressé avec le codec Gzip. Si DATA\_COMPRESSION n’est pas spécifié, le fichier texte est décompressé.
  
 Pour un fichier texte délimité, la méthode de compression des données peut être le Codec par défaut, « org.apache.hadoop.io.compress.DefaultCodec », ou le Codec Gzip « org.apache.hadoop.io.compress.GzipCodec ».
  
```  
CREATE EXTERNAL FILE FORMAT textdelimited1  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,  
    FORMAT_OPTIONS (  
        FIELD_TERMINATOR = '|',  
        DATE_FORMAT = 'MM/dd/yyyy' ),  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
);  
```  
  
### <a name="b-create-an-rcfile-external-file-format"></a>B. Créer un format de fichier externe RCFile  
 Cet exemple crée un format de fichier externe pour un RCFile qui utilise la méthode de sérialisation/désérialisation org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe. Il spécifie également l’utilisation du Codec par défaut pour la méthode de compression des données. Si DATA_COMPRESSION n’est pas spécifié, par défaut, les données ne sont pas compressées.
  
```  
CREATE EXTERNAL FILE FORMAT rcfile1  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe',  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'  
);  
```  
  
### <a name="c-create-an-orc-external-file-format"></a>C. Créer un format de fichier externe ORC  
 Cet exemple crée un format de fichier externe pour un fichier ORC qui compresse les données avec la méthode de compression des données org.apache.io.compress.SnappyCodec. Si DATA_COMPRESSION n’est pas spécifié, par défaut, les données ne sont pas compressées.
  
```  
CREATE EXTERNAL FILE FORMAT orcfile1  
WITH (  
    FORMAT_TYPE = ORC,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
  
### <a name="d-create-a-parquet-external-file-format"></a>D. Créer un format de fichier externe PARQUET  
 Cet exemple crée un format de fichier externe pour un fichier PARQUET qui compresse les données avec la méthode de compression des données org.apache.io.compress.SnappyCodec. Si DATA_COMPRESSION n’est pas spécifié, par défaut, les données ne sont pas compressées.  
  
```  
CREATE EXTERNAL FILE FORMAT parquetfile1  
WITH (  
    FORMAT_TYPE = PARQUET,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
### <a name="e-create-a-delimited-text-file-skipping-header-row-azure-sql-dw-only"></a>E. Créer un fichier texte délimité qui ignore la ligne d’en-tête (Azure SQL DW uniquement)
 Cet exemple crée un format de fichier externe pour un fichier CSV avec une seule ligne d’en-tête. 
  
```  
CREATE EXTERNAL FILE FORMAT skipHeader_CSV
WITH (FORMAT_TYPE = DELIMITEDTEXT,
      FORMAT_OPTIONS(
          FIELD_TERMINATOR = ',',
          STRING_DELIMITER = '"',
          FIRST_ROW = 2, 
          USE_TYPE_DEFAULT = True)
)
```   

## <a name="see-also"></a> Voir aussi
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE AS SELECT &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)  
