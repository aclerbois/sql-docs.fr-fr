---
title: sp_mergemetadataretentioncleanup (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- sp_mergemetadataretentioncleanup
- sp_mergemetadataretentioncleanup_TSQL
helpviewer_keywords:
- sp_mergemetadataretentioncleanup
ms.assetid: 4e8d6343-2a38-421d-a3f3-c37d437a0f88
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4ac0c7820ab4f336057a3d747409b0e1af09248d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spmergemetadataretentioncleanup-transact-sql"></a>sp_mergemetadataretentioncleanup (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Effectue un nettoyage manuel des métadonnées dans le [MSmerge_genhistory](../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md), [MSmerge_contents](../../relational-databases/system-tables/msmerge-contents-transact-sql.md), [MSmerge_tombstone](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md), [MSmerge_past_partition_mappings](../../relational-databases/system-tables/msmerge-past-partition-mappings-transact-sql.md), et [MSmerge_current_partition_mappings](../../relational-databases/system-tables/msmerge-current-partition-mappings.md) tables système. Cette procédure stockée est exécutée sur chaque serveur de publication et abonné dans la topologie.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergemetadataretentioncleanup [ [ @num_genhistory_rows = ] num_genhistory_rows OUTPUT ]  
    [ , [ @num_contents_rows = ] num_contents_rows OUTPUT ]   
    [ , [ @num_tombstone_rows = ] num_tombstone_rows OUTPUT ]   
    [ , [ @aggressive_cleanup_only = ] aggressive_cleanup_only ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@num_genhistory_rows=** ] *num_genhistory_rows* sortie  
 Retourne le nombre de lignes nettoyées dans la [MSmerge_genhistory](../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md) table. *num_genhistory_rows* est **int**, avec une valeur par défaut **0**.  
  
 [  **@num_contents_rows=** ] *num_contents_rows* sortie  
 Retourne le nombre de lignes nettoyées dans la [MSmerge_contents](../../relational-databases/system-tables/msmerge-contents-transact-sql.md) table. *num_contents_rows* est **int**, avec une valeur par défaut **0**.  
  
 [  **@num_tombstone_rows=** ] *num_tombstone_rows* sortie  
 Retourne le nombre de lignes nettoyées dans la [MSmerge_tombstone](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md) table. *num_tombstone_rows* est **int**, avec une valeur par défaut **0**.  
  
 [  **@aggressive_cleanup_only=** ] *aggressive_cleanup_only*  
 À usage interne uniquement  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
  
> [!IMPORTANT]  
>  S’il existe plusieurs publications sur une base de données, et l’une des publications utilise une période de rétention de publication infinie, en cours d’exécution **sp_mergemetadataretentioncleanup** ne nettoie pas les métadonnées pour la base de données de suivi de la modification de réplication de fusion. C'est pour cette raison qu'il faut utiliser la période de rétention infinie avec prudence. Pour déterminer si une publication dispose d’une période infinie de rétention, exécutez [sp_helpmergepublication &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) le serveur de publication et notez toutes les publications dans le résultat défini avec la valeur **0** pour **rétention**.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** fixe le rôle de base de données ou des utilisateurs dans la liste d’accès d’une base de données publiée peut exécuter **sp_mergemetadataretentioncleanup**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
