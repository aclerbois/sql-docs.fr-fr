---
title: sp_markpendingschemachange (Transact-SQL) | Documents Microsoft
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
- sp_markpendingschemachange
- sp_markpendingschemachange_TSQL
helpviewer_keywords:
- sp_markpendingschemachange
ms.assetid: 01100309-7bef-4154-85bf-f18489577e37
caps.latest.revision: 12
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e3bfd0bb51e6d269b84fdb57a5a64139ce23cedc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spmarkpendingschemachange-transact-sql"></a>sp_markpendingschemachange (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Procédure utilisée pour la capacité de prise en charge des publications de fusion permettant à un administrateur d'ignorer certaines modifications de schéma en attente pour qu'elles ne soient pas répliquées. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!CAUTION]  
>  Cette procédure stockée peut entraîner des modifications de schéma ne devant pas être répliquées. Elle ne doit être utilisée pour résoudre des problèmes qu'après avoir essayé la réinitialisation, ou d'autres méthodes trop coûteuses en termes de performance.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_markpendingschemachange [@publication = ] 'publication'  
    [ , [ @schemaversion = ] schemaversion ]  
    [ , [ @status = ] 'status' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [**@publication=** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@schemaversion=** ] *schemaversion*  
 Identifie une modification de schéma en attente. *SchemaVersion* est **int**, avec la valeur par défaut **0**. Utilisez [sp_enumeratependingschemachanges &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-enumeratependingschemachanges-transact-sql.md) pour répertorier les modifications de schéma en attente pour la publication.  
  
 [  **@status=** ] **'***état***'**  
 Indique si une modification de schéma en attente va être ignorée. *état* est **nvarchar (10)** avec la valeur par défaut **active**. Si la valeur de *état* est **ignorée**, puis la modification de schéma sélectionné ne sera pas répliquée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_markpendingschemachange** est utilisé avec la réplication de fusion.  
  
 **sp_markpendingschemachange** est une procédure stockée conçue pour la prise en charge de la réplication de fusion et doit être utilisée uniquement lorsque les autres actions correctives, telles que la réinitialisation, pas de résoudre la situation ou sont trop coûteuses dans conditions de performances.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_markpendingschemachange**.  
  
## <a name="see-also"></a>Voir aussi  
 [sysmergeschemachange &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
