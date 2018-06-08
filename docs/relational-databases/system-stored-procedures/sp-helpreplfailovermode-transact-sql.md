---
title: sp_helpreplfailovermode (Transact-SQL) | Documents Microsoft
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
- sp_helpreplfailovermode
- sp_helpreplfailovermode_TSQL
helpviewer_keywords:
- sp_helpreplfailovermode
ms.assetid: d1090e42-6840-4bf6-9aa9-327fd8987ec2
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8d54500307b05a5aa6c9cfeca4e55ff92b3062b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpreplfailovermode-transact-sql"></a>sp_helpreplfailovermode (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche le mode de basculement actuel d'un abonnement. Cette procédure stockée est exécutée au niveau de l'Abonné, sur n'importe quelle base de données. Pour plus d’informations sur les modes de basculement, consultez [à des abonnements pour la réplication transactionnelle](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpreplfailovermode [ @publisher= ] 'publisher'   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @publication = ] 'publication' ]   
    [ , [ @failover_mode_id= ] 'failover_mode_id'OUTPUT]   
    [ , [ @failover_mode = ] 'failover_mode'OUTPUT]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication qui participe à la mise à jour de l'Abonné. *serveur de publication* est **sysname**, sans valeur par défaut. Le serveur de publication doit déjà être configuré pour la publication.  
  
 [  **@publisher_db =**] **'***publisher_db***'**  
 Nom de la base de données de publication. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication qui participe à la mise à jour de l'Abonné. *publication*est **sysname**, sans valeur par défaut.  
  
 [  **@failover_mode_id=**] **'***failover_mode_id***' sortie**  
 Retourne la valeur entière du mode de basculement et est un **sortie** paramètre. *failover_mode_id* est un **tinyint** avec une valeur par défaut **0**. Elle retourne **0** pour la mise à jour immédiate et **1** pour la mise à jour de la file d’attente.  
  
 [**@failover_mode=**] **'***failover_mode***' sortie**  
 Renvoie le mode dans lequel les modifications sont effectuées au niveau de l'Abonné. *failover_mode* est un **nvarchar (10)** avec NULL comme valeur par défaut. Est un **sortie** paramètre.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Immédiate**|Mise à jour immédiate : les mises à jour réalisées sur l'Abonné sont immédiatement propagées au serveur de publication à l'aide du protocole de validation à deux phases (2PC).|  
|**En file d’attente**|Mise à jour en attente : les mises à jour effectuées sur l'Abonné sont stockées dans une file d'attente.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpreplfailovermode** est utilisé dans la réplication transactionnelle ou la réplication d’instantané pour les abonnements sont activés pour la mise à jour immédiate avec mise à jour sous forme de basculement, en cas de défaillance attente.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_helpreplfailovermode**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_setreplfailovermode &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setreplfailovermode-transact-sql.md)  
  
  
