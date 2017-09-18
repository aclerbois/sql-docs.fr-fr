---
title: '@@DBTS (Transact-SQL) | Documents Microsoft'
ms.custom: 
ms.date: 07/30/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- '@@DBTS_TSQL'
- '@@DBTS'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@DBTS function'
- timestamp data type
ms.assetid: 91842ddd-91c0-4445-a03f-116f6bc991d0
caps.latest.revision: 35
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: f044c71018186baaf2bd5c27076b78aae139b34f
ms.contentlocale: fr-fr
ms.lasthandoff: 09/01/2017

---
# <a name="dbts-transact-sql"></a>@@DBTS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Retourne la valeur du type de données actuel **timestamp** pour la base de données active. L'unicité de cette valeur timestamp dans la base de données est garantie.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
@@DBTS  
```  
  
## <a name="return-types"></a>Types de retour
**varbinary**
  
## <a name="remarks"></a>Notes  
@@DBTS renvoie la valeur timestamp utilisée en dernier de la base de données actuelle. Une nouvelle valeur est générée lors de l'insertion ou de la mise à jour d'une ligne comprenant une colonne **timestamp** .
  
Le @@DBTS fonction n’est pas affectée par les modifications apportées dans les niveaux d’isolation des transactions.
  
## <a name="examples"></a>Exemples  
L’exemple suivant renvoie l’actuel **timestamp** à partir de la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de données.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT @@DBTS;  
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions de configuration &#40; Transact-SQL &#41;](../../t-sql/functions/configuration-functions-transact-sql.md)  
[Accès concurrentiel au curseur &#40; ODBC &#41;](../../relational-databases/native-client-odbc-cursors/properties/cursor-concurrency-odbc.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[MIN_ACTIVE_ROWVERSION &#40; Transact-SQL &#41;](../../t-sql/functions/min-active-rowversion-transact-sql.md)
  
  
