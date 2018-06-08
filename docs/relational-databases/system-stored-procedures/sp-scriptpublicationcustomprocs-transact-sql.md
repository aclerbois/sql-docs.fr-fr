---
title: sp_scriptpublicationcustomprocs (Transact-SQL) | Documents Microsoft
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
- sp_scriptpublicationcustomprocs
- sp_scriptpublicationcustomprocs_TSQL
helpviewer_keywords:
- sp_scriptpublicationcustomprocs
ms.assetid: b06102d5-4284-4834-b126-bc0baea49be5
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7e3cad96acad0e5e08d92a70cd795a6022be26f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spscriptpublicationcustomprocs-transact-sql"></a>sp_scriptpublicationcustomprocs (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un script pour les procédures personnalisées INSERT, UPDATE et DELETE pour tous les articles de table d'une publication dans laquelle l'option de schéma de génération automatique de procédure stockée est activée. **sp_scriptpublicationcustomprocs** est particulièrement utile pour configurer des abonnements pour lesquels l’instantané est appliqué manuellement.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_scriptpublicationcustomprocs [ @publication = ] 'publication_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publication**=] **'***publication_name***'**  
 Nom de la publication. *publication_name* est **sysname** sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats qui se compose d’un seul **nvarchar (4000)** colonne. L'ensemble de résultats forme l'instruction CREATE PROCEDURE complète, nécessaire à la création de la procédure stockée personnalisée.  
  
## <a name="remarks"></a>Notes  
 Les procédures personnalisées ne font pas l'objet de scripts pour les articles sans l'option de schéma (0x2) de génération automatique de procédure personnalisée.  
  
 Les procédures suivantes sont utilisées par **sp_scriptpublicationcustomprocs** pour créer des procédures de l’abonné et ne doit pas être exécuté directement :  
  
 **sp_script_reconciliation_delproc**  
  
 **sp_script_reconciliation_insproc**  
  
 **sp_script_reconciliation_vdelproc**  
  
 **sp_script_reconciliation_xdelproc**  
  
 **sp_scriptdelproc**  
  
 **sp_scriptinsproc**  
  
 **sp_scriptmappedupdproc**  
  
 **sp_scriptupdproc**  
  
 **sp_scriptvdelproc**  
  
 **sp_scriptvupdproc**  
  
 **sp_scriptxdelproc**  
  
 **sp_scriptxupdproc**  
  
## <a name="permissions"></a>Autorisations  
 Exécuter autorisation est accordée aux **public**; une vérification de sécurité est effectuée à l’intérieur de cette procédure stockée pour restreindre l’accès aux membres de la **sysadmin** rôle serveur fixe et **db_owner** rôle de base de données fixe dans la base de données actuelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
