---
title: SET DATEFORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DATEFORMAT
- SET DATEFORMAT
- SET_DATEFORMAT_TSQL
- DATEFORMAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], formats
- dates [SQL Server], ordering date parts
- SET DATEFORMAT option [SQL Server]
- DATEFORMAT option [SQL Server]
- date and time [SQL Server], SET DATEFORMAT
- options [SQL Server], date
- date and time [SQL Server], DATEFORMAT
- dateparts [SQL Server], dateformat
ms.assetid: da217878-7ec4-477e-aa13-604073c948f8
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ee641bedeaf559b366c9dea38498b5648dadfeac
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-dateformat-transact-sql"></a>SET DATEFORMAT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Définit le classement des parties de date mois, jour et année pour interpréter les chaînes de caractères **date**, **smalldatetime**, **datetime**, **datetime2** et **datetimeoffset**.  
  
 Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SET DATEFORMAT { format | @format_var }   
```  
  
## <a name="arguments"></a>Arguments  
 *format* | **@***format_var*  
 Ordre des parties de la date. Les paramètres valides sont **mdy**, **dmy**, **ymd**, **ydm**, **myd** et **dym**. Il peut s'agir du format Unicode ou d'un jeu de caractères codés sur deux octets (DBCS) converti en Unicode. La valeur par défaut pour l’anglais des États-Unis est **mdy**. Pour connaître le paramètre DATEFORMAT par défaut de toutes les langues prises en charge, consultez [sp_helplanguage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md).  
  
## <a name="remarks"></a>Notes   
 Le paramètre DATEFORMAT **ydm** n’est pas pris en charge pour les types de données **date**, **datetime2** et **datetimeoffset**.  
  
 L’effet du paramètre DATEFORMAT sur l’interprétation des chaînes de caractères peut être différent pour les valeurs **datetime** et **smalldatetime** et pour les valeurs **date**, **datetime2** et **datetimeoffset**, en fonction du format de chaîne. Ce paramètre affecte l'interprétation de chaînes de caractères lorsqu'elles sont converties en valeurs de date pour le stockage dans la base de données. Il n'affecte pas l'affichage des valeurs du type de données date qui sont stockées dans la base de données ni le format de stockage.  
  
 Certains formats de chaînes de caractères, par exemple ISO 8601, sont interprétés indépendamment du paramètre DATEFORMAT.  
  
 L'option SET DATEFORMAT est appliquée lors de l'exécution, et non pas lors de l'analyse.  
  
 SET DATEFORMAT remplace le paramètre de format de date implicite de [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise différentes chaînes de date comme entrées dans les sessions avec le même paramètre `DATEFORMAT`.  
  
```  
-- Set date format to day/month/year.  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar datetime2 = '31/12/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: 2008-12-31 09:01:01.123  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar datetime2 = '12/31/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: Msg 241: Conversion failed when converting date and/or time -- from character string.  
  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

