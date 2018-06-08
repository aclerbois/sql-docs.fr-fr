---
title: Fonctions déterministes et non déterministes | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: udf
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-udf
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- built-in functions [SQL Server]
- nondeterministic functions
- extended stored procedures [SQL Server], functions that call
- deterministic functions
- user-defined functions [SQL Server], deterministic
ms.assetid: 2f3ce5f5-c81c-4470-8141-8144d4f218dd
caps.latest.revision: 43
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 533a304f007b171a59fdd17ccd92c10662f69385
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deterministic-and-nondeterministic-functions"></a>Fonctions déterministes et non déterministes
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Les fonctions déterministes retournent toujours le même résultat quel que soit le moment auquel elles sont appelées avec un ensemble spécifique de valeurs d'entrée et sur la base du même état de la base de données. Les fonctions non déterministes peuvent retourner différents résultats chaque fois qu'elles sont appelées avec un ensemble spécifique de valeurs d'entrée, même si l'état de la base de données à laquelle elles accèdent demeure inchangé. Par exemple, la fonction AVG retourne toujours le même résultat pour les conditions ci-dessus, mais la fonction GETDATE, qui retourne la valeur datetime actuelle, retourne toujours un résultat différent.  
  
 Plusieurs propriétés de fonctions définies par l’utilisateur déterminent la possibilité pour le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’indexer les résultats d’une fonction, par le biais d’index sur des colonnes calculées qui appellent la fonction, ou de vues indexées qui y font référence. Le déterminisme d'une fonction est l'une de ces propriétés. Par exemple, un index cluster ne peut pas être créé sur une vue si celle-ci fait référence à une fonction non déterministe. Pour plus d’informations sur les propriétés des fonctions, dont le déterminisme, consultez [Fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/user-defined-functions.md).  
  
 Cette rubrique identifie le déterminisme des fonctions système intégrées, ainsi que l'effet sur la propriété déterministe des fonctions définies par l'utilisateur lorsqu'elle contient un appel à des procédures stockées étendues.  
  
## <a name="built-in-function-determinism"></a>Déterminisme des fonctions intégrées  
 Vous ne pouvez pas influer sur le déterminisme d'une fonction intégrée. Chaque fonction intégrée est déterministe ou non déterministe en fonction de son implémentation par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, spécifier une clause ORDER BY dans une requête ne modifie pas le déterminisme d'une fonction utilisée dans cette requête.  
  
 Toutes les fonctions de chaîne intégrées sont déterministes. Pour obtenir la liste de ces fonctions, consultez [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md).  
  
 Les fonctions intégrées suivantes, qui ne sont pas des fonctions de chaîne, sont toujours déterministes :  
  
||||  
|-|-|-|  
|ABS|DATEDIFF|POWER|  
|ACOS|DAY|RADIANS|  
|ASIN|DEGREES|ROUND|  
|ATAN|EXP|SIGN|  
|ATN2|FLOOR|SIN|  
|CEILING|ISNULL|SQUARE|  
|COALESCE|ISNUMERIC|SQRT|  
|COS|LOG|TAN|  
|COT|LOG10|YEAR|  
|DATALENGTH|MONTH||  
|DATEADD|NULLIF||  
  
 Les fonctions suivantes ne sont pas toujours déterministes mais peuvent être utilisées dans les vues indexées ou les index de colonnes calculées si elles sont spécifiées de manière déterministe :  
  
|Fonction|Commentaires|  
|--------------|--------------|  
|Toutes les fonctions d'agrégation|Toutes les fonctions d'agrégation sont déterministes, sauf si elles sont spécifiées avec les clauses OVER et ORDER BY. Pour obtenir la liste de ces fonctions, consultez [Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md).|  
|CAST|Déterministe sauf si utilisée avec **datetime**, **smalldatetime**ou **sql_variant**.|  
|CONVERT|Déterministe sauf dans l'un des cas suivants :<br /><br /> <br /><br /> Le type de source est **sql_variant**.<br /><br /> Le type de cible est **sql_variant** et son type de source est non déterministe.<br /><br /> Le type de source ou de cible est **datetime** ou **smalldatetime**, l’autre type de source ou de cible est une chaîne de caractères et un style non déterministe est spécifié. Pour être déterministe, le paramètre de style doit être une constante. De plus, les styles inférieurs ou égaux à 100 sont non-déterministes, à l'exception des styles 20 et 21. Les styles supérieurs à 100 sont déterministes, à l'exception des styles 106, 107, 109 et 113.|  
|CHECKSUM|Déterministe, sauf CHECKSUM(*).|  
|ISDATE|Déterministe uniquement si utilisé avec la fonction CONVERT, si le paramètre de style CONVERT est spécifié et si le style est différent de 0, 100, 9 ou 109.|  
|RAND|RAND est déterministe uniquement quand un paramètre *seed* est spécifié.|  
  
 Toutes les fonctions relatives à la configuration, aux curseurs, aux métadonnées, à la sécurité et aux statistiques système sont non déterministes. Pour obtenir la liste de ces fonctions, consultez [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md), [Fonctions de curseur &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md), [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md), [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md) et [Fonctions statistiques du système &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md).  
  
 Les fonctions intégrées suivantes, qui appartiennent à d'autres catégories, sont toujours non déterministes.  
  
|||  
|-|-|  
|@@CONNECTIONS|GETDATE|  
|@@CPU_BUSY|GETUTCDATE|  
|@@DBTS|GET_TRANSMISSION_STATUS|  
|@@IDLE|LAG|  
|@@IO_BUSY|LAST_VALUE|  
|@@MAX_CONNECTIONS|LEAD|  
|@@PACK_RECEIVED|MIN_ACTIVE_ROWVERSION|  
|@@PACK_SENT|NEWID|  
|@@PACKET_ERRORS|NEWSEQUENTIALID|  
|@@TIMETICKS|NEXT VALUE FOR|  
|@@TOTAL_ERRORS|NTILE|  
|@@TOTAL_READ|PARSENAME|  
|@@TOTAL_WRITE|PERCENTILE_CONT|  
|AT TIME ZONE|PERCENTILE_DISC|
|CUME_DIST|PERCENT_RANK|  
|CURRENT_TIMESTAMP|RAND|  
|DENSE_RANK|RANK|  
|FIRST_VALUE|ROW_NUMBER|   
|FORMAT|TEXTPTR|  
  
## <a name="calling-extended-stored-procedures-from-functions"></a>Appel de procédures stockées étendues à partir de fonctions  
 Les fonctions qui appellent des procédures stockées étendues sont non déterministes car les procédures stockées étendues peuvent provoquer des effets secondaires sur la base de données. Ces effets secondaires sont des modifications de l'état global de la base de données, comme la mise à jour d'une table ou d'une ressource externe, telle qu'un fichier ou le réseau (par exemple la modification d'un fichier ou l'envoi d'un courrier électronique). N’attendez pas de jeu de résultats cohérent lors de l’exécution d’une procédure stockée étendue à partir d’une fonction définie par l’utilisateur. Les fonctions définies par l'utilisateur qui créent des effets secondaires sur la base de données ne sont pas recommandées.  
  
 Une procédure stockée étendue appelée depuis l'intérieur d'une fonction ne peut pas retourner de jeux de résultats au client. Toute API Open Data Services qui retourne des jeux de résultats au client a un code de retour FAIL.  
  
 La procédure stockée étendue ne peut pas se reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cependant, la procédure ne peut pas joindre la même transaction que la fonction originale ayant appelé la procédure stockée étendue.  
  
 Comme pour les appels à partir d'une procédure stockée ou de traitement par lot, la procédure stockée étendue est exécutée dans le contexte du compte de sécurité [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel fonctionne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le propriétaire de la procédure stockée étendue doit prendre en compte les autorisations de ce contexte de sécurité quand il accorde à d’autres utilisateurs l’autorisation d’exécuter la procédure.  
  
  
