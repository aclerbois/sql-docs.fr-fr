---
title: sp_helpsubscription_properties (Transact-SQL) | Documents Microsoft
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
- sp_helpsubscription_properties
- sp_helpsubscription_properties_TSQL
helpviewer_keywords:
- sp_helpsubscription_properties
ms.assetid: 7a76a645-97eb-47ac-b3ea-e2d75012cbed
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 83f6699993f9ee4d0d3df4662bb75e4e07edb942
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpsubscriptionproperties-transact-sql"></a>sp_helpsubscription_properties (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Récupère les informations de sécurité à partir de la [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) table. Cette procédure stockée est exécutée sur l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpsubscription_properties [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db =] 'publisher_db' ]   
    [ , [ @publication =] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur tous les serveurs de publication.  
  
 [  **@publisher_db=**] **'***publisher_db***'**  
 Nom de la base de données du serveur de publication. *publisher_db* est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur toutes les bases de données de serveur de publication.  
  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur toutes les publications.  
  
 [  **@publication_type=**] *publication_type*  
 Est du type de publication. *publication_type* est **int**, avec NULL comme valeur par défaut. Si fourni, *publication_type* doit être une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle|  
|**1**|Publication d'instantané|  
|**2**|Publication de fusion|  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher** (serveur de publication)|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = transactionnelle<br /><br /> **1** = instantané<br /><br /> **2** = fusion|  
|**publisher_login**|**sysname**|ID de connexion utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**publisher_password**|**nvarchar (524)**|Mot de passe utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (chiffré).|  
|**publisher_security_mode**|**int**|Mode de sécurité utilisé sur le serveur de publication.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification<br /><br /> **1** = authentification Windows|  
|**Serveur de distribution**|**sysname**|Nom du serveur de distribution.|  
|**argument**|**sysname**|Connexion du serveur de distribution.|  
|**argument**|**nvarchar (524)**|Mot de passe du serveur de distribution (chiffré).|  
|**argument distributor_security_mode**|**int**|Mode de sécurité utilisé sur le serveur de distribution.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification<br /><br /> **1** = authentification Windows|  
|**ftp_address**|**sysname**|Pour compatibilité descendante uniquement. Adresse réseau du service FTP (File Transfer Protocol) du serveur de distribution.|  
|**ftp_port**|**int**|Pour compatibilité descendante uniquement. Numéro de port du service FTP du serveur de distribution.|  
|**ftp_login**|**sysname**|Pour compatibilité descendante uniquement. Nom d'utilisateur permettant la connexion au service FTP.|  
|**ftp_password**|**nvarchar (524)**|Pour compatibilité descendante uniquement. Mot de passe de l’utilisateur, utilisé pour la connexion au service FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**working_directory**|**nvarchar(255)**|Nom du répertoire de travail utilisé pour stocker les fichiers de schéma et de données.|  
|**use_ftp**|**bit**|Spécifie l’utilisation de FTP au lieu du protocole usuel pour extraire les instantanés. Si **1**, FTP est utilisé.|  
|**argument dts_package_name**|**sysame**|Spécifie le nom du package DTS (Data Transformation Services).|  
|**dts_package_password**|**nvarchar (524)**|Spécifie le mot de passe du package, s'il existe.|  
|**dts_package_location**|**int**|Emplacement où le package DTS est stocké.<br /><br /> **0** = le package se trouve sur le serveur de distribution.<br /><br /> **1** = le package se trouve sur l’abonné.|  
|**offload_agent**|**bit**|Indique si l'agent peut être activé à distance. Si **0**, l’agent ne peut pas être activé à distance.|  
|**offload_server**|**sysname**|Indique le nom de réseau du serveur utilisé pour l'activation à distance.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Spécifie le chemin d'accès au dossier où les fichiers d'instantané sont enregistrés.|  
|**use_web_sync**|**bit**|Spécifie si l’abonnement peut être synchronisé via HTTPS, où la valeur **1** signifie que cette fonctionnalité est activée.|  
|**internet_url**|**nvarchar(260)**|URL qui représente l'emplacement de l'écouteur de réplication pour la synchronisation Web.|  
|**internet_login**|**nvarchar(128)**|Connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_password**|**nvarchar (524)**|Mot de passe de la connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_security_mode**|**int**|Le mode d’authentification utilisé pour se connecter au serveur Web qui héberge la synchronisation Web, où la valeur **1** signifie que l’authentification Windows et la valeur **0** signifie que l’authentification de base.|  
|**internet_timeout**|**int**|Délai en secondes avant l'expiration d'une demande de synchronisation Web.|  
|**Nom d’hôte**|**nvarchar(128)**|Indique la valeur de HOST_NAME() lorsque cette fonction est utilisée dans la clause WHERE du filtre de lignes paramétrable.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpsubscription_properties** est utilisé dans la réplication de capture instantanée, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_helpsubscription_properties**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
