---
title: CONTAINSTABLE (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 07/24/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CONTAINSTABLE
- CONTAINSTABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- fuzzy (less precise) word or phrase search [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- values [SQL Server], ranked
- LANGUAGE option
- NEAR option [full-text search]
- RANK column
- phrase searches [full-text search]
- conditions [SQL Server], CONTAINSTABLE
- relevance ranking values [full-text search]
- proximity searches [full-text search]
- CONTAINSTABLE function (Transact-SQL)
- ranked results [full-text search]
- rankings [full-text search]
- less precise (fuzzy) searches [full-text search]
ms.assetid: e580c210-cf57-419d-9544-7f650f2ab814
caps.latest.revision: 69
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6c1741644ab38afd4003265b659c06b4b9448e20
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="containstable-transact-sql"></a>CONTAINSTABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une table composée de zéros, d'une ou de plusieurs ligne(s) pour les colonnes contenant des correspondances exactes ou floues (moins précises) de mots simples ou d'expressions, la proximité de mots à une certaine distance les uns des autres ou des correspondances pondérées. CONTAINSTABLE est utilisé dans le [à partir de la clause](../../t-sql/queries/from-transact-sql.md) d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction SELECT et est référencé comme s’il s’agissait d’un nom de table classique. Il effectue une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche en texte intégral sur la recherche en texte intégral indexées colonnes contenant des types de données de type caractère.  
  
 CONTAINSTABLE est utile pour les mêmes types de correspondance que le [prédicat CONTAINS](../../t-sql/queries/contains-transact-sql.md) et utilise les mêmes conditions de recherche CONTAINS.  
  
 Toutefois, contrairement à CONTAINS, les requêtes utilisant CONTAINSTABLE retournent une valeur de classement de pertinence (RANK) et une clé de texte intégral (KEY) pour chaque ligne.  Pour plus d’informations sur les sortes de recherches en texte intégral prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CONTAINSTABLE   
( table , { column_name | ( column_list ) | * } , ' <contains_search_condition> '   
     [ , LANGUAGE language_term]   
  [ , top_n_by_rank ]   
)   
  
<contains_search_condition> ::=   
    { <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    |  <weighted_term>   
    }   
    | { ( <contains_search_condition> )   
    { { AND | & } | { AND NOT | &! } | { OR | | } }   
     <contains_search_condition> [ ...n ]   
    }  
  
<simple_term> ::=   
     { word | "phrase" }  
<prefix term> ::=   
     { "word*" | "phrase*" }   
<generation_term> ::=   
     FORMSOF ( { INFLECTIONAL | THESAURUS } , <simple_term> [ ,...n ] )   
  
<generic_proximity_term> ::=   
     { <simple_term> | <prefix_term> } { { { NEAR | ~ }   
     { <simple_term> | <prefix_term> } } [ ...n ] }  
  
<custom_proximity_term> ::=   
  NEAR (   
     {  
        { <simple_term> | <prefix_term> } [ ,…n ]  
     |  
        ( { <simple_term> | <prefix_term> } [ ,…n ] )   
      [, <maximum_distance> [, <match_order> ] ]  
     }  
       )   
  
      <maximum_distance> ::= { integer | MAX }  
      <match_order> ::= { TRUE | FALSE }   
  
<weighted_term> ::=   
     ISABOUT  
    ( { {   
  <simple_term>   
  | <prefix_term>   
  | <generation_term>   
  | <proximity_term>   
  }   
   [ WEIGHT ( weight_value ) ]   
   } [ ,...n ]   
    )  
  
```  
  
## <a name="arguments"></a>Arguments  
 *table*  
 Nom d'une table qui a été indexée en texte intégral. *table* peut être un un, deux, trois ou nom de l’objet en quatre parties de la base de données. Lors de l'interrogation d'une vue, une seule table de base indexée en texte intégral peut être impliquée.  
  
 *table* ne peut pas spécifier un nom de serveur et ne peut pas être utilisé dans les requêtes sur des serveurs liés.  
  
 *column_name*  
 Nom d'une ou de plusieurs colonnes indexées pour la recherche en texte intégral. Les colonnes peuvent être de type **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** ou **varbinary(max)**.  
  
 *column_list*  
 Indique que plusieurs colonnes, délimitées par des virgules, peuvent être spécifiées. *column_list* doit être mis entre parenthèses. Une seule et même langue doit être utilisée dans toutes les colonnes de *column_list*, sauf si *language_term* est spécifié.  
  
 \*  
 Spécifie que dans les colonnes indexées en texte intégral tous les *table* doit être utilisée pour rechercher la condition de recherche donnée. Une seule et même langue doit être utilisée dans toutes les colonnes de la table, sauf si *language_term* est spécifié.  
  
 LANGUAGE *language_term*  
 Est la langue dont les ressources seront utilisées pour l’analyse lexicale, recherche de radical, dictionnaire des synonymes et des mots vides (ou [mot vide](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) dans le cadre de la requête. Ce paramètre est facultatif et peut être spécifié sous la forme d'une chaîne, d'un entier ou d'une valeur hexadécimale correspondant à l'identificateur de paramètres régionaux (LCID) d'une langue. Si une langue est définie avec *language_term*, elle est appliquée à tous les éléments de la condition de recherche. Si aucune valeur n'est définie, la langue du texte intégral de la colonne est utilisée.  
  
 Si des documents de langues différentes sont stockés ensemble en tant qu'objets blob dans une colonne unique, l'identificateur de paramètres régionaux (LCID) d'un document donné détermine la langue utilisée pour l'indexation de son contenu. Quand une requête est effectuée sur la colonne, la spécification de *LANGUAGE**language_term* augmente la probabilité d’une meilleure correspondance.  
  
 Lorsque spécifié sous forme de chaîne, *language_term* correspond à la **alias** valeur de colonne dans la [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) affichage de compatibilité.  La chaîne doit être placée entre guillemets simples, comme dans '*language_term*'. Quand il est spécifié sous la forme d’un entier, *language_term* est le LCID qui identifie la langue. Quand il est spécifié sous la forme d’une valeur hexadécimale, *language_term* est 0x suivi de la valeur hexadécimale du LCID. La valeur hexadécimale ne doit pas dépasser huit caractères, y compris les zéros non significatifs.  
  
 Si la valeur est au format DBCS (jeu de caractères codés sur deux octets), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la convertit en Unicode.  
  
 Si la langue spécifiée n'est pas valide ou si aucune ressource correspondant à cette langue n'est installée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur. Pour utiliser des ressources linguistiques neutres, indiquez 0x0 pour *language_term*.  
  
 *top_n_by_rank*  
 Spécifie que seules les *n* des correspondances de rang le plus élevés, par ordre décroissant, sont retournées. S’applique uniquement lorsqu’une valeur entière, *n*, est spécifié. Si *top_n_by_rank* est associé à d’autres paramètres, la requête peut retourner moins de lignes que le nombre de lignes correspondant effectivement à tous les prédicats. *Top_n_by_rank* permet d’augmenter les performances des requêtes en rappelant uniquement les accès les plus pertinents.  
  
 <contains_search_condition>  
 Spécifie le texte à rechercher dans *column_name* et les conditions de correspondance à remplir. Pour plus d’informations sur les conditions de recherche, consultez [contient &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 Les prédicats et les fonctions de texte intégral s'appliquent à une table unique, ce qui est implicite dans le prédicat FROM. Pour effectuer des recherches sur plusieurs tables, utilisez une table jointe dans votre clause FROM afin de baser votre recherche sur un jeu de résultats qui est le produit de deux tables ou plus.  
  
 La table retournée possède une colonne nommée **clé** qui contient les valeurs de clé de recherche en texte intégral. Chaque table indexée en texte intégral possède une colonne dont les valeurs sont uniques et les valeurs renvoyées dans le **clé** colonne sont les valeurs de clé de recherche en texte intégral des lignes qui correspondent aux critères de sélection spécifiés dans le contient la condition de recherche. Le **TableFulltextKeyColumn** propriété, obtenue à partir de la fonction OBJECTPROPERTYEX, fournit l’identité de cette colonne clé unique. Pour obtenir l’ID de la colonne associée à la clé de recherche en texte intégral de l’index de recherche en texte intégral, utilisez **sys.fulltext_indexes**. Pour plus d’informations, consultez [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 Pour obtenir les lignes qui vous intéressent dans la table originale, spécifiez une jointure avec les lignes CONTAINSTABLE. La clause FROM d'une instruction SELECT utilisant la fonction CONTAINSTABLE se présente généralement comme suit :  
  
```  
SELECT select_list  
FROM table AS FT_TBL INNER JOIN  
   CONTAINSTABLE(table, column, contains_search_condition) AS KEY_TBL  
   ON FT_TBL.unique_key_column = KEY_TBL.[KEY];  
```  
  
 La table produite par CONTAINSTABLE comporte une colonne intitulée **rang**. Le **rang** colonne est une valeur (de 0 à 1000) pour chaque ligne indiquant le degré de correspondance avec les critères de sélection. Cette valeur de classement est généralement utilisée de l'une des manières suivantes dans l'instruction SELECT :  
  
-   dans la clause ORDER BY, pour que les lignes ayant le degré de correspondance le plus élevé soient retournées en premier dans la table ;  
  
-   dans la liste de sélection, pour indiquer la valeur de classement affectée à chaque ligne.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution sont disponibles uniquement aux utilisateurs bénéficiant des privilèges SELECT appropriés sur la table ou sur les colonnes de la table référencée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example"></a>A. Exemple simple  
 L’exemple suivant crée et remplit un tableau simple de deux colonnes, la liste des 3 comtés et les couleurs dans les indicateurs. L’informatique crée et remplit un catalogue de texte intégral et les index sur la table. Le **CONTAINSTABLE** syntaxe est illustrée. Cet exemple montre comment la valeur de classement augmente supérieure lorsque la valeur de recherche est remplie à plusieurs reprises. Dans la dernière requête Tanzanie qui contient le vert et noir a un rang plus élevé qu’Italie contenant uniquement une des couleurs interrogés.  
  
```  
CREATE TABLE Flags (Country nvarchar(30) NOT NULL, FlagColors varchar(200));  
CREATE UNIQUE CLUSTERED INDEX FlagKey ON Flags(Country);  
INSERT Flags VALUES ('France', 'Blue and White and Red');  
INSERT Flags VALUES ('Italy', 'Green and White and Red');  
INSERT Flags VALUES ('Tanzania', 'Green and Yellow and Black and Yellow and Blue');  
SELECT * FROM Flags;  
GO  
  
CREATE FULLTEXT CATALOG TestFTCat;  
CREATE FULLTEXT INDEX ON Flags(FlagColors) KEY INDEX FlagKey ON TestFTCat;  
GO   
  
SELECT * FROM Flags;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green') ORDER BY RANK DESC;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green or Black') ORDER BY RANK DESC;  
```  
  
### <a name="b-returning-rank-values"></a>B. Retour de valeurs de classement  
 L'exemple suivant recherche tous les noms de produits contenant les mots « frame », « wheel » ou « tire ». Une pondération différente est affectée à chacun des mots. Le degré de correspondance (valeur de classement) est indiqué pour chaque ligne retournée correspondant à ces critères de recherche. Les lignes ayant le degré de correspondance le plus élevé sont retournées en premier.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Name, KEY_TBL.RANK  
    FROM Production.Product AS FT_TBL   
        INNER JOIN CONTAINSTABLE(Production.Product, Name,   
        'ISABOUT (frame WEIGHT (.8),   
        wheel WEIGHT (.4), tire WEIGHT (.2) )' ) AS KEY_TBL  
            ON FT_TBL.ProductID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
### <a name="c-returning-rank-values-greater-than-a-specified-value"></a>C. Retour de valeurs de classement supérieures à une valeur spécifiée  
  
||  
|-|  
|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
 L'exemple suivant utilise NEAR pour rechercher « `bracket` » et « `reflector` » à proximité l'un de l'autre dans la table `Production.Document`. Seules les lignes avec une valeur de rang supérieur ou égal à 50 sont retournées.  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT DocumentNode, Title, DocumentSummary  
FROM Production.Document AS DocTable   
INNER JOIN CONTAINSTABLE(Production.Document, Document,  
  'NEAR(bracket, reflector)' ) AS KEY_TBL  
  ON DocTable.DocumentNode = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 50  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
> [!NOTE]  
>  Si une requête de texte intégral ne spécifie pas d'entier comme distance maximale, un document qui contient uniquement des correspondances dont l'espacement est supérieur à 100 termes logiques ne satisfera pas les exigences requises par NEAR, et son classement sera de 0.  
  
### <a name="d-returning-top-5-ranked-results-using-topnbyrank"></a>D. Retour des 5 meilleurs résultats à l'aide de top_n_by_rank  
 L'exemple suivant retourne la description des 5 premiers produits dont la colonne `Description` contient les mots « aluminum » à proximité du mot « light » ou « lightweight ».  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
 `GO`  
  
### <a name="e-specifying-the-language-argument"></a>E. Utilisation de l'argument LANGUAGE  
 L'exemple suivant illustre l'utilisation de l'argument `LANGUAGE`.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      LANGUAGE N'English',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
> [!NOTE]  
>  Le langage *language_term* argumentis ne pas requis pour l’utilisation de *top_n_by_rank.*  
  
## <a name="see-also"></a>Voir aussi  
 [Limiter les résultats de recherche avec RANK](../../relational-databases/search/limit-search-results-with-rank.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [Créer des requêtes de recherche en texte intégral &#40;Visual Database Tools&#41;](http://msdn.microsoft.com/library/537fa556-390e-4c88-9b8e-679848d94abc)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  
