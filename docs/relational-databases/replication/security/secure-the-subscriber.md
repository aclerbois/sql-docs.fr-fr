---
title: Sécuriser l’abonné | Microsoft Docs
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
- subscriptions [SQL Server replication], security
- Subscribers [SQL Server replication], security
- security [SQL Server replication], Subscribers
ms.assetid: c8f0d62a-8b5d-4a21-9aec-223da52bb708
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: eb04ff13dfcf8e05611b868140622cac68c3819f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="secure-the-subscriber"></a>Sécuriser l'abonné
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les Agents de fusion et les Agents de distribution se connectent à l'Abonné. Ces connexions peuvent être effectuées dans le contexte d'un nom de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou d'un nom de connexion Windows. Il est important de fournir un nom de connexion approprié à ces agents tout en respectant le principe consistant à attribuer les droits nécessaires minimaux et à protéger aussi le stockage de tous les mots de passe. Pour des informations sur les autorisations requises pour chaque agent, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="distribution-agent"></a>Agent de distribution  
 Il y a soit un Agent de distribution par abonnement (un agent indépendant, qui est l'agent par défaut pour les publications créées dans l'Assistant Nouvelle publication), soit un Agent de distribution par paire base de données de publication/base de données d'abonnement (un agent partagé). T  
  
 Pour spécifier des informations de connexion pour les abonnements par émission de données, consultez [Créer un abonnement par émission de données](../../../relational-databases/replication/create-a-push-subscription.md).  
  
 Pour spécifier les informations de connexion pour les abonnements par extraction, consultez [Créer un abonnement par extraction](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
## <a name="merge-agent"></a>Agent de fusion  
 Chaque abonnement de fusion a son propre Agent de fusion qui se connecte à la fois à l'éditeur et à l'abonné et les met à jour.  
  
 Pour spécifier des informations de connexion pour les abonnements par émission de données, consultez [Créer un abonnement par émission de données](../../../relational-databases/replication/create-a-push-subscription.md).  
  
 Pour spécifier les informations de connexion pour les abonnements par extraction, consultez [Créer un abonnement par extraction](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
## <a name="immediate-updating-subscriptions"></a>Abonnement mis à jour immédiatement  
 Quand vous configurez un abonnement mis à jour immédiatement, vous spécifiez un compte sur l'Abonné, sous lequel sont effectuées les connexions au serveur de publication. Les connexions sont utilisées par les déclencheurs qui s'exécutent pour l'abonné et propagent les modifications sur le serveur de publication. Trois options sont possibles pour le type de connexion :  
  
-   Un serveur lié qui est créé par la réplication ; la connexion est faite avec les informations d'identification que vous spécifiez au moment de la configuration.  
  
-   Un serveur lié créé par la réplication. La connexion se fait à l'aide des informations d'identification de l'utilisateur qui apporte la modification pour l'abonné.  
  
-   Un serveur lié ou distant que vous avez déjà défini.  
  
> [!IMPORTANT]  
>  Pour spécifier des informations de connexion, utilisez la procédure stockée [sp_link_publication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md). Vous pouvez également utiliser la page **Nom d'accès aux abonnements pouvant être mis à jour** de l'Assistant Nouvel abonnement qui appelle **sp_link_publication**. Dans certaines conditions, cette procédure stockée peut échouer si l'Abonné exécute [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] Service Pack 1 (SP1) ou une version ultérieure, et que le serveur de publication exécute une version antérieure. Si la procédure stockée échoue dans ce scénario, mettez à niveau le serveur de publication vers [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] SP1 ou une version ultérieure.  
  
 Pour plus d’informations, consultez [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle](../../../relational-databases/replication/publish/create-updatable-subscription-to-transactional-publication.md) et [Afficher et modifier les paramètres de sécurité de la réplication](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
> [!IMPORTANT]  
>  Le compte spécifié pour la connexion doit uniquement avoir l'autorisation d'insérer, de mettre à jour et de supprimer des données sur les vues créées par la réplication dans la base de données de publication. Il ne doit pas bénéficier d'autres autorisations. Octroyez des autorisations sur les vues de la base de données de publication qui sont mentionnées dans le panneau **syncobj_***\<Nombre_hexadécimal>* pour le compte que vous avez configuré pour chaque abonné.  
  
## <a name="queued-updating-subscriptions"></a>Abonnements mis à jour en attente  
 Quand vous configurez des abonnements mis à jour en attente, il faut garder à l'esprit deux points relatifs à la sécurité :  
  
-   Il n'y a qu'un seul Agent de lecture de la file d'attente pour chaque serveur de distribution. Il est recommandé de configurer, pour chaque serveur de distribution, au moins une publication qui est activée pour les abonnements mis à jour en attente.  
  
-   L'Agent de lecture de la file d'attente établit des connexions avec le serveur de distribution, avec le serveur de publication et avec chaque Abonné :  
  
    -   Le compte sous lequel l'agent s'exécute et établit des connexions avec le serveur de distribution est spécifié quand vous créez l'agent (si vous utilisez l'Assistant Nouvelle publication, l'agent est créé quand vous créez une nouvelle publication qui est activée pour les abonnements mis à jour).  
  
    -   Le compte sous lequel l'agent établit des connexions avec le serveur de publication est spécifié quand vous configurez la distribution pour un serveur de publication. Spécifiez le compte Windows sous lequel l'agent s'exécute ou un compte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   Le compte sous lequel l'agent établit des connexions avec l'Abonné est spécifié quand vous créez l'abonnement.  
  
    > [!IMPORTANT]  
    >  Utilisez l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour les connexions avec les Abonnés, et spécifiez un autre compte pour la connexion avec chaque Abonné. Si vous utilisez un abonnement par extraction de données (pull), la réplication définit toujours la connexion de façon à ce qu'elle utilise l'authentification Windows (pour les abonnements par extraction de données, la réplication ne peut pas accéder aux métadonnées sur l'Abonné qui doit utiliser l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ). Dans ce cas, modifiez la connexion pour qu'elle utilise l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] après la configuration de l'abonnement.  
  
     Pour plus d’informations, consultez le Guide pratique pour créer un abonnement pouvant être mis à jour à une publication transactionnelle (SQL Server Management Studio) et [Afficher et modifier les paramètres de sécurité de la réplication](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Activer les connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Sécurité et protection &#40;réplication&#41;](../../../relational-databases/replication/security/security-and-protection-replication.md)  
  
  
