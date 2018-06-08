---
title: MSpublications (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSpublications
- MSpublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublications system table
ms.assetid: 7a0b3457-7265-4f24-a255-7f055d908f20
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ab3987fddb89b3ec037d191be0d640922675428e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mspublications-transact-sql"></a>MSpublications (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSpublications** table contient une ligne pour chaque publication répliquée par un serveur de publication. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|L’ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**publication_id**|**int**|ID de la publication.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = transactionnelle.<br /><br /> **1** = instantané.<br /><br /> **2** = fusion.|  
|**thirdparty_flag**|**bit**|Indique si une publication est un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données :<br /><br /> **0** = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> **1** = source de données autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**independent_agent**|**bit**|Indique s'il existe un Agent de distribution autonome pour cette publication.|  
|**immediate_sync**|**bit**|Indique si des fichiers de synchronisation sont créés ou recréés lors de chaque exécution de l'Agent d'instantané.|  
|**allow_push**|**bit**|Indique si des abonnements par envoi de données (push) peuvent être créés pour la publication concernée|  
|**allow_pull**|**bit**|Indique si des abonnements par extraction de données (pull) peuvent être créés pour la publication concernée|  
|**allow_anonymous**|**bit**|Indique si des abonnements anonymes peuvent être créés pour la publication concernée|  
|**description**|**nvarchar(255)**|Description de la publication.|  
|**VENDOR_NAME**|**nvarchar(100)**|Nom du fournisseur si le serveur de publication n'est pas une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**retention**|**int**|Période de rétention de la publication, en heures.|  
|**sync_method**|**int**|Méthode de synchronisation :<br /><br /> **0** = native (génère la sortie de copie en bloc de mode natif de toutes les tables).<br /><br /> **1** = character (génère une sortie de copie en bloc en mode caractère de toutes les tables).<br /><br /> **3** = concurrent (génère une sortie de copie en bloc de mode natif de toutes les tables mais ne verrouille pas la table lors de l’instantané).<br /><br /> **4** = Concurrent_c (génère une sortie de copie en bloc en mode caractère de toutes les tables mais ne verrouille pas la table lors de l’instantané)<br /><br /> Les valeurs **3** et **4** sont disponibles pour la réplication transactionnelle et la réplication de fusion, mais pas pour la réplication d’instantané.|  
|**allow_subscription_copy**|**bit**|Active ou désactive la possibilité de copier les bases de données d'abonnement qui sont abonnées à la publication. **0** signifie que la copie est désactivée et **1** qu’elle est activée.|  
|**thirdparty_options**|**int**|Spécifie si l’affichage d’une publication dans le dossier réplication de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est supprimé :<br /><br /> **0** = affichage d’une publication hétérogène dans le dossier réplication de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].<br /><br /> **1** = suppression de l’affichage une publication hétérogène dans le dossier réplication de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].|  
|**allow_queued_tran**|**bit**|Indique si la publication autorise la mise à jour en attente :<br /><br /> **0 =** publication est non en file d’attente.<br /><br /> **1** = publication est en attente.|  
|**options**|**int**|Aucune information n'est disponible pour cette version.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
