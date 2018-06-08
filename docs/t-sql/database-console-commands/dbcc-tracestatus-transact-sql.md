---
title: DBCC TRACESTATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DBCC_TRACESTATUS_TSQL
- DBCC TRACESTATUS
- TRACESTATUS_TSQL
- TRACESTATUS
dev_langs:
- TSQL
helpviewer_keywords:
- global trace flags [SQL Server]
- status information [SQL Server], trace flags
- trace flags [SQL Server], status information
- DBCC TRACESTATUS statement
- session trace flags [SQL Server]
- displaying trace flag status
ms.assetid: 9be51199-78b4-4b87-ae6e-557246b7e29a
caps.latest.revision: 36
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: c4bcd9cb45503a872a752f950b96d2a363f88db0
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-tracestatus-transact-sql"></a>DBCC TRACESTATUS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

Affiche l'état des indicateurs de trace.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC TRACESTATUS ( [ [ trace# [ ,...n ] ] [ , ] [ -1 ] ] )   
[ WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
*trace#*  
Numéro d'indicateur de trace dont l'état est affiché. Si *trace#* et -1 ne sont pas spécifiés, tous les indicateurs de trace activés pour la session sont affichés.
  
*n*  
Espace réservé précisant qu'il est possible de spécifier plusieurs indicateurs de trace.
  
-1  
Affiche l'état des indicateurs de trace activés globalement. Si -1 est spécifié sans *trace#*, tous les indicateurs de trace globaux activés sont affichés.
  
WITH NO_INFOMSGS  
Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.
  
## <a name="result-sets"></a>Jeux de résultats  
Le tableau suivant décrit les informations du jeu de résultats.
  
|Nom de colonne|Description|  
|---|---|
|**TraceFlag**|Nom de l'indicateur de trace|  
|**État**|Indique si l'indicateur de trace est activé ou désactivé, globalement ou pour la session.<br /><br /> 1 = activé<br /><br /> 0 = désactivé|  
|**Global**|Indique si l'indicateur de trace est défini globalement<br /><br /> 1 = True<br /><br /> 0 = False|  
|**Session**|Indique si l'indicateur de trace est défini pour la session<br /><br /> 1 = True<br /><br /> 0 = False|  
  
DBCC TRACESTATUS retourne une colonne correspondant au numéro de l'indicateur de trace et une colonne pour l'état. Celle-ci précise si l'indicateur de trace est activé (1) ou non (0). L’en-tête de colonne du numéro d’indicateur de trace est **Global Trace Flag** ou **Session Trace Flag**, selon que vous vérifiez l’état d’un indicateur de trace global ou de session.
  
## <a name="remarks"></a>Notes   
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il existe deux types d'indicateurs de trace : les indicateurs de trace de session et les indicateurs de trace globaux. Les indicateurs de trace de session sont actifs pour une connexion et visibles uniquement pour celle-ci. Les indicateurs de trace globaux sont définis au niveau du serveur et sont visibles pour chaque connexion sur celui-ci.
  
## <a name="permissions"></a>Autorisations  
Nécessite l'appartenance au rôle **public** .
  
## <a name="examples"></a>Exemples  
L'exemple suivant affiche l'état de tous les indicateurs de trace actuellement activés globalement.
  
```sql  
DBCC TRACESTATUS(-1);  
GO  
```  
  
L'exemple suivant affiche l'état des indicateurs de trace `2528` et `3205`.
  
```sql  
DBCC TRACESTATUS (2528, 3205);  
GO  
```  
  
L'exemple suivant indique si l'indicateur de trace `3205` est activé globalement.
  
```sql  
DBCC TRACESTATUS (3205, -1);  
GO  
```  
  
L'exemple suivant recense tous les indicateurs de trace activés pour la session active.
  
```sql  
DBCC TRACESTATUS();  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
[DBCC TRACEON &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)  
[Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
  
  
