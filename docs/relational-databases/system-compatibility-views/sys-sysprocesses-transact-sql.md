---
title: Sys.sysprocesses (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-compatibility-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysprocesses_TSQL
- sys.sysprocesses_TSQL
- sys.sysprocesses
- sysprocesses
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysprocesses compatibility view
- sysprocesses system table
ms.assetid: 60a36d36-54b3-4bd6-9cac-702205a21b16
caps.latest.revision: 57
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: c3a27e699312793e734d9a94680677eb509a2bd5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssysprocesses-transact-sql"></a>sys.sysprocesses (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient des informations sur les processus en cours d'exécution sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il peut s'agir de processus client ou système. Pour accéder à sysprocesses, vous devez vous trouver dans le contexte de base de données master ou utiliser le nom en trois parties master.dbo.sysprocesses.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|spid|**smallint**|ID de la session [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|kpid|**smallint**|ID de thread Windows.|  
|blocked|**smallint**|ID de la session qui bloque la demande. Si cette colonne est NULL, la demande n'est pas bloquée, ou les informations de session de la session bloquant la demande ne sont pas disponibles (ou ne peuvent pas être identifiées).<br /><br /> -2 = La ressource qui bloque la demande appartient à une transaction distribuée orpheline.<br /><br /> -3 = La ressource qui bloque la demande appartient à une transaction de récupération différée.<br /><br /> -4 = L'ID de session du propriétaire du verrou qui bloque la demande n'a pas pu être déterminé en raison de transitions d'état de verrou interne.|  
|waittype|**binary(2)**|Réservé.|  
|waittime|**bigint**|Temps d'attente total (en millisecondes).<br /><br /> 0 = Le processus n'est pas en attente.|  
|lastwaittype|**nchar(32)**|Chaîne indiquant le nom du dernier type d'attente ou celui du type d'attente actuel.|  
|waitresource|**nchar(256)**|Description textuelle d'une ressource de verrouillage.|  
|dbid|**smallint**|ID de la base de données actuellement utilisée par le processus.|  
|uid|**smallint**|ID de l'utilisateur qui a exécuté la commande. Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
|cpu|**int**|Temps UC cumulé pour l'exécution du processus. L'entrée est mise à jour pour tous les processus, indépendamment de la valeur de l'option SET STATISTICS TIME (ON ou OFF).|  
|physical_io|**bigint**|Nombre total d'opérations d'écriture et de lecture sur disque pour le processus.|  
|memusage|**int**|Nombre de pages du cache de procédures actuellement allouées à ce processus. Un nombre négatif indique que le processus libère de la mémoire allouée par un autre processus.|  
|login_time|**datetime**|Heure à laquelle le processus client s'est connecté au serveur.|  
|last_batch|**datetime**|Dernière exécution par un processus client d'un appel de procédure stockée distante ou d'une instruction EXECUTE.|  
|ecid|**smallint**|ID du contexte d'exécution utilisé pour identifier de façon unique les sous-threads exécutés pour le compte d'un seul et même processus.|  
|open_tran|**smallint**|Nombre de transactions en cours pour le processus.|  
|status|**nchar(30)**|État de l'ID processus. Les valeurs possibles sont les suivantes :<br /><br /> **dormant**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réinitialise la session.<br /><br /> **en cours d’exécution** = la session est en cours d’un ou plusieurs lots. Lorsque la fonctionnalité MARS (Multiple Active Result Sets) est activée, une session peut exécuter plusieurs traitements. Pour plus d’informations, consultez [à l’aide de Multiple Active Result Sets & #40 ; MARS & #41 ; ](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).<br /><br /> **arrière-plan** = la session s’exécute une tâche en arrière-plan, telles que la détection de blocage.<br /><br /> **restauration** = la session a une annulation de la transaction dans le processus.<br /><br /> **en attente** = la session est en attente d’un thread de travail soient disponibles.<br /><br /> **exécutable** = la tâche dans la session est dans la file d’attente exécutable d’un planificateur lors de l’attente pour obtenir un quantum.<br /><br /> **spinloop** = la tâche dans la session est en attente d’un verrouillage spinlock se libère.<br /><br /> **suspendu** = la session est en attente d’un événement, telles que les e/s, pour terminer.|  
|sid|**binary(86)**|GUID (Globally Unique Identifier) de l'utilisateur.|  
|hostname|**nchar(128)**|Nom de la station de travail.|  
|program_name|**nchar(128)**|Nom du logiciel d'application.|  
|hostprocess|**nchar(10)**|Numéro d'identification du processus de la station de travail.|  
|cmd|**nchar(16)**|Commande actuellement en cours d’exécution.|  
|nt_domain|**nchar(128)**|Domaine Windows du client (s'il utilise l'authentification Windows) ou d'une connexion approuvée.|  
|nt_username|**nchar(128)**|Nom d'utilisateur Windows pour le processus (s'il utilise l'authentification Windows) ou une connexion approuvée.|  
|net_address|**nchar(12)**|Identificateur unique affecté à la carte réseau de la station de travail de chaque utilisateur. Lorsqu'un utilisateur se connecte, cet identificateur est inséré dans la colonne net_address.|  
|net_library|**nchar(12)**|Colonne dans laquelle est enregistrée la bibliothèque réseau du client. Chaque processus client arrive sur une connexion réseau. Les connexions réseau ont une bibliothèque réseau associée qui leur permet de se connecter.|  
|loginame|**nchar(128)**|Nom de connexion.|  
|context_info|**binary(128)**|Données stockées dans un lot à l'aide de l'instruction SET CONTEXT_INFO.|  
|sql_handle|**binary(20)**|Représente le lot ou l'objet en cours d'exécution.<br /><br /> **Remarque** cette valeur est dérivée de l’adresse du lot ou de la mémoire de l’objet. Cette valeur n'est pas calculée à l'aide de l'algorithme de hachage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|stmt_start|**int**|Décalage de début de l'instruction SQL en cours pour la colonne sql_handle spécifiée.|  
|stmt_end|**int**|Décalage de fin de l'instruction SQL actuelle pour la colonne sql_handle spécifiée.<br /><br /> -1 = L'instruction en cours s'exécute jusqu'à la fin des résultats renvoyés par la fonction fn_get_sql pour la colonne sql_handle spécifiée.|  
|request_id|**int**|ID de demande. Utilisé pour identifier les requêtes qui s'exécutent dans une session spécifique.|  
  
## <a name="remarks"></a>Notes  
 Si un utilisateur dispose de l'autorisation VIEW SERVER STATE sur le serveur, il voit toutes les sessions en cours d'exécution dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; sinon, il ne voit que la session actuelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
