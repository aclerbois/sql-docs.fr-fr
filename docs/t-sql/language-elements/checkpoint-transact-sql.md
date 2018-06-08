---
title: CHECKPOINT (Transact-SQL) | Microsoft Docs
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
- CHECKPOINT_TSQL
- CHECKPOINT
dev_langs:
- TSQL
helpviewer_keywords:
- events [SQL Server], checkpoints
- automatic checkpoints
- writing dirty pages to disk
- pages [SQL Server], dirty
- checkpoints [SQL Server]
- CHECKPOINT statement
- log truncate mode [SQL Server]
- dirty pages
- logs [SQL Server], checkpoints
- manual checkpoints [SQL Server]
- pages [SQL Server], checkpoints
ms.assetid: ccdfc689-ad4e-44c0-83f7-0f2cfcfb6406
caps.latest.revision: 59
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2c7232d1494ad48987d91ffe4dfc05e5ef47e4b5
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="checkpoint-transact-sql"></a>CHECKPOINT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Génère un point de contrôle manuel dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté.  
  
> [!NOTE]  
>  Pour plus d’informations sur différents types de points de contrôle de base de données et sur les opérations de point de contrôle en général, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CHECKPOINT [ checkpoint_duration ]  
```  
  
## <a name="arguments"></a>Arguments  
 *checkpoint_duration*  
 Spécifie le temps requis (en secondes) pour terminer le point de contrôle manuel. Quand *checkpoint_duration* est spécifiée, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] tente de réaliser un point de contrôle dans le laps de temps imparti. *checkpoint_duration* doit être une expression de type **int** et sa valeur doit être supérieure à zéro. Lorsque ce paramètre est omis, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] règle la durée du point de contrôle pour minimiser l'impact sur les performances des applications de base de données. *checkpoint_duration* est une option avancée.  
  
## <a name="factors-affecting-the-duration-of-checkpoint-operations"></a>Facteurs ayant une incidence sur la durée des opérations de point de contrôle  
 En règle générale, le temps nécessaire à la réalisation d'un point de contrôle augmente avec le nombre de pages incorrectes à écrire. Par défaut, pour réduire l'impact sur les performances sur d'autres applications, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajuste la fréquence des écritures qu'une opération de point de contrôle effectue. La diminution de la fréquence d'écriture augmente la durée de l'opération de point de contrôle. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise cette stratégie pour un point de contrôle manuel, sauf si une valeur *checkpoint_duration* est spécifiée dans la commande CHECKPOINT.  
  
 L’incidence de l’utilisation de *checkpoint_duration* sur les performances dépend du nombre de pages incorrectes, de l’activité sur le système et de la durée réelle spécifiée. Par exemple, si le point de contrôle s’effectue normalement en 120 secondes, le fait de spécifier une valeur *checkpoint_duration* de 45 secondes oblige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à allouer plus de ressources au point de contrôle que ne le ferait la valeur par défaut. À l’inverse, une valeur de 180 secondes spécifiée pour *checkpoint_duration* fait en sorte que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribue moins de ressources que ce qui serait attribué par défaut. En règle générale, une valeur faible pour *checkpoint_duration* augmente les ressources attribuées au point de contrôle tandis qu’une valeur plus élevée les réduit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue toujours un point de contrôle lorsque cela est possible et l'instruction CHECKPOINT renvoie immédiatement les informations lorsqu'un point de contrôle se termine. Ainsi, dans certains cas, un point de contrôle peut se terminer avant ou après que la durée spécifiée soit écoulée.  
  
##  <a name="Security"></a> Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Les autorisations de CHECKPOINT sont accordées par défaut aux membres du rôle serveur fixe **sysadmin** et aux membres des rôles de base de données fixes **db_owner** et **db_backupoperator** ; elles ne sont pas transférables.  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Configurer l’option de configuration de serveur d’intervalle de récupération](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)   
 [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)  
  
  
