---
title: JSON_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- dbe-json
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- JSON_VALUE
- JSON_VALUE_TSQL
helpviewer_keywords:
- JSON_VALUE function
- JSON, extracting
- JSON, querying
ms.assetid: cd016e14-11eb-4eaf-bf05-c7cfcc820a10
caps.latest.revision: 18
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 9bbd4dd23c4c536fa70f679ceabf6d96ddde121c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jsonvalue-transact-sql"></a>JSON_VALUE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Extrait une valeur scalaire d’une chaîne JSON.  
  
 Pour extraire un objet ou un tableau d’une chaîne JSON à la place d’une valeur scalaire, consultez [JSON_QUERY &#40;Transact-SQL&#41;](../../t-sql/functions/json-query-transact-sql.md). Pour obtenir des informations sur les différences entre **JSON_VALUE** et **JSON_QUERY**, consultez [Comparer JSON_VALUE et JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
JSON_VALUE ( expression , path )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Expression. En règle générale, nom d’une variable ou d’une colonne qui contient du texte JSON.  
 
 Si **JSON_VALUE** trouve des données JSON non valides dans *expression* avant de rechercher la valeur identifiée par *path*, la fonction retourne une erreur. Si **JSON_VALUE* ne trouve pas la valeur identifiée par *path*, la fonction analyse l’intégralité du texte et retourne une erreur si elle trouve des données JSON non valides, n’importe où dans *expression*.
  
 *path*  
 Chemin JSON qui spécifie la propriété à extraire. Pour plus d’informations, consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
 
Dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et dans [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], vous pouvez fournir une variable comme valeur de *path*.
  
 Si le format de *path* n’est pas valide, **JSON_VALUE** retourne une erreur.  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne une valeur de texte unique de type nvarchar(4000). Le classement de la valeur renvoyée est le même que le classement de l’expression d’entrée.  
  
 Si la valeur est supérieure à 4 000 caractères :  
  
-   En mode lax, **JSON_VALUE** retourne la valeur NULL.  
  
-   En mode strict, **JSON_VALUE** retourne une erreur.  
  
 Si vous devez renvoyer des valeurs scalaires de plus de 4 000 caractères, utilisez **OPENJSON** au lieu de **JSON_VALUE**. Pour plus d’informations, consultez [OPENJSON &#40;Transact-SQL&#41;](../../t-sql/functions/openjson-transact-sql.md).  
  
## <a name="remarks"></a>Notes 

### <a name="lax-mode-and-strict-mode"></a>Mode lax et mode strict

 Considérons le texte JSON suivant :  
  
```json  
DECLARE @jsonInfo NVARCHAR(MAX)

SET @jsonInfo=N'{  
     "info":{    
       "type":1,  
       "address":{    
         "town":"Bristol",  
         "county":"Avon",  
         "country":"England"  
       },  
       "tags":["Sport", "Water polo"]  
    },  
    "type":"Basic"  
 }'  
```  
  
 Le tableau suivant compare le comportement de **JSON_VALUE** en mode lax et en mode strict. Pour plus d’informations sur la spécification du mode de chemin facultatif (lax ou strict), consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Chemin d'accès|Valeur renvoyée en mode lax|Valeur renvoyée en mode strict|En savoir plus|  
|----------|------------------------------|---------------------------------|---------------|  
|$|NULL|Error|Valeur non scalaire.<br /><br /> Utilisez **JSON_QUERY** à la place.|  
|$.info.type|N'1'|N'1'|N/a|  
|$.info.address.town|N'Bristol'|N'Bristol'|N/a|  
|$.info."address"|NULL|Error|Valeur non scalaire.<br /><br /> Utilisez **JSON_QUERY** à la place.|  
|$.info.tags|NULL|Error|Valeur non scalaire.<br /><br /> Utilisez **JSON_QUERY** à la place.|  
|$.info.type[0]|NULL|Error|Pas un tableau.|  
|$.info.none|NULL|Error|La propriété n’existe pas.|  
  
## <a name="examples"></a>Exemples  
  
### <a name="example-1"></a>Exemple 1  
 L’exemple suivant utilise les valeurs des propriétés JSON `town` et `state` dans les résultats de la requête. Étant donné que **JSON_VALUE** conserve le classement de la source, l’ordre de tri des résultats dépend du classement de la colonne `jsonInfo`. 

> [!NOTE]
> (Cet exemple suppose qu’une table nommée `Person.Person` contient une colonne `jsonInfo` de texte JSON, et que cette colonne a la structure présentée précédemment dans la description du mode lax et du mode strict. Dans l’exemple de base de données AdventureWorks, la table `Person` ne contient en fait pas de colonne `jsonInfo`.)
  
```sql  
SELECT FirstName, LastName,
 JSON_VALUE(jsonInfo,'$.info.address[0].town') AS Town
FROM Person.Person
WHERE JSON_VALUE(jsonInfo,'$.info.address[0].state') LIKE 'US%'
ORDER BY JSON_VALUE(jsonInfo,'$.info.address[0].town')
```  
  
### <a name="example-2"></a>Exemple 2  
 L’exemple suivant extrait la valeur de la propriété JSON dans une variable locale `town`.  
  
```sql  
DECLARE @jsonInfo NVARCHAR(MAX)
DECLARE @town NVARCHAR(32)

SET @jsonInfo=N'<array of address info>'

SET @town=JSON_VALUE(@jsonInfo,'$.info.address.town')
```  
  
### <a name="example-3"></a>Exemple 3  
 L’exemple suivant crée des colonnes calculées basées sur les valeurs des propriétés JSON.  
  
```sql  
CREATE TABLE dbo.Store
 (
  StoreID INT IDENTITY(1,1) NOT NULL,
  Address VARCHAR(500),
  jsonContent NVARCHAR(8000),
  Longitude AS JSON_VALUE(jsonContent, '$.address[0].longitude'),
  Latitude AS JSON_VALUE(jsonContent, '$.address[0].latitude')
 )
```  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
 [Données JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
  
