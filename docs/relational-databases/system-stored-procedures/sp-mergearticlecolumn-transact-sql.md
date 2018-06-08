---
title: sp_mergearticlecolumn (Transact-SQL) | Documents Microsoft
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
- sp_mergearticlecolumn
- sp_mergearticlecolumn_TSQL
helpviewer_keywords:
- sp_mergearticlecolumn
ms.assetid: b4f2b888-e094-4759-a472-d893638995eb
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e426586be6229cb62e36d8fdcab13663785240b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spmergearticlecolumn-transact-sql"></a>sp_mergearticlecolumn (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Partitionne une publication de fusion verticalement. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergearticlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column'  
    [ , [ @operation = ] 'operation'   
    [ , [ @schema_replication = ] 'schema_replication' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article =**] **'***article***'**  
 Nom de l'article dans la publication. *article* est **sysname**, sans valeur par défaut.  
  
 [  **@column =**] **'***colonne***'**  
 Identifie les colonnes sur lesquelles la partition verticale doit être créée. *colonne* est **sysname**, avec NULL comme valeur par défaut. Si la valeur NULL est spécifiée et que `@operation = N'add'`, toutes les colonnes de la table source sont ajoutées à l'article par défaut. *colonne* ne peut pas être NULL lorsque *opération* a la valeur **supprimer**. Pour exclure des colonnes à partir d’un article, exécutez **sp_mergearticlecolumn** et spécifiez *colonne* et `@operation = N'drop'` pour chaque colonne doit être supprimée à partir du spécifié *article*.  
  
 [  **@operation =**] **'***opération***'**  
 État de la réplication. *opération* est **nvarchar (4)**, avec ADD comme valeur par défaut. **ajouter** marque la colonne pour la réplication. **DROP** supprime la colonne.  
  
 [  **@schema_replication=**] **'***schema_replication***'**  
 Indique qu'une modification du schéma sera propagée lors de l'exécution de l'Agent de fusion. *schema_replication* est **nvarchar (5)**, avec FALSE comme valeur par défaut.  
  
> [!NOTE]  
>  Uniquement **FALSE** est pris en charge pour *schema_replication*.  
  
 [  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Active ou désactive la possibilité d'invalider un instantané. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications apportées à l’article de fusion n’entraînent pas l’instantané n’est pas valide.  
  
 **1** Spécifie que les modifications apportées à l’article de fusion peuvent invalider l’instantané n’est pas valide, et si c’est le cas, la valeur **1** autorise le nouvel instantané de se produire.  
  
 [* *@force_reinit_subscription =] *** force_reinit_subscription*  
 Active ou désactive la possibilité de réinitialiser l'abonnement. *force_reinit_subscription* est de type bit, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications apportées à l’article de fusion n’entraînent pas la réinitialisation des abonnements.  
  
 **1** Spécifie que les modifications apportées à l’article de fusion peuvent invalider l’abonnement pour réinitialisation, et si c’est le cas, la valeur **1** autorise la réinitialisation des abonnements se produise.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_mergearticlecolumn** est utilisé dans la réplication de fusion.  
  
 La colonne d'identité ne peut pas être supprimée de l'article si la gestion automatique des plages d'identité est utilisée. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Si une application définit une nouvelle partition verticale après la création de l'instantané initial, un nouvel instantané doit être généré et réappliqué à chaque abonnement. Les instantanés sont appliqués lors de l'exécution de l'instantané planifié suivant et de l'Agent de distribution ou de fusion.  
  
 Si le suivi de lignes est utilisé pour la détection de conflits (valeur par défaut), la table de base peut inclure 1 024 colonnes au maximum, mais les colonnes doivent être filtrées à partir de l'article afin que 246 colonnes au maximum soient publiées. Si le suivi de colonnes est utilisé, la table de base peut inclure 246 colonnes au maximum.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-mergearticlecolumn-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_mergearticlecolumn**.  
  
## <a name="see-also"></a>Voir aussi  
 [Define and Modify a Join Filter Between Merge Articles](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
