---
title: Sécurité de l’Agent de lecture du journal (réplication d’égal à égal) | Microsoft Docs
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
- sql13.rep.p2pwizard.LRA.f1
ms.assetid: 6575e2a8-16bb-449c-bdca-4a4202d0972f
caps.latest.revision: 17
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e089716aefc86c0a4753cb5bd0a0d3150101d725
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="log-reader-agent-security-peer-to-peer-replication"></a>Sécurité de l'Agent de lecture du journal (réplication d'égal à égal)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Sécurité de l'Agent de lecture du journal** permet de définir les comptes sous lesquels l'Agent de lecture du journal sur chaque homologue s'exécute et établit des connexions. Pour plus d’informations sur les autorisations exigées par les agents et les bonnes pratiques méthodes pour la sécurité de la réplication, consultez [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md) et [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md).  
  
> [!NOTE]  
>  Il existe un Agent de lecture du journal pour chaque base de données publiée utilisant la réplication transactionnelle. Si l'Agent de lecture du journal d'une base de données est déjà configuré (pour la publication dans une exécution précédente de cet Assistant ou une autre publication transactionnelle dans la même base de données), vous ne pouvez pas modifier les informations d'identification qu'il utilise dans l'Assistant. Si vous spécifiez de nouvelles informations d'identification, elles sont ignorées. Pour changer les informations d'identification, utilisez la boîte de dialogue **Propriétés de la publication** . Pour plus d’informations, consultez [View and Modify Replication Security Settings](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
## <a name="options"></a>Options  
 Cliquez sur le bouton avec des points de suspension (**...**) dans la ligne de chaque homologue pour accéder à la boîte de dialogue **Sécurité de l'Agent de lecture du journal** . Cliquez sur **Aide** dans la boîte de dialogue **Sécurité de l'Agent de lecture du journal** qui s'ouvre pour plus d'informations sur les autorisations nécessaires aux comptes utilisés par les agents.  
  
 Après avoir défini les paramètres de la boîte de dialogue, les informations de connexion de l'Abonné s'affichent dans la grille.  
  
 **Agents du serveur de publication**  
 Nom de chaque instance de serveur homologue.  
  
 **Base de données d'homologues**  
 Base de données qui fait office de base de données de publication et de base de données d'abonnement sur chaque homologue.  
  
 **Connexion au serveur de distribution**  
 Le contexte dans lequel la connexion au serveur de distribution s'établit. La connexion locale au serveur de distribution est toujours établie en utilisant le contexte du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l’Agent s’exécute. Par conséquent, ce champ contient toujours **Emprunter l’identité '\<Domaine>\\<Connexion>\>'** ou **Emprunter l’identité '\<Ordinateur>\\<Connexion>\>'**.  
  
 **Connexion au serveur de publication**  
 Contexte sous lequel la connexion au serveur de publication est établie. La connexion au serveur de publication peut être établie en utilisant une connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le contexte du compte Windows sous lequel l'Agent s'exécute. Le champ contient l’un des éléments suivants : **Utiliser la connexion '\<Connexion>'**, **Emprunter l’identité '\<Domaine>\\<Connexion\>'** ou **Emprunter l’identité '\<Ordinateur>\\<Connexion\>'**. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'établir toutes les connexions dans le contexte du compte Windows.  
  
## <a name="see-also"></a> Voir aussi  
 [Administrer une topologie d’égal à égal &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)  
  
  
