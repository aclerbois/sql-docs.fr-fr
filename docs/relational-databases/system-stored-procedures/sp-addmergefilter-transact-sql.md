---
title: sp_addmergefilter (Transact-SQL) | Documents Microsoft
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
- sp_addmergefilter
- sp_addmergefilter_TSQL
helpviewer_keywords:
- sp_addmergefilter
ms.assetid: 4c118cb1-2008-44e2-a797-34b7dc34d6b1
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: fcb8e6ae00e7dfe644cbbd05eb72d6f574c993f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddmergefilter-transact-sql"></a>sp_addmergefilter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un nouveau filtre de fusion pour créer une partition basée sur une jointure avec une autre table. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergefilter [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
        , [ @filtername = ] 'filtername'   
        , [ @join_articlename = ] 'join_articlename'   
        , [ @join_filterclause = ] join_filterclause  
    [ , [ @join_unique_key = ] join_unique_key ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @filter_type = ] filter_type ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=** ] **'***publication***'**  
 Nom de la publication dans laquelle le filtre de fusion est ajouté. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=** ] **'***article***'**  
 Nom de l'article dans lequel le filtre de fusion est ajouté. *article* est **sysname**, sans valeur par défaut.  
  
 [  **@filtername=** ] **'***filtername***'**  
 Nom du filtre. *FilterName* est un paramètre obligatoire. *FilterName*est **sysname**, sans valeur par défaut.  
  
 [  **@join_articlename=** ] **'***join_articlename***'**  
 Nom de l’article parent auquel l’article enfant, spécifié par *article*, doit être joint à l’aide de la clause de jointure spécifiée par *join_filterclause*, afin de déterminer les lignes de l’article enfant qui répondent au critère du filtre de fusion. *join_articlename* est **sysname**, sans valeur par défaut. L’article doit figurer dans la publication donnée par *publication*.  
  
 [  **@join_filterclause=** ] *join_filterclause*  
 Clause de jointure qui doit être utilisé pour joindre l’article enfant spécifié par *article*et l’article parent spécifié par *join_article*, afin de déterminer les lignes qualifiant le filtre de fusion. *join_filterclause* est **nvarchar (1000)**.  
  
 [  **@join_unique_key=** ] *join_unique_key*  
 Spécifie si la jointure entre l’article enfant *article*et l’article parent *join_article*est un-à-plusieurs, un à un, plusieurs-à-un ou plusieurs-à-plusieurs. *join_unique_key* est **int**, avec 0 comme valeur par défaut. **0** indique une jointure plusieurs-à-un ou plusieurs-à-plusieurs. **1** indique une jointure un à un ou un-à-plusieurs. Cette valeur est **1** quand les colonnes de jointure forment une clé unique dans *join_article*, ou si *join_filterclause* entre une clé étrangère dans *article* et une clé primaire dans *join_article*.  
  
> [!CAUTION]  
>  Ne définissez ce paramètre **1** si vous avez une contrainte sur la colonne de jointure dans la table sous-jacente pour l’article parent qui garantit l’unicité. Si *join_unique_key* a la valeur **1** est incorrecte, une non-convergence des données peut se produire.  
  
 [  **@force_invalidate_snapshot=** ] *force_invalidate_snapshot*  
 Signale que l'action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications apportées à l’article de fusion n’entraînent pas l’instantané n’est pas valide. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur est générée et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article de fusion peuvent invalider l’instantané n’est pas valide, s’il existe des abonnements nécessitant un nouvel instantané pour l’instantané existant soit marqué comme obsolète et un nouvel instantané généré.  
  
 [  **@force_reinit_subscription=** ] *force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits**, avec 0 comme valeur par défaut.  
  
 **0** Spécifie que les modifications apportées à l’article de fusion n’entraînent pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification requiert la réinitialisation des abonnements, une erreur est générée et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article de fusion entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
 [  **@filter_type=** ] *filter_type*  
 Spécifie le type de filtre à ajouter. *filter_type* est **tinyint**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Filtre de jointure uniquement. Requis pour prendre en charge [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés.|  
|**2**|Uniquement relation logique.|  
|**3**|Filtre de jointure et relation logique.|  
  
 Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergefilter** est utilisé dans la réplication de fusion.  
  
 **sp_addmergefilter** peut uniquement être utilisé avec les articles de table. Les articles de vue et de vue indexée ne sont pas pris en charge.  
  
 Vous pouvez également l'utiliser pour ajouter une relation logique entre deux articles qui peuvent ou non avoir un filtre de jointure entre eux. *filter_type* est utilisée pour spécifier si le filtre de fusion ajouté est un filtre de jointure, une relation logique ou les deux.  
  
 Pour utiliser des enregistrements logiques, la publication et les articles doivent répondre à certaines conditions. Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 Cette option est généralement utilisée pour un article qui fait référence à une clé étrangère dans une table de clés primaires publiée, et lorsque la table de clés primaires a un filtre défini dans cet article. Le sous-ensemble de lignes de clés primaires est utilisé pour déterminer les lignes de clés étrangères répliquées côté abonné.  
  
 Vous ne pouvez pas ajouter de filtre de jointure entre deux articles publiés, lorsque les tables source des deux articles partagent le même nom d'objet de table. Dans ce cas, la création du filtre de jointure échouera même si les deux tables sont détenues par des schémas différents et ont des noms d'article unique.  
  
 Lorsqu'un filtre de lignes paramétrable et un filtre de jointure sont tous deux utilisés sur un article de table, la réplication détermine si une ligne appartient à une partition de l'abonné. S’effectue à l’évaluation de la fonction de filtrage ou le filtre de jointure (à l’aide de la [OR](../../t-sql/language-elements/or-transact-sql.md) opérateur), plutôt que d’évaluer l’intersection des deux conditions (à l’aide de la [AND](../../t-sql/language-elements/and-transact-sql.md) opérateur).  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergefilter](../../relational-databases/replication/codesnippet/tsql/sp-addmergefilter-transa_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addmergefilter**.  
  
## <a name="see-also"></a>Voir aussi  
 [Définir un article](../../relational-databases/replication/publish/define-an-article.md)   
 [Define and Modify a Join Filter Between Merge Articles](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Filtres de jointure](../../relational-databases/replication/merge/join-filters.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
