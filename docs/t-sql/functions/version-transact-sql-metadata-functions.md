---
title: VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 95a79b33-98f2-4929-a1a5-93b522a9e152
caps.latest.revision: 7
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 369be3f7377efb1211023e199809ccf1780e1880
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="version---transact-sql-metadata-functions"></a>Version - Fonctions de métadonnées Transact SQL
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

 Retourne la version de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou de [!INCLUDE[ssPDW_md](../../includes/sspdw-md.md)] actuellement exécutée sur l’appliance.  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Azure SQL Data Warehouse and Parallel Data Warehouse  
VERSION ( )  
```  
  
## <a name="arguments"></a>Arguments  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
Vous devez spécifier un nom de table dans une clause [FROM](../../t-sql/queries/from-transact-sql.md) pour que cette fonction retourne des résultats. Une ligne de résultats est retournée pour chaque ligne du jeu de résultats de la requête. Utilisez [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md) pour limiter le nombre de lignes retournées.  
  
## <a name="examples"></a>Exemples  
L’exemple suivant retourne le numéro de version.  
  
```  
SELECT VERSION();  
```  
  
## <a name="see-also"></a> Voir aussi 
[SESSION_ID (Transact-SQL)](../../t-sql/functions/session-id-transact-sql.md)  
[DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)  
  
  
