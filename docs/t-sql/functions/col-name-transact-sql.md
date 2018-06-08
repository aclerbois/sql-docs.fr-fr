---
title: COL_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- COL_NAME
- COL_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- COL_NAME function
- column names [SQL Server]
- names [SQL Server], columns
ms.assetid: 214144ab-f2bc-4052-83cf-caf0a85c4cc6
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c79414874a166ad005a2caf9e65ca0a051a732de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="colname-transact-sql"></a>COL_NAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Cette fonction retourne le nom d’une colonne de table à partir des valeurs du numéro d’identification de table et du numéro d’identification de colonne de cette colonne de table.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
COL_NAME ( table_id , column_id )  
```  
  
## <a name="arguments"></a>Arguments  
*table_id*  
Numéro d’identification de la table contenant cette colonne. L’argument *table_id* a un type de données **int**.
  
*column_id*  
Numéro d’identification de la colonne. L’argument *column_id* a un type de données **int**.
  
## <a name="return-types"></a>Types de retour
**sysname**
  
## <a name="exceptions"></a>Exceptions  
Retourne NULL en cas d’erreur ou si un appelant ne dispose pas de l’autorisation appropriée pour voir l’objet.
  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un utilisateur peut seulement voir les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d’un droit d’accès. Cela signifie que les fonctions intégrées générant des métadonnées, comme `COL_NAME`, peuvent retourner NULL si l’utilisateur ne dispose pas des autorisations appropriées sur l’objet. Pour plus d’informations, consultez [Configuration de la visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Notes   
Les paramètres *table_id* et *column_id* génèrent ensemble une chaîne de nom de colonne.
  
Pour plus d’informations sur l’obtention des numéros d’identification de table et de colonne, consultez [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md).
  
## <a name="examples"></a>Exemples  
L’exemple suivant retourne le nom de la première colonne d’un exemple de table `Employee`.
  
```sql
-- Uses AdventureWorks  
  
SELECT COL_NAME(OBJECT_ID('dbo.FactResellerSales'), 1) AS FirstColumnName,  
COL_NAME(OBJECT_ID('dbo.FactResellerSales'), 2) AS SecondColumnName;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
ColumnName          
------------   
BusinessEntityID  
```  
  
## <a name="see-also"></a>Voir aussi
[Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)  
[COL_LENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/col-length-transact-sql.md)
  
  

