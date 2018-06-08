---
title: sp_reinitsubscription (Transact-SQL) | Documents Microsoft
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
- sp_reinitsubscription
- sp_reinitsubscription_TSQL
helpviewer_keywords:
- sp_reinitsubscription
ms.assetid: d56ae218-6128-4ff9-b06c-749914505c7b
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 233448ed17ee55bb2d2c1c2b0a906191c73d4cf6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spreinitsubscription-transact-sql"></a>sp_reinitsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Marque l'abonnement pour la réinitialisation. Cette procédure stockée est exécutée sur le serveur de publication pour les abonnements par envoi de données (push).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_reinitsubscription [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
        , [ @subscriber = ] 'subscriber'  
    [ , [ @destination_db = ] 'destination_db']  
    [ , [ @for_schema_change = ] 'for_schema_change']  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @ignore_distributor_failure = ] ignore_distributor_failure ]   
    [ , [ @invalidate_snapshot = ] invalidate_snapshot ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec une valeur par défaut de tous les.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article. *article* est **sysname**, avec une valeur par défaut de tous les. Pour une publication de la mise à jour immédiate, *article* doit être **tous les**; sinon, la procédure stockée ignore la publication et signale une erreur.  
  
 [  **@subscriber=**] **'***abonné***'**  
 Nom de l'Abonné. *abonné* est **sysname**, sans valeur par défaut.  
  
 [  **@destination_db=**] **'***destination_db***'**  
 Nom de la base de données de destination. *destination_db* est **sysname**, avec une valeur par défaut de tous les.  
  
 [  **@for_schema_change=**] **'***for_schema_change***'**  
 Indique si la réinitialisation se produit à la suite de la modification d'un schéma dans la base de données de publication. *for_schema_change* est **bits**, avec 0 comme valeur par défaut. Si **0**, les abonnements actifs pour les publications qui autorisent la mise à jour immédiate sont réactivés tant que la totalité de la publication et pas seulement certains de ses articles, sont réinitialisés. Ceci implique que la réinitialisation est effectuée à la suite de modifications du schéma. Si **1**, les abonnements actifs ne sont pas réactivés jusqu'à ce que l’Agent d’instantané s’exécute.  
  
 [  **@publisher=** ] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveurs de publication.  
  
 [  **@ignore_distributor_failure=** ] *ignore_distributor_failure*  
 Autorise la réinitialisation même si le serveur de distribution n'existe pas ou est hors connexion. *ignore_distributor_failure* est **bits**, avec 0 comme valeur par défaut. Si **0**, réinitialisation échoue si le serveur de distribution n’existe pas ou est hors connexion.  
  
 [  **@invalidate_snapshot=** ] *invalidate_snapshot*  
 Invalide l'instantané existant de la publication. *invalidate_snapshot* est **bits**, avec 0 comme valeur par défaut. Si **1**, un nouvel instantané est généré pour la publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_reinitsubscription** est utilisé dans la réplication transactionnelle.  
  
 **sp_reinitsubscription** n’est pas pris en charge pour la réplication transactionnelle d’égal à égal.  
  
 Pour les abonnements dans lesquels l'instantané initial est appliqué automatiquement et où la publication n'autorise pas les abonnements pouvant être mis à jour, l'Agent d'instantané doit être exécuté après l'exécution de cette procédure stockée pour que les fichiers du schéma et du programme de copie en bloc soient préparés et que les Agents de distribution puissent ensuite resynchroniser les abonnements.  
  
 Lorsque l'instantané initial est appliqué automatiquement et que la publication autorise les abonnements pouvant être mis à jour, l'Agent de distribution resynchronise l'abonnement à partir des fichiers de schéma et de programme de copie en bloc les plus récents préalablement créés par l'Agent d'instantané. L’Agent de Distribution resynchronise l’abonnement immédiatement après l’exécution de l’utilisateur **sp_reinitsubscription**, si l’Agent de Distribution n’est pas occupé ; sinon, synchronisation peut se produire après l’intervalle de message (spécifié par le paramètre d’invite de commandes de l’Agent de Distribution : **MessageInterval**).  
  
 **sp_reinitsubscription** n’a aucun effet sur les abonnements où l’instantané initial est appliqué manuellement.  
  
 Pour resynchroniser les abonnements anonymes à une publication, passez **tous les** ou NULL en tant que *abonné*.  
  
 La réplication transactionnelle prend en charge la réinitialisation d'abonnements au niveau de l'article. L'instantané de l'article est réappliqué sur l'Abonné au cours de la synchronisation suivante après que l'article a été marqué pour la réinitialisation. Toutefois, si le même Abonné a également souscrit à des articles dépendants, la réapplication de l'instantané sur l'article peut échouer sauf si les articles dépendants de la publication sont également automatiquement réinitialisés sous certaines conditions :  
  
-   Si la commande de pré-création de l'article est 'drop', les articles des vues liées au schéma et les procédures stockées liées au schéma sur l'objet de base de cet article sont également marqués pour la réinitialisation.  
  
-   Si l'option de schéma sur l'article inclut le script d'intégrité référentielle déclarée sur les clés primaires, les articles qui ont des tables de base avec des relations de clés étrangères à des tables de base de l'article réinitialisé sont également marqués pour la réinitialisation.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_reinittranpushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitsubscription-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** , les membres du rôle serveur fixe le **db_owner** rôle de base de données fixe, ou le créateur de l’abonnement peut exécuter **sp_reinitsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Réinitialiser un abonnement](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
