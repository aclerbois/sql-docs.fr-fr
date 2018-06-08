---
title: sp_mergedummyupdate (Transact-SQL) | Documents Microsoft
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
- sp_mergedummyupdate_TSQL
- sp_mergedummyupdate
helpviewer_keywords:
- sp_mergedummyupdate
ms.assetid: b834f7f6-9588-4d59-a3e2-83d8e8e722e1
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3d31ad74486c8be2b964f4dd3f02c5b5a04ac0b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spmergedummyupdate-transact-sql"></a>sp_mergedummyupdate (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Effectue une mise à jour fictive de la ligne indiquée, de sorte qu'elle soit renvoyée lors de la prochaine fusion. Cette procédure stockée peut être exécutée sur la base de données de publication du serveur de publication ou sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergedummyupdate [ @source_object =] 'source_object', [ @rowguid =] 'rowguid'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@source_object=**] **'***source_object***'**  
 Est le nom de l’objet source. *source_object*est **nvarchar (386)**, sans valeur par défaut.  
  
 [  **@rowguid=**] **'***rowguid***'**  
 Identificateur de la ligne. *ROWGUID* est **uniqueidentifier**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_mergedummyupdate** est utilisé dans la réplication de fusion.  
  
 **sp_mergedummyupdate** est utile si vous écrivez votre propre pour les conflits de réplication (Wzcnflct.exe).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** du rôle de base de données fixe peut exécuter **sp_mergedummyupdate**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
