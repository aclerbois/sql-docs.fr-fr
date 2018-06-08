---
title: sp_articlecolumn (Transact-SQL) | Documents Microsoft
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
- sp_articlecolumn
- sp_articlecolumn_TSQL
helpviewer_keywords:
- sp_articlecolumn
ms.assetid: 8abaa8c1-d99e-4788-970f-c4752246c577
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 407c4470ae7dad6a871736df822cb00a22882122
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sparticlecolumn-transact-sql"></a>sp_articlecolumn (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet de spécifier les colonnes incluses dans un article pour filtrer verticalement des données dans une table publiée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_articlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column' ]  
    [ , [ @operation = ] 'operation' ]  
    [ , [ @refresh_synctran_procs = ] refresh_synctran_procs ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @change_active = ] change_actve ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @internal = ] 'internal' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication qui contient cet article. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article. *article* est **sysname**, sans valeur par défaut.  
  
 [  **@column=**] **'***colonne***'**  
 Nom de la colonne à ajouter ou supprimer. *colonne* est **sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, toutes les colonnes sont publiées.  
  
 [  **@operation=**] **'***opération***'**  
 Spécifie s'il faut ajouter ou supprimer des colonnes dans un article. *opération* est **nvarchar (5)**, avec add comme valeur par défaut. **ajouter** marque la colonne pour la réplication. **DROP** annule la désignation de la colonne.  
  
 [  **@refresh_synctran_procs=**] *refresh_synctran_procs*  
 Indique si les procédures stockées qui prennent en charge les abonnements de mise à jour immédiate sont actualisées pour qu'il y ait correspondance avec le nombre de colonnes répliquées. *refresh_synctran_procs* est **bits**, avec une valeur par défaut **1**. Si **1**, les procédures stockées sont régénérées.  
  
 [  **@ignore_distributor =**] *ignore_distributor*  
 Indique si cette procédure stockée s'exécute sans connexion au serveur de distribution. *ignore_distributor* est **bits**, avec une valeur par défaut **0**. Si **0**, la base de données doit être activée pour la publication et le cache des articles doit être actualisé pour refléter les nouvelles colonnes répliquées par l’article. Si **1**, les colonnes des articles qui résident dans une base de données non publiée ; doivent être utilisées uniquement dans les situations de récupération peuvent être supprimées.  
  
 [  **@change_active =** ] *change_active*  
 Autorise la modification des colonnes dans les publications possédant des abonnements. *change_active* est un **int** avec une valeur par défaut **0**. Si **0**, les colonnes ne sont pas modifiées. Si **1**, les colonnes peuvent être ajoutés ou supprimés à partir d’articles actifs possédant des abonnements.  
  
 [  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Signale que l'action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article n’invalident pas l’instantané n’est pas valide. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article peuvent invalider l’instantané n’est pas valide, s’il existe des abonnements nécessitant un nouvel instantané pour générer un nouvel instantané et de l’instantané existant soit marqué comme obsolète.  
  
 [ **@force_reinit_subscription =** ] *force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article ne provoquent pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification requiert la réinitialisation des abonnements, une erreur se produit et aucune modification n'est effectuée. **1** indique que les modifications apportées à l’article entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
 [  **@publisher=** ] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé avec un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
 [  **@internal=** ] **'***interne***'**  
 À usage interne uniquement  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_articlecolumn** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
 Seul un article ne peut être filtré à l’aide de **sp_articlecolumn**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articlecolumn-transac_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_articlecolumn**.  
  
## <a name="see-also"></a>Voir aussi  
 [Définir un article](../../relational-databases/replication/publish/define-an-article.md)   
 [Définir et modifier un filtre de colonne](../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)   
 [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articleview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
