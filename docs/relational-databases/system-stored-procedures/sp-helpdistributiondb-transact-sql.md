---
title: sp_helpdistributiondb (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- sp_helpdistributiondb_TSQL
- sp_helpdistributiondb
helpviewer_keywords:
- sp_helpdistributiondb
ms.assetid: a2917020-26d1-4011-99f8-9212d120fd2d
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 02065dbaa89a16c0d00ce8737bb79f29c5256495
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpdistributiondb-transact-sql"></a>sp_helpdistributiondb (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche les propriétés de la base de données de distribution spécifiée. Cette procédure stockée est exécutée au niveau du serveur de distribution sur la base de données de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdistributiondb [ [ @database= ] 'database_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@database=**] **'***nom_base_de_données***'**  
 Nom de base de données dont les propriétés sont retournées. *database_name* est **sysname**, avec une valeur par défaut **%** pour toutes les bases de données associées au serveur de distribution et sur lequel l’utilisateur dispose d’autorisations.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom de la base de données de distribution.|  
|**rétention_de_distribution_minimale**|**int**|Durée minimale de rétention (exprimée en heures) avant la suppression des transactions|  
|**max_distretention**|**int**|Durée maximale de rétention (exprimée en heures) avant la suppression des transactions|  
|**rétention de l’historique**|**int**|Nombre d'heures de rétention de l'historique|  
|**history_cleanup_agent**|**sysname**|Nom de l'Agent de nettoyage de l'historique|  
|**distribution_cleanup_agent**|**sysname**|Nom de l'Agent de nettoyage de distribution.|  
|**status**|**int**|À usage interne uniquement|  
|**argument dossier_de_données**|**nvarchar(255)**|Nom du répertoire utilisé pour stocker les fichiers de la base de données.|  
|**data_file**|**nvarchar(255)**|Nom du fichier de base de données.|  
|**argument taille_du_fichier_de_données**|**int**|Taille initiale du fichier de données en mégaoctets.|  
|**argument dossier_journal**|**nvarchar(255)**|Nom du répertoire du fichier de base de données.|  
|**fichier_journal**|**nvarchar(255)**|Nom du fichier journal.|  
|**l’argument taille_du_fichier_journal**|**int**|Taille initiale du fichier du journal en mégaoctets.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpdistributiondb** est utilisée dans tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Membres de la **db_owner** rôle de base de données fixe ou **replmonitor** rôle dans une base de données de distribution et les utilisateurs dans la liste d’accès d’une publication à l’aide de la base de données de distribution peuvent exécuter **sp_helpdistributiondb** pour renvoyer des informations relatives au fichier. Membres de la **public** du rôle peuvent exécuter **sp_helpdistributiondb** pour retourner des informations de non lié à un fichier pour les bases de données de distribution auxquels ils ont accès.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_changedistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
