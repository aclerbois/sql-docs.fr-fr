---
title: MSSQL_ENG021797 | Microsoft Docs
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
- MSSQL_ENG021797 error
ms.assetid: 54d83a1e-43fd-449c-a2b2-fdda2609a534
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0590d2fe828185cc0354c4b97b4c9f307f86d5de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng021797"></a>MSSQL_ENG021797
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|21797|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|'%s' doit être une connexion Windows valide sous la forme 'MACHINE\Login' ou 'DOMAIN\Login'. Consultez la documentation de '%s'.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur est émise par les procédures stockées de réplication ci-dessous, si la valeur spécifiée pour le paramètre **@job_login** est NULL ou n'est pas correcte. Cette erreur peut se produire si un membre du rôle de base de données fixe **db_owner** exécute des scripts à partir d'anciennes versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le modèle de sécurité a changé dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]et ces scripts doivent être mis à jour.  
  
-   [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
-   [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)  
  
-   [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
-   [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)  
  
-   [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)  
  
-   [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)  
  
-   [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)  
  
 Ces procédures stockées peuvent être exécutées par un membre du rôle de serveur fixe **sysadmin** sur le serveur approprié, ou par un membre du rôle de base de données fixe **db_owner** dans la base de données appropriée. Les procédures stockées créent chacune un travail d'Agent et vous permettent de spécifier le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] sous lequel l'Agent s'exécute. Pour les utilisateurs membres du rôle **sysadmin** , les travaux d'Agent sont créés implicitement même si aucun compte Windows n'est spécifié (si un compte est spécifié, il doit être valide) ; les Agents s'exécutent dans le contexte du compte du service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sur le serveur approprié. Bien que ce compte ne soit pas nécessaire, il est recommandé de spécifier un compte distinct pour les Agents. Pour plus d’informations, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Assurez-vous de spécifier un compte Windows valide pour le paramètre **@job_login** de chaque procédure. Si vous avez des scritps de réplication provenant de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mettez-les à jour pour qu'ils incluent les procédures stockées et les paramètres requis par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations, consultez [Mettre à niveau les scripts de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
