---
title: Sys.dm_exec_connections (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_connections_TSQL
- sys.dm_exec_connections_TSQL
- sys.dm_exec_connections
- dm_exec_connections
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_connections dynamic management view
ms.assetid: 6bd46fe1-417d-452d-a9e6-5375ee8690d8
caps.latest.revision: 50
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 27499bf31561addd909d92a81222662e4470b8b5
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecconnections-transact-sql"></a>sys.dm_exec_connections (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations sur les connexions établies à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les détails de chaque connexion. Retourne des informations de connexion au niveau serveur pour SQL Server. Retourne des informations de connexion de base de données en cours pour la base de données SQL.  
  
> [!NOTE]
> Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez [sys.dm_pdw_exec_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql.md).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Identifie la session associée à cette connexion. Autorise la valeur NULL.|  
|most_recent_session_id|**int**|Représente l'ID de session de la requête la plus récente associée à cette connexion. (Les connexions SOAP peuvent être réutilisées par une autre session.) Autorise la valeur NULL.|  
|connect_time|**datetime**|Cachet temporel d'établissement de la connexion. N'accepte pas la valeur NULL.|  
|net_transport|**nvarchar(40)**|Retourne toujours **Session** lorsqu’une connexion comporte plusieurs jeux de résultats actifs (MARS) activé.<br /><br /> **Remarque :** décrit le protocole de transport physique utilisé par cette connexion. N'accepte pas la valeur NULL.|  
|protocol_type|**nvarchar(40)**|Spécifie le type de protocole de la charge utile. Il effectue la distinction entre TDS (TSQL) et SOAP. Autorise la valeur NULL.|  
|protocol_version|**int**|Version du protocole d'accès aux données associé à cette connexion. Autorise la valeur NULL.|  
|endpoint_id|**int**|Identificateur qui décrit le type de connexion. endpoint_id peut être utilisé pour interroger la vue sys.endpoints. Autorise la valeur NULL.|  
|encrypt_option|**nvarchar(40)**|Valeur booléenne pour décrire le chiffrement activé pour cette connexion. N'accepte pas la valeur NULL.|  
|auth_scheme|**nvarchar(40)**|Spécifie le schéma d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Windows utilisé avec cette connexion. N'accepte pas la valeur NULL.|  
|node_affinity|**smallint**|Identifie le nœud de mémoire avec lequel cette connexion présente une affinité. N'accepte pas la valeur NULL.|  
|num_reads|**int**|Nombre de lectures d’octets qui se sont produites sur cette connexion. Autorise la valeur NULL.|  
|num_writes|**int**|Nombre d’écritures d’octets qui se sont produites sur cette connexion. Autorise la valeur NULL.|  
|last_read|**datetime**|Cachet temporel de la dernière lecture à travers cette connexion. Autorise la valeur NULL.|  
|last_write|**datetime**|Cachet temporel de la dernière écriture à travers cette connexion. N'accepte pas la valeur NULL.|  
|net_packet_size|**int**|Taille du paquet réseau utilisé pour le transfert d'informations et de données. Autorise la valeur NULL.|  
|client_net_address|**varchar(48)**|Adresse hôte du client se connectant à ce serveur. Autorise la valeur NULL.<br /><br /> Avant V12 dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette colonne renvoie toujours la valeur NULL.|  
|client_tcp_port|**int**|Numéro de port sur l'ordinateur client associé à cette connexion. Autorise la valeur NULL.<br /><br /> Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette colonne retourne toujours NULL.|  
|local_net_address|**varchar(48)**|Représente l'adresse IP sur le serveur ciblé par cette connexion. Disponible uniquement pour les connexions utilisant le fournisseur de transport TCP. Autorise la valeur NULL.<br /><br /> Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette colonne retourne toujours NULL.|  
|local_tcp_port|**int**|Représente le port TCP du serveur ciblé par cette connexion s'il s'agissait d'une connexion utilisant le transport TCP. Autorise la valeur NULL.<br /><br /> Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette colonne retourne toujours NULL.|  
|connection_id|**uniqueidentifier**|Identifie chaque connexion de façon unique. N'accepte pas la valeur NULL.|  
|parent_connection_id|**uniqueidentifier**|Identifie la connexion principale utilisée par la session MARS. Autorise la valeur NULL.|  
|most_recent_sql_handle|**varbinary(64)**|Descripteur SQL de la dernière requête exécutée sur cette connexion. La colonne most_recent_sql_handle est toujours synchronisée avec la colonne most_recent_session_id. Autorise la valeur NULL.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures pour sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-exec-connections-1.gif "jointures pour sys.dm_exec_connections")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
||||  
|-|-|-|  
|dm_exec_sessions.session_id|dm_exec_connections.session_id|Un à un|  
|dm_exec_requests.connection_id|dm_exec_connections.connection_id|Plusieurs-à-un|  
|dm_broker_connections.connection_id|dm_exec_connections.connection_id|Un-à-un|  
  
## <a name="examples"></a>Exemples  
 Requête typique pour collecter des informations sur une connexion propre aux requêtes.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>Voir aussi  

 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


