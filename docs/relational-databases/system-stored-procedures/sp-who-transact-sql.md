---
title: sp_who (Transact-SQL) | Documents Microsoft
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
- sp_who_TSQL
- sp_who
dev_langs:
- TSQL
helpviewer_keywords:
- sp_who
ms.assetid: 132dfb08-fa79-422e-97d4-b2c4579c6ac5
caps.latest.revision: 48
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f21ac270ac0f448291b5e6eb8874182598e933ef
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spwho-transact-sql"></a>sp_who (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Fournit des informations sur les utilisateurs actuels, les sessions et les processus dans une instance de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Les informations peuvent être filtrées afin de retourner uniquement les processus qui ne sont pas inactifs, ou qui appartiennent à un utilisateur ou à une session spécifique.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_who [ [ @loginame = ] 'login' | session ID | 'ACTIVE' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@loginame =** ] **'***connexion***'** | *ID de session* | **'ACTIVE'**  
 Permet de filtrer l'ensemble de résultats.  
  
 *connexion* est **sysname** qui identifie les processus appartenant à une connexion particulière.  
  
 *ID de session* est un numéro d’identification de session appartenant à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance. *ID de session* est **smallint**.  
  
 **ACTIVE** exclut les sessions en attente pour la commande suivante à partir de l’utilisateur.  
  
 Si aucune valeur n'est fournie, la procédure répertorie toutes les sessions appartenant à l'instance.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_who** renvoie un jeu de résultats avec les informations suivantes.  
  
|Colonne|Data type| Description|  
|------------|---------------|-----------------|  
|**spid**|**smallint**|ID de la session.|  
|**ECID**|**smallint**|ID du contexte d'exécution d'un thread donné associé à un ID de session spécifique.<br /><br /> ECID = {0, 1, 2, 3,... *n*}, où 0 représente toujours le principal ou thread parent et {1, 2, 3,... *n*} les sous-threads.|  
|**status**|**nchar(30)**|État du processus Les valeurs possibles sont les suivantes :<br /><br /> **dormant**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réinitialise la session.<br /><br /> **En cours d’exécution**. La session exécute un ou plusieurs traitements. Lorsque la fonctionnalité MARS (Multiple Active Result Sets) est activée, une session peut exécuter plusieurs traitements. Pour plus d’informations, consultez [à l’aide de Multiple Active Result Sets & #40 ; MARS & #41 ; ](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).<br /><br /> **arrière-plan**. La session exécute une tâche en arrière-plan, telle qu'une détection de blocage.<br /><br /> **restauration**. Un processus d'annulation est en cours dans la session.<br /><br /> **en attente**. La session attend qu'un thread de travail soit disponible.<br /><br /> **exécutable**. La tâche de la session figure dans la file d'attente exécutable d'un planificateur lors de l'attente de l'obtention d'un quantum.<br /><br /> **spinloop**. La tâche de la session attend qu'un verrouillage total de l'UC se libère.<br /><br /> **suspendu**. La session attend la fin d'un événement, tel qu'une E/S.|  
|**nom de connexion**|**nchar(128)**|Nom de connexion associé à ce processus particulier|  
|**Nom d’hôte**|**nchar(128)**|Nom de l'hôte ou de l'ordinateur pour chaque processus|  
|**blk**|**char (5)**|ID de session du processus bloquant, s'il en existe un. Dans les autres cas, cette colonne a la valeur NULL.<br /><br /> Lorsqu'une transaction associée à un ID de session spécifié est bloquée par une transaction distribuée orpheline, cette colonne renvoie la valeur « -2 » pour la transaction orpheline qui bloque.|  
|**dbname**|**nchar(128)**|Base de données dont se sert le processus|  
|**Cmd**|**nchar(16)**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] commande ([!INCLUDE[tsql](../../includes/tsql-md.md)] instruction interne [!INCLUDE[ssDE](../../includes/ssde-md.md)] processus et ainsi de suite) l’exécution du processus.|  
|**request_id**|**int**|ID des demandes s'exécutant dans une session spécifique|  
  
 En cas de traitement parallèle, des sous-threads sont créés pour l'lD de session spécifique. Le thread principal est indiqué sous la forme `spid = <xxx>` et `ecid =0`. Les autres sous-threads ont le même `spid = <xxx>`, mais avec **ecid** > 0.  
  
## <a name="remarks"></a>Notes  
 Un processus bloquant, qui peut disposer d'un verrou exclusif, est un processus qui conserve les ressources dont un autre processus a besoin.  
  
 Toutes les transactions distribuées orphelines reçoivent la valeur d'ID de session « -2 ». Les transactions distribuées orphelines sont des transactions distribuées qui ne sont associées à aucun ID de session. Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
 Requête le **is_user_process** colonne de sys.dm_exec_sessions pour séparer les processus système des processus utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur le serveur pour voir toutes les sessions en cours d'exécution dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans le cas contraire, l'utilisateur ne voit que la session en cours.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-current-processes"></a>A. Affichage de tous les processus en cours  
 L'exemple suivant utilise `sp_who` sans paramètres pour donner la liste de tous les utilisateurs actuels.  
  
```  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
### <a name="b-listing-a-specific-users-process"></a>B. Affichage des processus d'un utilisateur spécifique  
 L'exemple suivant montre comment afficher des informations sur un utilisateur actuel par nom de connexion.  
  
```  
USE master;  
GO  
EXEC sp_who 'janetl';  
GO  
```  
  
### <a name="c-displaying-all-active-processes"></a>C. Affichage de tous les processus actifs  
  
```  
USE master;  
GO  
EXEC sp_who 'active';  
GO  
```  
  
### <a name="d-displaying-a-specific-process-identified-by-a-session-id"></a>D. Affichage d'un processus spécifique identifié par un ID de session  
  
```  
USE master;  
GO  
EXEC sp_who '10' --specifies the process_id;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sys.sysprocesses &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
