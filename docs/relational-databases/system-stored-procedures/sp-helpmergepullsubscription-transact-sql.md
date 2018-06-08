---
title: sp_helpmergepullsubscription (Transact-SQL) | Documents Microsoft
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
- sp_helpmergepullsubscription
- sp_helpmergepullsubscription_TSQL
helpviewer_keywords:
- sp_helpmergepullsubscription
ms.assetid: 6f3125f3-0dfa-40bd-b725-8aa1591234f6
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 905ead01709a932639f72e874f1246228f2f9c21
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpmergepullsubscription-transact-sql"></a>sp_helpmergepullsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur des abonnements par extraction de données (pull) existant sur l'Abonné. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergepullsubscription [ [ @publication=] 'publication']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
```  
  
## <a name="argument"></a>Argument  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec une valeur par défaut **%**. Si *publication* est **%**, plus d’informations sur toutes les publications de fusion et les abonnements dans la base de données actuelle sont retournées.  
  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication*est **sysname**, avec une valeur par défaut **%**.  
  
 [  **@publisher_db=**] **'***publisher_db***'**  
 Nom de la base de données du serveur de publication. *publisher_db*est **sysname**, avec une valeur par défaut **%**.  
  
 [  **@subscription_type=**] **'***subscription_type***'**  
 Indique si les abonnements par extraction de données sont affichés. *subscription_type*est **nvarchar (10)**, avec une valeur par défaut **'pull'**. Les valeurs valides sont **'push'**, **'pull'**, ou **'both'**.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**nvarchar(1000)**|Nom de l'abonnement.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**publisher** (serveur de publication)|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**subscriber** (Abonné)|**sysname**|Nom de l'Abonné.|  
|**argument subscription_db**|**sysname**|Nom de la base de données d'abonnement.|  
|**status**|**int**|État de l'abonnement :<br /><br /> **0** = abonnement inactif<br /><br /> **1** = abonnement actif<br /><br /> **2** = abonnement supprimé<br /><br /> **3** = abonnement détaché<br /><br /> **4** = abonnement attaché<br /><br /> **5** = abonnement a été marqué pour réinitialisation avec chargement<br /><br /> **6** = attachement de l’abonnement a échoué<br /><br /> **7** = abonnement restauré à partir de la sauvegarde|  
|**subscriber_type**|**int**|Type d'Abonné :<br /><br /> **1** = global<br /><br /> **2** = local<br /><br /> **3** = anonyme|  
|**subscription_type**|**int**|Type d'abonnement :<br /><br /> **0** = par envoi de données<br /><br /> **1** = par extraction de données<br /><br /> **2** = anonyme|  
|**priority**|**float(8)**|Priorité de l'abonnement. La valeur doit être inférieure à **100,00**.|  
|**sync_type**|**tinyint**|Type de synchronisation d'abonnement :<br /><br /> **1** = automatique<br /><br /> **2** = instantané n’est pas utilisé.|  
|**description**|**nvarchar(255)**|Brève description de l’abonnement par extraction de données.|  
|**merge_jobid**|**binary (16)**|ID de travail de l'Agent de fusion.|  
|**argument enabled_for_syncmgr**|**int**|Indique si l'abonnement peut être synchronisé à l'aide du gestionnaire de synchronisation de [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**last_updated**|**nvarchar(26)**|Date et heure de la dernière synchronisation de l'abonnement effectuée par l'Agent de fusion.|  
|**publisher_login**|**sysname**|Nom de connexion du serveur de publication.|  
|**publisher_password**|**sysname**|Le mot de passe du serveur de publication.|  
|**publisher_security_mode**|**int**|Spécifie le mode de sécurité du serveur de publication :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification<br /><br /> **1** = authentification Windows|  
|**Serveur de distribution**|**sysname**|Nom du serveur de distribution.|  
|**argument**|**sysname**|Nom de connexion du serveur de distribution.|  
|**argument**|**sysname**|Le mot de passe du serveur de distribution.|  
|**argument distributor_security_mode**|**int**|Spécifie le mode de sécurité du serveur de distribution :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification<br /><br /> **1** = authentification Windows|  
|**ftp_address**|**sysname**|Disponible pour compatibilité descendante uniquement. Est l’adresse réseau du service de protocole (FTP) de transfert de fichier pour le serveur de distribution.|  
|**ftp_port**|**int**|Disponible pour compatibilité descendante uniquement. Numéro de port du service FTP du serveur de distribution.|  
|**ftp_login**|**sysname**|Disponible pour compatibilité descendante uniquement. Le nom d’utilisateur est utilisé pour se connecter au service FTP.|  
|**ftp_password**|**sysname**|Disponible pour compatibilité descendante uniquement. Mot de passe de l'utilisateur utilisé pour la connexion au service FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Emplacement de stockage du dossier d'instantané si cet emplacement est différent ou en complément de l'emplacement par défaut.|  
|**working_directory**|**nvarchar(255)**|Chemin complet du répertoire dans lequel les fichiers d'instantané sont transférés via FTP lorsque cette option est spécifiée.|  
|**use_ftp**|**bit**|Indique si l'abonnement à la publication s'effectue via Internet, et si les propriétés d'adressage FTP sont configurées. Si **0**, abonnement n’utilise pas FTP. Si **1**, l’abonnement utilise FTP.|  
|**offload_agent**|**bit**|Indique si l'Agent peut être activé et exécuté à distance. Si **0**, l’agent ne peut pas être activé à distance.|  
|**offload_server**|**sysname**|Nom du serveur utilisé pour l'activation à distance.|  
|**use_interactive_resolver**|**int**|Indique si le composant résolveur interactif est utilisé ou non au cours de la réconciliation. Si **0**, le résolveur interactif n’est pas utilisé.|  
|**subid**|**uniqueidentifier**|ID de l'Abonné.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Chemin d'accès du dossier dans lequel les fichiers d'instantané sont enregistrés.|  
|**last_sync_status**|**int**|État de la synchronisation :<br /><br /> **1** = début<br /><br /> **2** = a réussi<br /><br /> **3** = en cours<br /><br /> **4** = inactif<br /><br /> **5** = nouvelle tentative après un échec<br /><br /> **6** = Échec<br /><br /> **7** = Échec de validation<br /><br /> **8** = validation réussie<br /><br /> **9** = arrêt demandé|  
|**last_sync_summary**|**sysname**|Description des résultats de la dernière synchronisation.|  
|**use_web_sync**|**bit**|Spécifie si l’abonnement peut être synchronisé via HTTPS, où la valeur **1** signifie que cette fonctionnalité est activée.|  
|**internet_url**|**nvarchar(260)**|URL qui représente l'emplacement de l'écouteur de réplication pour la synchronisation Web.|  
|**internet_login**|**nvarchar(128)**|Connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_password**|**nvarchar (524)**|Mot de passe de la connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_security_mode**|**int**|Mode d'authentification utilisé pour se connecter au serveur Web hôte de la synchronisation Web. La valeur **1** signifie que l’authentification Windows et la valeur **0** signifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**internet_timeout**|**int**|Délai en secondes avant l'expiration d'une demande de synchronisation Web.|  
|**Nom d’hôte**|**nvarchar(128)**|Spécifie une valeur surchargée pour [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) lorsque cette fonction est utilisée dans la clause WHERE du filtre de lignes paramétrable.|  
|**job_login**|**nvarchar(512)**|Est le compte Windows sous lequel l’agent de fusion s’exécute, ce qui est retourné sous la forme *domaine*\\*nom d’utilisateur*.|  
|**job_password**|**sysname**|Pour des raisons de sécurité, une valeur de «**\*\*\*\*\*\*\*\*\*\***» est toujours retournée.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergepullsubscription** est utilisé dans la réplication de fusion. Dans le jeu de résultats, la date renvoyée dans **last_updated** sous la forme *aaaammjj hh:mm:ss.fff*.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe et le **db_owner** du rôle de base de données fixe peut exécuter **sp_helpmergepullsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
