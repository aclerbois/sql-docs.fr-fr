---
title: Audit Database Mirroring Login, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- Audit Database Mirroring Login event class
- database mirroring [SQL Server], event notifications
ms.assetid: d0bd436d-aade-4208-a7e5-75cf3b5d0ce9
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ac0ba1d2748dbb6d4e30c62af36ce2dd823a63c3
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="audit-database-mirroring-login-event-class"></a>Audit Database Mirroring Login, classe d'événements
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un événement **Audit Database Mirroring Login** pour retourner les messages d’audit relatifs à la sécurité du transport de la mise en miroir de bases de données.  
  
## <a name="audit-database-mirroring-login-event-class-data-columns"></a>Colonnes de données de la classe d'événements Audit Database Mirroring Login  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Inutilisé dans cette classe d'événements.|10|Oui|  
|**ClientProcessID**|**Int**|Inutilisé dans cette classe d'événements.|9|Oui|  
|**DatabaseID**|**Int**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**EventClass**|**Int**|Type de classe d'événements capturée. Toujours **154** pour **Audit Database Mirroring Login**.|27|non|  
|**EventSequence**|**Int**|Numéro de séquence de cet événement.|51|non|  
|**EventSubClass**|**Int**|Type de sous-classe d’événements, qui fournit des informations complémentaires concernant chaque classe d’événements. Le tableau suivant répertorie les valeurs des sous-classes d'événements pour cet événement.|21|Oui|  
|**FileName**|**nvarchar**|Méthode d'authentification prise en charge configurée sur le point de terminaison de mise en miroir de bases de données distantes. Lorsque plusieurs méthodes sont disponibles, le point de terminaison cible détermine la première méthode qui sera tentée. Les valeurs possibles sont :<br /><br /> <br /><br /> **Aucune**. Aucune méthode d'authentification n'est configurée.<br /><br /> **NTLM**. Requiert l'authentification NTLM.<br /><br /> **KERBEROS**. Requiert l'authentification Kerberos.<br /><br /> **NEGOTIATE**. Windows négocie la méthode d'authentification.<br /><br /> **CERTIFICATE**. Requiert le certificat configuré pour le point de terminaison, qui est stocké dans la base de données **master** .<br /><br /> **NTLM, CERTIFICATE**. Accepte NTLM ou le certificat du point de terminaison pour l'authentification.<br /><br /> **KERBEROS, CERTIFICATE**. Accepte Kerberos ou le certificat du point de terminaison pour l'authentification.<br /><br /> **NEGOCIATE, CERTIFICAT**. Windows négocie la méthode d'authentification ou un certificat de point de terminaison peut être utilisé pour l'authentification.<br /><br /> **CERTIFICATE, NTLM**. Accepte un certificat de point de terminaison ou NTLM pour l'authentification.<br /><br /> **CERTIFICATE, KERBEROS**. Accepte un certificat de point de terminaison ou Kerberos pour l'authentification.<br /><br /> **CERTIFICAT, NEGOTIATE**. Accepte un certificat de point de terminaison pour l'authentification ou Windows négocie la méthode d'authentification.|36|non|  
|**HostName**|**nvarchar**|Inutilisé dans cette classe d'événements.|8|Oui|  
|**IsSystem**|**Int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|non|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|**ObjectName**|**nvarchar**|Chaîne de connexion utilisée pour cette connexion.|34|non|  
|**OwnerName**|**nvarchar**|Méthode d'authentification prise en charge configurée sur le point de terminaison de mise en miroir de bases de données locales. Lorsque plusieurs méthodes sont disponibles, le point de terminaison cible détermine la première méthode qui sera tentée. Les valeurs possibles sont :<br /><br /> <br /><br /> **Aucune**. Aucune méthode d'authentification n'est configurée.<br /><br /> **NTLM**. Requiert l'authentification NTLM.<br /><br /> **KERBEROS**. Requiert l'authentification Kerberos.<br /><br /> **NEGOTIATE**. Windows négocie la méthode d'authentification.<br /><br /> **CERTIFICATE**. Requiert le certificat configuré pour le point de terminaison, qui est stocké dans la base de données **master** .<br /><br /> **NTLM, CERTIFICATE**. Accepte NTLM ou le certificat du point de terminaison pour l'authentification.<br /><br /> **KERBEROS, CERTIFICATE**. Accepte Kerberos ou le certificat du point de terminaison pour l'authentification.<br /><br /> **NEGOCIATE, CERTIFICAT**. Windows négocie la méthode d'authentification ou un certificat de point de terminaison peut être utilisé pour l'authentification.<br /><br /> **CERTIFICATE, NTLM**. Accepte un certificat de point de terminaison ou NTLM pour l'authentification.<br /><br /> **CERTIFICATE, KERBEROS**. Accepte un certificat de point de terminaison ou Kerberos pour l'authentification.<br /><br /> **CERTIFICAT, NEGOTIATE**. Accepte un certificat de point de terminaison pour l'authentification ou Windows négocie la méthode d'authentification.|37|non|  
|**ProviderName**|**nvarchar**|Méthode d'authentification utilisée pour cette connexion.|46|non|  
|**RoleName**|**nvarchar**|Rôle de la connexion. Il peut prendre la valeur **initiator** ou la valeur **target**.|38|non|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|non|  
|**SPID**|**Int**|ID du processus serveur affecté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus associé au client.|12|Oui|  
|**StartTime**|**datetime**|Heure de début de l'événement, le cas échéant.|14|Oui|  
|**État**|**Int**|Indique l'emplacement dans le code source [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a produit l'événement. Chaque emplacement susceptible de générer cet événement possède un code d'état spécifique. Un spécialiste de l'assistance technique Microsoft peut se servir de ce code d'état afin de déterminer où l'événement s'est produit.|30|non|  
|**TargetUserName**|**nvarchar**|État de connexion. Une des valeurs suivantes :<br /><br /> **INITIAL**<br /><br /> **WAIT LOGIN NEGOTIATE**<br /><br /> **ONE ISC**<br /><br /> **ONE ASC**<br /><br /> **TWO ISC**<br /><br /> **TWO ASC**<br /><br /> **WAIT ISC Confirm**<br /><br /> **WAIT ASC Confirm**<br /><br /> **WAIT REJECT**<br /><br /> **WAIT PRE-MASTER SECRET**<br /><br /> **WAIT VALIDATION**<br /><br /> **WAIT ARBITRATION**<br /><br /> **ONLINE**<br /><br /> **ERROR**<br /><br /> <br /><br /> Remarque : ISC = Initiate Security Context (initialiser le contexte de sécurité). ASC = Accept Security Context.|39|non|  
|**TransactionID**|**bigint**|ID affecté à la transaction par le système.|4|non|  
  
 Le tableau suivant répertorie les valeurs des sous-classes pour cette classe d'événements.  
  
|ID|Sous-classe|Description|  
|--------|--------------|-----------------|  
| 1|Login Success|Un événement Login Success indique que le processus de connexion de la mise en miroir de bases de données adjacentes s'est terminé avec succès.|  
|2|Login Protocol Error|Un événement Login Protocol Error indique que la connexion de mise en miroir des bases de données reçoit un message qui a un format correct mais n'est pas valide pour l'état du processus de connexion en cours. Le message a peut-être été perdu ou envoyé hors séquence.|  
|3|Message Format Error|Un événement Message Format Error indique qu'une connexion de mise en miroir de bases de données a reçu un message qui ne correspond pas au format attendu. Le message a peut-être été corrompu ou un programme autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] envoie peut-être des messages au port que la mise en miroir des bases de données utilise.|  
|4|Negotiate Failure|Un événement Negotiate Failure indique que le point de terminaison de la mise en miroir de bases de données locales et le point de terminaison de la mise en miroir de bases de données distantes prennent mutuellement en charge des niveaux d'authentification exclusifs.|  
|5|Authentication Failure|Un événement Authentication Failure indique qu'un point de terminaison de mise en miroir de bases de données ne peut pas effectuer l'authentification pour la connexion en raison d'une erreur. En ce qui concerne l'authentification Windows, cet événement signale que le point de terminaison de la mise en miroir de bases de données ne peut pas utiliser l'authentification Windows. En ce qui concerne l'authentification basée sur les certificats, cet événement signale que le point de terminaison de la mise en miroir de bases de données ne peut pas accéder au certificat.|  
|6|Authorization Failure|Un événement Authorization Failure indique qu'un point de terminaison de mise en miroir de bases de données a refusé l'autorisation pour la connexion. En ce qui concerne l'authentification Windows, cet événement indique que l'identificateur de sécurité pour la connexion ne correspond pas à un utilisateur de base de données. En ce qui concerne l’authentification basée sur les certificats, cet événement indique que la clé publique fournie dans le message ne correspond pas à un certificat de la base de données **master** .|  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
