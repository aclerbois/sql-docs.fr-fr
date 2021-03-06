---
title: FREETEXTTABLE (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 06/10/2016
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database
ms.service: 
ms.component: system-functions
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- FREETEXTTABLE_TSQL
- FREETEXTTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- search conditions [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXTTABLE function (Transact-SQL)
- ranked results [full-text search]
- column searches [full-text search]
ms.assetid: 4523ae15-4260-40a7-a53c-8df15e1fee79
caps.latest.revision: 
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: On Demand
ms.openlocfilehash: 412de75f061da97a82e8494c442e17ba00b03ab7
ms.sourcegitcommit: c556eaf60a49af7025db35b7aa14beb76a8158c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="freetexttable-transact-sql"></a>FREETEXTTABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Est une fonction utilisée dans le [à partir de la clause](../../t-sql/queries/from-transact-sql.md) d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] une instruction SELECT pour effectuer une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche en texte intégral sur la recherche en texte intégral indexées colonnes contenant des types de données de type caractère. Cette fonction retourne une table de zéro, un ou plusieurs lignes pour les colonnes contenant des valeurs qui correspondent à la signification et pas seulement au libellé exact, du texte spécifié *chaîne_texte_libre*. FREETEXTTABLE est référencé comme s'il s'agissait d'un nom de table classique.  
  
 FREETEXTTABLE est utile pour les mêmes types de correspondance que le [FREETEXT &#40; Transact-SQL &#41; ](../../t-sql/queries/freetext-transact-sql.md),  
  
 Les requêtes utilisant FREETEXTTABLE retournent une valeur de classement de pertinence (RANK) et une clé de texte intégral (KEY) pour chaque ligne.  
  
> [!NOTE]  
>  Pour plus d’informations sur les formes de recherches en texte intégral qui sont prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [requête avec la recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md).  
  
(http://azure.microsoft.com/documentation/articles/sql-database-preview-whats-new/?WT.mc_id=TSQL_GetItTag)).|  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
FREETEXTTABLE (table , { column_name | (column_list) | * }   
          , 'freetext_string'   
     [ , LANGUAGE language_term ]   
     [ , top_n_by_rank ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *table*  
 Nom de la table marquée pour les requêtes de texte intégral. *table* ou *affichage*peut être un un, deux ou nom de l’objet de base de données en trois parties. Lors de l'interrogation d'une vue, une seule table de base indexée en texte intégral peut être impliquée.  
  
 *table* ne peut pas spécifier un nom de serveur et ne peut pas être utilisé dans les requêtes sur des serveurs liés.  
  
 *column_name*  
 Nom d'une ou de plusieurs colonnes de texte intégral indexées de la table spécifiée dans la clause FROM. Les colonnes peuvent être de type **char**, **varchar**, **nchar**, **nvarchar**, **texte**, **ntext**, **image**, **xml**, **varbinary**, ou **varbinary (max)**.  
  
 *column_list*  
 Indique que plusieurs colonnes, délimitées par des virgules, peuvent être spécifiées. *column_list* doit être mis entre parenthèses. À moins que *language_term* est spécifié, la langue de toutes les colonnes de *column_list* doivent être identiques.  
  
 \*  
 Spécifie que toutes les colonnes qui ont été enregistrés pour la recherche en texte intégral doivent être utilisés pour rechercher la donnée *chaîne_texte_libre*. À moins que *language_term* est spécifié, la langue de toutes les colonnes indexées en texte intégral dans la table doit être le même.  
  
 *freetext_string*  
 Est du texte à rechercher dans le *column_name*. Tout texte, y compris des mots, expressions et phrases peuvent être entrés. Des correspondances sont générées si l'un des termes ou les formes de quelque terme que ce soit sont trouvés dans l'index de texte intégral.  
  
 Contrairement à dans le CONTAINS recherche condition where et est un mot clé, lorsqu’il est utilisé dans *chaîne_texte_libre* le mot « et » est considéré comme un mot parasite, ou [mot vide](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)et seront ignorées.  
  
 L'utilisation de WEIGHT, de FORMSOF, de caractères génériques, de NEAR et d'autres syntaxes n'est pas autorisée. *chaîne_texte_libre* est découpée, traitées et transmises via le dictionnaire des synonymes.  
  
 LANGUAGE *language_term*  
 Langue dont les ressources seront utilisées pour l'analyse lexicale, la recherche de radical, l'utilisation du dictionnaire de synonymes et la suppression de mots vides dans la requête. Ce paramètre est facultatif et peut être spécifié sous la forme d'une chaîne, d'un entier ou d'une valeur hexadécimale correspondant à l'identificateur de paramètres régionaux (LCID) d'une langue. Si *language_term* est spécifié, la langue représentée est appliquée à tous les éléments de la condition de recherche. Si aucune valeur n'est définie, la langue du texte intégral de la colonne est utilisée.  
  
 Si des documents de langues différentes sont stockés ensemble en tant qu'objets blob dans une colonne unique, l'identificateur de paramètres régionaux (LCID) d'un document donné détermine la langue utilisée pour l'indexation de son contenu. Lorsque vous interrogez une telle colonne, en spécifiant *langage ** language_term* permet d’augmenter la probabilité d’une correspondance correcte.  
  
 Lorsque spécifié sous forme de chaîne, *language_term* correspond à la **alias** valeur de colonne dans le [sys.syslanguages &#40; Transact-SQL &#41; ](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) affichage de compatibilité.  La chaîne doit être entourée de guillemets simples, comme dans «*language_term*'. Lorsque spécifié en tant qu’entier, *language_term* est l’identificateur LCID réel qui identifie la langue. Lorsque spécifié en tant que valeur hexadécimale *language_term* est 0 x suivi de la valeur hexadécimale du LCID. La valeur hexadécimale ne doit pas dépasser huit caractères, y compris les zéros non significatifs.  
  
 Si la valeur est dans un format de caractères de deux octets (DBCS), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit en Unicode.  
  
 Si la langue spécifiée n'est pas valide ou si aucune ressource correspondant à cette langue n'est installée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur. Pour utiliser les ressources de langue neutre, spécifiez 0 x 0 *language_term*.  
  
 *top_n_by_rank*  
 Spécifie que seules les  *n* des correspondances de rang le plus élevés, par ordre décroissant, sont retournées. S’applique uniquement lorsqu’une valeur entière,  *n* , est spécifié. Si *top_n_by_rank* est associé à d’autres paramètres, la requête peut retourner moins de lignes que le nombre de lignes correspondant effectivement à tous les prédicats. *Top_n_by_rank* permet d’augmenter les performances des requêtes en rappelant uniquement les accès les plus pertinents.  
  
## <a name="remarks"></a>Notes  
 Les prédicats et les fonctions de texte intégral s'appliquent à une table unique, ce qui est implicite dans le prédicat FROM. Pour effectuer des recherches sur plusieurs tables, utilisez une table jointe dans votre clause FROM afin de baser votre recherche sur un jeu de résultats qui est le produit de deux tables ou plus.  
  
 FREETEXTTABLE utilise les mêmes conditions de recherche que le prédicat FREETEXT.  
  
 Comme CONTAINSTABLE, la table renvoyée possède des colonnes nommées **clé** et **rang**, qui sont référencé dans la requête pour obtenir les lignes appropriées et utiliser la ligne de valeurs de classement.  
  
## <a name="permissions"></a>Autorisations  
 FREETEXTTABLE ne peut être appelée que par les utilisateurs disposant des privilèges SELECT appropriés sur la table spécifiée ou les colonnes référencées de la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example"></a>A. Exemple simple  
 L’exemple suivant crée et remplit un tableau simple de deux colonnes, la liste des 3 comtés et les couleurs dans les indicateurs. L’informatique crée et remplit un catalogue de texte intégral et les index sur la table. Le **FREETEXTTABLE** syntaxe est illustrée.  
  
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
SELECT * FROM FREETEXTTABLE (Flags, FlagColors, 'Blue');  
SELECT * FROM FREETEXTTABLE (Flags, FlagColors, 'Yellow');  
```  
  
### <a name="b-using-freetext-in-an-inner-join"></a>B. Utilisation de FREETEXT dans un INNER JOIN  
 L'exemple suivant renvoie le nom et la description de toutes les catégories qui ont un rapport avec les mots `sweet`, `candy`, `bread`, `dry` ou `meat`.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Description  
    ,KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL   
    INNER JOIN FREETEXTTABLE(Production.ProductDescription,  
    Description,   
    'high level of performance') AS KEY_TBL  
ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
ORDER BY RANK DESC;  
GO  
```  
  
### <a name="c-specifying-language-and-highest-ranked-matches"></a>C. Spécification de la langue et des correspondances de classement supérieur  
 L’exemple suivant est identique et illustre l’utilisation de la `LANGUAGE` *language_term* et *top_n_by_rank* paramètres.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Description  
    ,KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL   
    INNER JOIN FREETEXTTABLE(Production.ProductDescription,  
    Description,   
    'high level of performance',  
    LANGUAGE N'English', 2) AS KEY_TBL  
ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
ORDER BY RANK DESC;  
GO  
```  
  
> [!NOTE]  
>  Le langage *language_term* paramete*r* n’est pas nécessaire d’utiliser le *top_n_by_rank* paramètre*.*  
  
## <a name="see-also"></a>Voir aussi  
 [Commencer à utiliser la recherche en texte intégral](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Créer et gérer des catalogues de texte intégral](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CRÉER le catalogue de texte intégral &#40; Transact-SQL &#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Requête avec la recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [Créer des requêtes de recherche en texte intégral &#40;Visual Database Tools&#41;](http://msdn.microsoft.com/library/537fa556-390e-4c88-9b8e-679848d94abc)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [Fonctions rowset &#40; Transact-SQL &#41;](../../t-sql/functions/rowset-functions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [Précalculer le rang (option de configuration de serveur)](../../database-engine/configure-windows/precompute-rank-server-configuration-option.md)  
  
  
