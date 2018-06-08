---
title: sp_setsubscriptionxactseqno (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- sp_setsubscriptionxactseqno
- sp_setsubscriptionxactseqno_TSQL
helpviewer_keywords:
- sp_setsubscriptionxactseqno
ms.assetid: cdb4e0ba-5370-4905-b03f-0b0c6f080ca6
caps.latest.revision: 16
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f59325c709b8d16d5e120a135d5d9f9697692b1e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spsetsubscriptionxactseqno-transact-sql"></a>sp_setsubscriptionxactseqno (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Utilisé lors de la résolution des incidents pour définir le numéro séquentiel dans le journal de la transaction suivante que doit appliquer l'Agent de distribution au niveau de l'Abonné, ce qui permet à l'Agent d'ignorer une transaction qui a échoué. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné. Non pris en charge pour les Abonnés non-SQL Server.  
  
> [!CAUTION]  
>  Si vous n'utilisez pas correctement cette procédure stockée ou si vous spécifiez une valeur de numéro d'enregistrement incorrecte, l'Agent de distribution annule les modifications appliquées au niveau de l'Abonné ou ignore toutes les modifications restantes.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_setsubscriptionxactseqno [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @xact_seqno = ] xact_seqno   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publisher=** ] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher_db=** ] **'***publisher_db***'**  
 Nom de la base de données de publication. *publisher_db* est **sysname**, sans valeur par défaut. Pour un serveur non-SQL, *publisher_db* est le nom de la base de données de distribution.  
  
 [  **@publication=** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut. Lorsque l’Agent de Distribution est partagé par plusieurs publications, vous devez spécifier une valeur All pour *publication*.  
  
 [  **@xact_seqno=** ] *xact_seqno*  
 Numéro séquentiel d'enregistrement de la transaction suivante sur le serveur de distribution à appliquer à l'Abonné. *xact_seqno* est **varbinary (16)**, sans valeur par défaut.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**XACT_SEQNO D’ORIGINE**|**varbinary(16)**|Numéro séquentiel d'enregistrement d'origine de la transaction suivante à appliquer à l'Abonné.|  
|**XACT_SEQNO MIS À JOUR**|**varbinary(16)**|Numéro séquentiel d'enregistrement mis à jour de la transaction suivante à appliquer à l'Abonné.|  
|**NOMBRE DE FLUX DE DONNÉES D’ABONNEMENT**|**int**|Nombre de flux d'abonnements utilisés au cours de la dernière synchronisation.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_setsubscriptionxactseqno** est utilisé dans la réplication transactionnelle.  
  
 **sp_setsubscriptionxactseqno** ne peut pas être utilisé dans une topologie de réplication transactionnelle d’égal à égal.  
  
 **sp_setsubscriptionxactseqno** peut être utilisé pour ignorer une transaction spécifique qui provoque une erreur lorsque appliquée à l’abonné. Lors d’un échec et après l’arrêt de l’Agent de Distribution, appelez [sp_helpsubscriptionerrors &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-helpsubscriptionerrors-transact-sql.md) sur le serveur de distribution pour récupérer la valeur xact_seqno de la transaction en échec, puis appelez **sp_setsubscriptionxactseqno**, en passant cette valeur pour *xact_seqno*. Ainsi, seules les commandes consécutives à ce numéro séquentiel d'enregistrement sont traitées.  
  
 Spécifiez la valeur **0** pour *xact_seqno* pour fournir toutes les commandes en attente dans la base de données de distribution vers l’abonné.  
  
 **sp_setsubscriptionxactseqno** risque d’échouer si l’Agent de Distribution utilise des flux multi-abonnements.  
  
 Lorsque cette erreur se produit, vous devez exécuter l'Agent de distribution avec un seul flux d'abonnements. Pour plus d'informations, consultez [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_setsubscriptionxactseqno**.  
  
  
