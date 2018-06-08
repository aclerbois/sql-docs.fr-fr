---
title: Sys.dm_db_uncontained_entities (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_uncontained_entities
- dm_db_uncontained_entities_TSQL
- sys.dm_db_uncontained_entities_TSQL
- dm_db_uncontained_entities
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_uncontained_entities dynamic management view
ms.assetid: f417efd4-8c71-4f81-bc9c-af13bb4b88ad
caps.latest.revision: 29
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 18f89d3a275a36d32d72524a1c2e650a9e2dc663
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbuncontainedentities-transact-sql"></a>sys.dm_db_uncontained_entities (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche tous les objets sans relation contenant-contenu utilisés dans la base de données. Les objets sans relation contenant-contenu sont des objets qui traversent la limite de la base de données dans une base de données à relation contenant-contenu. Cette vue est accessible depuis une base de données à relation contenant-contenu et une base de données sans relation contenant-contenu. Si sys.dm_db_uncontained_entities est vide, votre base de données n’utilise pas toutes les entités de relation contenant-contenues.  
  
 Si un module traverse la limite de la base de données plusieurs fois, seule la première traversée découverte est signalée.  
  
||||  
|-|-|-|  
|**Nom de colonne**|**Type**|**Description**|  
|*class*|**int**|1 = objet ou colonne (inclut des modules, XPs, vues, synonymes et tables).<br /><br /> 4 = Principal de la base de données<br /><br /> 5 = Assembly<br /><br /> 6 = Type<br /><br /> 7 = Index (index de texte intégral)<br /><br /> 12 = déclencheur DDL de base de données<br /><br /> 19 = Itinéraire<br /><br /> 30 = Spécification d'audit|  
|*class_desc*|**nvarchar(120)**|Description de la classe de l'entité. Une des options suivantes pour correspondre à la classe :<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **DATABASE_PRINCIPAL**<br /><br /> **ASSEMBLY**<br /><br /> **TYPE**<br /><br /> **INDEX**<br /><br /> **DATABASE_DDL_TRIGGER**<br /><br /> **ROUTE**<br /><br /> **AUDIT_SPECIFICATION**|  
|*major_id*|**int**|ID de l'entité.<br /><br /> Si *classe* = 1, puis object_id<br /><br /> Si *classe* = 4, puis sys.database_principals.principal_id.<br /><br /> Si *classe* = 5, puis sys.assemblies.assembly_id.<br /><br /> Si *classe* = 6, puis sys.types.user_type_id.<br /><br /> Si *classe* = 7, puis sys.indexes.index_id.<br /><br /> Si *classe* = 12, puis sys.triggers.object_id.<br /><br /> Si *classe* = 19, sys.routes.Route_ID.<br /><br /> Si *classe* = 30, puis sys. database_audit_specifications.databse_specification_id.|  
|*statement_line_number*|**int**|Si la classe est un module, retourne le numéro de ligne sur lequel l'utilisation sans relation contenant-contenu se trouve.  Sinon, la valeur est Null.|  
|*statement_ offset_begin*|**int**|Si la classe est un module, indique, en octets, en commençant par 0, la position de départ où l'utilisation sans relation contenant-contenu démarre. Sinon, la valeur de retour est Null.|  
|*statement_ offset_end*|**int**|Si la classe est un module, indique, en octets, en commençant par 0, la position de fin de l'utilisation sans relation contenant-contenu. La valeur -1 indique la fin du module. Sinon, la valeur de retour est Null.|  
|*statement_type*|**nvarchar(512)**|Type d'instruction.|  
|*nom de feature_*|**nvarchar (256)**|Retourne le nom externe de l'objet.|  
|*feature_type_name*|**nvarchar (256)**|Renvoie le type de fonctionnalité.|  
  
## <a name="remarks"></a>Notes  
 Sys.dm_db_uncontained_entities affiche les entités qui peuvent potentiellement traverser la limite de la base de données. Toutes les entités de l'utilisateur qui ont la possibilité d'utiliser des objets en dehors de la base de données sont retournées.  
  
 Les types de fonctionnalité suivants sont signalés.  
  
-   Comportement de relation contenant contenu inconnu (SQL dynamique ou résolution de noms différée)  
  
-   Commande DBCC  
  
-   Procédure stockée système  
  
-   Fonction scalaire système  
  
-   Fonction table système  
  
-   Fonction système intégrée  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Sys.dm_db_uncontained_entities retourne uniquement les objets pour lesquels l’utilisateur a un type d’autorisation. Pour évaluer entièrement la relation contenant-contenu de la base de données, cette fonction doit être utilisée par un utilisateur à privilèges élevés tel qu’un membre de la **sysadmin** rôle serveur fixe ou **db_owner** rôle.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une procédure nommée P1, puis interroge `sys.dm_db_uncontained_entities`. La requête indique que P1 utilise **sys.endpoints** qui se trouve en dehors de la base de données.  
  
```sql  
CREATE DATABASE Test;  
GO  
  
USE Test;  
GO  
CREATE PROC P1  
AS   
SELECT * FROM sys.endpoints ;  
GO  
SELECT SO.name, UE.* FROM sys.dm_db_uncontained_entities AS UE  
LEFT JOIN sys.objects AS SO  
    ON UE.major_id = SO.object_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)  
  
  
