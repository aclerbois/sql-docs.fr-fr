---
title: sp_articleview (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- sp_articleview
- sp_articleview_TSQL
helpviewer_keywords:
- sp_articleview
ms.assetid: a3d63fd6-f360-4a2f-8a82-a0dc15f650b3
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 68d7b62faa098eedb77bf1576020f01c090fcbd5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sparticleview-transact-sql"></a>sp_articleview (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée la vue qui définit l'article publié lorsqu'une table est filtrée verticalement ou horizontalement. Cette vue est utilisée comme source filtrée du schéma et des données des tables de destination. Seuls les articles ne faisant pas l'objet d'un abonnement peuvent être modifiés par cette procédure stockée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_articleview [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @view_name = ] 'view_name']  
    [ , [ @filter_clause = ] 'filter_clause']  
    [ , [ @change_active = ] change_active ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @refreshsynctranprocs = ] refreshsynctranprocs ]  
    [ , [ @internal = ] internal ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication contenant l'article. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article. *article* est **sysname**, sans valeur par défaut.  
  
 [  **@view_name=**] **'***view_name***'**  
 Nom de la vue qui définit l'article publié. *view_name* est **nvarchar (386)**, avec NULL comme valeur par défaut.  
  
 [  **@filter_clause=**] **'***filter_clause***'**  
 Clause de restriction (WHERE) qui définit un filtre horizontal. Quand vous entrez la clause de restriction, omettez le mot clé WHERE. *filter_clause* est **ntext**, avec NULL comme valeur par défaut.  
  
 [  **@change_active =** ] *change_active*  
 Autorise la modification des colonnes dans les publications possédant des abonnements. *change_active* est un **int**, avec une valeur par défaut **0**. Si **0**, les colonnes ne sont pas modifiés. Si **1**, les vues peuvent être créées ou recréées sur des articles actifs possédant des abonnements.  
  
 [  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Signale que l'action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article n’invalident pas l’instantané n’est pas valide. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article peuvent invalider l’instantané n’est pas valide, s’il existe des abonnements nécessitant un nouvel instantané pour générer un nouvel instantané et de l’instantané existant soit marqué comme obsolète.  
  
 [  **@force_reinit_subscription =]** *force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits** avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article ne provoquent pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification requiert la réinitialisation des abonnements, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
 [ **@publisher**=] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de la publication d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
 [ **@refreshsynctranprocs** =] *refreshsynctranprocs*  
 Indique si les procédures stockées utilisées pour synchroniser la réplication sont automatiquement recréées. *refreshsynctranprocs* est **bits**, avec 1 comme valeur par défaut.  
  
 **1** signifie que les procédures stockées sont recréées.  
  
 **0** signifie que les procédures stockées ne sont pas recréées.  
  
 [ **@internal**=] *interne*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_articleview** crée la vue qui définit l’article publié et insère l’ID de cette vue dans le **sync_objid** colonne de la [sysarticles &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/sysarticles-transact-sql.md) table et insère le texte de la clause de restriction dans la **filter_clause** colonne. Si toutes les colonnes sont répliquées et il est sans **filter_clause**, le **sync_objid** dans les [sysarticles &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/sysarticles-transact-sql.md) table est définie sur l’ID de la table de base et l’utilisation de **sp_articleview** n’est pas obligatoire.  
  
 Pour publier une table filtrée verticalement (c'est-à-dire pour filtrer des colonnes) exécutez d’abord **sp_addarticle** sans aucune *sync_object* paramètre, exécutez [sp_articlecolumn &#40;&#41; ](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne à répliquer (définition du filtre vertical) et puis exécutez **sp_articleview** pour créer la vue qui définit l’article publié.  
  
 Pour publier une table filtrée horizontalement (c'est-à-dire pour filtrer des lignes), exécutez [sp_addarticle &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) sans aucune *filtre* paramètre. Exécutez [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md), fournissant tous les paramètres y compris *filter_clause*. Puis exécutez **sp_articleview**, en fournissant tous les paramètres y compris ce même *filter_clause*.  
  
 Pour publier une table filtrée verticalement et horizontalement, exécutez [sp_addarticle &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) sans aucune *sync_object* ou *filtre* paramètres. Exécutez [sp_articlecolumn &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne à répliquer, puis exécutez [sp_articlefilter &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) et **sp_ articleview**.  
  
 Si l’article possède déjà une vue qui définit l’article publié, **sp_articleview** supprime la vue existante et en crée un automatiquement. Si la vue a été créée manuellement (**type** dans [sysarticles &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/sysarticles-transact-sql.md) est **5**), la vue existante n’est pas supprimée.  
  
 Si vous créez une procédure stockée de filtre personnalisé et une vue qui définit l’article publié manuellement, n’exécutez pas **sp_articleview**. Définissez-les plutôt comme le *filtre* et *sync_object* paramètres [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), ainsi que le approprié*type* valeur.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articleview-transact-_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_articleview**.  
  
## <a name="see-also"></a>Voir aussi  
 [Définir un article](../../relational-databases/replication/publish/define-an-article.md)   
 [Définir et modifier un filtre de lignes statique](../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
