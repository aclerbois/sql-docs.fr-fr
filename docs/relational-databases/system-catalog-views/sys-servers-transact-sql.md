---
title: Sys.Servers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- servers_TSQL
- sys.servers_TSQL
- servers
- sys.servers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.servers catalog view
ms.assetid: 4e774ed9-4e83-4726-9f1d-8efde8f9feff
caps.latest.revision: 53
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 17a270028d94974643c1993730e353cea30dd897
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysservers-transact-sql"></a>sys.servers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]

  Contient une ligne pour chaque serveur distant ou lié enregistré et une ligne pour le serveur local a **server_id** = 0.  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|ID local du serveur lié.|  
|**nom**|**sysname**|Lorsque **server_id** = 0, il s’agit du nom du serveur.<br /><br /> Lorsque **server_id** > 0, c’est le nom local du serveur lié.|  
|**product**|**sysname**|Nom de produit du serveur lié. « SQL Server » indique qu'il s'agit d'une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Fournisseur**|**sysname**|Nom du fournisseur OLE DB permettant de se connecter au serveur lié.|  
|**data_source**|**nvarchar(4000)**|Propriété de connexion à la source de données OLE DB.|  
|**Emplacement**|**nvarchar(4000)**|Propriété de connexion de l'emplacement OLE DB. NULL si aucun.|  
|**provider_string**|**nvarchar(4000)**|Propriété de connexion à la chaîne du fournisseur OLE DB.<br /><br /> A pour valeur NULL sauf si l'appelant dispose de l'autorisation ALTER ANY LINKED SERVER.|  
|**catalog**|**sysname**|Propriété d'une connexion au catalogue OLEDB. NULL si aucun.|  
|**connect_timeout**|**int**|Délai d'expiration de la connexion en secondes, 0 si aucun.|  
|**query_timeout**|**int**|Délai d'expiration de la requête en secondes, 0 si aucun.|  
|**is_linked**|**bit**|0 = est un ancien serveur ajouté à l’aide de **sp_addserver**, avec un comportement de transaction distribuée et RPC différents.<br /><br /> 1 = Serveur lié standard.|  
|**is_remote_login_enabled**|**bit**|L'option RPC est active et permet les connexions entrantes à distance pour ce serveur.|  
|**is_rpc_out_enabled**|**bit**|RPC sortant (depuis ce serveur) activé.|  
|**is_data_access_enabled**|**bit**|Les requêtes distribuées sont activées sur ce serveur.|  
|**is_collation_compatible**|**bit**|Le classement des données distantes est supposé être compatible avec les données locales en l'absence d'informations sur le classement.|  
|**uses_remote_collation**|**bit**|Si 1, utiliser le classement indiqué par le serveur distant. Sinon, utiliser le classement spécifié dans la colonne suivante.|  
|**collation_name**|**sysname**|Nom du classement à utiliser ou valeur NULL s'il faut simplement utiliser le classement local.|  
|**lazy_schema_validation**|**bit**|Si 1, la validation de schéma n'est pas activée au démarrage de la requête.|  
|**is_system**|**bit**|Ce serveur est uniquement accessible par le système interne.|  
|**is_publisher**|**bit**|Le serveur est un serveur de publication de réplication.|  
|**is_subscriber**|**bit**|Le serveur est un abonné de réplication.|  
|**is_distributor**|**bit**|Le serveur est un serveur de distribution de réplication.|  
|**is_nonsql_subscriber**|**bit**|Le serveur est un abonné de réplication non-SQL Server.|  
|**is_remote_proc_transaction_promotion_enabled**|**bit**|Si la valeur est 1, l'appel d'une procédure stockée distante démarre une transaction distribuée et enregistre la transaction dans MS DTC. Pour plus d’informations, consultez [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md).|  
|**modify_date**|**datetime**|Date de la dernière modification des informations de serveur.|  
  
## <a name="permissions"></a>Autorisations  
 La valeur de **provider_string** est toujours NULL sauf si l’appelant a l’autorisation ALTER ANY LINKED SERVER.  
  
 Autorisations ne sont pas requises pour afficher le serveur local (**server_id** = 0).  
  
 Lorsque vous créez un serveur lié ou distant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un mappage de connexion par défaut pour le **public** rôle de serveur. Cela signifie que, par défaut, toutes les connexions peuvent accéder à l'ensemble des serveurs liés et distants. Pour restreindre la visibilité de ces serveurs, supprimez le mappage de connexion par défaut en exécutant [sp_droplinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md) et en spécifiant la valeur NULL pour le *locallogin* paramètre.  
  
 Si le mappage de connexion par défaut est supprimé, seuls les utilisateurs ayant explicitement été ajoutés en tant que connexion liée ou connexion distante peuvent voir les serveurs liés ou distants pour lesquels ils disposent d'un nom de connexion. Pour pouvoir accéder à tous les serveurs liés et distants après la suppression du mappage de connexion par défaut, vous devez disposer des autorisations suivantes :  
  
-   ALTER ANY LINKED SERVER ou ALTER ANY LOGIN ON SERVER  
  
-   L’appartenance à la **setupadmin** ou **sysadmin** rôles serveur fixes  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues du catalogue des serveurs liés &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)  
  
  
