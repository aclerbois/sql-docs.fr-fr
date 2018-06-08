---
title: KILL STATS JOB (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- KILL STATS JOB
- KILL_STATS_JOB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending statistics update jobs [SQL Server]
- stopping statistics update jobs
- terminating statistics update jobs
- asynchronous statistics updates [SQL Server]
- KILL STATS JOB statement
- statistics update jobs [SQL Server]
ms.assetid: 55a8f9f1-3259-45c0-8ab9-60b9c088b4b4
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1303ca8ace54e5b5590a7d7179347c00601f2a76
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="kill-stats-job-transact-sql"></a>KILL STATS JOB (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Met fin à un travail de mise à jour asynchrone des statistiques dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
KILL STATS JOB job_id   
```  
  
## <a name="arguments"></a>Arguments  
 *job_id*  
 Champ job_id renvoyé par la vue de gestion dynamique sys.dm_exec_background_job_queue du travail.  
  
## <a name="remarks"></a>Notes   
 L'argument job_id n'est pas lié à session_id ou à l'unité de travail utilisée dans d'autres formes de l'instruction KILL.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit bénéficier de l'autorisation VIEW SERVER STATE pour accéder aux données de la vue de gestion dynamique sys.dm_exec_background_job_queue.  
  
 Les autorisations de l'instruction KILL STATS JOB sont octroyées par défaut aux membres des rôles de base de données fixes sysadmin et processadmin et ne sont pas transmissibles.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment mettre fin à la mise à jour des statistiques associée à un travail où *job_id* = `53`.  
  
```  
KILL STATS JOB 53;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [sys.dm_exec_background_job_queue &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md)   
 [Statistiques](../../relational-databases/statistics/statistics.md)  
  
  
