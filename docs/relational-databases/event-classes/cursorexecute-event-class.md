---
title: "CursorExecute, classe d’événements | Microsoft Docs"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database
ms.service: 
ms.component: event-classes
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- CursorExecute event class
ms.assetid: 83399fd8-cc25-4d3c-8985-7a824ef08e08
caps.latest.revision: 
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: d93f12bc5aa56b08b5cb2c8a5ee7c80f6b352665
ms.sourcegitcommit: 37f0b59e648251be673389fa486b0a984ce22c81
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
# <a name="cursorexecute-event-class"></a>CursorExecute (classe d'événements)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
La classe d’événements **CursorExecute** décrit les événements d’exécution de curseur qui se produisent dans les curseurs de l’interface de programmation d’applications (API). Les événements d’exécution de curseur se produisent lorsque le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée et remplit un curseur à partir du plan d’exécution généré par un événement de préparation de curseur.  
  
 Incluez la classe d’événements **CursorExecute** dans les traces qui enregistrent les performances des curseurs. Lorsque la classe d’événements **CursorExecute** figure dans une trace, la quantité de charge inhérente dépend de la fréquence d’utilisation des curseurs sur la base de données pendant la trace. Si les curseurs sont fortement utilisés, la trace peut dégrader notablement les performances.  
  
## <a name="cursorexecute-event-class-data-columns"></a>Colonnes de données de la classe d'événements CursorExecute  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**ClientProcessID**|**Int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|**DatabaseID**|**Int**|ID de la base de données spécifiée par l’instruction USE *database* ou ID de la base de données par défaut si aucune instruction USE *database*n’a été émise pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**DatabaseName**|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|**EventClass**|**Int**|Type d’événement enregistré = 74.|27|non|  
|**EventSequence**|**Int**|Séquence de la classe d’événements **CursorExecute** dans le traitement.|51|non|  
|**GroupID**|**Int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|**Handle**|**Int**|Entier utilisé par ODBC, OLE DB ou la bibliothèque de base de données pour coordonner l'exécution avec le serveur.|33|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IntegerData**|**Int**|Type de curseur. Valeurs possibles :<br /><br /> 1 = Jeu de clés<br /><br /> 2 = Dynamique<br /><br /> 4 = Avant uniquement<br /><br /> 8 = Statique<br /><br /> 16 = Avance rapide|25|non|  
|**IsSystem**|**Int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LoginName**|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification Windows sous la forme DOMAINE\Nomutilisateur).|11|Oui|  
|**LoginSid**|**image**|Identificateur de sécurité (SID) de l'utilisateur connecté. Vous trouverez ces informations dans l’affichage catalogue **sys.server_principals** . Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|**RequestID**|**Int**|Identification de la demande à l'origine de l'exécution du curseur.|49|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|non|  
|**SessionLoginName**|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**Int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|**XactSequence**|**bigint**|Jeton utilisé pour décrire la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a> Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Curseurs](../../relational-databases/cursors.md)  
  
  
