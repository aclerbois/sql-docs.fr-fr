---
title: sp_replmonitorhelpmergesessiondetail (Transact-SQL) | Documents Microsoft
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
- sp_replmonitorhelpmergesessiondetail
- sp_replmonitorhelpmergesessiondetail_TSQL
helpviewer_keywords:
- sp_replmonitorhelpmergesessiondetail
ms.assetid: 805c92fc-3169-410c-984d-f37e063b791d
caps.latest.revision: 16
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3d76b4c7001f946ad01836c36982d81f90df397c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spreplmonitorhelpmergesessiondetail-transact-sql"></a>sp_replmonitorhelpmergesessiondetail (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations détaillées de niveau article sur une session spécifique de l'Agent de fusion des réplications, qui permettent de surveiller la réplication des fusions. Le jeu de résultats comprend une ligne de détails pour chaque article synchronisé pendant la session. Il comprend également une ligne qui représente l'initialisation de la session et des lignes qui récapitulent les différentes phases de téléchargement (Upload et Download) de la session. Cette procédure stockée est exécutée sur la base de données de distribution du serveur de distribution ou sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorhelpmergesessiondetail [ @session_id = ] session_id  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@session_id** =] *session_id*  
 Spécifie une session d'agent. *session_id* est **int** sans valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**PhaseID**|**int**|Phase de la session de synchronisation, qui peut prendre l'une des valeurs suivantes :<br /><br /> **0** = l’initialisation ou la ligne de synthèse<br /><br /> **1** = téléchargement<br /><br /> **2** = téléchargement|  
|**ArticleName**|**sysname**|Nom de l'article en cours de synchronisation. **ArticleName** contient également des informations de résumé pour les lignes du jeu de résultats qui ne représentent pas les détails de l’article.|  
|**PercentComplete**|**decimal**|Indique le pourcentage de modifications appliqué dans une ligne de détails d'article donnée pour des sessions en cours d'exécution ou ayant échoué.|  
|**RelativeCost**|**decimal**|Indique le temps consacré à la synchronisation de l'article en pourcentage de la durée de synchronisation totale pour la session.|  
|**Duration**|**int**|Durée de la session de l'Agent.|  
|**Inserts**|**int**|Nombre d'insertions dans une session.|  
|**Mises à jour**|**int**|Nombre de mises à jour dans une session.|  
|**Suppressions**|**int**|Nombre de suppressions dans une session.|  
|**Conflits**|**int**|Nombre de conflits qui se sont produits dans une session.|  
|**Code d’erreur**|**int**|ID d'une erreur de session.|  
|**SeqNo**|**int**|Ordre des sessions dans le jeu de résultats.|  
|**RowType**|**int**|Indique le type d'informations que représente chaque ligne du jeu de résultats.<br /><br /> **0** = initialisation<br /><br /> **1** = résumé du téléchargement<br /><br /> **2** = détail de téléchargement de l’article<br /><br /> **3** = résumé du téléchargement<br /><br /> **4** = détail de téléchargement de l’article|  
|**Modifications de schéma**|**int**|Nombre de modifications de schéma dans une session.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorhelpmergesessiondetail** est utilisé pour surveiller la réplication de fusion.  
  
 Lors de l’exécution sur l’abonné, **sp_replmonitorhelpmergesessiondetail** retourne uniquement des informations détaillées sur les 5 dernières sessions de l’Agent de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** ou **replmonitor** du rôle de base de données fixe sur la base de données de distribution sur le serveur de distribution ou sur la base de données d’abonnement sur l’abonné permettre exécuter **sp_replmonitorhelpmergesessiondetail**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
