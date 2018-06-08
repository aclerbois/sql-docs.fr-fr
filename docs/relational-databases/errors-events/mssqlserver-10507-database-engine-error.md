---
title: MSSQLSERVER_10507 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 10507 (Database Engine error)
ms.assetid: cd83fa81-ac37-4eda-a3c3-17610b051de2
caps.latest.revision: 9
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: c5f69dc55a2b80a47b9d014c50fa03074a76e31d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver10507"></a>MSSQLSERVER_10507
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|10507|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_STMT_DOES_NOT_MATCH|  
|Texte du message|Impossible de créer le repère de plan '%.\*ls', car l’instruction spécifiée par **@stmt** et **@module_or_batch** ou par **@plan_handle** et **@statement_start_offset** ne correspond à aucune instruction dans le module ou lot spécifié. Modifiez les valeurs pour qu'elles correspondent à une instruction dans le module ou le lot.|  
  
## <a name="explanation"></a>Explication  
Une instruction dans le module ou le lot spécifié n'a pas pu être mise en correspondance avec l'instruction ou la valeur de décalage de l'instruction spécifiée.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Modifiez les valeurs de paramètre spécifiées pour qu'elles correspondent à une instruction dans le module ou le lot.  
  
## <a name="see-also"></a> Voir aussi  
[Repères de plan](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
