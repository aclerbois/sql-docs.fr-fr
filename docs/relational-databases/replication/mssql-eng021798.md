---
title: MSSQL_ENG021798 | Microsoft Docs
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
- MSSQL_ENG021798 error
ms.assetid: 596f5092-75ab-4a19-8582-588687c7b089
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9c987c78e1e509963849c03f83dc6e8aecdba2d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng021798"></a>MSSQL_ENG021798
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|21798|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le travail de l'Agent « %1!s! » doit être ajouté à l'aide de « %2!s! » avant de continuer. Consultez la documentation de '%s'.|  
  
## <a name="explanation"></a>Explication  
 Pour créer une publication, vous devez être membre du rôle de serveur fixe **sysadmin** sur le serveur de publication, ou membre du rôle de base de données fixe **db_owner** dans la base de données de publication. Si vous êtes membre du rôle **db_owner** , cette erreur est émise si :  
  
-   Vous exécutez des scripts à partir de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Le modèle de sécurité a changé dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]et ces scripts doivent être mis à jour.  
  
-   La procédure stockée **sp_addpublication** est exécutée avant l’exécution de [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Ceci s'applique à toutes les publications transactionnelles.  
  
-   La procédure stockée **sp_addpublication** est exécutée avant l’exécution de [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md). Ceci s'applique aux publications transactionnelles qui sont activées pour les abonnements de mise à jour en attente (valeur TRUE pour le paramètre **@allow_queued_tran** de **sp_addpublication**).  
  
 Les procédures stockées **sp_addlogreader_agent** et **sp_addqreader_agent** créent chacune un travail d'Agent et vous permettent de spécifier le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent s'exécute. Pour les utilisateurs membres du rôle **sysadmin** , les travaux d'Agents sont créés implicitement si **sp_addlogreader_agent** et **sp_addqreader_agent** ne sont pas exécutées ; les Agents s'exécutent dans le contexte du compte de service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de distribution. Bien que **sp_addlogreader_agent** et **sp_addqreader_agent** ne soient pas nécessaires pour les utilisateurs membres du rôle **sysadmin** , il est recommandé par mesure de sécurité de spécifier un compte distinct pour les Agents. Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Veillez à exécuter les procédures dans le bon ordre. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md). Si vous avez des scripts de réplication provenant de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mettez-les à jour pour qu'ils incluent les procédures stockées et les paramètres requis par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures. Pour plus d’informations, consultez [Mettre à niveau les scripts de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
