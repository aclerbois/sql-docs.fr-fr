---
title: sp_help_downloadlist (Transact-SQL) | Documents Microsoft
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
- sp_help_downloadlist_TSQL
- sp_help_downloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_downloadlist
ms.assetid: 745b265b-86e8-4399-b928-c6969ca1a2c8
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e48423bc91413518abe3002a3c3d8da2cef330c2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpdownloadlist-transact-sql"></a>sp_help_downloadlist (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Répertorie toutes les lignes de la **sysdownloadlist** (table système) pour le travail fourni, ou toutes les lignes si aucun travail n’est spécifié.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_downloadlist { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }   
     [ , [ @operation = ] 'operation' ]   
     [ , [ @object_type = ] 'object_type' ]   
     [ , [ @object_name = ] 'object_name' ]   
     [ , [ @target_server = ] 'target_server' ]   
     [ , [ @has_error = ] has_error ]   
     [ , [ @status = ] status ]   
     [ , [ @date_posted = ] date_posted ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id=** ] *job_id*  
 Numéro d'identification du travail pour lequel renvoyer des informations. *job_id* est **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
 [  **@job_name=** ] **'***job_name***'**  
 Nom du travail. *job_name* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@operation=** ] **'***opération***'**  
 Opération valide pour le travail spécifié. *opération* est **varchar(64)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**ERREUR**|Opération de serveur qui demande le serveur cible pour annuler l’inscription du serveur maître **SQLServerAgent** service.|  
|**DELETE**|Opération qui supprime intégralement un travail.|  
|**INSERT**|Opération qui insère un travail ou actualise un travail existant. Cette opération comporte toutes les étapes et planifications du travail, le cas échéant.|  
|**INSCRIRE DE NOUVEAU**|Opération serveur qui fait renvoyer les informations d'inscription par le serveur cible, y compris la fréquence d'interrogation et le fuseau horaire, au domaine multiserveur. Le serveur cible télécharge également les **MSXOperator** détails.|  
|**SET-POLL**|Opération de serveur qui définit l'intervalle, en secondes, que doivent respecter les serveurs cibles pour l'interrogation du domaine multiserveur. Si spécifié, *valeur* est interprété comme la valeur d’intervalle requis, et peut être une valeur à partir de **10** à **28 800**.|  
|**DÉMARRER**|Opération de travail qui requiert le début de l'exécution d'un travail.|  
|**ARRÊTER**|Opération de travail qui nécessite l'interruption de l'exécution d'un travail.|  
|**HEURE DE SYNCHRONISATION**|Opération de serveur qui commande au serveur cible de synchroniser son horloge système avec le domaine multiserveur. Cette opération étant coûteuse, il est préférable de l'exécuter le plus rarement possible.|  
|**UPDATE**|Opération de travail qui met à jour uniquement la **sysjobs** informations pour un travail, pas les étapes de travail ou les planifications. Est appelée automatiquement par **sp_update_job**.|  
  
 [ **@object_type=** ] **'***object_type***'**  
 Type de l'objet du travail spécifié. *object_type* est **varchar(64)**, avec NULL comme valeur par défaut. *object_type* peut être soit JOB soit SERVER. Pour plus d’informations sur *object_type*valeurs, consultez [sp_add_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md).  
  
 [  **@object_name=** ] **'***nom_objet***'**  
 Nom de l'objet. *nom_objet* est **sysname**, avec NULL comme valeur par défaut. Si *object_type* est travail, *nom_objet*est le nom du travail. Si *object_type*est serveur, *nom_objet*est le nom du serveur.  
  
 [ **@target_server=** ] **'***target_server***'**  
 Le nom du serveur cible. *serveur_cible* est **nvarchar (128)**, avec NULL comme valeur par défaut.  
  
 [  **@has_error=** ] *a_erreur*  
 Indique si le travail doit signaler les erreurs. *a_erreur* est **tinyint**, avec NULL comme valeur par défaut, qui n’indique aucune erreur doit être signalée. **1** indique que toutes les erreurs doivent être signalées.  
  
 [  **@status=** ] *état*  
 État du travail. *état* est **tinyint**, avec NULL comme valeur par défaut.  
  
 [  **@date_posted=** ] *date_d*  
 Date et heure à partir desquelles toutes les entrées doivent être incluses dans le jeu de résultats. *date_d* est **datetime**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Numéro d'identification entier unique de l'instruction.|  
|**source_server**|**nvarchar(30)**|Nom de l'ordinateur du serveur qui émet l'instruction. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0, il est toujours le nom d’ordinateur du serveur maître (MSX).|  
|**operation_code**|**nvarchar(4000)**|Code d'opération pour l'instruction.|  
|**object_name**|**sysname**|Objet affecté par l'instruction.|  
|**object_id**|**uniqueidentifier**|Numéro d’identification de l’objet affecté par l’instruction (**job_id** pour un objet de traitement, ou 0 x 00 pour un objet serveur) ou une valeur de données spécifique à la **operation_code**.|  
|**target_server**|**nvarchar(30)**|Serveur cible devant télécharger cette instruction.|  
|**error_message**|**nvarchar(1024)**|Message d'erreur (le cas échéant) émis par le serveur cible s'il y a eu un problème lors du traitement de l'instruction.<br /><br /> Remarque : N’importe quel message d’erreur stoppe les téléchargements par le serveur cible.|  
|**date_posted**|**datetime**|Date à laquelle l'instruction a été envoyée à la table.|  
|**date_downloaded**|**datetime**|Date à laquelle l'instruction a été téléchargée par le serveur cible.|  
|**status**|**tinyint**|État du travail :<br /><br /> **0** ne = pas encore téléchargé<br /><br /> **1** = téléchargé avec succès.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution de cette procédure sont accordées par défaut aux membres du rôle de serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant répertorie des lignes dans `sysdownloadlist` pour le travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_downloadlist  
    @job_name = N'NightlyBackups',  
    @operation = N'UPDATE',   
    @object_type = N'JOB',   
    @object_name = N'NightlyBackups',  
    @target_server = N'SEATTLE2',   
    @has_error = 1,   
    @status = NULL,   
    @date_posted = NULL ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
