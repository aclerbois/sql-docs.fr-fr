---
title: Sys.database_firewall_rules (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.database_firewall_rules_TSQL
- database_firewall_rules_TSQL
- sys.database_firewall_rules
- database_firewall_rules
dev_langs:
- TSQL
helpviewer_keywords:
- database_firewall_rules
- sys.database_firewall_rules
ms.assetid: 2e821593-3b9f-43d6-a99b-1ceffe177faf
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: ea5b9d5cb50740e349aa6d3cf58bd20f620a8ab6
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabasefirewallrules-azure-sql-database"></a>sys.database_firewall_rules (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Retourne des informations sur les paramètres de pare-feu de niveau base de données associé à votre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Les paramètres de pare-feu de niveau base de données sont particulièrement utiles lors de l’utilisation des utilisateurs de base de données à relation contenant-contenu. Pour plus d’informations, consultez [Utilisateurs de base de données à relation contenant-contenu - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
 Le `sys.database_firewall_rules` vue contient les colonnes suivantes :  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|id|**INTEGER**|Identificateur du paramètre de pare-feu de niveau base de données.|  
|name|**NVARCHAR (128)**|Nom que vous avez choisi pour décrire et distinguer le paramètre de pare-feu de niveau base de données.|  
|start_ip_address|**VARCHAR (50)**|Adresse IP la plus basse dans la plage de paramètres de pare-feu au niveau base de données. Les adresses IP supérieures ou égales à celle-ci peuvent essayer de se connecter à l'instance [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus basse possible est `0.0.0.0`.|  
|end_ip_address|**VARCHAR (50)**|Adresse IP la plus élevée dans la plage du paramètre de pare-feu. Les adresses IP inférieures ou égales à celle-ci peuvent essayer de se connecter à l'instance [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus élevée possible est `255.255.255.255`.<br /><br /> Remarque : Windows Azure de tentatives de connexion sont autorisées lorsque ce champ et le **start_ip_address** ont la valeur `0.0.0.0`.|  
|create_date|**DATE/HEURE**|Date et heure UTC de création du paramètre de pare-feu de niveau base de données.|  
|modify_date|**DATE/HEURE**|Date et heure UTC de la dernière modification du paramètre de pare-feu de niveau base de données.|  
  
## <a name="remarks"></a>Notes  
 Pour supprimer une règle de pare-feu de base de données, utilisez [sp_delete_database_firewall_rule &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md). Pour définir une règle de pare-feu pour tous les [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consultez [sp_set_firewall_rule &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md). Pour retourner des règles de pare-feu plus d’informations sur la base de données existante, interrogez [sys.database_firewall_rules (base de données de SQL Azure)](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md).  
  
## <a name="permissions"></a>Autorisations  
 Cette vue est disponible dans le **master** base de données et dans chaque base de données utilisateur. L'accès en lecture seule à cette vue est disponible pour tous les utilisateurs autorisés à se connecter à la base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_set_firewall_rule &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [Sys.database_firewall_rules (base de données de SQL Azure)](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)   
 [sp_delete_database_firewall_rule &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)   
 [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)   
 [Configurer un pare-feu pour l’accès FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)   
 [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
  
