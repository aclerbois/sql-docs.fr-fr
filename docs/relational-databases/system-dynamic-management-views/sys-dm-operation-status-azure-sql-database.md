---
title: Sys.dm_operation_status (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 06/05/2017
ms.prod: ''
ms.prod_service: sql-database, sql-data-warehouse
ms.reviewer: ''
ms.service: sql-database
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_operation_status_TSQL
- dm_operation_status
- sys.dm_operation_status
- sys.dm_operation_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_operation_status dynamic management view
- sys.dm_operation_status dynamic management view
ms.assetid: cc847784-7f61-4c69-8b78-5f971bb24d61
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 6d1df62f4ac877ed82ba1d7b555f8fd9ef759362
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmoperationstatus-azure-sql-database"></a>sys.dm_operation_status (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-asdw-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md)]

  Retourne des informations sur les opérations effectuées sur des bases de données sur un serveur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
|Nom de la colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|session_activity_id|**uniqueidentifier**|ID de l'opération. Non null.|  
|resource_type|**int**|Indique le type de ressource sur lequel l'opération est effectuée. Non null. Dans la version actuelle, cette vue trace les opérations effectuées sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] uniquement, et la valeur entière correspondante est 0.|  
|resource_type_desc|**nvarchar(2048)**|Description du type de ressource sur lequel l'opération est effectuée. Dans la version actuelle, cette vue trace les opérations effectuées sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] uniquement.|  
|major_resource_id|**sql_variant**|Nom de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] sur laquelle l'opération est effectuée. Non Null.|  
|minor_resource_id|**sql_variant**|À usage interne uniquement Non null.|  
|opération|**nvarchar(60)**|Opération effectuée sur une [!INCLUDE[ssSDS](../../includes/sssds-md.md)], telle que CREATE ou ALTER.|  
|state|**tinyint**|L’état de l’opération.<br /><br /> 0 = En attente<br />1 = Opération en cours<br />2 = Opération terminée<br />3 = Échec<br />4 = Opération annulée|  
|state_desc|**nvarchar(120)**|PENDING = Opération en attente de la disponibilité d'une ressource ou d'un quota.<br /><br /> IN_PROGRESS = L'opération a démarré et est en cours.<br /><br /> COMPLETED = L'opération s'est terminée avec succès.<br /><br /> FAILED = L'opération a échoué. Consultez le **error_desc** colonne pour plus d’informations.<br /><br /> CANCELLED = Opération arrêtée à la demande de l'utilisateur.|  
|percent_complete|**int**|Pourcentage de l'opération terminée. Les valeurs ne sont pas continues et les valeurs valides sont répertoriés ci-dessous. Non NULL.<br/><br/>0 = l’opération n’a ne pas démarrée<br/>50 = opération en cours<br/>100 = opération terminée|  
|error_code|**int**|Code indiquant l'erreur qui s'est produite pendant une opération ayant échoué. 0 indique que l'opération pour cette étape s'est terminée avec succès.|  
|error_desc|**nvarchar(2048)**|Description de l'erreur qui s'est produite pendant une opération ayant échoué.|  
|error_severity|**int**|Niveau de gravité de l'erreur qui s'est produite pendant une opération ayant échoué. Pour plus d’informations sur les niveaux de gravité des erreurs, consultez [gravité des erreurs du moteur de base de données](http://go.microsoft.com/fwlink/?LinkId=251052).|  
|error_state|**int**|Réservé pour un usage ultérieur. La compatibilité future n'est pas garantie.|  
|start_time|**datetime**|Horodateur du début de l'opération.|  
|last_modify_time|**datetime**|Horodateur de la dernière modification de l'enregistrement d'une opération longue. Si les opérations se terminent avec succès, ce champ contient l'horodateur du moment où l'opération s'est terminée.|  
  
## <a name="permissions"></a>Autorisations  
 Cette vue est disponible uniquement dans les **master** base de données pour la connexion du principal au niveau du serveur.  
  
## <a name="remarks"></a>Notes  
 Pour utiliser cette vue, vous devez être connecté à la **master** base de données. Utilisez le `sys.dm_operation_status` afficher dans le **master** base de données de la [!INCLUDE[ssSDS](../../includes/sssds-md.md)] server pour suivre l’état des opérations suivantes effectuées sur une [!INCLUDE[ssSDS](../../includes/sssds-md.md)]:  
  
-   Créer la base de données  
  
-   Copier une base de données. La copie de base de données entraîne la création d'un enregistrement dans cette vue, à la fois sur le serveur source et le serveur cible.  
  
-   Modifier une base de données  
  
-   Modifier le niveau de performance d'une couche de service  
  
-   Modifier le niveau de service d'une base de données (par exemple, passer de De base à Standard).  
  
-   Configurer une relation de géo-réplication  
  
-   Terminer une relation de géo-réplication  
  
-   Restaurer la base de données  
  
-   Supprimer la base de données  
  
## <a name="example"></a>Exemple  
 Présentent les opérations de géo-réplication plus récentes liées à la base de données 'mydb'.  
  
```  
SELECT * FROM sys.dm_ operation_status   
   WHERE major_resource_id = ‘myddb’   
   ORDER BY start_time DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de géo-réplication &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)   
 [Sys.dm_geo_replication_link_status &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [Sys.geo_replication_links &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [ALTER DATABASE &#40;Azure SQL Database&#41;](../../t-sql/statements/alter-database-azure-sql-database.md)  
  
  
