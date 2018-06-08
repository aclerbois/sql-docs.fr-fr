---
title: Sys.security_predicates (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- Azure SQL Database
- SQL Server 2016 Preview
f1_keywords:
- SYS.SECURITY_PREDICATES
- SECURITY_PREDICATES
- SECURITY_PREDICATES_TSQL
- SYS.SECURITY_PREDICATES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_predicates catalog view
- security_predicates catalog view
ms.assetid: c7a2f28c-98da-463d-8b8a-8e5619e2c6a6
caps.latest.revision: 9
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 539ca48e5c55485a5a4b3fdecb3044c1feae6826
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssecuritypredicates-transact-sql"></a>Sys.security_predicates (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque prédicat de sécurité dans la base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID de la stratégie de sécurité qui contient ce prédicat.|  
|security_predicate_id|**int**|ID de prédicat dans cette stratégie de sécurité.|  
|target_object_id|**int**|ID de l'objet sur lequel le prédicat de la sécurité est lié.|  
|predicate_definition|**nvarchar(max)**|Nom qualifié complet de la fonction qui sera utilisée comme prédicat de sécurité, y compris les arguments. Notez que le `schema.function` nom peut être normalisé (autrement dit, avec séquence d’échappement), ainsi que tout autre élément dans le texte de la cohérence. Par exemple :<br /><br /> `[dbo].[fn_securitypredicate]([wing], [startTime], [endTime])`|  
|predicate_type|**int**|Type de prédicat utilisé par la stratégie de sécurité :<br /><br /> 0 = LE PRÉDICAT DE FILTRE<br /><br /> 1 = LE PRÉDICAT DE BLOC|  
|predicate_type_desc|**nvarchar(60)**|Type de prédicat utilisé par la stratégie de sécurité :<br /><br /> FILTER<br /><br /> BLOC|  
|opération|**int**|Le type d’opération spécifié pour le prédicat :<br /><br /> NULL = toutes les opérations applicables<br /><br /> 1 = APRÈS UNE INSERTION<br /><br /> 2 = APRÈS MISE À JOUR<br /><br /> 3 = MISE À JOUR<br /><br /> 4 = AVANT LA SUPPRESSION|  
|operation_desc|**nvarchar(60)**|Le type d’opération spécifié pour le prédicat :<br /><br /> NULL<br /><br /> APRÈS UNE INSERTION<br /><br /> AFTER UPDATE<br /><br /> AVANT LA MISE À JOUR<br /><br /> AVANT DE SUPPRIMER|  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec le **ALTER ANY SECURITY POLICY** autorisation ont accès à tous les objets dans cet affichage catalogue, ainsi que toute personne ayant **VIEW DEFINITION** sur l’objet.  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
