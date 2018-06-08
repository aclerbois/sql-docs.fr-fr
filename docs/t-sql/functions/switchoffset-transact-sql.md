---
title: SWITCHOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/02/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SWITCHTZ
- SWITCHTZ_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- functions [SQL Server], time
- functions [SQL Server], date and time
- SWITCHOFFSET function [SQL Server]
- time [SQL Server], functions
- date and time [SQL Server], SWITCHOFFSET
- time zones [SQL Server]
ms.assetid: 32a48e36-0aa4-4260-9fe9-cae9197d16c5
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 28c1f0a91b81a3b7e3bf5a04b562cd530cef1740
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="switchoffset-transact-sql"></a>SWITCHOFFSET (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une valeur **datetimeoffset** du décalage de fuseau horaire stocké qui est remplacée par un nouveau décalage de fuseau horaire spécifié.  
  
 Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SWITCHOFFSET ( DATETIMEOFFSET, time_zone )   
```  
  
## <a name="arguments"></a>Arguments  
 *DATETIMEOFFSET*  
 Expression qui peut être résolue en une valeur **datetimeoffset(n)**.  
  
 *time_zone*  
 Chaîne de caractères au format [+|-]TZH:TZM ou entier signé (de minutes) qui représente le décalage de fuseau horaire, et qui est supposée être réglée et prendre en charge l'heure d'été.  
  
## <a name="return-type"></a>Type de retour  
 **datetimeoffset** avec la précision fractionnelle de l’argument *DATETIMEOFFSET*.  
  
## <a name="remarks"></a>Notes   
 Utilisez SWITCHOFFSET pour sélectionner une valeur **datetimeoffset** dans un décalage de fuseau horaire qui est différent du décalage de fuseau horaire stocké à l’origine. SWITCHOFFSET ne met pas à jour la valeur *time_zone* stockée.  
  
 La fonction SWITCHOFFSET peut être utilisée pour mettre à jour une colonne **datetimeoffset**.  
  
 L'utilisation de SWITCHOFFSET avec la fonction GETDATE() peut entraîner un ralentissement de l'exécution de la requête. Cela est dû au fait que l'optimiseur de requête n'est pas en mesure d'obtenir les estimations de cardinalité exactes pour la valeur datetime. Pour résoudre ce problème, utilisez l'indicateur de requête OPTION (RECOMPILE) pour forcer l'optimiseur de requête à recompiler un plan de requête lors de la prochaine exécution de cette même requête. L'optimiseur dispose alors d'estimations de cardinalité précises et produit un plan de requête plus efficace. Pour plus d’informations sur l’indicateur de requête RECOMPILE, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
```  
DECLARE @dt datetimeoffset = switchoffset (CONVERT(datetimeoffset, GETDATE()), '-04:00');   
SELECT * FROM t    
WHERE c1 > @dt OPTION (RECOMPILE);  
  
```  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `SWITCHOFFSET` pour afficher un décalage de fuseau horaire différent de la valeur stockée dans la base de données.  
  
```  
CREATE TABLE dbo.test   
    (  
    ColDatetimeoffset datetimeoffset  
    );  
GO  
INSERT INTO dbo.test   
VALUES ('1998-09-20 7:45:50.71345 -5:00');  
GO  
SELECT SWITCHOFFSET (ColDatetimeoffset, '-08:00')   
FROM dbo.test;  
GO  
--Returns: 1998-09-20 04:45:50.7134500 -08:00  
SELECT ColDatetimeoffset  
FROM dbo.test;  
--Returns: 1998-09-20 07:45:50.7134500 -05:00  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


