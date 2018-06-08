---
title: sp_publisherproperty (Transact-SQL) | Documents Microsoft
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
- sp_publisherproperty
- sp_publisherproperty_TSQL
helpviewer_keywords:
- sp_publisherproperty
ms.assetid: 0ed1ebc1-a1bd-4aed-9f46-615c5cf07827
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 647bd0de356a8a31c531a027dffeca88cd8c3083
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sppublisherproperty-transact-sql"></a>sp_publisherproperty (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche ou modifie les propriétés de l’éditeur de non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveurs de publication. Cette procédure stockée est exécutée sur le serveur de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_publisherproperty [ @publisher = ] 'publisher'   
   [ , [ @propertyname = ] 'propertyname' ]   
   [ , [ @propertyvalue = ] 'propertyvalue' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [**@publisher** =] **'***publisher***'**  
 Nom du serveur de publication hétérogène. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [**@propertyname** =] **'***propertyname***'**  
 Nom de la propriété qui est définie. *PropertyName* est **sysname**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**xactsetbatching**|Si les transactions sur le serveur de publication sont regroupées en ensembles cohérents sur le plan transactionnel pour un traitement ultérieur, elles sont nommées Xactsets. La valeur **activé** signifie que les Xactsets peuvent être créés, qui est la valeur par défaut. La valeur **désactivé** signifie que les Xactsets sont traités par aucun nouveau XactSet est créés.|  
|**xactsetjob**|Indique si le travail Xactset est activé pour la création des Xactsets. La valeur **activé** signifie que le travail Xactset s’exécute périodiquement pour créer des Xactsets sur le serveur de publication. La valeur **désactivé** signifie que les Xactsets sont uniquement créés par l’Agent de lecture du journal lorsqu’il interroge le serveur de publication pour les modifications.|  
|**xactsetjobinterval**|Intervalle entre les exécutions du travail Xactset, en minutes.|  
  
 Lorsque *propertyname* est omis toutes les propriétés définissables sont retournées.  
  
 [**@propertyvalue** =] **'***propertyvalue***'**  
 Nouvelle valeur du paramètre de la propriété. *PropertyValue* est **sysname**, avec NULL comme valeur par défaut. Lorsque *propertyvalue* est omis, le paramètre actuel de la propriété est renvoyée.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**propertyname**|**sysname**|Retourne les propriétés de publication suivantes qui peuvent être définies :<br /><br /> **xactsetbatching**<br /><br /> **xactsetjob**<br /><br /> **xactsetjobinterval**|  
|**PropertyValue**|**sysname**|Est la valeur actuelle de la propriété dans le **propertyname** colonne.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_publisherproperty** est utilisé dans la réplication transactionnelle pour non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveurs de publication.  
  
 Lorsque seul *publisher* est spécifié, le jeu de résultats inclut les paramètres actuels pour toutes les propriétés qui peuvent être définies.  
  
 Lorsque *propertyname* est spécifié, seule la propriété nommée apparaît dans le jeu de résultats.  
  
 Lorsque tous les paramètres sont spécifiés, la propriété est modifiée et aucun jeu de résultats n'est retourné.  
  
 Lorsque vous modifiez le **xactsetjobinterval** propriété pour un travail en cours d’exécution, vous devez redémarrer le travail pour le nouvel intervalle prenne effet.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe du serveur de distribution peuvent exécuter **sp_publisherproperty**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer le travail d’un jeu de transactions pour un serveur de publication Oracle &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
