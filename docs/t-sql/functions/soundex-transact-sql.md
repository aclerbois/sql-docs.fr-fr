---
title: SOUNDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SOUNDEX
- SOUNDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SOUNDEX function
- comparing string data
- strings [SQL Server], similarity
- strings [SQL Server], comparing
- SOUNDEX values
ms.assetid: 8f1ed34e-8467-4512-a211-e0f43dee6584
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1256b573bd1fd2c1bee7de27e0a25a6dae3510a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="soundex-transact-sql"></a>SOUNDEX (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne un code à quatre caractères (SOUNDEX) pour évaluer la similitude entre deux chaînes.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SOUNDEX ( character_expression )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression_caractère*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) alphanumérique de données caractères. *character_expression* peut être une constante, une variable ou une colonne.  
  
## <a name="return-types"></a>Types de retour  
 **varchar**  
  
## <a name="remarks"></a>Notes   
 SOUNDEX convertit une chaîne alphanumérique en un code à quatre caractères basé sur la façon dont la chaîne est énoncée. Le premier caractère du code est le premier caractère de *character_expression*, converti en majuscules. Les deuxième et quatrième caractères de ce code sont des chiffres qui représentent des lettres dans l'expression. Les lettres A, E, I, O, U, H, W et Y sont ignorées sauf s'il s'agit de la première lettre de la chaîne. Les zéros sont ajoutés à la fin si nécessaire pour produire un code à quatre caractères. Pour plus d’informations sur le code SOUNDEX, consultez le [Système d’indexation Soundex](https://www.archives.gov/research/census/soundex.html).  
  
 Les codes SOUNDEX de chaînes individuelles peuvent être comparés pour voir comment sont énoncées les chaînes similaires. La fonction DIFFERENCE effectue un SOUNDEX sur deux chaînes, et retourne un entier qui représente le degré de similitude des codes SOUNDEX pour ces chaînes.  
  
 SOUNDEX respecte le classement. Il est possible d'imbriquer des fonctions de chaîne.  
  
## <a name="soundex-compatibility"></a>Compatibilité SOUNDEX  
 Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la fonction SOUNDEX appliquait un sous-ensemble des règles SOUNDEX. Lorsque le niveau de compatibilité de la base de données est 110 ou supérieur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique un ensemble de règles plus complet.  
  
 Après la mise à niveau vers le niveau de compatibilité 110 ou supérieur, vous pouvez être amené à reconstruire les index, les segments de mémoire ou les contraintes CHECK qui utilisent la fonction SOUNDEX.  
  
-   Un segment de mémoire qui contient une colonne calculée persistante définie avec SOUNDEX ne peut pas être interrogé tant qu'il n'a pas été reconstruit en exécutant l'instruction `ALTER TABLE <table> REBUILD`.  
  
-   Les contraintes CHECK définies avec SOUNDEX sont désactivées lors de la mise à niveau. Pour activer la contrainte, exécutez l'instruction `ALTER TABLE <table> WITH CHECK CHECK CONSTRAINT ALL`.  
  
-   Les index (y compris les vues indexées) qui contiennent une colonne calculée persistante définie avec SOUNDEX ne peuvent pas être interrogés tant qu'ils n'ont pas été reconstruits en exécutant l'instruction `ALTER INDEX ALL ON <object> REBUILD`.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre la fonction SOUNDEX et la fonction associée DIFFERENCE. Dans le premier exemple, les valeurs standard `SOUNDEX` sont retournées pour toutes les consonnes. La fonction `SOUNDEX` exécutée sur `Smith` et `Smythe` retourne le même résultat SOUNDEX parce que toutes les voyelles, la lettre « `y` », les lettres doubles et la lettre « `h` » ne sont pas comprises.  
  
```  
-- Using SOUNDEX  
SELECT SOUNDEX ('Smith'), SOUNDEX ('Smythe');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Valide pour un classement Latin1_General.  
  
```  
  
----- -----   
S530  S530    
  
(1 row(s) affected)  
```  
  
 La fonction `DIFFERENCE` calcule la différence des résultats du modèle `SOUNDEX`. L'exemple suivant illustre deux chaînes de caractères qui ne diffèrent que par les voyelles. La différence retournée est `4`, soit la plus petite différence possible.  
  
```  
-- Using DIFFERENCE  
SELECT DIFFERENCE('Smithers', 'Smythers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Valide pour un classement Latin1_General.  
  
```  
-----------   
4             
  
(1 row(s) affected)  
```  
  
 Dans l'exemple suivant, les chaînes de caractères diffèrent par leurs consonnes ; la valeur retournée est donc `2`, soit la différence la plus importante.  
  
```  
SELECT DIFFERENCE('Anothers', 'Brothers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Valide pour un classement Latin1_General.  
  
```  
-----------   
2             
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DIFFERENCE &#40;Transact-SQL&#41;](../../t-sql/functions/difference-transact-sql.md)   
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
 [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
  

