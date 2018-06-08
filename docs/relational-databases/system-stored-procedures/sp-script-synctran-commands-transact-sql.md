---
title: sp_script_synctran_commands (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- sp_script_synctran_commands
- sp_script_synctran_commands_TSQL
helpviewer_keywords:
- sp_script_synctran_commands
ms.assetid: f132694a-dd05-405b-9d84-21acce9e564a
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3705a28d85c7375aad701d77a517719778954c25
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spscriptsynctrancommands-transact-sql"></a>sp_script_synctran_commands (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Génère un script qui contient le **sp_addsynctrigger** appels à appliquer aux abonnés pour les abonnements pouvant être. Il y a un **sp_addsynctrigger** appeler pour chaque article de la publication. Le script généré contient également le **sp_addqueued_artinfo** appels qui créent le **MSsubsciption_articles** table est nécessaire pour traiter les publications en file d’attente. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_script_synctran_commands [@publication = ] 'publication'  
    [ , [@article = ] 'article']  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publication** =] **'***publication***'**  
 Nom de la publication à écrire. *publication* est **sysname**, sans valeur par défaut.  
  
 [ **@article** =] **'***article***'**  
 Nom de l'article à écrire. *article* est **sysname**, avec une valeur par défaut **tous les**, qui spécifie tous les articles sont écrites.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="results-set"></a>Ensemble de résultats  
 **sp_script_synctran_commands** retourne un jeu de résultats qui se compose d’un seul **nvarchar (4000)** colonne. Le jeu de résultats forme les scripts complets nécessaires pour créer les le **sp_addsynctrigger** et **sp_addqueued_artinfo** appels à appliquer aux abonnés.  
  
## <a name="remarks"></a>Notes  
 **sp_script_synctran_commands** est utilisé dans la réplication transactionnelle et d’instantané.  
  
 **sp_addqueued_artinfo** est utilisée pour les abonnements pouvant être mis en file d’attente.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_script_synctran_commands**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsynctriggers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsynctriggers-transact-sql.md)   
 [sp_addqueued_artinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqueued-artinfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
