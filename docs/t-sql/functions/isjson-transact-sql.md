---
title: ISJSON (Transact-SQL) | Microsoft Docs
ms.custom: 
ms.date: 07/17/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database
ms.service: 
ms.component: t-sql|functions
ms.reviewer: 
ms.suite: sql
ms.technology:
- dbe-json
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- ISJSON
- ISJSON_TSQL
helpviewer_keywords:
- ISJSON function
- JSON, validating
ms.assetid: c836f3d3-3e17-44ae-92bf-f341918896c3
caps.latest.revision: 
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: On Demand
ms.openlocfilehash: 0ecbcc4ced2b9503ec9161f7aa93a1a5266960ef
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2017
---
# <a name="isjson-transact-sql"></a>ISJSON (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Teste si une chaîne contient des données JSON valides.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
ISJSON ( expression )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Chaîne à tester.  
  
## <a name="return-value"></a>Valeur retournée  
 Renvoie 1 si la chaîne contient des données JSON valides ; sinon, renvoie 0. Renvoie Null si *expression* est Null.  
  
 Ne renvoie pas d’erreurs.  
  
## <a name="remarks"></a>Notes   
 **ISJSON** ne vérifie pas l’unicité des clés au même niveau.  
  
## <a name="examples"></a>Exemples  
  
### <a name="example-1"></a>Exemple 1  
L’exemple suivant exécute un bloc d’instructions de manière conditionnelle si la valeur de paramètre `@param` contient des données JSON valides.  
  
```sql  
DECLARE @param <data type>
SET @param = <value>

IF (ISJSON(@param) > 0)  
BEGIN  
     -- Do something with the valid JSON value of @param.  
END
 
```  
  
### <a name="example-2"></a>Exemple 2  
L’exemple suivant retourne des lignes dans lesquelles la colonne `json_col` contient un JSON valide.  
  
```sql  
SELECT id, json_col
FROM tab1
WHERE ISJSON(json_col) > 0 
```  
  
## <a name="see-also"></a> Voir aussi  
 [Données JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
  
