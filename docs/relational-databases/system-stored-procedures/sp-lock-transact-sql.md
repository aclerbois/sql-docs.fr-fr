---
title: sp_lock (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_lock_TSQL
- sp_lock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_lock
ms.assetid: 9eaa0ec2-2ad9-457c-ae48-8da92a03dcb0
caps.latest.revision: 56
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7e081db089d628372f4d884a6eb801965acc5516
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="splock-transact-sql"></a>sp_lock (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche des informations sur les verrous.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Pour obtenir des informations sur les verrous dans la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], utilisez le [sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md) vue de gestion dynamique.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_lock [ [ @spid1 = ] 'session ID1' ] [ , [@spid2 = ] 'session ID2' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@spid1 =** ] **'***session ID1***'**  
 Est un [!INCLUDE[ssDE](../../includes/ssde-md.md)] numéro d’ID de session du **sys.dm_exec_sessions** pour lequel l’utilisateur souhaite obtenir des informations de verrouillage. *session ID1* est **int** avec une valeur par défaut NULL. Exécutez **sp_who** pour obtenir des informations de processus sur la session. Si *session ID1* n’est pas spécifié, les informations sur tous les verrous sont affichées.  
  
 [  **@spid2 =** ] **'***session ID2***'**  
 Une autre [!INCLUDE[ssDE](../../includes/ssde-md.md)] numéro d’ID de session du **sys.dm_exec_sessions** qui peut détenir un verrou en même temps que *session ID1* et sur lequel l’utilisateur souhaite également obtenir plus d’informations. *session ID2* est **int** avec une valeur par défaut NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le **sp_lock** jeu de résultats contient une ligne pour chaque verrou détenu par les sessions spécifiées dans le **@spid1** et **@spid2** paramètres. Si ni **@spid1** ni **@spid2** est spécifié, l’ensemble de résultats indique les verrous pour toutes les sessions actuellement actifs dans l’instance de la [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**spid**|**smallint**|Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] numéro d’ID de session pour le processus qui demande le verrou.|  
|**dbid**|**smallint**|Numéro d'identification de la base de données qui contient le verrou. Vous pouvez utiliser la fonction DB_NAME() pour identifier la base de données.|  
|**ID d’objet**|**int**|Numéro d'identification de l'objet sur lequel le verrou est maintenu. Vous pouvez utiliser la fonction OBJECT_NAME() dans la base de données associée pour identifier l'objet. La valeur 99 indique l'existence d'un verrou sur l'une des pages système utilisées pour enregistrer l'allocation des pages dans une base de données.|  
|**IndId**|**smallint**|Numéro d'identification de l'index sur lequel le verrou est maintenu.|  
|**Type**|**NCHAR(4)**|Type du verrou :<br /><br /> RID = verrou sur une seule ligne d'une table, identifiée par un identificateur de ligne (RID).<br /><br /> KEY = verrou dans un index qui protège une plage de clés dans les transactions sérialisables.<br /><br /> PAG = verrou sur une page de données ou d'index.<br /><br /> EXT = verrou sur une extension.<br /><br /> TAB = verrou sur une table complète comprenant l'ensemble des index et des données.<br /><br /> DB = verrou sur une base de données.<br /><br /> FIL = verrou sur un fichier de base de données.<br /><br /> APP = verrou sur une ressource spécifiée par l'application.<br /><br /> MD = verrous sur des métadonnées ou informations de catalogue.<br /><br /> HBT = verrou sur un segment ou index B-Tree. Ces informations sont incomplètes dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> AU = verrou sur une unité d'allocation. Ces informations sont incomplètes dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Ressource**|**nchar(32)**|Valeur identifiant la ressource verrouillée. Le format de la valeur varie selon le type de ressource identifiée dans le **Type** colonne :<br /><br /> **Type** valeur : **ressource** valeur<br /><br /> RID : identificateur de la forme fileid:pagenumber:rid, où fileid désigne le fichier contenant la page, pagenumber la page comportant la ligne et rid la ligne spécifique de la page. fileid correspond à la **file_id** colonne dans la **sys.database_files** vue de catalogue.<br /><br /> CLÉ : Un nombre hexadécimal utilisé en interne par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].<br /><br /> PAG : nombre au format fileid:pagenumber, où fileid identifie le fichier contenant la page et pagenumber la page elle-même.<br /><br /> EXT : nombre identifiant la première page de l'étendue. Ce nombre est de la forme fileid:pagenumber.<br /><br /> TAB : Aucune information fournie car la table est déjà identifiée dans le **ObjId** colonne.<br /><br /> DB : Aucune information fournie car la base de données est déjà identifiée dans le **dbid** colonne.<br /><br /> FIL : Identificateur de fichier, ce qui correspond à la **file_id** colonne dans la **sys.database_files** vue de catalogue.<br /><br /> APP : identificateur unique de la ressource d'application verrouillée. Dans le format DbPrincipleId :\<des deux premières à 16 caractères de la chaîne de ressource >\<valeur de hachage >.<br /><br /> MD : varie d'un type de ressource à l'autre. Pour plus d’informations, consultez la description de la **resource_description** colonne [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> HBT : aucune information n'est fournie. Utilisez le **sys.dm_tran_locks** vue de gestion dynamique à la place.<br /><br /> AU : aucune information n'est fournie. Utilisez le **sys.dm_tran_locks** vue de gestion dynamique à la place.|  
|**Mode**|**nvarchar(8)**|Le mode de verrou est demandé. Valeurs possibles :<br /><br /> NULL = aucun accès n'est accordé à la ressource. Sert d'espace réservé.<br /><br /> Sch-S = stabilité du schéma. Garantit que l'élément d'un schéma, tel qu'une table ou un index, n'est pas supprimé alors qu'une session contient un verrou de stabilité du schéma sur l'élément du schéma.<br /><br /> Sch-M = modification du schéma. Doit être détenu par toute session destinée à modifier le schéma de la ressource spécifiée. Garantit qu'aucune autre session ne fait référence à l'objet indiqué.<br /><br /> S = partage. La session détenant le verrou peut disposer d'un accès partagé à la ressource.<br /><br /> U = mise à jour. Indique qu'un verrouillage de mise à jour a été posé sur des ressources qui peuvent finalement être mises à jour. Utilisé pour éviter les formes courantes de blocages qui se produisent lorsque plusieurs sessions verrouillent des ressources en vue d'une mise à jour éventuelle.<br /><br /> X = exclusif. La session détenant le verrou peut disposer d'un accès exclusif à la ressource.<br /><br /> IS = partage intentionnel. Indique l'intention de placer des verrous S sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> IU = mise à jour intentionnelle. Indique l'intention de placer des verrous U sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> IX = exclusion intentionnelle. Indique l'intention de placer des verrous X sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> SIU = mise à jour intentionnelle partagée. Signale des accès partagés à une ressource dans le but de poser des verrous de mise à jour sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> SIX = partage intentionnel exclusif. Signale des accès partagés à une ressource dans le but de poser des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> UIX = mise à jour exclusive intentionnelle. Signale un verrou de mise à jour sur une ressource dans le but de poser des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> BU = mise à jour en bloc. Utilisé par les opérations en bloc.<br /><br /> RangeS_S = verrou de groupes de clés partagés et de ressources partagées. Indique une analyse de plage sérialisable.<br /><br /> RangeS_U = verrou de groupes de clés partagés et de ressources de mise à jour. Indique une analyse de mise à jour sérialisable.<br /><br /> RangeI_N = insérer le verrou de groupes de clés et de ressources NULL. Utilisé pour tester les étendues avant l'insertion d'une nouvelle clé dans un index.<br /><br /> RangeI_S = verrou de conversion de groupes de clés. Créé par une superposition des verrous RangeI_N et S.<br /><br /> RangeI_U = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et U.<br /><br /> RangeI_X = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et X.<br /><br /> RangeX_S = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et RangeS_S.<br /><br /> RangeX_U = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et RangeS_U.<br /><br /> RangeX_X = verrou de groupes de clés exclusifs et de ressources exclusives. Verrou de conversion utilisé lors de la mise à jour d'une clé dans une étendue.|  
|**État**|**nvarchar(5)**|État de la demande de verrouillage :<br /><br /> CNVRT : le verrou est converti depuis un autre mode, mais la conversion est bloquée par un autre processus qui maintient un verrou dont le mode est en conflit.<br /><br /> GRANT : un verrou a été obtenu.<br /><br /> WAIT : le verrou est bloqué par un autre processus qui maintient un verrou dont le mode est en conflit.|  
  
## <a name="remarks"></a>Notes  
 Les utilisateurs peuvent contrôler le verrouillage des opérations de lecture en utilisant :  
  
-   SET TRANSACTION ISOLATION LEVEL pour spécifier le niveau de verrouillage d'une session. Pour la syntaxe et les restrictions, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
-   des indicateurs de table de verrouillage pour spécifier le niveau de verrouillage d'une référence spécifique d'une table dans une clause FROM. Pour la syntaxe et les restrictions, consultez [indicateurs de Table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Toutes les transactions distribuées qui ne sont pas associées à une session sont des transactions orphelines. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] affecte les transactions distribuées orphelines toutes la valeur SPID -2, ce qui le rend plus facile pour un utilisateur identifier le blocage des transactions distribuées. Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-locks"></a>A. Affichage de tous les verrous  
 L’exemple suivant affiche des informations sur tous les verrous actuellement maintenus dans une instance de la [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
USE master;  
GO  
EXEC sp_lock;  
GO  
```  
  
### <a name="b-listing-a-lock-from-a-single-server-process"></a>B. Affichage des verrous d'un processus de serveur unique  
 L'exemple suivant affiche des informations, notamment à propos des verrous, sur l'ID de processus `53`.  
  
```  
USE master;  
GO  
EXEC sp_lock 53;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
 [DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)   
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_os_threads &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md)  
  
  
