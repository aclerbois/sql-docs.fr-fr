---
title: sp_configure_peerconflictdetection (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
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
- sp_configure_peerconflictdetection_TSQL
- sp_configure_peerconflictdetection
helpviewer_keywords:
- sp_configure_peerconflictdetection
ms.assetid: 45117cb2-3247-433f-ba3d-7fa19514b1c3
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8ad2e7b3c0fd877dad8b14360d7c3f65331cb8cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spconfigurepeerconflictdetection-transact-sql"></a>sp_configure_peerconflictdetection (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Configure la détection de conflit pour une publication impliquée dans une topologie de réplication transactionnelle d'égal à égal. Pour plus d'informations, consultez [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md). Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_configure_peerconflictdetection [ @publication = ] 'publication'  
    [ , [ @action = ] 'action']  
    [ , [ @originator_id = ] originator_id ]  
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @continue_onconflict = ] 'continue_onconflict']  
    [ , [ @local = ] 'local']  
    [ , [ @timeout = ] timeout ]  
  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication=] '*publication*'  
 Nom de la publication pour laquelle configurer la détection de conflit. *publication* est **sysname**, sans valeur par défaut.  
  
 [ @action=] '*action*'  
 Spécifie s'il faut activer ou désactiver la détection de conflit pour une publication. *action* est **nvarchar (5)**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**enable**|Active la détection de conflit pour une publication.|  
|**disable**|Désactive la détection de conflit pour une publication.|  
|NULL (par défaut)||  
  
 [ @originator_id=] *originator_id*  
 Spécifie un ID pour un nœud dans une topologie d'égal à égal. *originator_id* est **int**, avec NULL comme valeur par défaut. Cet ID est utilisé pour la détection de conflit si *action* a la valeur **activer**. Spécifiez un ID positif différent de zéro qui n'a jamais été utilisé dans la topologie. Pour obtenir la liste des ID qui ont déjà été utilisés, interrogez la table système [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) .  
  
 [ @conflict_retention=] *conflict_retention*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @continue_onconflict=] '*continue_onconflict*']  
 Détermine si l'Agent de distribution continue à traiter les modifications lorsqu'un conflit est détecté. *continue_onconflict* est **nvarchar (5)** avec la valeur par défaut FALSE.  
  
> [!CAUTION]  
>  Nous vous recommandons de conserver la valeur par défaut FALSE. Lorsque cette option a la valeur TRUE, l'Agent de distribution tente de converger les données dans la topologie en appliquant la ligne en conflit du nœud doté de l'ID d'appelant le plus élevé. Cette méthode ne garantit pas la convergence. Vous devez vous assurer que la topologie est cohérente après la détection d'un conflit. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
 [ @local=] '*local*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @timeout=] *délai d’attente*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_configure_peerconflictdetection est utilisé dans la réplication transactionnelle d'égal à égal. Pour utiliser la détection de conflit, tous les nœuds doivent exécuter [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou versions ultérieures et la détection doivent être activé pour tous les nœuds.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner.  
  
## <a name="see-also"></a>Voir aussi  
 [Détection de conflit dans la réplication d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [Réplication transactionnelle d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
