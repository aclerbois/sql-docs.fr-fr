---
title: sp_link_publication (Transact-SQL) | Documents Microsoft
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
- sp_link_publication_TSQL
- sp_link_publication
helpviewer_keywords:
- sp_link_publication
ms.assetid: 1945ed24-f9f1-4af6-94ca-16d8e864706e
caps.latest.revision: 41
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 17e1b051fed32e78cd18cc634b7688245ecb0972
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="splinkpublication-transact-sql"></a>sp_link_publication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Définit les informations de configuration et de sécurité utilisées par les déclencheurs de synchronisation des abonnements avec mise à jour immédiate lors de la connexion au serveur de publication. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
> [!IMPORTANT]  
>  Sous certaines conditions, cette procédure stockée peut échouer si l’abonné est en cours d’exécution [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 ou version ultérieure et le serveur de publication exécute une version antérieure. Si la procédure stockée échoue dans ce scénario, mettez à niveau l'Éditeur vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 ou version ultérieure.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_link_publication [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'   
        , [ @publication = ] 'publication'   
        , [ @security_mode = ] security_mode  
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ]'password' ]  
    [ , [ @distributor = ] 'distributor' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publisher**=] **'***publisher***'**  
 Nom du serveur de publication auquel se connecter. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [ **@publisher_db**=] **'***publisher_db***'**  
 Nom de la base de données du serveur de publication à laquelle se connecter. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@publication**=] **'***publication***'**  
 Nom de la publication à laquelle se connecter. *publication* est **sysname**, sans valeur par défaut.  
  
 [ **@security_mode**=] *security_mode*  
 Mode de sécurité utilisé par l'Abonné pour se connecter à un serveur de publication distant pour la mise à jour immédiate. *security_mode* est **int**, et peut prendre l’une des valeurs suivantes. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification avec la connexion spécifiée dans cette procédure stockée en tant que *connexion* et *mot de passe*.<br /><br /> Remarque : dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette option a été utilisée pour spécifier un appel de procédure distante dynamique (RPC).|  
|**1**|Utilise le contexte de sécurité (authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Windows) de l'utilisateur apportant la modification sur l'Abonné.<br /><br /> Remarque : Ce compte doit également exister sur le serveur de publication avec des privilèges suffisants. Lorsque vous utilisez l'authentification Windows, la délégation de compte de sécurité doit être prise en charge.|  
|**2**|Utilise une connexion au serveur lié existante, définie par l’utilisateur créée à l’aide de **sp_link_publication**.|  
  
 [ **@login**=] **'***connexion***'**  
 Connexion d'accès. *login* est de type **sysname**, avec NULL comme valeur par défaut. Ce paramètre doit être spécifié quand *security_mode* est **0**.  
  
 [ **@password**=] **'***mot de passe***'**  
 Est le mot de passe. *mot de passe* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre doit être spécifié quand *security_mode* est **0**.  
  
 [  **@distributor=** ] **'***distributeur***'**  
 Est le nom du serveur de distribution. *serveur de distribution* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_link_publication** est utilisé par les abonnements avec mise à jour immédiatement dans la réplication transactionnelle.  
  
 **sp_link_publication** peut être utilisé pour les abonnements envoyés et extraits. Cette procédure peut être appelée avant ou après la création de l’abonnement. Une entrée est insérée ou mise à jour dans le [MSsubscription_properties &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) (table système).  
  
 Pour les abonnements envoyés, l’entrée peut être nettoyée par [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). Pour les abonnements extraits, l’entrée peut être nettoyée par [sp_droppullsubscription &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md) ou [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). Vous pouvez également appeler **sp_link_publication** avec un mot de passe NULL pour effacer l’entrée dans le [MSsubscription_properties &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) (table système) pour les problèmes de sécurité.  
  
 Le mode par défaut utilisé par un Abonné de mise à jour immédiate lors de sa connexion au serveur de publication n'autorise pas l'utilisation de l'authentification Windows. La connexion avec un mode d'authentification Windows suppose qu'un serveur lié a été configuré au niveau du serveur de publication, et l'Abonné de mise à jour immédiate doit utiliser cette connexion lors de la mise à jour de l'Abonné. Cela requiert la **sp_link_publication** de s’exécuter avec *security_mode* = **2**. Lorsque vous utilisez l'authentification Windows, la délégation de compte de sécurité doit être prise en charge.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent_failover](../../relational-databases/replication/codesnippet/tsql/sp-link-publication-tran_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_link_publication**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
