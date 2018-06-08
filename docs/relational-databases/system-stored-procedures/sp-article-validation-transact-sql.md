---
title: sp_article_validation (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- sp_article_validation_TSQL
- sp_article_validation
helpviewer_keywords:
- sp_article_validation
ms.assetid: 44e7abcd-778c-4728-a03e-7e7e78d3ce22
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8eabbaf95392de7e5389fdd54f8724045682ec2a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sparticlevalidation-transact-sql"></a>sp_article_validation (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Lance une demande de validation de données pour l'article spécifié. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication et sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_article_validation [ @publication = ] 'publication'  
    [ , [ @article = ] 'article' ]  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @subscription_level = ] subscription_level ]  
    [ , [ @reserved = ] reserved ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication dans laquelle existe l'article. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article à valider. *article* est **sysname**, sans valeur par défaut.  
  
 [  **@rowcount_only=**] *type_of_check_requested*  
 Spécifie si seul le décompte de lignes est retourné pour la table. *type_of_check_requested* est **smallint**, avec une valeur par défaut **1**.  
  
 Si **0**, effectuer un décompte de lignes et un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 somme de contrôle compatible.  
  
 Si **1**, effectuer une vérification du nombre de lignes uniquement.  
  
 Si **2**, effectuer une somme de contrôle du nombre de lignes et binaires.  
  
 [  **@full_or_fast=**] *full_or_fast*  
 Méthode utilisée pour calculer le nombre de lignes. *full_or_fast* est **tinyint**, et peut prendre l’une des valeurs suivantes.  
  
|**Value**|**Description**|  
|---------------|---------------------|  
|**0**|Effectue un comptage total à l’aide de Count.|  
|**1**|Effectue un comptage rapide à partir de **sysindexes.rows**. Le décompte de lignes **sysindexes** est plus rapide que le décompte de lignes dans la table. Toutefois, **sysindexes** est mis à jour immédiatement, et le nombre de lignes peut être inexact.|  
|**2** (par défaut)|Exécute un comptage rapide conditionnel en essayant d'abord la méthode rapide. Si la méthode rapide affiche des différences, revient à la méthode totale. Si *expected_rowcount* a la valeur NULL et la procédure stockée est en cours d’utilisation pour obtenir la valeur, un Count complète est toujours utilisée.|  
  
 [  **@shutdown_agent=**] *shutdown_agent*  
 Spécifie si l'Agent de distribution doit être fermé immédiatement après l'achèvement de la validation. *shutdown_agent* est **bits**, avec une valeur par défaut **0**. Si **0**, l’Agent de Distribution ne s’arrête pas. Si **1**, l’Agent de Distribution s’arrête après la validation de l’article.  
  
 [  **@subscription_level=**] *subscription_level*  
 Spécifie si la validation est récupérée par un ensemble d'abonnés. *subscription_level* est **bits**, avec une valeur par défaut **0**. Si **0**, la validation est appliquée à tous les abonnés. Si **1**, la validation est appliquée uniquement à un sous-ensemble des abonnés spécifiés par les appels à **sp_marksubscriptionvalidation** dans la transaction actuellement ouverte.  
  
 [  **@reserved=**] *réservé*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ **@publisher**=] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de la demande de validation sur un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_article_validation** est utilisé dans la réplication transactionnelle.  
  
 **sp_article_validation** entraîne la validation des informations doivent être collectées sur l’article spécifié et envoie une requête de validation dans le journal des transactions. Lorsque l'Agent de distribution reçoit la requête, il compare les informations de validation de la requête à la table des Abonnés. Les résultats de la validation sont affichés dans les messages d'alerte du gestionnaire de réplication et de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Seuls les utilisateurs avec toutes les autorisations SELECT sur la table source pour l’article en cours de validation peut exécuter **sp_article_validation**.  
  
## <a name="see-also"></a>Voir aussi  
 [Valider des données répliquées](../../relational-databases/replication/validate-replicated-data.md)   
 [sp_marksubscriptionvalidation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md)   
 [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [sp_table_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
