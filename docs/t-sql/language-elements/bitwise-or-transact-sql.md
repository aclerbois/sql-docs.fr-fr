---
title: '| (Opérateur OR au niveau du bit) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '|'
- '|_TSQL'
- Bitwise OR
- bitwise
- OR
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator
- bitwise OR (|)
- '| (bitwise OR operator)'
ms.assetid: 86a3b87f-9688-4eaf-a552-29f1b01d880a
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ff195b0248ac0f47421325fd23dbebacbcd68327
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="-bitwise-or-transact-sql"></a>| (OR au niveau du bit) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Exécute une opération logique OR au niveau du bit entre deux valeurs entières spécifiées, traduites en expressions binaires dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```   
expression | expression  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de la catégorie de type de données entier ou des types de données **bit**, **binary** ou **varbinary**. L’*expression* est traitée comme un nombre binaire pour l’opération au niveau du bit.  
  
> [!NOTE]  
>  Une seule *expression* peut être de type **binary** ou **varbinary** dans une opération au niveau du bit.  
  
## <a name="result-types"></a>Types des résultats  
 Retourne un type **int** si les valeurs d’entrée sont **int**, un type **smallint** si les valeurs d’entrée sont **smallint** ou un type **tinyint** si les valeurs d’entrée sont **tinyint**.  
  
## <a name="remarks"></a>Notes   
 L'opérateur | au niveau du bit exécute un OR logique au niveau du bit entre les deux expressions, évaluant chaque bit se correspondant dans les deux expressions. Dans le résultat, les bits prennent la valeur 1 si l'un des bits ou les deux (pour la position en cours d'évaluation) des expressions d'entrées ont la valeur 1 ; si aucun des deux bits des deux expressions d'entrée ne vaut 1, le bit du résultat est mis à 0.  
  
 Si les expressions de droite et de gauche ont des types de données integer (entier) différents (par exemple, l’*expression* de gauche est de type **smallint** et l’*expression* de droite est de type **int**), l’argument du type de données le plus petit est converti dans le type de données plus grand. Dans cet exemple, l’*expression *smallint**** est convertie en **int**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une table avec des types de données **int** pour afficher les valeurs d’origine et présenter la table sur une seule ligne.  
  
```sql  
CREATE TABLE bitwise  
(   
 a_int_value int NOT NULL,  
b_int_value int NOT NULL  
);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 Cette requête exécute l’opération OR au niveau du bit sur les colonnes **a_int_value** et **b_int_value**.  
  
```  
SELECT a_int_value | b_int_value  
FROM bitwise;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-----------   
235           
  
(1 row(s) affected)  
```  
  
 La représentation binaire de 170 (**a_int_value** ou `A` ci-dessous) est `0000 0000 1010 1010`. La représentation binaire de 75 (**b_int_value** ou `B` ci-dessous) est `0000 0000 0100 1011`. L'exécution de l'opération OR au niveau du bit entre ces deux valeurs produit le résultat binaire `0000 0000 1110 1011`, c'est-à-dire 235 en décimal.  
  
```  
(A | B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 1110 1011  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs au niveau du bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&#124;= &#40;Affectation après OR au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


