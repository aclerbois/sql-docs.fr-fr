---
title: sp_help_fulltext_catalog_components (Transact-SQL) | Microsoft Docs
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sp_help_fulltext_catalog_components_TSQL
- sp_help_fulltext_catalog_components
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_catalog_components
ms.assetid: fbd6a3d4-6a4c-42a2-bff8-2a5eb0745e47
caps.latest.revision: 
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 144443acbf2cd4195bacf71a4b1fce5a1ba3eb33
ms.sourcegitcommit: c556eaf60a49af7025db35b7aa14beb76a8158c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="sphelpfulltextcatalogcomponents-transact-sql"></a>sp_help_fulltext_catalog_components (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la liste de tous les composants (filtres, analyseurs lexicaux et gestionnaires de protocoles) utilisés pour tous les catalogues de texte intégral dans la base de données active.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_catalog_components  
```  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom du catalogue de texte intégral**|**int**|Nom du catalogue de texte intégral.|  
|**id de catalogue de texte intégral**|**sysname**|Identificateur du catalogue de texte intégral.|  
|**componenttype**|**sysname**|Type de composant. Il peut s'agir :<br /><br /> Filtre<br /><br /> Gestionnaire de protocole<br /><br /> Analyseur lexical|  
|**componentname**|**sysname**|Nom du composant.|  
|**clsid**|**uniqueidentifier**|Identificateur de classe du composant.|  
|**fullpath**|**nvarchar (256)**|Chemin d'accès de l'emplacement du composant.<br /><br /> NULL = l’appelant n’est pas membre **serveradmin** rôle serveur fixe.|  
|**version**|**nvarchar(30)**|Numéro de version du composant.|  
|**manufacturer**|**sysname**|Nom du fabricant du composant.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral et la recherche sémantique stockées procédures &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [sp_help_fulltext_system_components &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
