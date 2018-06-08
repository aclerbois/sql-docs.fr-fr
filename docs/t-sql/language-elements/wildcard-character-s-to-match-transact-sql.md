---
title: '[^] (Caractère générique - Caractères à rechercher) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- Match
- wildcard
- '[ ]'
- '[_]_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[ ] (wildcard - character(s) to match)'
ms.assetid: 57817576-0bf1-49ed-b05d-fac27e8fed7a
caps.latest.revision: 32
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6a6ef55462237d322dd8fc231dded2546053927c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="--wildcard---characters-to-match-transact-sql"></a>\[ \] (Caractère générique - Caractères à rechercher) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all_md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Recherche la correspondance de chaque caractère, dans la plage ou l’ensemble spécifié entre crochets `[ ]`. Ces caractères génériques peuvent être utilisés dans des comparaisons de chaînes qui impliquent des critères spéciaux tels que `LIKE` et `PATINDEX`.  
  
## <a name="examples"></a>Exemples  
### <a name="a-simple-example"></a>A. Exemple simple   
L’exemple suivant retourne les noms qui commencent par la lettre `m`. `[n-z]` spécifie que la deuxième lettre doit être comprise dans la plage allant de `n` à `z`. Le caractère générique de pourcentage `%` autorise tout caractère ou aucun caractère à partir du troisième caractère. Les bases de données `model` et `msdb` répondent à ces critères. La base de données `master` n’y répond pas et est donc exclue du jeu de résultats.
 
```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm[n-z]%';
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
name
-----
model
msdb
```   
 Vous avez peut-être d’autres bases de données installées qui satisfont ces critères.


### <a name="b-more-complex-example"></a>B. Exemple plus complexe   
 L'exemple suivant utilise l'opérateur [] pour rechercher l'ID et le nom de tous les employés de [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] dont le code postal se compose de quatre chiffres.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.BusinessEntityID, p.FirstName, p.LastName, a.PostalCode  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
INNER JOIN Person.Address AS a ON a.AddressID = ea.AddressID  
WHERE a.PostalCode LIKE '[0-9][0-9][0-9][0-9]';  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
EmployeeID      FirstName      LastName      PostalCode  
----------      ---------      ---------     ----------  
290             Lynn           Tsoflias      3000  
```  



  
## <a name="see-also"></a> Voir aussi  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% &#40;Caractère générique - Caractères à rechercher &#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
 [&#91; ^&#93; &#40;Caractère générique - Caractères à exclure&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [\_ &#40;Caractère générique - Recherche d’un seul caractère&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
