---
title: Sécurité de l’agent de fusion | Microsoft Docs
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
f1_keywords:
- sql13.rep.security.MA.f1
helpviewer_keywords:
- Merge Agent Security dialog box
ms.assetid: 9b86171a-4381-4b39-869a-cdc161e7cd15
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 294edd5ff81d55703b4b8354bec749d4dbda0fee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="merge-agent-security"></a>Sécurité de l'agent de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Sécurité de l'agent de fusion** vous permet de spécifier le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent de fusion doit s'exécuter. L'Agent de fusion s'exécute sur le serveur de distribution pour les abonnements envoyés et sur l'Abonné pour les abonnements extraits. Ce compte Windows est également baptisé *compte de processus*du fait que le processus agent s'exécute sous ce compte. La boîte de dialogue propose des options supplémentaires en fonction de la façon d'y accéder :  
  
-   Si la boîte de dialogue est ouverte à partir de l'Assistant Nouvel abonnement, elle vous propose ainsi en plus d'indiquer le contexte dans lequel l'agent de fusion établit les connexions avec l'Abonné (dans le cas d'abonnements envoyés au serveur) ou avec le serveur de publication et le serveur de distribution (dans le cas d'abonnements extraits du serveur). La connexion peut s'établir à travers un compte Windows ou sous un compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous spécifiez.  
  
-   Si la boîte de dialogue est ouverte à partir de celle intitulée **Propriétés de l'abonnement** , vous devez indiquer le contexte dans lequel l'agent de fusion établit les connexions en cliquant sur le bouton des propriétés (**...**) de la ligne **Connexion de l'Abonné** ou **Connexion du serveur de publication** . Pour plus d’informations sur l’accès à la boîte de dialogue **Propriétés de l’abonnement**, consultez [Afficher et modifier les propriétés d’un abonnement par émission de données (push)](../../relational-databases/replication/view-and-modify-push-subscription-properties.md) et la procédure indiquant comment [Afficher et modifier les propriétés d’un abonnement par extraction (pull)](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
 Tous les comptes doivent être valides, le mot de passe correct étant spécifié pour chaque compte. Les comptes et les mots de passe ne sont pas validés tant qu'un agent ne s'exécute pas.  
  
## <a name="options"></a>Options  
 **Process Account**  
 Permet de saisir un compte sous lequel l'agent de fusion peut s'exécuter.  
  
-   Pour les abonnements par envoi de données, le compte doit :  
  
    -   être au moins un membre du rôle de base de données fixe **db_owner** dans la base de données de distribution ;  
  
    -   être membre de la liste d'accès à la publication (PAL) ;  
  
    -   un compte de session associé à un utilisateur enregistré dans la base de données de publication ;  
  
    -   avoir les autorisations de lecture sur le partage des fichiers d'instantanés.  
  
-   Pour les abonnements par extraction, le compte doit être au moins un membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement.  
  
 Des autorisations supplémentaires sont indispensables si l'identité du compte de processus est empruntée lorsque les connexions sont établies. Voir les sections **Connexion au serveur de publication et au serveur de distribution** et **Connexion à l'Abonné** ci-dessous.  
  
 Le**compte de processus** ne peut être indiqué à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]pour des abonnements par extraction, car l'Agent de fusion ne s'exécute pas sur des instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].  
  
 **Mot de passe** et **Confirmer le mot de passe**  
 Entrez le mot de passe du compte Windows.  
  
 **Connexion au serveur de publication et au serveur de distribution**  
 Dans le cas d'abonnements envoyés, les connexions au serveur de publication et au serveur de distribution s'établissent toujours en empruntant l'identité du compte indiqué dans la zone de texte **Compte de processus** .  
  
 Dans le cas d'abonnements extraits, indiquez si vous voulez que l'agent de fusion établisse les connexions au serveur de publication et au serveur de distribution en empruntant l'identité du compte précisé dans la zone de texte **Compte de processus** ou en utilisant un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous choisissez d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un nom de connexion et un mot de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande de choisir d'emprunter l'identité du compte Windows plutôt que d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le compte Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour la connexion doit :  
  
-   être membre de la liste d'accès à la publication (PAL) ;  
  
-   un compte de session associé à un utilisateur enregistré dans la base de données de publication ;  
  
-   un compte de session associé à un utilisateur enregistré dans la base de données de distribution (bien que l'utilisateur puisse aussi ouvrir sa session en tant qu'Invité) ;  
  
-   avoir les autorisations de lecture sur le partage des fichiers d'instantanés.  
  
 **Connexion à l'Abonné**  
 Pour les abonnements par extraction, les connexions à l'Abonné ont toujours lieu en empruntant l'identité du compte spécifié dans la zone de texte **Compte de processus** .  
  
 Dans le cas d'abonnements envoyés, indiquez si vous voulez que l'agent de fusion établisse les connexions au serveur de publication et au serveur de distribution en empruntant l'identité du compte précisé dans la zone de texte **Compte de processus** ou en utilisant un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous choisissez d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un nom de connexion et un mot de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Il est recommandé de choisir d'emprunter l'identité du compte Windows plutôt que d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le compte Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour la connexion avec l'abonné doit être au moins un membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer les comptes de connexion et les mots de passe dans la réplication](../../relational-databases/replication/security/manage-logins-and-passwords-in-replication.md)   
 [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Présentation des Agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
