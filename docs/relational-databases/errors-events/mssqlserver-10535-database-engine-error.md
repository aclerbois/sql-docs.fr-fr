---
title: MSSQLSERVER_10535 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 10535 (Database Engine error)
ms.assetid: 478fd978-11d9-4155-8329-f599fdbec14b
caps.latest.revision: 9
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 60c4b47511392e606323c3e51ec6e36f6a089a15
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver10535"></a>MSSQLSERVER_10535
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|10535|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_NO_PLAN|  
|Texte du message|Impossible de créer le repère de plan '%.*ls', car aucun plan n'a été trouvé dans le cache du plan qui correspond au handle de plan spécifié. Indiquez un handle de plan mis en cache. Pour obtenir la liste des handles de plan mis en cache, interrogez la vue de gestion dynamique sys.dm_exec_query_stats.|  
  
## <a name="explanation"></a>Explication  
Aucun plan correspondant au handle de plan spécifié n'a été trouvé dans le cache du plan.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Indiquez un handle de plan mis en cache. Pour obtenir la liste des handles de plan mis en cache, interrogez la vue de gestion dynamique sys.dm_exec_query_stats.  
  
## <a name="see-also"></a> Voir aussi  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
