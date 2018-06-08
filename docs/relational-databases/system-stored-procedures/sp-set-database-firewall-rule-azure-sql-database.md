---
title: sp_set_database_firewall_rule (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 08/04/2017
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-stored-procedures
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_set_database_firewall_rule
- sp_set_database_firewall_rule_TSQL
- sys.sp_set_database_firewall_rule
- sys.sp_set_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_database_firewall_rule
- firewall_rules, setting database rules
ms.assetid: 8f0506b6-a4ac-4e4d-91db-8077c40cb17a
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 6fa2b1effae12bd8132c331d4e1ba33055c656e0
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsetdatabasefirewallrule-azure-sql-database"></a>sp_set_database_firewall_rule (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Crée ou met à jour les règles de pare-feu de niveau base de données pour votre [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Les règles de pare-feu de base de données peuvent être configurées pour le **master** base de données et pour les bases de données utilisateur sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Les règles de pare-feu de base de données sont particulièrement utiles à l’aide de la relation contenant-contenu aux utilisateurs de base de données. Pour plus d’informations, consultez [Utilisateurs de base de données à relation contenant-contenu - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_set_database_firewall_rule [@name = ] [N]'name'  
, [@start_ip_address =] 'start_ip_address'  
, [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 **[@name**  =] [N]'*nom*'  
 Le nom utilisé pour décrire et distinguer le paramètre de pare-feu au niveau base de données. *nom* est **nvarchar (128)** sans valeur par défaut. L’identificateur Unicode `N` est facultatif pour [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. 
  
 **[@start_ip_address**  =] '*start_ip_address*'  
 Adresse IP la plus basse dans la plage de paramètres de pare-feu au niveau base de données. Les adresses IP supérieures ou égales à celle-ci peuvent essayer de se connecter à l'instance [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus basse possible est `0.0.0.0`. *start_ip_address* est **varchar (50)** sans valeur par défaut.  
  
 [**@end_ip_address** =] '*end_ip_address*'  
 Adresse IP la plus élevée dans la plage de paramètres de pare-feu au niveau base de données. Les adresses IP inférieures ou égales à celle-ci peuvent essayer de se connecter à l'instance [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus élevée possible est `255.255.255.255`. *end_ip_address* est **varchar (50)** sans valeur par défaut.  
  
 Le tableau suivant montre les arguments pris en charge et les options dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
> [!NOTE]  
>  Les tentatives de connexion à Azure sont autorisées lorsque ce champ et le *start_ip_address* ont la valeur `0.0.0.0`.  
  
## <a name="remarks"></a>Notes  
 Les noms des paramètres de pare-feu au niveau base de données doivent être uniques. Si le nom du paramètre de pare-feu au niveau base de données spécifié pour la procédure stockée existe déjà dans le tableau des paramètres de pare-feu au niveau base de données, les adresses IP de début et de fin sont mises à jour. Sinon, un nouveau paramètre de pare-feu au niveau base de données est créé.  
  
 Lorsque vous ajoutez un paramètre de pare-feu de niveau base de données de début et fin des adresses IP sont égales à `0.0.0.0`, vous activez l’accès à votre base de données dans le [!INCLUDE[ssSDS](../../includes/sssds-md.md)] serveur à partir de n’importe quelle ressource Azure. Spécifiez une valeur pour le *nom* n’oubliez pas de paramètre qui vous aideront à ce qui concerne le paramètre de pare-feu.  
  
## <a name="permissions"></a>Autorisations  
 Exige l’autorisation **CONTROL** sur la base de données.  
  
## <a name="examples"></a>Exemples  
 Le code suivant crée un pare-feu de niveau base de données appelé `Allow Azure` qui permet l’accès à votre base de données à partir d’Azure.  
  
```  
-- Enable Azure connections.  
EXECUTE sp_set_database_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 Le code suivant crée un paramètre de pare-feu au niveau base de données appelé `Example DB Setting 1` uniquement pour l'adresse IP `0.0.0.4`. Ensuite, la `sp_set_database firewall_rule` procédure stockée est appelée à nouveau pour mettre à jour l’adresse IP de fin pour `0.0.0.6`, dans ce paramètre de pare-feu. Cette opération crée une plage qui permet d’utiliser les adresses IP `0.0.0.4`, `0.0.0.5`, et `0.0.0.6` pour accéder à la base de données.
  
```  
-- Create database-level firewall setting for only IP 0.0.0.4  
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.4';  
  
-- Update database-level firewall setting to create a range of allowed IP addresses
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.6';  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Pare-feu de base de données SQL Azure](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/)   
 [Comment : configurer les paramètres de pare-feu (base de données SQL Azure)](https://azure.microsoft.com/documentation/articles/sql-database-configure-firewall-settings/)   
 [sp_set_firewall_rule &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sp_delete_database_firewall_rule &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)   
 [Sys.database_firewall_rules &#40;base de données SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
  
