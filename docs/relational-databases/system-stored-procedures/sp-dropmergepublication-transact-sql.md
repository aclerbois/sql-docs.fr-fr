---
title: sp_dropmergepublication (Transact-SQL) | Documents Microsoft
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
- sp_dropmergepublication
- sp_dropmergepublication_TSQL
helpviewer_keywords:
- sp_dropmergepublication
ms.assetid: 9e1cb96e-5889-4f97-88cd-f60cf313ce68
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 9a360ce767a80dd9f77f35a22d92b65d3a52777c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spdropmergepublication-transact-sql"></a>sp_dropmergepublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime une publication de fusion et l'Agent d'instantané qui lui est associé. Tous les abonnements doivent être supprimés avant de supprimer une publication de fusion. Les articles de la publication sont supprimés automatiquement. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropmergepublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
    [ , [ @ignore_merge_metadata = ] ignore_merge_metadata ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Est le nom de la publication à supprimer. *publication* est **sysname**, sans valeur par défaut. Si **tous les**, toutes les publications de fusion existantes sont supprimées, ainsi que le travail de l’Agent d’instantané qui s’y rapportent. Si vous spécifiez une valeur particulière pour *publication*, seuls cette publication et son travail de l’Agent de capture instantanée associé sont supprimés.  
  
 [  **@ignore_distributor =**] *ignore_distributor*  
 Permet de supprimer une publication sans effectuer de tâches de nettoyage au niveau du serveur de distribution. *ignore_distributor* est **bits**, avec une valeur par défaut **0**. Ce paramètre est également utilisé lors de la réinstallation du serveur de distribution.  
  
 [  **@reserved=**] *réservé*  
 Elle est réservée pour un usage futur. *réservé* est **bits**, avec une valeur par défaut **0**.  
  
 [  **@ignore_merge_metadata=** ] *ignore_merge_metadata*  
 À usage interne uniquement  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropmergepublication** est utilisé dans la réplication de fusion.  
  
 **sp_dropmergepublication** supprime tous les articles associés à une publication de manière récursive et puis supprime la publication elle-même. Une publication ne peut être supprimée si elle fait l'objet d'un ou de plusieurs abonnements. Pour plus d’informations sur la suppression des abonnements, consultez [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md) et [supprimer un abonnement par extraction](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
 L’exécution de **sp_dropmergepublication** pour supprimer une publication ne supprime pas les objets publiés à partir de la base de données de publication ou les objets correspondants de la base de données d’abonnement. Utilisez DROP \<objet > pour supprimer ces objets manuellement si nécessaire.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_dropmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepublication-_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_dropmergepublication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer une Publication](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
