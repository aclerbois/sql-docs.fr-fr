---
title: sp_helpsubscription (Transact-SQL) | Documents Microsoft
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
f1_keywords:
- sp_helpsubscription_TSQL
- sp_helpsubscription
helpviewer_keywords:
- sp_helpsubscription
ms.assetid: ff96bcbf-e2b9-4da8-8515-d80d4ce86c16
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 0634a1b6cd117b82d31324e58590d9217f402528
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpsubscription-transact-sql"></a>sp_helpsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche des informations sur les abonnements associés à une publication, un article, un Abonné ou un ensemble d'abonnements particuliers. Cette procédure stockée est exécutée sur la base de données de publication d'un serveur de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpsubscription [ [ @publication = ] 'publication' ]   
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]   
    [ , [ @found=] found OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =** ] **'***publication***'**  
 Nom de la publication associée. *publication* est **sysname**, avec une valeur par défaut **%**, qui retourne toutes les informations d’abonnement pour ce serveur.  
  
 [  **@article=** ] **'***article***'**  
 Nom de l'article. *article* est **sysname**, avec une valeur par défaut **%**, qui retourne toutes les informations d’abonnement pour les publications sélectionnées et les abonnés. Si **tous les**, une seule entrée est renvoyée pour l’abonnement complet à une publication.  
  
 [  **@subscriber=** ] **'***abonné***'**  
 Nom de l'Abonné dont vous voulez connaître les informations sur l'abonnement. *abonné* est **sysname**, avec une valeur par défaut **%**, qui retourne toutes les informations d’abonnement pour les publications et articles sélectionnés.  
  
 [  **@destination_db=** ] **'***destination_db***'**  
 Nom de la base de données de destination. *destination_db* est **sysname**, avec une valeur par défaut **%**.  
  
 [  **@found=** ] **'***trouvé***'** sortie  
 Indicateur désignant les lignes retournées. *trouvé*est **int** d’un paramètre OUTPUT, avec 23456 comme valeur par défaut.  
  
 **1** indique la publication a été trouvée.  
  
 **0** indique la publication est introuvable.  
  
 [ **@publisher**=] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**et la valeur par défaut est le nom du serveur actuel.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être spécifié, sauf si elle est un serveur de publication Oracle.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**subscriber** (Abonné)|**sysname**|Nom de l'Abonné.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**article**|**sysname**|Nom de l'article.|  
|**base de données de destination**|**sysname**|Nom de la base de données de destination où sont placées les données répliquées.|  
|**état de l’abonnement**|**tinyint**|État de l'abonnement :<br /><br /> **0** = inactif<br /><br /> **1** = abonné<br /><br /> **2** = actif|  
|**type de synchronisation**|**tinyint**|Type de synchronisation d'abonnement :<br /><br /> **1** = automatique<br /><br /> **2** = none|  
|**type d’abonnement**|**int**|Type d'abonnement :<br /><br /> **0** = par envoi de données<br /><br /> **1** = par extraction de données<br /><br /> **2** = anonyme|  
|**abonnement complet**|**bit**|Indique si l'abonnement concerne tous les articles de la publication :<br /><br /> **0** = Non<br /><br /> **1** = Oui|  
|**Nom de l’abonnement**|**nvarchar(255)**|Nom de l'abonnement.|  
|**mode de mise à jour**|**int**|**0** = lecture seule<br /><br /> **1** = abonnement de mise à jour immédiate|  
|**id de tâche de distribution**|**binary (16)**|ID du travail de l'Agent de distribution.|  
|**loopback_detection**|**bit**|La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **0** = renvoie les transactions.<br /><br /> **1** = ne pas renvoyer.<br /><br /> Utilisé avec la réplication transactionnelle bidirectionnelle. Pour plus d'informations, voir [Réplication transactionnelle bidirectionnelle](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|**offload_enabled**|**bit**|Indique si l'exécution du déchargement d'un Agent de réplication est configurée pour être exécuté sur l'Abonné.<br /><br /> Si **0**, l’agent est exécuté sur le serveur de publication.<br /><br /> Si **1**, l’agent est exécuté sur l’abonné.|  
|**offload_server**|**sysname**|Nom du serveur activé pour l'activation d'Agent à distance. Si NULL, puis la valeur offload_server figurant [MSdistribution_agents](../../relational-databases/system-tables/msdistribution-agents-transact-sql.md) table est utilisée.|  
|**argument dts_package_name**|**sysname**|Spécifie le nom du package DTS (Data Transformation Services).|  
|**dts_package_location**|**int**|Emplacement du package DTS (si un lot est affecté à l'abonnement). S’il existe un package, une valeur de **0** Spécifie l’emplacement du package à le **distributeur**. La valeur **1** Spécifie le **abonné**.|  
|**subscriber_security_mode**|**smallint**|Mode de sécurité sur l’abonné, où **1** signifie que l’authentification Windows, et **0** signifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**subscriber_login**|**sysname**|Nom de connexion sur l'Abonné.|  
|**subscriber_password**||Le mot de passe réel de l'Abonné n'est jamais renvoyé. Le résultat est masqué par un «**\*\*\*\*\*\***« chaîne.|  
|**job_login**|**sysname**|Nom du compte Windows sous lequel l'Agent de distribution s'exécute.|  
|**job_password**||Le mot de passe réel du travail n'est jamais renvoyé. Le résultat est masqué par un «**\*\*\*\*\*\***« chaîne.|  
|**distrib_agent_name**|**nvarchar(100)**|Nom du travail de l'Agent qui synchronise l'abonnement.|  
|**subscriber_type**|**tinyint**|Type d'Abonné, parmi les types suivants :<br /><br /> **0** = abonné SQL Server<br /><br /> **1** = serveur de source de données ODBC<br /><br /> **2** = base de données Microsoft JET (déconseillée)<br /><br /> **3** = fournisseur OLE DB|  
|**subscriber_provider**|**sysname**|Identificateur de programme unique (PROGID) avec lequel le fournisseur OLE DB de la source de données non-SQL Server est inscrit.|  
|**subscriber_datasource**|**nvarchar(4000)**|Nom de la source de données tel qu'il est interprété par le fournisseur OLE DB.|  
|**subscriber_providerstring**|**nvarchar(4000)**|Chaîne de connexion propre au fournisseur OLE DB qui identifie la source de données.|  
|**subscriber_location**|**nvarchar(4000)**|Emplacement de la base de données tel qu'il est interprété par le fournisseur OLE DB.|  
|**subscriber_catalog**|**sysname**|Catalogue à utiliser lors d’une connexion au fournisseur OLE DB.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpsubscription** est utilisé dans la réplication transactionnelle et d’instantané.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution reviennent par défaut au rôle **public** . Seules les informations des abonnements qu'ils ont créés sont renvoyées aux utilisateurs. Pour plus d’informations sur tous les abonnements sont renvoyées aux membres de la **sysadmin** rôle serveur fixe sur le serveur de publication ou les membres de la **db_owner** rôle de base de données fixe sur la base de données de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
