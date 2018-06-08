---
title: MSSQL_ENG024070 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG024070 error
ms.assetid: 23ac7e00-fab6-429b-9f85-2736a322aa65
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: d1ee8888551ef434c2cf8885066d0d24116cc284
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng024070"></a>MSSQL_ENG024070
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|24070|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le client ne dispose pas d'un privilège qui est obligatoire.|  
  
## <a name="explanation"></a>Explication  
 C'est une erreur générale qui peut être déclenchée qu'une réplication soit utilisée ou non. Dans le cas d'un serveur appartenant à une topologie de réplication, l'erreur est généralement déclenchée car le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est modifié à l'aide du Gestionnaire de contrôle des services [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows et non du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Lorsque vous essayez d'exécuter un travail d'agent après avoir modifié le compte de service, le travail peut échouer avec un message d'erreur similaire au message suivant :  
  
 `Executed as user: \<UserAccount>. Replication-Replication Snapshot Subsystem: agent \<AgentName> failed. Executed as user: \<UserAccount>. A required privilege is not held by the client. The step failed. [SQLSTATE 42000] (Error 14151). The step failed.`  
  
 Ce problème est dû au fait que le Gestionnaire de contrôle des services Windows ne peut pas accorder les autorisations requises au nouveau compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Pour éviter ce problème à l'avenir, utilisez toujours le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la place du Gestionnaire de contrôle des services Windows pour modifier les comptes de service et les mots de passe.  
  
 Pour résoudre ce problème, à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , rétablissez le compte d'origine. Ensuite, à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , passez au nouveau compte. Lors de cette opération, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajoute le nouveau compte au groupe de sécurité suivant :  
  
 SQLServer2008SQLAgentUser$ComputerName$InstanceName  
  
 En tant que membre de ce groupe de sécurité, le nouveau compte est habilité à exécuter le travail de l'agent de réplication.  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Gérer les comptes de connexion et les mots de passe dans la réplication](../../relational-databases/replication/security/manage-logins-and-passwords-in-replication.md)   
 [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)  
  
  
