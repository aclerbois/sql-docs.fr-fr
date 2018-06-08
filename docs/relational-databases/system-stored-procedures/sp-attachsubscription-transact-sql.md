---
title: sp_attachsubscription (Transact-SQL) | Documents Microsoft
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
- sp_attachsubscription
- sp_attachsubscription_TSQL
helpviewer_keywords:
- sp_attachsubscription
ms.assetid: b9bbda36-a46a-4327-a01e-9cd632e4791b
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f01aa56a511e6a9800e543bb034cf21a9f3496ba
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spattachsubscription-transact-sql"></a>sp_attachsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Attache une base de données d'abonnement existante à un Abonné. Cette procédure stockée est exécutée sur la base de données master du nouvel Abonné.  
  
> [!IMPORTANT]  
>  Cette fonctionnalité est déconseillée et sera retirée dans une version prochaine. Elle ne doit pas être utilisée dans tout nouveau travail de développement. Dans le cas des publications de fusion partitionnées par le biais de filtres paramétrés, nous vous recommandons d'utiliser plutôt les nouvelles fonctionnalités d'instantanés partitionnés qui simplifient l'initialisation de larges volumes d'abonnements. Pour plus d’informations, voir [Instantanés des publications de fusion avec des filtres paramétrés](../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md). Dans le cas de publications qui ne sont pas partitionnées, vous pouvez initialiser un abonnement par le biais d'une sauvegarde. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_attachsubscription [ @dbname = ] 'dbname'  
        , [ @filename = ] 'filename'  
    [ , [ @subscriber_security_mode = ] 'subscriber_security_mode' ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @db_master_key_password = ] 'db_master_key_password' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbname=** ] **'***dbname***'**  
 Chaîne signalant une base de données d'abonnement de destination par son nom. *dbname* est **sysname**, sans valeur par défaut.  
  
 [  **@filename=** ] **'***nom de fichier***'**  
 Est le nom et l’emplacement physique du MDF primaire (**master** fichier de données). *nom de fichier* est **nvarchar (260)**, sans valeur par défaut.  
  
 [  **@subscriber_security_mode=** ] **'***subscriber_security_mode***'**  
 Mode de sécurité à utiliser lors de la connexion à un Abonné au cours d'une synchronisation. *subscriber_security_mode* est **int**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  L'authentification Windows doit être la méthode à employer. Si *subscriber_security_mode* n’est pas **1** (authentification Windows), une erreur est retournée.  
  
 [  **@subscriber_login=** ] **'***subscriber_login***'**  
 Nom d'accès utilisé lors de la connexion à un Abonné au cours d'une synchronisation. *subscriber_login* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts. Si *subscriber_security_mode* n’est pas **1** et *subscriber_login* est spécifié, une erreur est retournée.  
  
 [  **@subscriber_password=** ] **'***subscriber_password***'**  
 Mot de passe de l'Abonné. *subscriber_password* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts. Si *subscriber_security_mode* n’est pas **1** et *subscriber_password* est spécifié, une erreur est retournée.  
  
 [  **@distributor_security_mode=** ] *distributor_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un serveur de distribution au cours d'une synchronisation. *l’argument distributor_security_mode* est **int**, avec une valeur par défaut **0**. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** Spécifie l’authentification Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 [  **@distributor_login=** ] **'***distributor_login***'**  
 Nom de connexion du serveur de distribution à utiliser lors de la connexion au cours d'une synchronisation. *Cet argument* est requise si *distributor_security_mode* a la valeur **0**. *Cet argument* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@distributor_password=** ] **'***distributor_password***'**  
 Mot de passe du serveur de distribution. *distributor_password* est requise si *distributor_security_mode* a la valeur **0**. *distributor_password* est **sysname**, avec NULL comme valeur par défaut. La valeur de *distributor_password* doit être inférieure à 120 caractères Unicode.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@publisher_security_mode=** ] *publisher_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un serveur de publication au cours d'une synchronisation. *publisher_security_mode* est **int**, avec une valeur par défaut **1**. Si **0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 [  **@publisher_login=** ] **'***publisher_login***'**  
 Nom de connexion à utiliser lors de la connexion à un serveur de publication pendant la synchronisation. *publisher_login* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@publisher_password=** ] **'***publisher_password***'**  
 Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est **sysname**, avec NULL comme valeur par défaut. La valeur de *publisher_password* doit être inférieure à 120 caractères Unicode.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@job_login=** ] **'***job_login***'**  
 Nom de connexion du compte Windows sous lequel l'Agent s'exécute. *job_login* est **nvarchar (257)**, sans valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions des agents au serveur de distribution.  
  
 [  **@job_password=** ] **'***job_password***'**  
 Mot de passe du compte Windows sous lequel l'Agent s'exécute. *job_password* est **sysname**, sans valeur par défaut. La valeur de *job_password* doit être inférieure à 120 caractères Unicode.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@db_master_key_password=** ] **'***db_master_key_password***'**  
 Mot de passe d'une clé principale personnalisée de base de données. *db_master_key_password* est **nvarchar (524)**, avec NULL comme valeur par défaut. Si *db_master_key_password* n’est pas spécifié, une clé principale de base de données existante sera supprimée et recréée.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_attachsubscription** est utilisé dans la réplication de capture instantanée, la réplication transactionnelle et la réplication de fusion.  
  
 Un abonnement ne peut pas être attaché à la publication si la période de rétention de celle-ci a expiré. Si vous spécifiez un abonnement dont la période de rétention est écoulée, une erreur se produit lors de l'attachement de l'abonnement ou de sa première synchronisation. Publications avec une période de rétention de publication de **0** (n’expire jamais) sont ignorés.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_attachsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
