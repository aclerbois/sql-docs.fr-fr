---
title: CHECKSUM_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CHECKSUM_AGG
- CHECKSUM_AGG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checksum values
- CHECKSUM_AGG function
- groups [SQL Server], checksum values
ms.assetid: cdede70c-4eb5-4c92-98ab-b07787ab7222
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 54e82e66d2f5ba49291cadf0cef198733cb7e0e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="checksumagg-transact-sql"></a>CHECKSUM_AGG (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Cette fonction retourne la somme de contrôle des valeurs dans un groupe. `CHECKSUM_AGG` ignore les valeurs NULL. La [clause OVER](../../t-sql/queries/select-over-clause-transact-sql.md) peut suivre `CHECKSUM_AGG`.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CHECKSUM_AGG ( [ ALL | DISTINCT ] expression )  
```  
  
## <a name="arguments"></a>Arguments  
**ALL**  
Applique la fonction d'agrégation à toutes les valeurs. ALL est l’argument par défaut.
  
DISTINCT  
Spécifie que `CHECKSUM_AGG` retourne la somme de contrôle de valeurs uniques.
  
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) d’entier. `CHECKSUM_AGG` n’autorise pas l’utilisation de fonctions d’agrégation ni de sous-requêtes.
  
## <a name="return-types"></a>Types de retour
Renvoie la somme de contrôle de toutes les valeurs d’*expression* en tant que **int**.
  
## <a name="remarks"></a>Notes   
`CHECKSUM_AGG` peut détecter les modifications effectuées dans une table.
  
L’ordre des lignes dans la table n’influe pas sur le résultat de `CHECKSUM_AGG`. En outre, les fonctions `CHECKSUM_AGG` permettent d’utiliser le mot clé DISTINCT et la clause GROUP BY.
  
Si une valeur de la liste d’expressions change, la liste de valeurs de la somme de contrôle de liste est susceptible de changer aussi. Il existe toutefois une faible probabilité pour que la somme de contrôle calculée ne change pas.
  
`CHECKSUM_AGG` a des fonctionnalités similaires à celles des autres fonctions d’agrégation. Pour plus d’informations, consultez les [fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md).
  
## <a name="examples"></a>Exemples  
Ces exemples utilisent `CHECKSUM_AGG` pour détecter des modifications dans la colonne `Quantity` de la table `ProductInventory` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
--Get the checksum value before the column value is changed.  
SELECT CHECKSUM_AGG(CAST(Quantity AS int))  
FROM Production.ProductInventory;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
------------------------  
262  
```  
  
```sql
UPDATE Production.ProductInventory   
SET Quantity=125  
WHERE Quantity=100;  
GO  
--Get the checksum of the modified column.  
SELECT CHECKSUM_AGG(CAST(Quantity AS int))  
FROM Production.ProductInventory;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
------------------------  
287  
```  
  
## <a name="see-also"></a>Voir aussi
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)  
[OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
