---
title: Sys.availability_group_listener_ip_addresses (Transact-SQL) | Documents Microsoft
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
- availability_group_listener_ip_addresses
- sys.availability_group_listener_ip_addresses
- availability_group_listener_ip_addresses_TSQL
- sys.availability_group_listener_ip_addresses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.availability_group_listener_ip_addresses catalog view
ms.assetid: e515fa6b-1354-4110-9b70-ab2e6164c992
caps.latest.revision: 12
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 4040838a66333fb72ec4f1beb4b55fba7dfe6195
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysavailabilitygrouplisteneripaddresses-transact-sql"></a>sys.availability_group_listener_ip_addresses (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque adresse IP qui est associé à n’importe quel Always On écouteur dans le cluster de Clustering de basculement Windows Server (WSFC).  
  
 Clé primaire : **listener_id** + **adresse_IP** + **ip_sub_mask**  
  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**listener_id**|**nvarchar(36)**|GUID de ressource de cluster de clustering de basculement Windows Server (WSFC).|  
|**ip_address**|**nvarchar(48)**|Adresse IP virtuelle configurée de l'écouteur du groupe de disponibilité. Retourne une seule adresse IPv4 ou IPv6.|  
|**ip_subnet_mask**|**nvarchar(15)**|Masque de sous-réseau IP configuré pour l'adresse IPv4, le cas échéant, qui est configurée pour l'écouteur du groupe de disponibilité.<br /><br /> NULL = Sous-réseau IPv6|  
|**is_dhcp**|**bit**|Indique si l'adresse IP est configurée par DHCP, une des valeurs suivantes :<br /><br /> 0 = L'adresse IP n'est pas configurée par DHCP.<br /><br /> 1 = L'adresse IP est configurée par DHCP|  
|**network_subnet_ip**|**nvarchar(48)**|Adresse IP de sous-réseau de réseau qui spécifie le sous-réseau auquel l'adresse IP appartient.|  
|**network_subnet_prefix_length**|**int**|Longueur de préfixe de sous-réseau de réseau du sous-réseau auquel l'adresse IP appartient.|  
|**network_subnet_ipv4_mask**|**nvarchar(45)**|Masque de sous-réseau de réseau du sous-réseau auquel l'adresse IP appartient. **network_subnet_ipv4_mask** pour spécifier les options DHCP < network_subnet_option > dans une clause WITH DHCP de la [créer un groupe de disponibilité](../../t-sql/statements/create-availability-group-transact-sql.md) ou [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.<br /><br /> NULL = Sous-réseau IPv6|  
|**state**|**tinyint**|État de la ressource IP ONLINE/OFFLINE du cluster WSFC, un des suivants :<br /><br /> 1 = En ligne. La ressource IP est en ligne.<br /><br /> 0 = Hors connexion. La ressource IP est hors connexion.<br /><br /> 2 = En ligne en attente. La ressource IP est hors connexion mais est remise en ligne.<br /><br /> 3 = Échec. La ressource IP est remise en ligne mais un échec se produit.|  
|**state_desc**|**nvarchar(60)**|Description de **état**, un des :<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> ONLINE_PENDING<br /><br /> FAILED|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Interrogation des catalogues système SQL Server FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
