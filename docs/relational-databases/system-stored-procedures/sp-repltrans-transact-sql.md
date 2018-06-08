---
title: sp_repltrans (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_repltrans_TSQL
- sp_repltrans
helpviewer_keywords:
- sp_repltrans
ms.assetid: 738e2322-335b-44fa-820e-f31c02743978
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: dd00140e94d77e4daed159a97e41ad4a493fa624
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sprepltrans-transact-sql"></a>sp_repltrans (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie un jeu de résultats pour toutes les transactions du journal des transactions de la base de données de publication qui sont marquées pour la réplication mais qui n'ont pas été signalées comme distribuées. Cette procédure stockée est exécutée sur une base de données de publication du serveur de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_repltrans  
```  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_repltrans** retourne des informations sur la base de données de publication à partir de laquelle elle est exécutée, ce qui vous permet d’afficher les transactions non distribuées actuellement (les transactions qui demeurent dans le journal des transactions qui n’ont pas été envoyés à la Serveur de distribution). L'ensemble de résultats affiche les numéros séquentiels dans le journal du premier et du dernier enregistrement de chaque transaction. **sp_repltrans** est similaire à [sp_replcmds &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) mais ne retourne ne pas les commandes pour les transactions.  
  
## <a name="remarks"></a>Notes  
 **sp_repltrans** est utilisé dans la réplication transactionnelle.  
  
 **sp_repltrans** n’est pas pris en charge pour non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveurs de publication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_repltrans**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
