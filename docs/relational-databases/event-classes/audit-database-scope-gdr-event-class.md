---
title: Audit ﻿Database﻿ Scope GDR, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Audit Database Scope GDR event class
ms.assetid: 1641a38a-ef24-46ce-b2f4-bf732858c771
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d1f0904404f6358647235672dd5be8639f2ea7ae
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="audit-database-scope-gdr-event-class"></a>Audit Database Scope GDR (classe d'événements)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La classe d’événements **Audit Database Scope GDR** se produit quand une instruction GRANT, REVOKE ou DENY est exécutée pour une autorisation d’instruction par un utilisateur dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les actions de base de données uniquement, comme l’octroi d’autorisations sur une base de données.  
  
## <a name="audit-database-scope-gdr-event-class-data-columns"></a>Colonnes de la classe d'événements Audit Database Scope GDR  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**ClientProcessID**|**Int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|**DatabaseID**|**Int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**DatabaseName**|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|**DBUserName**|**nvarchar**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du client.|40|Oui|  
|**EventClass**|**Int**|Type d’événement = 102.|27|non|  
|**EventSequence**|**Int**|Séquence d'un événement donné au sein de la demande.|51|non|  
|**EventSubClass**|**Int**|Type de sous-classe d'événements.<br /><br /> 1=Grant<br /><br /> 2=Revoke<br /><br /> 3=Deny|21|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IsSystem**|**Int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LoginName**|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows au format DOMAINE\nom_utilisateur).|11|Oui|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l’affichage catalogue **sys.server_principals** . Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|**ObjectName**|**nvarchar**|Nom de l'objet référencé.|34|Oui|  
|**ObjectType**|**Int**|Valeur représentant le type de l'objet qui intervient dans l'événement. Cette valeur correspond à la colonne type de l'affichage catalogue **sys.objects** . Pour connaître les valeurs, consultez [Colonne d’événements de trace ObjectType](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Oui|  
|**OwnerName**|**nvarchar**|Nom d'utilisateur de base de données du propriétaire de l'objet.|37|Oui|  
|**Autorisations**|**bigint**|Valeur entière représentant le type d'autorisations vérifiées.<br /><br /> 1=CREATE DATABASE<br /><br /> 2 = CREATE TABLE<br /><br /> 4 = CREATE PROCEDURE<br /><br /> 8 = CREATE VIEW<br /><br /> 16 = CREATE RULE<br /><br /> 32 = CREATE DEFAULT<br /><br /> 64 = BACKUP DATABASE<br /><br /> 128 = BACKUP LOG<br /><br /> 256 = BACKUP TABLE<br /><br /> 512 = CREATE FUNCTION|19|Oui|  
|**RequestID**|**Int**|ID de la demande contenant l'instruction.|49|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|non|  
|**SessionLoginName**|**Nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**Int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**Réussi**|**Int**|1 = réussite. 0 = échec. Exemple : 1 indique la réussite de la vérification d'autorisations, 0 l'échec de cette vérification.|23|Oui|  
|**TargetLoginName**|**nvarchar**|Pour les actions qui ciblent une connexion (l'ajout d'une nouvelle connexion, par exemple), le nom de la connexion ciblée.|42|Oui|  
|**TargetLoginSid**|**image**|Pour les actions qui ciblent une connexion (l'ajout d'une nouvelle connexion, par exemple), numéro d'identification de sécurité (SID) de la connexion ciblée.|43|Oui|  
|**TargetUserName**|**nvarchar**|Pour les actions qui ciblent un utilisateur de base de données (par exemple, accorder une autorisation à un utilisateur), le nom de cet utilisateur.|39|Oui|  
|**TextData**|**ntext**|Texte SQL de l'instruction Grant/Revoke/Deny.| 1|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|**XactSequence**|**bigint**|Jeton utilisé pour décrire la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)  
  
  
