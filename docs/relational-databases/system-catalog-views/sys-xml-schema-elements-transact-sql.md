---
title: Sys.xml_schema_elements (Transact-SQL) | Documents Microsoft
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
- sys.xml_schema_elements
- sys.xml_schema_elements_TSQL
- xml_schema_elements
- xml_schema_elements_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_elements catalog view
ms.assetid: 190ed0cd-0c5e-4607-9db4-9e77cacf17d7
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c21c0a2aaa70e520fcda65acc72316f1c6414519
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysxmlschemaelements-transact-sql"></a>sys.xml_schema_elements (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque composant de schéma XML qui est un Type, **symbol_space** de **E**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**\<héritée de colonnes >**|**--**|Hérite des colonnes de [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_default_fixed**|**bit**|1 = La valeur par défaut est une valeur fixe. Cette valeur ne peut pas être remplacée dans une instance XML.<br /><br /> 0 = La valeur par défaut n'est pas une valeur fixe pour cet élément. (Valeur par défaut.)|  
|**is_abstract**|**bit**|1 = L'élément est abstrait et ne peut pas être utilisé dans un document de l'instance. Un membre du groupe de substitution de l'élément doit figurer dans le document de l'instance.<br /><br /> 0 = L'élément n'est pas abstrait. (Valeur par défaut.)|  
|**is_nillable**|**bit**|1 = L'élément peut avoir la valeur NULL.<br /><br /> 0 = L'élément ne peut pas avoir la valeur NULL. (par défaut)|  
|**must_be_qualified**|**bit**|1 = L'élément doit être qualifié explicitement par un espace de noms.<br /><br /> 0 = L'élément peut être qualifié implicitement par un espace de noms. (par défaut)|  
|**is_extension_blocked**|**bit**|1 = Le remplacement par une instance d'un type d'extension est interdit.<br /><br /> 0 = Le remplacement par un type d'extension est autorisé. (par défaut)|  
|**is_restriction_blocked**|**bit**|1 = Le remplacement par une instance d'un type de restriction est interdit.<br /><br /> 0 = Le remplacement par un type de restriction est autorisé. (par défaut)|  
|**is_substitution_blocked**|**bit**|1 = Impossible d'utiliser une instance d'un groupe de substitution.<br /><br /> 0 = Le remplacement par un groupe de substitution est autorisé. (par défaut)|  
|**is_final_extension**|**bit**|1 = Le remplacement par une instance d'un type d'extension n'est pas autorisé.<br /><br /> 0 = Le remplacement par une instance d'un type d'extension est autorisé. (par défaut)|  
|**is_final_restriction**|**bit**|1 = Le remplacement par une instance d'un type de restriction n'est pas autorisé.<br /><br /> 0 = Le remplacement par une instance d'un type de restriction est autorisé. (par défaut)|  
|**default_value**|**nvarchar (4000)**|Valeur par défaut de l'élément. NULL si aucune valeur par défaut n'est fournie.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;système de Type XML&#41; affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
