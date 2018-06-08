---
title: MSSQLSERVER_10509 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 10509 (Database Engine error)
ms.assetid: e9dd5357-ee3d-420a-9a89-d12ab5404e73
caps.latest.revision: 9
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: cf371bbc4a9a80608c5171be9bd9891c71cde1a6
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver10509"></a>MSSQLSERVER_10509
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|10509|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_INVALID_STMT|  
|Texte du message|Impossible de créer le repère de plan '%.\*ls', car l’instruction spécifiée par **@stmt** ou **@statement_start_offset** contient une erreur de syntaxe ou n’est pas éligible pour le repère de plan. Fournissez une seule instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] valide ou une position de départ de l'instruction valide dans le lot. Pour obtenir une position de départ valide, interrogez la colonne statement_start_offset de la fonction de gestion dynamique sys.dm_exec_query_stats.|  
  
## <a name="explanation"></a>Explication  
L’instruction spécifiée par **@stmt** ou **@statement_start_offset** contient une erreur de syntaxe ou n’est pas éligible pour une utilisation dans un repère de plan.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Fournissez une seule instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] valide ou une position de départ de l'instruction valide dans le lot. Pour obtenir une position de départ valide, interrogez la colonne statement_start_offset de la fonction de gestion dynamique sys.dm_exec_query_stats.  
  
## <a name="see-also"></a> Voir aussi  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[Repères de plan](~/relational-databases/performance/plan-guides.md)  
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
