---
title: sp_changesubscriptiondtsinfo (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/04/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: 
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_changesubscriptiondtsinfo
- sp_changesubscriptiondtsinfo_TSQL
helpviewer_keywords:
- sp_changesubscriptiondtsinfo
ms.assetid: 64fc085f-f81b-493b-b59a-ee6192d9736d
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 6b551a265d30632f7ee96c86b78191388099c7d0
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="spchangesubscriptiondtsinfo-transact-sql"></a>sp_changesubscriptiondtsinfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés de package DTS (Data Transformation Services) d'un abonnement. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubscriptiondtsinfo [ [ @job_id = ] job_id ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id=**] *job_id*  
 ID du travail de l'Agent de distribution pour l'abonnement par envoi de données (push). *job_id* est **varbinary (16)**, sans valeur par défaut. Pour rechercher l’ID de tâche de Distribution, exécutez **sp_helpsubscription** ou **sp_helppullsubscription**.  
  
 [  **@dts_package_name** =] **'***l’argument dts_package_name***'**  
 Spécifie le nom du package DTS. *l’argument dts_package_name* est un **sysname**, avec NULL comme valeur par défaut. Par exemple, pour spécifier un package nommé **DTSPub_Package**, vous devez spécifier `@dts_package_name = N'DTSPub_Package'`.  
  
 [  **@dts_package_password** =] **'***dts_package_password***'**  
 Spécifie le mot de passe du package. *dts_package_password* est **sysname** avec une valeur par défaut NULL, qui indique que la propriété de mot de passe ne doit être modifiée.  
  
> [!NOTE]  
>  Un package DTS doit avoir un mot de passe.  
  
 [  **@dts_package_location** =] **'***dts_package_location***'**  
 Spécifie l'emplacement du package. *dts_package_location* est un **nvarchar(12)**, avec NULL comme valeur par défaut qui indique que l’emplacement du package ne doit être modifiée. L’emplacement du package peut être modifié pour **distributeur** ou **abonné**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubscriptiondtsinfo** est utilisé pour la réplication de capture instantanée et la réplication transactionnelle qui sont uniquement les abonnements par envoi de données.  
  
## <a name="permissions"></a>Permissions  
 Seuls les membres de la **sysadmin** rôle de serveur fixe **db_owner** rôle de base de données fixe, ou le créateur de l’abonnement peut exécuter **sp_changesubscriptiondtsinfo**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
