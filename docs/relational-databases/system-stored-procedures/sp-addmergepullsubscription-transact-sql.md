---
title: sp_addmergepullsubscription (Transact-SQL) | Documents Microsoft
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
- sp_addmergepullsubscription_TSQL
- sp_addmergepullsubscription
helpviewer_keywords:
- sp_addmergepullsubscription
ms.assetid: d63909a0-8ea7-4734-9ce8-8204d936a3e4
caps.latest.revision: 44
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6d02490784dbbae3dba91aab940257300b4445eb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddmergepullsubscription-transact-sql"></a>sp_addmergepullsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un abonnement par extraction de données (pull) à une publication de fusion. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepullsubscription [ @publication= ] 'publication'   
    [ , [ @publisher= ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @subscriber_type= ] 'subscriber_type' ]   
    [ , [ @subscription_priority= ] subscription_priority ]   
    [ , [ @sync_type= ] 'sync_type' ]   
    [ , [ @description= ] 'description' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *Serveur de publication* est **sysname**, avec la valeur par défaut est le nom du serveur local. Le serveur de publication doit être un serveur valide.  
  
 [  **@publisher_db =**] **'***publisher_db***'**  
 Nom de la base de données du serveur de publication. *publisher_db* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subscriber_type=**] **'***subscriber_type***'**  
 Type d'abonné. *subscriber_type* est **nvarchar (15)** et peut être **global**, **local** ou **anonyme**. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, abonnements locaux sont désignés comme des abonnements clients et les abonnements globaux sont désignés comme des abonnements serveur.  
  
 [  **@subscription_priority=**] *priorité_d*  
 Est la priorité d’abonnement. *priorité_d*est **réel**, avec NULL comme valeur par défaut. Pour les abonnements locaux et anonymes, la priorité est **0.0**. La priorité est utilisée par le résolveur par défaut pour déterminer un gagnant lorsque des conflits sont détectés. Pour les abonnés globaux, la priorité de l'abonnement doit être inférieure à 100, qui correspond à la priorité du serveur de publication.  
  
 [  **@sync_type=**] **'***sync_type***'**  
 Type de synchronisation d'abonnement. *sync_type*est **nvarchar (15)**, avec une valeur par défaut **automatique**. Peut être **automatique** ou **aucun**. Si **automatique**, le schéma et les données initiales des tables publiées sont transférées tout d’abord à l’abonné. Si **aucun**, il est supposé que l’abonné possède déjà le schéma et les données initiales des tables publiées. Les données et les tables système sont toujours transférées.  
  
> [!NOTE]  
>  Nous ne recommandons pas la valeur de **aucun**.  
  
 [  **@description=**] **'***description***'**  
 Brève description de l'abonnement par extraction de données (pull). *Description*est **nvarchar (255)**, avec NULL comme valeur par défaut. Cette valeur est affichée par le moniteur de réplication dans le **nom convivial** colonne, qui peut être utilisé pour trier les abonnements d’une publication analysée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepullsubscription** est utilisé pour la réplication de fusion.  
  
 Si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour synchroniser l’abonnement, le [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) procédure stockée doit être exécutée sur l’abonné pour créer un agent et un travail à synchroniser avec la Publication.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_1.sql)]  
  
 [!code-sql[HowTo#sp_addmergepullsub_websync_anon](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_2.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addmergepullsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un abonnement par extraction](../../relational-databases/replication/create-a-pull-subscription.md)   
 [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
