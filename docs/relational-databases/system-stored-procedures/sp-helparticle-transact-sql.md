---
title: sp_helparticle (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
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
- sp_helparticle_TSQL
- sp_helparticle
helpviewer_keywords:
- sp_helparticle
ms.assetid: 9c4a1a88-56f1-45a0-890c-941b8e0f0799
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b0ac39cd43a2db35512fc806eec56cbd87f4b3de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelparticle-transact-sql"></a>sp_helparticle (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche des informations sur un article. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication. Dans le cas des serveurs de publication Oracle, cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helparticle [ @publication = ] 'publication'   
    [ , [ @article = ] 'article' ]  
    [ , [ @returnfilter = ] returnfilter ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @found = ] found OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom d'un article dans la publication. *article* est **sysname**, avec une valeur par défaut **%**. Si *article* est ne pas fourni, les informations sur tous les articles pour la publication spécifiée sont retournées.  
  
 [  **@returnfilter=**] *filtre_de_renvoi*  
 Indique si la clause filter doit être retournée. *filtre_de_renvoi* est **bits**, avec une valeur par défaut **1**, qui retourne la clause de filtre.  
  
 [ **@publisher**=] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être spécifié quand demander des informations sur un article publié par un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
 [  **@found=** ] *trouvé* sortie  
 À usage interne uniquement  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id d’article**|**int**|ID de l’article.|  
|**nom de l’article**|**sysname**|Nom de l'article.|  
|**objet de base**|**nvarchar (257)**|Nom de la table sous-jacente représentée par l'article ou la procédure stockée.|  
|**objet de destination**|**sysname**|Nom de la table de destination (abonnement)|  
|**objet de synchronisation**|**nvarchar (257)**|Nom de la vue qui définit l’article publié.|  
|**type**|**smallint**|Type d'article :<br /><br /> **1** = basé sur journal.<br /><br /> **3** = basé sur journal avec filtre manuel.<br /><br /> **5** = basé sur journal avec vue manuelle.<br /><br /> **7** = basé sur journal avec filtre manuel et vue manuelle.<br /><br /> **8** = exécution d’une procédure stockée.<br /><br /> **24** = exécution d’une procédure stockée sérialisable.<br /><br /> **32** = procédure stockée (schéma uniquement).<br /><br /> **64** = vue (schéma uniquement).<br /><br /> **96** = la fonction d’agrégation (schéma uniquement).<br /><br /> **128** = fonction (schéma uniquement).<br /><br /> **257** = vue indexée basé sur un journal.<br /><br /> **259** = vue indexée basé sur le journal avec filtre manuel.<br /><br /> **261** = la vue indexée basé sur journal avec vue manuelle.<br /><br /> **263** = la vue indexée basé sur journal avec filtre manuel et vue manuelle.<br /><br /> **320** = la vue indexée (schéma uniquement).<br /><br />|  
|**status**|**tinyint**|Peut être le [& (Bitwise AND)](../../t-sql/language-elements/bitwise-and-transact-sql.md) résultat d’une ou plusieurs ou ces propriétés de l’article :<br /><br /> **0 x 00** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **0 x 01** = l’article est actif.<br /><br /> **0 x 08** = inclut le nom de colonne dans les instructions insert.<br /><br /> **0 x 16** = utilise des instructions paramétrée.<br /><br /> **0 x 32** = utilise des instructions et inclure le nom de colonne dans les instructions insert.|  
|**Filter**|**nvarchar (257)**|Procédure stockée utilisée pour filtrer la table horizontalement. Cette procédure stockée doit avoir été créée à l'aide de la clause FOR REPLICATION.|  
|**description**|**nvarchar(255)**|Entrée descriptive de l'article|  
|**insert_command**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des insertions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**update_command**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des mises à jour avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**delete_command**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des suppressions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**chemin d’accès du script de création**|**nvarchar(255)**|Chemin d'accès et nom d'un script de schéma d'article utilisé pour créer des tables cibles.|  
|**partition verticale**|**bit**|Indique si le partitionnement vertical est activé pour l’article ; où la valeur **1** signifie que le partitionnement vertical est activé.|  
|**pre_creation_cmd**|**tinyint**|Commande de pré-création pour les instructions DROP TABLE, DELETE TABLE, ou TRUNCATE TABLE.|  
|**filter_clause**|**ntext**|Clause WHERE spécifiant le filtrage horizontal.|  
|**schema_option**|**binary(8)**|Bitmap de l’option de génération de schéma pour l’article donné. Pour une liste complète des **schema_option** valeurs, consultez [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Nom du propriétaire de l’objet de destination.|  
|**source_owner**|**sysname**|Propriétaire de l'objet source.|  
|**unqua_source_object**|**sysname**|Nom de l'objet source sans le nom du propriétaire.|  
|**lui**|**sysname**|Propriétaire de la vue qui définit l'article publié. .|  
|**unqualified_sync_object**|**sysname**|Nom de la vue qui définit l'article publié, sans le nom du propriétaire.|  
|**lui**|**sysname**|Propriétaire du filtre.|  
|**unqua_filter**|**sysname**|Nom du filtre, sans le nom du propriétaire.|  
|**auto_identity_range**|**int**|Indicateur signalant si la gestion automatique de plages d'identité était activée sur la publication au moment de sa création. **1** signifie que la plage d’identité automatique est activée ; **0** signifie qu’il est désactivé.|  
|**publisher_identity_range**|**int**|Plage de taille de la plage d’identité sur le serveur de publication si l’article a *identityrangemanagementoption* la valeur **automatique** ou **auto_identity_range** la valeur **true**.|  
|**identity_range**|**bigint**|Plage de taille de la plage d’identité sur l’abonné si l’article a *identityrangemanagementoption* la valeur **automatique** ou **auto_identity_range** la valeur **true**.|  
|**Seuil**|**bigint**|Valeur de pourcentage indiquant le moment où l'Agent de distribution affecte une nouvelle plage d'identité.|  
|**identityrangemanagementoption**|**int**|Indique la gestion des plages d'identité appliquée à l'article.|  
|**fire_triggers_on_snapshot**|**bit**|Indique si les déclencheurs de l'utilisateur répliqués sont exécutés lorsque l'instantané initial est appliqué.<br /><br /> **1** = utilisateur déclencheurs sont exécutés.<br /><br /> **0** = utilisateur déclencheurs ne sont pas exécutées.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helparticle** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe le **db_owner** rôle de base de données fixe ou de la liste d’accès de la publication active peut exécuter **sp_helparticle**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_helptranarticle](../../relational-databases/replication/codesnippet/tsql/sp-helparticle-transact-_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés de l’Article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
