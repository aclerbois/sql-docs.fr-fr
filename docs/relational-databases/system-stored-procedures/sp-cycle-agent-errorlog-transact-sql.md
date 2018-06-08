---
title: sp_cycle_agent_errorlog (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_cycle_agent_errorlog
- sp_cycle_agent_errorlog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cycle_agent_errorlog
ms.assetid: 8aa96182-60b7-4d7b-b2a7-ccce70378c6e
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 74b0bc568dfa883b6b2eb4c6b19fcf3a38512e9e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcycleagenterrorlog-transact-sql"></a>sp_cycle_agent_errorlog (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ferme actuel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fichier de journal des erreurs de l’Agent et les cycles de le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] numéros d’extension de l’Agent erreurs journal comme un redémarrage du serveur. Le nouveau journal d'erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent contient une ligne indiquant que le nouveau journal a été créé.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cycle_agent_errorlog  
```  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Chaque fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est démarré, en cours [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] journal des erreurs de l’Agent est renommé en **SQLAgent.1**; **SQLAgent.1** devient **SQLAgent.2**, **SQLAgent.2** devient **SQLAgent.3**, et ainsi de suite. **sp_cycle_agent_errorlog** vous permet de parcourir les fichiers journaux des erreurs sans arrêter et démarrer le serveur.  
  
 Cette procédure stockée doit être exécutée à partir de la **msdb** base de données.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution **sp_cycle_agent_errorlog** sont limitées aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant parcourt le journal d'erreurs de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_cycle_agent_errorlog ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cycle_errorlog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cycle-errorlog-transact-sql.md)  
  
  
