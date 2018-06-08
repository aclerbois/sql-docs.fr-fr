---
title: sp_helpmergefilter (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_helpmergefilter
- sp_helpmergefilter_TSQL
helpviewer_keywords:
- sp_helpmergefilter
ms.assetid: f133a094-0009-4771-b93b-e86a5c01e40b
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6e35fe61b91fb503b87ba0a0195e77ad7ea0de50
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpmergefilter-transact-sql"></a>sp_helpmergefilter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur les filtres de fusion. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergefilter [ @publication= ] 'publication'   
    [ , [ @article= ] 'article']  
    [ , [ @filtername= ] 'filtername']  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article. *article* est **sysname**, avec une valeur par défaut **%**, qui retourne les noms de tous les articles.  
  
 [  **@filtername=**] **'***filtername***'**  
 Nom du filtre pour lequel il faut renvoyer des informations. *FilterName* est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur tous les filtres définis sur l’article ou de la publication.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**join_filterid**|**int**|Identificateur du filtre de jointure.|  
|**nom de filtre**|**sysname**|Nom du filtre.|  
|**nom de l’article**|**sysname**|Nom de l'article de jointure.|  
|**join_filterclause**|**nvarchar(2000)**|Clause FILTER qualifiant la jointure.|  
|**join_unique_key**|**int**|Indique si la jointure porte sur une clé unique.|  
|**propriétaire de la table de base**|**sysname**|Nom du propriétaire de la table de base.|  
|**nom de la table de base**|**sysname**|Nom de la table de base.|  
|**propriétaire de la table jointure**|**sysname**|Nom du propriétaire de la table jointe à la table de base.|  
|**nom de table de jointure**|**sysname**|Nom de la table jointe à la table de base.|  
|**nom de l’article**|**sysname**|Nom de l'article de la table jointe à la table de base.|  
|**filter_type**|**tinyint**|Type de filtre de fusion parmi les types suivants :<br /><br /> **1** = filtre de jointure uniquement<br /><br /> **2** = relation d’enregistrements logiques<br /><br /> **3** = les deux|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergefilter** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe et le **db_owner** du rôle de base de données fixe peut exécuter **sp_helpmergefilter**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
