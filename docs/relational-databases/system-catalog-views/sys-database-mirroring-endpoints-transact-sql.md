---
title: Sys.database_mirroring_endpoints (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.database_mirroring_endpoints_TSQL
- database_mirroring_endpoints
- sys.database_mirroring_endpoints
- database_mirroring_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HADR [SQL Server], endpoint
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_endpoints catalog view
ms.assetid: f2285199-97ad-473c-a52d-270044dd862b
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 614a57100ffa8e66b1f03757355605409ac5848f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabasemirroringendpoints-transact-sql"></a>sys.database_mirroring_endpoints (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour le point de terminaison de mise en miroir de bases de données d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Le point de terminaison de mise en miroir de base de données prend en charge les sessions entre les partenaires de mise en miroir de base de données et les témoins et les sessions entre le réplica principal d’un groupe de disponibilité Always On et ses réplicas secondaires.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**\<héritée de colonnes >**|—|Hérite des colonnes de **sys.endpoints** (pour plus d’informations, consultez [sys.endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)).|  
|**Rôle**|**tinyint**|Rôle de mise en miroir. Peut prendre l'une des valeurs suivantes :<br /><br /> **0** = none<br /><br /> **1** = partner<br /><br /> **2** = témoin<br /><br /> **3** = all<br /><br /> Remarque : Cette valeur s’applique uniquement à la mise en miroir de base de données.|  
|**role_desc**|**nvarchar(60)**|Description du rôle de mise en miroir. Peut prendre l'une des valeurs suivantes :<br /><br /> **NONE**<br /><br /> **PARTENAIRE**<br /><br /> **TÉMOIN**<br /><br /> **ALL**<br /><br /> Remarque : Cette valeur s’applique uniquement à la mise en miroir de base de données.|  
|**is_encryption_enabled**|**bit**|**1** signifie que le chiffrement est activé.<br /><br /> **0** signifie que le chiffrement est désactivé.|  
|**connection_auth**|**tinyint**|Type d'authentification de connexion requis pour les connexions à ce point de terminaison. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** -NTLM<br /><br /> **2** -KERBEROS<br /><br /> **3** -NÉGOCIER<br /><br /> **4** -CERTIFICAT<br /><br /> **5** -NTLM, DE CERTIFICAT<br /><br /> **6** -KERBEROS, LE CERTIFICAT<br /><br /> **7** -NEGOCIATE, CERTIFICAT<br /><br /> **8** -CERTIFICAT, NTLM<br /><br /> **9** -CERTIFICAT, KERBEROS<br /><br /> **10** -CERTIFICAT, NEGOTIATE|  
|**connection_auth_desc**|**Nvarchar (60)**|Description du type d'authentification requis pour les connexions à ce point de terminaison. Peut prendre l'une des valeurs suivantes :<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE|  
|**certificate_id**|**int**|ID du certificat utilisé pour l'authentification, le cas échéant.<br /><br /> 0 = L'authentification Windows et utilisée.|  
|**encryption_algorithm**|**tinyint**|Algorithme de chiffrement. Peut prendre l'une des valeurs suivantes :<br /><br /> **0** : NONE<br /><br /> **1** – RC4<br /><br /> **2** – AES<br /><br /> **3** : NONE, RC4<br /><br /> **4** : AUCUN, AES<br /><br /> **5** – RC4, AES<br /><br /> **6** – AES, RC4<br /><br /> **7** : NONE, RC4, AES<br /><br /> **8** : AUCUN, AES, RC4|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Description de l'algorithme de chiffrement. Peut prendre l'une des valeurs suivantes :<br /><br /> Aucune<br /><br /> RC4<br /><br /> AES<br /><br /> NONE, RC4<br /><br /> NONE, AES<br /><br /> RC4, AES<br /><br /> AES, RC4<br /><br /> NONE, RC4, AES<br /><br /> NONE, AES, RC4|  
  
## <a name="remarks"></a>Notes  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, le matériel chiffré à l’aide de RC4 ou RC4_128 peut être déchiffré dans n’importe quel niveau de compatibilité.  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Spécifiez l’URL de point de terminaison lorsque vous ajoutez ou modifiez un réplica de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [La base de données mise en miroir du point de terminaison & #40 ; SQL Server & #41 ;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
