---
title: sp_syscollector_upload_collection_set (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_syscollector_upload_collection_set
- sp_syscollector_upload_collection_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_upload_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: eed9232c-2b0a-4b6a-8ba0-76b7c99f48dc
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 081a5eccfdec4ea8582efb00f6c5f2e74e7f510e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyscollectoruploadcollectionset-transact-sql"></a>sp_syscollector_upload_collection_set (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Démarre un téléchargement de données d'un jeu d'éléments de collecte si le jeu d'éléments de collecte est activé.  
  
> [!IMPORTANT]  
>  Cette procédure stockée peut être uniquement utilisée pour les jeux d'éléments de collecte configurés pour la collecte et le téléchargement de données en mode mis en cache.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_upload_collection_set [[ @collection_set_id = ] collection_set_id ]  
          , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ **@collection_set_id =** ] *collection_set_id*  
 Identificateur local unique pour le jeu d'éléments de collecte. *collection_set_id* est **int** et doit avoir une valeur si *nom* est NULL.  
  
 [  **@name =** ] **'***nom***'**  
 Est le nom de l’ensemble de la collection. *nom* est **sysname** et doit avoir une valeur si *collection_set_id* a la valeur NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Soit *collection_set_id* ou *nom* doit avoir une valeur, les deux ne peut pas être NULL.  
  
 Cette procédure peut être utilisée pour démarrer un téléchargement à la demande pour un jeu d'éléments de collecte en cours d'exécution. Elle peut être uniquement utilisée pour les jeux d'éléments de collecte configurés pour la collecte et le téléchargement de données en mode mis en cache. Cela permet à un utilisateur d'obtenir des données à analyser sans devoir attendre un téléchargement planifié.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **dc_operator** (avec autorisation EXECUTE) rôle de base de données fixe pour exécuter cette procédure.  
  
## <a name="example"></a>Exemple  
 Exécute un téléchargement à la demande d'un jeu d'éléments de collecte nommé `Simple Collection Set`.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_upload_collection_set @name = 'Simple Collection Set' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
