---
title: REFERENTIAL_CONSTRAINTS (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- REFERENTIAL_CONSTRAINTS
- REFERENTIAL_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS view
- REFERENTIAL_CONSTRAINTS view
ms.assetid: 5d358f18-0a85-4b55-af4b-98d5f4cd1020
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4eca39fc2fab5193b9dee9875a754fd224f2c1ce
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="referentialconstraints-transact-sql"></a>REFERENTIAL_CONSTRAINTS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie une ligne pour chaque contrainte FOREIGN KEY dans la base de données active. Cette vue de schémas d'informations renvoie des informations sur les objets pour lesquels l'utilisateur actuel dispose d'autorisations.  
  
 Pour récupérer des informations à partir de ces vues, spécifiez le nom qualifié complet de **INFORMATION_SCHEMA. *** view_name*.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar(** 128 **)**|Qualificateur de la contrainte.|  
|**CONSTRAINT_SCHEMA**|**nvarchar(** 128 **)**|Nom du schéma qui contient la contrainte.<br /><br /> **\*\* Important \* \***  n’utilisez pas les vues INFORMATION_SCHEMA pour déterminer le schéma d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet est d’interroger l’affichage catalogue sys.objects.|  
|**CONSTRAINT_NAME**|**sysname**|Nom de la contrainte.|  
|**UNIQUE_CONSTRAINT_CATALOG**|**nvarchar(** 128 **)**|Identificateur de la contrainte unique.|  
|**UNIQUE_CONSTRAINT_SCHEMA**|**nvarchar(** 128 **)**|Nom du schéma qui contient la contrainte UNIQUE.<br /><br /> **\*\* Important \* \***  n’utilisez pas les vues INFORMATION_SCHEMA pour déterminer le schéma d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet est d’interroger l’affichage catalogue sys.objects.|  
|**UNIQUE_CONSTRAINT_NAME**|**sysname**|Contrainte UNIQUE.|  
|**MATCH_OPTION**|**varchar (** 7 **)**|Conditions référentielles de correspondance de contraintes. Renvoie toujours la valeur SIMPLE. Signifie qu'aucune correspondance n'est définie. La condition est considérée comme correspondante si l'une des conditions suivantes est réalisée :<br /><br /> au moins une valeur de la colonne clé étrangère est NULL ;<br /><br /> toutes les valeurs de la colonne clé étrangère ne comportent pas la valeur NULL, et une ligne de la table de clé primaire contient la même clé.|  
|**UPDATE_RULE**|**varchar (** 11 **)**|Action entreprise lorsqu’une [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction viole l’intégrité référentielle définie par cette contrainte. Retourne l'une des valeurs suivantes : <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> Si NO ACTION est spécifié sur une instruction ON UPDATE de cette contrainte, la mise à jour de la clé primaire référencée dans la contrainte n'est pas propagée vers la clé étrangère. Si une telle mise à jour enfreint l'intégrité référentielle car au moins une clé étrangère contient la même valeur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'apporte aucune modification aux tables parentes et de référence. Par ailleurs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur.<br /><br /> Si CASCADE est spécifié sur une instruction ON UPDATE de cette contrainte, toute modification de valeur de la clé primaire est automatiquement propagée vers la valeur de la clé étrangère.|  
|**DELETE_RULE**|**varchar (** 11 **)**|Action entreprise lorsqu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] viole l'intégrité référentielle définie par cette contrainte. Retourne l'une des valeurs suivantes : <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> Si NO ACTION est spécifié sur une instruction ON DELETE de cette contrainte, la suppression de la clé primaire référencée dans la contrainte n'est pas propagée vers la clé étrangère. Si la suppression d'une clé primaire enfreint l'intégrité référentielle car au moins une clé étrangère contient la même valeur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'apporte aucune modification aux tables parentes et de référence. Par ailleurs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur.<br /><br /> Si CASCADE est spécifié sur une instruction ON DELETE de cette contrainte, toute modification de valeur de la clé primaire est automatiquement propagée vers la valeur de la clé étrangère.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [Sys.Foreign_Keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)  
  
  
