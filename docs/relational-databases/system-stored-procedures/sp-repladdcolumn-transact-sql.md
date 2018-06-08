---
title: sp_repladdcolumn (Transact-SQL) | Documents Microsoft
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
- sp_repladdcolumn_TSQL
- sp_repladdcolumn
helpviewer_keywords:
- sp_repladdcolumn
ms.assetid: d6220f9f-c738-4f9c-bcf8-419994e86c81
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ad4517c4dfa5d71b63fdd07ff7a116f36f6c5038
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sprepladdcolumn-transact-sql"></a>sp_repladdcolumn (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute une colonne à un article de table existant qui a été publié. Permet d'ajouter la nouvelle colonne à tous les serveurs de publication, ou à une publication spécifique, qui publient cette table. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Cette procédure stockée est déconseillée ; elle est prise en charge pour des raisons de compatibilité descendante. Il ne doit être utilisé avec [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] éditeurs et [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] les abonnés de republication. Cette procédure ne doit pas être utilisée sur des colonnes avec des types de données qui ont été introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou ultérieur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_repladdcolumn [ @source_object = ] 'source_object', [ @column = ] 'column' ]  
    [ , [ @typetext = ] 'typetext' ]  
    [ , [ @publication_to_add = ] 'publication_to_add' ]  
    [ , [ @from_agent = ] from_agent ]  
    [ , [ @schema_change_script = ] 'schema_change_script' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @source_object =] '*source_object*'  
 Nom de l'article de table contenant la nouvelle colonne à ajouter. *source_object* est **nvarchar (358**), sans valeur par défaut.  
  
 [ @column =] '*colonne*'  
 Nom de la colonne dans la table à ajouter pour la réplication. *colonne* est **sysname**, sans valeur par défaut.  
  
 [ @typetext =] '*typetext*'  
 Définition de la colonne ajoutée. *TypeText* est **nvarchar (3000)**, sans valeur par défaut. Par exemple, si la colonne order_filled est ajoutée, et il s’agit d’un seul caractère de champ, et non NULL et qu’il a la valeur par défaut **N**, order_filled est le *colonne* paramètre, alors que la définition de la colonne, **char (1) non NULL constraint_name de contrainte par défaut « n »** serait le *typetext* la valeur du paramètre.  
  
 [ @publication_to_add =] '*publication_to_add*'  
 Nom de la publication à laquelle la nouvelle colonne est ajoutée. *publication_to_add* est **nvarchar (4000)**, avec une valeur par défaut **tous les**. Si **tous les**, puis toutes les publications contenant cette table sont affectées. Si *publication_to_add* est spécifié, seule cette publication contient la nouvelle colonne ajoutée.  
  
 [ @from_agent =] *from_agent*  
 Indique si la procédure stockée est exécutée par un agent de réplication. *from_agent* est **int**, avec une valeur par défaut **0**, où la valeur **1** est utilisé lors de cette procédure stockée est exécutée par un agent de réplication et chaque autres cas, la valeur par défaut **0**doit être utilisé.  
  
 [ @schema_change_script =] '*schema_change_script*'  
 Spécifie le nom et le chemin d'accès d'un script [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour modifier les procédures stockées personnalisées générées par le système. *schema_change_script* est **nvarchar (4000)**, avec NULL comme valeur par défaut. La réplication permet aux procédures stockées personnalisées définies par l'utilisateur de remplacer une ou plusieurs procédures par défaut utilisées dans la réplication transactionnelle. *schema_change_script* est exécuté après une modification de schéma est apportée à un article de table répliqué à l’aide de sp_repladdcolumn et peut être utilisée pour effectuer l’une des opérations suivantes :  
  
-   Si des procédures stockées personnalisées sont automatiquement régénérées, *schema_change_script* peut être utilisé pour supprimer ces procédures stockées personnalisées et de les remplacer par les procédures personnalisées stockées définies par l’utilisateur qui prend en charge le nouveau schéma.  
  
-   Si des procédures stockées personnalisées ne sont pas régénérées automatiquement, *schema_change_script*peut être utilisé pour régénérer ces procédures stockées ou personnalisées définies par l’utilisateur de créer des procédures stockées.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Active ou désactive la possibilité d'invalider un instantané. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **1**.  
  
 **1** Spécifie que les modifications apportées à l’article peuvent invalider l’instantané n’est pas valide, et si c’est le cas, la valeur **1** autorise le nouvel instantané de se produire.  
  
 **0** Spécifie que les modifications de l’article n’invalident pas l’instantané n’est pas valide.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Active ou désactive la possibilité de réinitialiser l'abonnement. *force_reinit_subscription* est un **bits** avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article ne provoquent pas la réinitialisation des abonnements.  
  
 **1** Spécifie que les modifications apportées à l’article peuvent invalider l’abonnement pour réinitialisation, et si c’est le cas, la valeur **1** autorise la réinitialisation des abonnements se produise.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe sysadmin et du rôle de base de données fixe db_owner peuvent exécuter sp_repladdcolumn.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités déconseillées dans la réplication SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
