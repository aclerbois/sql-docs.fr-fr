---
title: sp_ivindexhasnullcols (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
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
- sp_ivindexhasnullcols
- sp_ivindexhasnullcols_TSQL
helpviewer_keywords:
- sp_ivindexhasnullcols
ms.assetid: ed2cde63-37e1-43cf-b6ba-3b6114a0f797
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4431ae915c43d6ceb96200c3ebbf6aa976dd4ff3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spivindexhasnullcols-transact-sql"></a>sp_ivindexhasnullcols (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Valide le fait que l'index cluster de la vue indexée est unique et qu'il ne contient aucune colonne acceptant des valeurs NULL lorsque la vue indexée va servir à la création d'une publication transactionnelle. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_ivindexhasnullcols [ @viewname = ] 'view_name'  
        , [ @fhasnullcols= ] field_has_null_columns OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@viewname**=] **'***view_name***'**  
 Nom de la vue à vérifier. *view_name* est **sysname**, sans valeur par défaut.  
  
 [ **@fhasnullcols**=] *field_has_null_columns* sortie  
 Indicateur qui précise si l'index de la vue a des colonnes acceptant des valeurs NULL. *view_name* est **sysname**, sans valeur par défaut. Retourne une valeur de **1** si l’index de la vue a des colonnes acceptant des valeurs NULL. Retourne une valeur de **0** si la vue ne contient-elle pas les colonnes qui acceptent les valeurs NULL.  
  
> [!NOTE]  
>  Si la procédure stockée elle-même renvoie un code de retour de **1**, ce qui signifie que l’exécution de la procédure stockée une défaillance, cette valeur est **0** et doit être ignorée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_ivindexhasnullcols** est utilisé par la réplication transactionnelle.  
  
 Par défaut, les articles de vue indexée d'une publication sont créés en tant que tables sur les Abonnés. Toutefois, lorsque la colonne indexée autorise les valeurs NULL, la vue indexée est créée en tant que vue indexée sur l'Abonné, au lieu de l'être en tant que table. L'exécution de cette procédure stockée indique à l'utilisateur si la vue indexée active est concernée par ce problème.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_ivindexhasnullcols**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
