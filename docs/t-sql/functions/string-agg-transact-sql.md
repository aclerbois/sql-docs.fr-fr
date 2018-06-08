---
title: STRING_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- STRING_AGG
- STRING_AGG_TSQL
helpviewer_keywords:
- STRING_AGG function
ms.assetid: 8860ef3f-142f-4cca-aa64-87a123e91206
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: 7c270729aa66c05f835cba507884e8d5cda102d0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stringagg-transact-sql"></a>STRING_AGG (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

Concatène les valeurs des expressions de chaîne et place les valeurs de séparateur entre elles. Le séparateur n’est pas ajouté à la fin de la chaîne.
 
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
STRING_AGG ( expression, separator ) [ <order_clause> ]

<order_clause> ::=   
    WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )   
```

## <a name="arguments"></a>Arguments 

*separator*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de type `NVARCHAR` ou `VARCHAR` qui est utilisée comme séparateur pour les chaînes concaténées. Ce peut être un littéral ou une variable. 

*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type. Les expressions sont converties en types `NVARCHAR` ou `VARCHAR` durant la concaténation. Les types autres que chaîne sont convertis en type `NVARCHAR`.


<order_clause>   
Spécifiez éventuellement l’ordre des résultats concaténés à l’aide de la clause `WITHIN GROUP` :
```
WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )
```   
<order_by_expression_list>   
 
  Liste d’[expressions](../../t-sql/language-elements/expressions-transact-sql.md) non constantes qui peuvent être utilisées pour le tri des résultats. Un seul `order_by_expression` est autorisé par requête. L’ordre de tri par défaut est croissant.   
  

## <a name="return-types"></a>Types de retour 

Le type de retour dépend du premier argument (expression). Si l’argument d’entrée est de type chaîne (`NVARCHAR`, `VARCHAR`), le type de résultat sera identique au type d’entrée. Le tableau suivant répertorie les conversions automatiques :  

|Type d’expression d’entrée |Résultats | 
|-------|-------|
|NVARCHAR(MAX) |NVARCHAR(MAX) |
|VARCHAR(MAX) |VARCHAR(MAX) |
|NVARCHAR(1…4000) |NVARCHAR(4000) |
|VARCHAR(1…8000) |VARCHAR(8000) |
|int, bigint, smallint, tinyint, numeric, float, real, bit, decimal, smallmoney, money, datetime, datetime2, |NVARCHAR(4000) |


## <a name="remarks"></a>Notes   
 
`STRING_AGG` est une fonction d’agrégation qui accepte toutes les expressions à partir des lignes et les concatène en une seule chaîne. Les valeurs d’expression sont implicitement converties en types chaîne, puis concaténées. La conversion implicite en chaînes respecte les règles existantes de conversion de type de données. Pour plus d’informations sur les conversions de type de données, consultez [CAST et CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md). 

Si l’expression d’entrée est de type `VARCHAR`, le séparateur ne peut pas être de type `NVARCHAR`. 

Les valeurs NULL sont ignorées et le séparateur correspondant n’est pas ajouté. Pour renvoyer un espace réservé pour les valeurs NULL, utilisez la fonction `ISNULL` comme illustré dans l’exemple B.

`STRING_AGG` est disponible dans n’importe quel niveau de compatibilité.


## <a name="examples"></a>Exemples 

### <a name="a-generate-list-of-names-separated-in-new-lines"></a>A. Générer une liste de noms séparés sur plusieurs lignes 
L’exemple suivant génère une liste de noms dans une cellule de résultat unique, séparés par des retours chariot.
```sql
SELECT STRING_AGG (FirstName, CHAR(13)) AS csv 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|csv | 
|--- |
|Syed <br />Catherine <br />Kim <br />Kim <br />Kim <br />Hazem <br />... | 

Les valeurs `NULL` trouvées dans les cellules `name` ne sont pas renvoyées dans le résultat.   
> [!NOTE]  
>  Si vous utilisez l’éditeur de requête de Management Studio, l’option **Results to Grid** ne peut pas implémenter le retour chariot. Basculez vers **Results to Text** pour afficher correctement le jeu de résultats.   


### <a name="b-generate-list-of-names-separated-with-comma-without-null-values"></a>B. Générer une liste de noms séparés par des virgules sans valeurs NULL   
L’exemple suivant remplace les valeurs NULL par « N/A » et renvoie les noms séparés par des virgules dans une cellule de résultat unique.  
```sql
SELECT STRING_AGG ( ISNULL(FirstName,'N/A'), ',') AS csv 
FROM Person.Person; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]
 

|Csv | 
|--- |
|John,N/A,Mike,Peter,N/A,N/A,Alice,Bob |  


### <a name="c-generate-comma-separated-values"></a>C. Générer des valeurs séparées par des virgules 

```sql   
SELECT 
STRING_AGG(CONCAT(FirstName, ' ', LastName, ' (', ModifiedDate, ')'), CHAR(13)) 
  AS names 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|noms | 
|--- |
|Ken Sánchez (8 février 2003 12h00) <br />Terri Duffy (24 février 2002 12h00) <br />Roberto Tamburello (5 décembre 2001 12h00) <br />Rob Walters (29 décembre 2001 12h00) <br />... |

> [!NOTE]  
>  Si vous utilisez l’éditeur de requête de Management Studio, l’option **Results to Grid** ne peut pas implémenter le retour chariot. Basculez vers **Results to Text** pour afficher correctement le jeu de résultats.   
 

### <a name="d-return-news-articles-with-related-tags"></a>D. Retourner des articles d’actualité avec les balises associées 

Les articles et leurs balises sont séparés dans différentes tables. Le développeur souhaite renvoyer une ligne pour chaque article avec toutes les balises associées. À l’aide de la requête suivante : 
```sql
SELECT a.articleId, title, STRING_AGG (tag, ',') as tags 
FROM dbo.Article AS a       
LEFT JOIN dbo.ArticleTag AS t 
    ON a.ArticleId = t.ArticleId 
GROUP BY a.articleId, title;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|articleId |title |tags |
|--- |--- |--- |
|172 |Les sondages prévoient des résultats serrés aux élections |politique,sondages,conseil municipal | 
|176 |La nouvelle autoroute devrait réduire les embouteillages |NULL |
|177 |Les chiens restent plus populaires que les chats |sondages,animaux| 

### <a name="e-generate-list-of-emails-per-towns"></a>E. Générer une liste d’adresses e-mail par ville

La requête suivante recherche les adresses e-mail des employés et les regroupe par ville : 
```sql
SELECT town, STRING_AGG (email, ';') AS emails 
FROM dbo.Employee 
GROUP BY town; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|town |emails |
|--- |--- |
|Seattle |syed0@adventure-works.com;catherine0@adventure-works.com;kim2@adventure-works.com |
|LA |sam1@adventure-works.com;hazem0@adventure-works.com |

Les adresses e-mail renvoyées dans la colonne emails peuvent être utilisées directement pour envoyer des e-mails à un groupe de personnes travaillant dans certaines villes particulières. 

### <a name="f-generate-a-sorted-list-of-emails-per-towns"></a>F. Générer une liste ordonnée d’adresses e-mail par ville   
   
Similaire à l’exemple précédent, la requête suivante recherche les adresses e-mail des employés, les regroupe par ville, puis trie les adresses e-mail par ordre alphabétique :   
```sql
SELECT town, 
    STRING_AGG (email, ';') WITHIN GROUP (ORDER BY email ASC) AS emails 
FROM dbo.Employee 
GROUP BY town; 
```
   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|town |emails |
|--- |--- |
|Seattle |catherine0@adventure-works.com;kim2@adventure-works.com;syed0@adventure-works.com |
|LA |hazem0@adventure-works.com;sam1@adventure-works.com |


## <a name="see-also"></a> Voir aussi  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  

