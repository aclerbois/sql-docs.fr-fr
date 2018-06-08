---
title: sp_restoremergeidentityrange (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- sp_restoremergeidentityrange_TSQL
- sp_restoremergeidentityrange
helpviewer_keywords:
- sp_restoremergeidentityrange
ms.assetid: 7923e422-2748-40c0-b5a8-6410c48d5b70
caps.latest.revision: 12
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b8be3de617713868a755dbab55e4ac4674dfd07a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sprestoremergeidentityrange-transact-sql"></a>sp_restoremergeidentityrange (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Cette procédure stockée permet de mettre à jour les affectations de plage d'identité. Elle garantit le bon fonctionnement de la gestion automatique des plages d'identité lorsqu'un serveur de publication a été restauré à partir d'une sauvegarde. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_restoremergeidentityrange [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publication** =] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec la valeur par défaut de **tous les**. Lorsque cet argument est spécifié, seules les plages d'identité de la publication correspondante sont restaurées.  
  
 [ **@article** =] **'***article***'**  
 Nom de l'article. *article* est **sysname**, avec la valeur par défaut **tous les**. Lorsque cet argument est spécifié, seules les plages d'identité de l'article correspondant sont restaurées.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_restoremergeidentityrange** est utilisé avec la réplication de fusion.  
  
 **sp_restoremergeidentityrange** Obtient des informations d’allocation de plage d’identité maximale du serveur de distribution et met à jour les valeurs dans le **max_used** colonne de [MSmerge_identity_range_allocations &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/msmerge-identity-range-allocations-transact-sql.md) pour les articles qui utilisent la gestion de plages d’identité automatique.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_restoremergeidentityrange**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  
