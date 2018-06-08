---
title: sp_adddistributor (Transact-SQL) | Documents Microsoft
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
- sp_adddistributor
- sp_adddistributor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adddistributor
ms.assetid: 35415502-68d0-40f6-993c-180e50004f1e
caps.latest.revision: 35
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: bb7c48d0726b51d18c878317325bca642cdf6874
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spadddistributor-transact-sql"></a>sp_adddistributor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une entrée dans le [sys.sysservers](../../relational-databases/system-compatibility-views/sys-sysservers-transact-sql.md) table (s’il n’est pas le cas), marque l’entrée de serveur comme serveur de distribution et stocke les informations sur les propriétés. Cette procédure stockée est exécutée sur la base de données master du serveur de distribution pour enregistrer et marquer le serveur en tant que serveur de distribution. Dans le cas d'un serveur de distribution distant, la procédure stockée est également exécutée sur le serveur de publication à partir de la base de données master.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddistributor [ @distributor= ] 'distributor'   
    [ , [ @heartbeat_interval= ] heartbeat_interval ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @from_scripting= ] from_scripting ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@distributor=**] **'***distributeur***'**  
 Est le nom de serveur de distribution. *serveur de distribution* est **sysname**, sans valeur par défaut. Ce paramètre est uniquement utilisé lors de la configuration d'un serveur de distribution distant. Il ajoute des entrées pour les propriétés du serveur de distribution dans le **msdb... MSdistributor** table.  
  
 [  **@heartbeat_interval=**] *heartbeat_interval*  
 Est le nombre maximal de minutes pendant lesquelles un agent peut fonctionner sans enregistrer un message de progression. *heartbeat_interval* est **int**, avec une valeur par défaut de 10 minutes. Un travail de l'Agent SQL Server est créé et s'exécute pendant cet intervalle de temps pour contrôler l'état des agents de réplication en cours d'exécution.  
  
 [  **@password=**] **'***mot de passe***'**]  
 Mot de passe de la **distributor_admin** connexion. *mot de passe* est **sysname**, avec NULL comme valeur par défaut. Si le mot de passe est NULL ou est une chaîne vide, une valeur aléatoire est redéfinie pour lui. Le mot de passe doit être configuré lors de l'ajout du premier serveur de distribution distant. **distributor_admin** connexion et *mot de passe* sont stockés pour l’entrée de serveur lié utilisée pour une *distributeur* connexion RPC, y compris les connexions locales. Si *distributeur* est local, le mot de passe **distributor_admin** est définie sur une nouvelle valeur. Pour les serveurs de publication avec un serveur de distribution distant, la même valeur pour *mot de passe* doit être spécifié lors de l’exécution **sp_adddistributor** le serveur de publication et le serveur de distribution. [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) peut être utilisé pour modifier le mot de passe du serveur de distribution.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@from_scripting=** ] *from_scripting*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddistributor** est utilisé dans la réplication de capture instantanée, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistributor-transa_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_adddistributor**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributor_property &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_dropdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)  
  
  
