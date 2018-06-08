---
title: Priorité des types de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 7/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- precedence [SQL Server]
- data types [SQL Server], converting
- data types [SQL Server], precedence
- converting data types [SQL Server], precedence
- precedence [SQL Server], data types
ms.assetid: f4c804ab-ed3f-43b1-a024-c9ac6944b66b
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 068154021ceb7566142cc320fadcc89e9cca74aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-precedence-transact-sql"></a>Priorité des types de données (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

Lorsqu'un opérateur combine deux expressions de type de données différents, les règles de priorité des types de données spécifient que le type ayant la priorité plus faible est converti dans le type ayant la priorité plus élevée. Si la conversion n'est pas prise en charge en tant que conversion implicite, une erreur est renvoyée. Lorsque deux opérandes ont le même type de données, le résultat de l'opération a également ce type de données.
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise l'ordre de priorité suivant pour les types de données :
  
1.  types de données définis par l'utilisateur (plus haut niveau de priorité)  
1.  **sql_variant**  
1.  **xml**  
1.  **datetimeoffset**  
1.  **datetime2**  
1.  **datetime**  
1.  **smalldatetime**  
1.  **date**  
1. **time**  
1. **float**  
1. **real**  
1. **decimal**  
1. **money**  
1. **smallmoney**  
1. **bigint**  
1. **Int**  
1. **smallint**  
1. **tinyint**  
1. **bit**  
1. **ntext**  
1. **texte**  
1. **image**  
1. **timestamp**  
1. **uniqueidentifier**  
1. **nvarchar** (dont **nvarchar(max)** )  
1. **nchar**  
1. **varchar** (dont **varchar(max)** )  
1. **char**  
1. **varbinary** (dont **varbinary(max)** )  
1. **binary** (minimum)  
  
## <a name="see-also"></a>Voir aussi
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
