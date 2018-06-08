---
title: sp_repldropcolumn (Transact-SQL) | Documents Microsoft
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
- sp_repldropcolumn_TSQL
- sp_repldropcolumn
helpviewer_keywords:
- sp_repldropcolumn
ms.assetid: fdc1ec5f-f108-42b4-a2d8-f06a71913ab8
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2613b4c2aded7ee192bdd456b3d9d51ec1587eda
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sprepldropcolumn-transact-sql"></a>sp_repldropcolumn (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime une colonne d'un article de table existant publié. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Cette procédure stockée est déconseillée ; elle est essentiellement prise en charge pour des raisons de compatibilité descendante. Il ne doit être utilisé avec [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] éditeurs et [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] les abonnés de republication. Cette procédure ne doit pas être utilisée sur des colonnes avec des types de données qui ont été introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou ultérieur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_repldropcolumn [ @source_object = ] 'source_object', [ @column = ] 'column'   
    [ , [ @from_agent = ] from_agent]   
    [ , [ @schema_change_script = ] 'schema_change_script' ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ @source_object =] '*source_object*'  
 Nom de l'article de table contenant la nouvelle colonne à supprimer. *source_object* est nvarchar (258), sans valeur par défaut.  
  
 [ @column =] '*colonne*'  
 Nom de la colonne dans la table à supprimer. *colonne* est de type sysname, sans valeur par défaut.  
  
 [ @from_agent =] *from_agent*  
 Indique si la procédure stockée est exécutée par un agent de réplication. *from_agent* est de type int, avec une valeur par défaut 0, où la valeur 1 est utilisée lorsque cette procédure stockée est exécutée par un agent de réplication, et dans tous les autres cas la valeur par défaut 0 doit être utilisée.  
  
 [ @schema_change_script =] '*schema_change_script*'  
 Spécifie le nom et le chemin d'accès d'un script [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour modifier les procédures stockées personnalisées générées par le système. *schema_change_script* est nvarchar (4000), avec NULL comme valeur par défaut. La réplication permet aux procédures stockées personnalisées définies par l'utilisateur de remplacer une ou plusieurs procédures par défaut utilisées dans la réplication transactionnelle. *schema_change_script* est exécuté après une modification de schéma est apportée à un article de table répliqué à l’aide de sp_repldropcolumn et peut être utilisée pour effectuer l’une des opérations suivantes :  
  
-   Si des procédures stockées personnalisées sont automatiquement régénérées, *schema_change_script* peut être utilisé pour supprimer ces procédures stockées personnalisées et de les remplacer par les procédures personnalisées stockées définies par l’utilisateur qui prend en charge le nouveau schéma.  
  
-   Si des procédures stockées personnalisées ne sont pas régénérées automatiquement, *schema_change_script*peut être utilisé pour régénérer ces procédures stockées ou personnalisées définies par l’utilisateur de créer des procédures stockées.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Active ou désactive la possibilité d'invalider un instantané. *force_invalidate_snapshot* est de type bit, avec 1 comme valeur par défaut.  
  
 1 spécifie que les modifications de l'article peuvent invalider l'instantané et dans ce cas, la valeur 1 autorise la réalisation du nouvel instantané.  
  
 0 spécifie que les modifications de l'article n'invalident pas l'instantané.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Active ou désactive la possibilité de réinitialiser l'abonnement. *force_reinit_subscription* est de type bit, avec 0 comme valeur par défaut.  
  
 0 spécifie que les modifications de l'article ne provoquent pas la réinitialisation de l'abonnement.  
  
 1 spécifie que les modifications de l'article peuvent provoquer la réinitialisation de l'abonnement et dans ce cas, la valeur 1 autorise cette réinitialisation.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe sysadmin sur le serveur de publication ou les membres des rôles de base de données fixes db_owner ou db_ddladmin de la base de données de publication peuvent exécuter sp_repldropcolumn.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités déconseillées dans la réplication SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
