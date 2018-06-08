---
title: sp_fulltext_pendingchanges (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fulltext_pendingchanges_TSQL
- sp_fulltext_pendingchanges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_pendingchanges
ms.assetid: fee042fe-4781-4a33-a01b-d98fb5629f1b
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4a6defefbc225d2f8301977d5826c74597eefb2c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfulltextpendingchanges-transact-sql"></a>sp_fulltext_pendingchanges (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne les modifications non traitées (par exemple les insertions, mises à jour et suppressions en attente) pour une table spécifiée qui utilise le suivi des modifications.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_fulltext_pendingchanges table_id  
```  
  
## <a name="arguments"></a>Arguments  
 *table_id*  
 ID de la table. Si la table n'est pas indexée sur le texte intégral ou si le suivi des modifications n'est pas activé sur la table, une erreur est renvoyée.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Clé**|*|Valeur de la clé de texte intégral pour une table spécifiée.|  
|**ID de document**|**bigint**|Colonne de l'ID interne de document (DocId) qui correspond à la valeur de la clé.|  
|**État**|**int**|0 = La ligne est supprimée de l'index de texte intégral.<br /><br /> 1 = La ligne est indexée sur le texte intégral.<br /><br /> 2 = La ligne est à jour.<br /><br /> -1 = La ligne est en état de transition (traitée en lot mais non validée) ou en erreur.|  
|**DocState**|**tinyint**|Vidage brut de la colonne d'état du mappage de l'ID interne du document (DocId).|  
  
 <sup>* Le type de données pour la clé est identique au type de données de la colonne de clé de recherche en texte intégral dans la table de base.</sup>  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="remarks"></a>Notes  
 S'il n'y a pas de modification à traiter, un ensemble de lignes vide est renvoyé.  
  
 Requêtes de recherche en texte intégral ne retournent pas de lignes avec un **état** la valeur 0. Cela est dû au fait que la ligne a été supprimée de la table de base et attend d'être supprimée de l'index de texte intégral.  
  
 Pour connaître le nombre de modifications en attente dans une table donnée, utilisez la propriété **TableFullTextPendingChanges** de la fonction OBJECTPROPERTYEX.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de recherche en texte intégral et la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)  
  
  
