---
title: sp_adddistpublisher (Transact-SQL) | Documents Microsoft
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
- sp_adddistpublisher
- sp_adddistpublisher_TSQL
helpviewer_keywords:
- sp_adddistpublisher
ms.assetid: 04e15011-a902-4074-b38c-3ec2fc73b838
caps.latest.revision: 35
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3ccd9c550df21ea904c4ff8d8c7a3941908e59a2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spadddistpublisher-transact-sql"></a>sp_adddistpublisher (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Configure un serveur de publication pour qu'il utilise une base de données de distribution spécifique. Cette procédure stockée est exécutée sur une base de données du serveur. Notez que les procédures stockées [sp_adddistributor &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) et [sp_adddistributiondb &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) doit avoir été exécuté avant d’utiliser cette procédure procédure.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddistpublisher [ @publisher= ] 'publisher'   
        , [ @distribution_db= ] 'distribution_db'   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @working_directory= ] 'working_directory' ]   
    [ , [ @trusted= ] 'trusted' ]   
    [ , [ @encrypted_password= ] encrypted_password ]   
    [ , [ @thirdparty_flag = ] thirdparty_flag ]  
    [ , [ @publisher_type = ] 'publisher_type' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [  **@distribution_db=**] **'***distribution_db***'**  
 Est le nom de la base de données de distribution. *distributor_db* est **sysname**, sans valeur par défaut. Il est utilisé par les agents de réplication pour se connecter au serveur de publication.  
  
 [  **@security_mode=**] *security_mode*  
 Représente le mode de sécurité implémenté. Ce paramètre est uniquement utilisé par les agents de réplication afin de se connecter au serveur de publication pour les abonnements de mise à jour en attente ou à un serveur de publication non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *security_mode* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Les Agents de réplication situés sur le serveur de distribution utilisent l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter au serveur de publication.|  
|**1** (par défaut)|Les agents de réplication situés sur le serveur de distribution utilisent l'authentification Windows pour se connecter au serveur de publication.|  
  
 [  **@login=**] **'***connexion***'**  
 Connexion d'accès. Ce paramètre est obligatoire si *security_mode* est **0**. *login* est de type **sysname**, avec NULL comme valeur par défaut. Il est utilisé par les agents de réplication pour se connecter au serveur de publication.  
  
 [  **@password=**] **'***mot de passe***'**]  
 Est le mot de passe. *mot de passe* est **sysname**, avec NULL comme valeur par défaut. Il est utilisé par les agents de réplication pour se connecter au serveur de publication.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort.  
  
 [  **@working_directory=**] **'***working_directory***'**  
 Nom du répertoire de travail utilisé pour stocker les fichiers de données et de schéma destinés à la publication. *working_directory* est **nvarchar (255)** et les valeurs par défaut pour le dossier ReplData pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple 'C:\Program Files\Microsoft SQL Server\MSSQL\MSSQ.1\ReplData'. Le nom doit être indiqué au format UNC.  
  
 [  **@trusted=**] **'***approuvé***'**  
 Ce paramètre est déconseillé et n'est fourni qu'à des fins de compatibilité ascendante. *approuvé* est **nvarchar (5)** et la valeur n’est pas défini **false** entraîne une erreur.  
  
 [  **@encrypted_password=**] *encrypted_password*  
 Paramètre *encrypted_password* n’est plus pris en charge. Tentative de définition de cette **bits** paramètre **1** entraîne une erreur.  
  
 [  **@thirdparty_flag =**] *thirdparty_flag*  
 Indique que le serveur de publication est [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *thirdparty_flag* est **bits**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données.|  
|**1**|Base de données autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
 [ **@publisher_type**=] **'***publisher_type***'**  
 Spécifie le type du serveur de publication lorsque celui-ci n'est pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *publisher_type* est de type sysname et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**MSSQLSERVER**<br /><br /> (par défaut)|Spécifie un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Spécifie un serveur de publication Oracle standard.|  
|**ORACLE GATEWAY**|Spécifie un serveur de publication Oracle Gateway.|  
  
 Pour plus d’informations sur les différences entre un serveur de publication Oracle et un serveur de publication Oracle Gateway, consultez [configurer un serveur de publication Oracle](../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddistpublisher** est utilisé par la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistpublisher-tran_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_adddistpublisher**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)  
  
  
