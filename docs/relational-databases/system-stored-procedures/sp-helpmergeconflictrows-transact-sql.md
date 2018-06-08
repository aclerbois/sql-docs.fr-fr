---
title: sp_helpmergeconflictrows (Transact-SQL) | Documents Microsoft
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
- sp_helpmergeconflictrows_TSQL
- sp_helpmergeconflictrows
helpviewer_keywords:
- sp_helpmergeconflictrows
ms.assetid: 131395a5-cb18-4795-a7ae-fa09d8ff347f
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 60759dfe84d22e919cf14d6fb33454b2d11bae49
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpmergeconflictrows-transact-sql"></a>sp_helpmergeconflictrows (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie les lignes de la table de conflits spécifiée. Cette procédure stockée est exécutée sur l'ordinateur qui héberge la table de conflits.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergeconflictrows [ [ @publication = ] 'publication' ]  
        , [ @conflict_table = ] 'conflict_table'  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publsher_db' ]   
    [ , [ @logical_record_conflicts = ] logical_record_conflicts ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec une valeur par défaut **%**. Si la publication est spécifiée, tous les conflits qualifiés par la publication sont renvoyés. Par exemple, si le **MSmerge_conflict_Customers** table a des lignes de conflits pour le **WA** et le **autorité de certification** publications, en passant un nom de publication **autorité de certification** récupère les conflits qui se rapportent à la **autorité de certification** publication.  
  
 [  **@conflict_table=**] **'***conflict_table***'**  
 Nom de la table de conflits. *conflict_table* est **sysname**, sans valeur par défaut. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, les tables de conflits sont nommés en utilisant les noms de format avec **MSmerge_conflict_* publication *_* article ***, avec une table pour chaque publication article.  
  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@publisher_db=**] **'***publisher_db***'**  
 Est le nom de la base de données du serveur de publication. *publisher_db* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@logical_record_conflicts=** ] *logical_record_conflicts*  
 Indique si le jeu de résultats contient des informations sur les conflits au niveau des enregistrements logiques. *logical_record_conflicts* est **int**, avec 0 comme valeur par défaut. **1** signifie que les informations de conflit d’enregistrement logique sont retournées.  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_helpmergeconflictrows** renvoie un jeu de résultats composé de la structure de table de base et des colonnes supplémentaires suivantes.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**origin_datasource**|**varchar(255)**|Origine du conflit.|  
|**conflict_type**|**int**|Code indiquant le type de conflit :<br /><br /> **1** = conflit mise à jour : le conflit est détecté au niveau de la ligne.<br /><br /> **2** = conflit de mise à jour de colonne : le conflit est détecté au niveau de la colonne.<br /><br /> **3** = mise à jour supprimer Wins conflit : la suppression gagne le conflit.<br /><br /> **4** = conflit de suppression Wins mise à jour : le rowguid supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **5** = télécharger Insert a échoué : l’insertion provenant de l’abonné n’a pas pu être appliquée sur le serveur de publication.<br /><br /> **6** = Échec du téléchargement du insérer : l’insertion provenant du serveur de publication n’a pas pu être appliquée sur l’abonné.<br /><br /> **7** = télécharger supprimer a échoué : la suppression à l’abonné n’a pas pu être téléchargée sur le serveur de publication.<br /><br /> **8** = Échec du téléchargement du supprimer : la suppression appliquée au serveur de publication n’a pas pu être téléchargée à l’abonné.<br /><br /> **9** = Échec de télécharger la mise à jour : la mise à jour à l’abonné n’a pas pu être appliquée sur le serveur de publication.<br /><br /> **10** = Échec du téléchargement du jour : la mise à jour au serveur de publication n’a pas pu être appliquée à l’abonné.<br /><br /> **12** = logique mise à jour Wins suppression de l’enregistrement : l’enregistrement logique supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **13** = logique enregistrement conflit Insérer Mettre à jour : insertion dans un enregistrement logique est en conflit avec une mise à jour.<br /><br /> **14** = logique enregistrement supprimer Wins mise à jour en conflit : l’enregistrement logique mis à jour qui perd le conflit est enregistré dans cette table.|  
|**reason_code**|**int**|Code d'erreur pouvant dépendre du contexte.|  
|**reason_text**|**varchar(720)**|Description de l'erreur qui peut dépendre du contexte.|  
|**pubid**|**uniqueidentifier**|Identificateur de publication.|  
|**MSrepl_create_time**|**datetime**|Moment où l'information sur les conflits a été ajoutée.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergeconflictrows** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe le **db_owner** rôle de base de données fixe et le **replmonitor** du rôle dans la base de données de distribution peuvent exécuter **sp_helpmergeconflictrows**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher des informations sur les conflits pour les Publications de fusion &#40;programmation Transact-SQL&#41;](../../relational-databases/replication/view-conflict-information-for-merge-publications.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
