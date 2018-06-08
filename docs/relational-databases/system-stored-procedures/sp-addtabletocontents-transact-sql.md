---
title: sp_addtabletocontents (Transact-SQL) | Documents Microsoft
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
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 47400ed08fed28f8b2c6a83189cd640b9c727f9a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Insère des références dans les tables de suivi de fusion pour toutes les lignes d'une table source qui ne sont pas actuellement incluses dans les tables de suivi. Utilisez cette option si vous avez chargé par une grande quantité de données à l’aide **bcp**, lequel ne déclenchent pas de déclencheurs de suivi de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table_name=**] **'***table_name***'**  
 Est le nom de la table. *nom_table* est **sysname**, sans valeur par défaut.  
  
 [  **@owner_name=**] **'***owner_name***'**  
 Est le nom du propriétaire de la table. *owner_name* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@filter_clause=** ] **'***filter_clause***'**  
 Spécifie une clause de filtre qui contrôle les lignes des données récemment chargées à ajouter aux tables de suivi de fusion. *filter_clause* est **nvarchar (4000)**, avec NULL comme valeur par défaut. Si *filter_clause* est **null**, bulk toutes les lignes chargées sont ajoutés.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addtabletocontents** est utilisé uniquement dans la réplication de fusion.  
  
 Les lignes de la *table_name* sont désignés par leur **rowguidcol** et les références sont ajoutées aux tables de suivi de fusion. **sp_addtabletocontents** doit être utilisée après la copie en bloc données dans une table qui est publiée à l’aide de la réplication de fusion. La procédure stockée commence le suivi des lignes qui ont été copiées et garantit que les nouvelles lignes seront incluses lors de la prochaine synchronisation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addtabletocontents**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
