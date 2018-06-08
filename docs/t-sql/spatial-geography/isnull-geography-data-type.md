---
title: IsNull (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|spatial-geography
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- IsNull (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull method
ms.assetid: c031074f-bfda-4584-a3bf-4e7c324f237f
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 438b51547823e190d568681a904c2fbfcc1e8a74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="isnull-geography-data-type"></a>IsNull (type de données geography)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Propriété qui spécifie si l’instance **geography** a une valeur Null. Retourne 'TRUE' si l'instance est Null ; retourne 0 si l'instance n'est pas Null.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.IsNull  
```  
  
## <a name="return-types"></a>Types de retour  
 Type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **bit**  
  
 Type CLR : **SqlBoolean**  
  
## <a name="remarks"></a>Notes   
 `IsNull` permet de tester si une instance **geography** a une valeur Null. Les résultats peuvent prêter à confusion, la méthode retournant 0 si l'instance n'est pas Null, mais Null si l'instance est Null.  
  
 Cette méthode est principalement utilisée par l’infrastructure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il est recommandé d’utiliser le prédicat T-SQL IS NULL pour tester si une instance **geography** a une valeur Null. Pour plus d’informations sur le prédicat T-SQL IS NULL, consultez [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
