---
title: sp_helptrigger (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_helptrigger
- sp_helptrigger_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helptrigger
ms.assetid: e486d39b-771d-488d-a786-7136433a2203
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 61c6c78c302266ce3a9e29c432e1a913c263d704
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelptrigger-transact-sql"></a>sp_helptrigger (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne le ou les types de déclencheurs DML définis sur la table spécifiée de la base de données actuelle. sp_helptrigger ne peut pas être utilisé avec des déclencheurs DDL. Requête le [procédures stockées système](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) affichage du catalogue à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helptrigger [ @tabname = ] 'table'   
     [ , [ @triggertype = ] 'type' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@tabname=** ] **'***table***'**  
 Nom de la table de la base de données active pour laquelle il faut renvoyer des informations sur les déclencheurs. *table* est **nvarchar(776)**, sans valeur par défaut.  
  
 [  **@triggertype=** ] **'***type***'**  
 Type du déclencheur DML pour lequel des informations doivent être renvoyées. *type* est **char (6)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**DELETE**|Renvoie des informations sur le déclencheur DELETE.|  
|**INSERT**|Renvoie des informations sur le déclencheur INSERT.|  
|**UPDATE**|Renvoie des informations sur le déclencheur UPDATE.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 La table ci-dessous décrit les informations figurant dans le jeu de résultats.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**trigger_name**|**sysname**|Nom du déclencheur.|  
|**trigger_owner**|**sysname**|Nom du propriétaire de la table pour laquelle le déclencheur est défini.|  
|**isupdate**|**int**|1 = déclencheur UPDATE<br /><br /> 0 = n'est pas un déclencheur UPDATE|  
|**isdelete**|**int**|1 = déclencheur DELETE<br /><br /> 0 = n'est pas un déclencheur DELETE|  
|**propriété EstInsertion**|**int**|1 = déclencheur INSERT<br /><br /> 0 = n'est pas un déclencheur INSERT|  
|**isafter**|**int**|1 = déclencheur AFTER<br /><br /> 0 = n'est pas un déclencheur AFTER|  
|**isinsteadof**|**int**|1 = déclencheur INSTEAD OF<br /><br /> 0 = n'est pas un déclencheur INSTEAD OF|  
|**trigger_schema**|**sysname**|Nom du schéma auquel le déclencheur appartient.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md) autorisation sur la table.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant exécute `sp_helptrigger` pour produire des informations sur le ou les déclencheurs de la table `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helptrigger 'Person.Person';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
