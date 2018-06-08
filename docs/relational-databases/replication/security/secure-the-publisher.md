---
title: Sécuriser le serveur de publication | Microsoft Docs
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
- logins [SQL Server replication], publication access list
- publications [SQL Server replication], publication access lists
- publication access list (PAL)
- PAL (publication access list)
- Publishers [SQL Server replication], security
- publications [SQL Server replication], security
ms.assetid: 4513a18d-dd6e-407a-b009-49dc9432ec7e
caps.latest.revision: 48
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9be3105dc5264a655487b650eeda270331d4df63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="secure-the-publisher"></a>Sécuriser le serveur de publication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les Agents de réplication suivants se connectent au serveur de publication :  
  
-   l'Agent de lecture du journal ;  
  
-   Agent d'instantané  
  
-   Agent de lecture de la file d'attente  
  
-   Agent de fusion  
  
 Il est recommandé de fournir un nom de connexion approprié pour ces agents, de respecter le principe consistant à attribuer les droits nécessaires minimaux et de protéger le stockage de tous les mots de passe. Pour plus d'informations sur les autorisations requises pour chaque agent, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 Outre la gestion correcte des connexions et des mots de passe, il convient de comprendre le rôle de la liste d'accès à la publication (PAL, Publication Access List). La liste d'accès à la publication sert à activer les connexions pour qu'elles accèdent aux données de publication tout en limitant l'accès ad hoc à la base de données sur le serveur de publication.  
  
## <a name="publication-access-list"></a>Liste d'accès aux publications (PAL)  
 Cette dernière constitue le mécanisme principal assurant la sécurité des publications sur le serveur de publication. La liste d'accès à la publication fonctionne de manière similaire à une liste de contrôle d'accès [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Lorsque vous créez une publication, la réplication crée une liste d'accès aux publications pour cette première. Cette PAL peut être configurée pour contenir un ensemble de noms de connexions et de groupes disposant de l'autorisation d'accès à la publication. Lorsqu'un agent se connecte au serveur de publication ou de distribution et demande l'accès à une publication, les informations d'authentification figurant dans la PAL sont comparées aux informations de connexion fournies par l'agent. Ce processus permet d'assurer un niveau de sécurité supplémentaire vis-à-vis du serveur de publication en empêchant l'utilisation des informations de connexion du serveur de publication et du serveur de distribution par un outil client pouvant procéder à des modifications directement sur le serveur de publication.  
  
> [!NOTE]  
>  La réplication crée un rôle sur le serveur de publication pour que chaque publication exige l'appartenance à la PAL. Le rôle a un nom de la forme **Msmerge_***\<ID_Publication>* pour la réplication de fusion et **MSReplPAL_***\<Id_base_de_données_publication>***_***\<ID_Publication>* pour la réplication transactionnelle et d’instantané.  
  
 Par défaut, les connexions suivantes figurent dans la PAL : les membres du rôle serveur fixe **sysadmin** (au moment de la création de la publication) et le nom de connexion utilisé pour créer la publication. Par défaut, toutes les connexions qui sont membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** sur la base de données de publication peuvent s'abonner à une publication sans être explicitement ajoutées à la PAL.  
  
 Lorsque vous utilisez la PAL, respectez les consignes suivantes :  
  
-   Vous devez associer la connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à un utilisateur de la base de données de publication avant d'ajouter cette connexion à la liste d'accès à la publication.  
  
-   Appliquez le principe du privilège minimal, en n'accordant aux connexions situées dans la PAL que les autorisations dont elles ont besoin pour exécuter les tâches de réplication. N'ajoutez ces noms de connexions à aucun rôle de base de données ou de serveur fixe qui n'est pas nécessaire à la réplication. Pour plus d'informations sur les autorisations nécessaires, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) et [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
-   En cas d'utilisation d'un serveur de distribution distant, les comptes de la liste d'accès à la publication doivent être disponibles à la fois auprès du serveur de publication et auprès du serveur de distribution. Le compte doit être un compte de domaine ou un compte local défini sur les deux serveurs. Les mots de passe associés aux deux connexions doivent être identiques.  
  
-   Si la PAL contient des comptes Windows et si le domaine utilise Active Directory, le compte sous lequel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute doit disposer des autorisations nécessaires pour lire dans Active Directory. Si vous rencontrez des problèmes avec les comptes Windows, assurez-vous que le compte sous lequel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute détient des droits suffisants. Pour plus d'informations, consultez la documentation Windows.  
  
 Pour gérer la liste d’accès à la publication, consultez [Gérer des connexions dans la liste d’accès à la publication](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md).  
  
## <a name="snapshot-agent"></a>Agent d'instantané  
 Il y a un Agent d'instantané pour chaque publication. Pour plus d’informations, consultez [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
## <a name="ftp-snapshot-delivery"></a>Remise d'un instantané via FTP  
 Si vous spécifiez que les instantanés doivent être remis via un partage FTP plutôt qu'un partage UNC, vous devez spécifier une connexion et un mot de passe lors de la configuration de l'accès à FTP : Pour plus d’informations, consultez [Remettre un instantané via FTP](../../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md).  
  
## <a name="log-reader-agent"></a>Agent de lecture du journal  
 Il existe un Agent de lecture du journal pour chaque base de données publiée en vue de la réplication transactionnelle. Pour plus d’informations, consultez [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
## <a name="queue-reader-agent"></a>Agent de lecture de la file d'attente  
 Il existe un Agent de lecture de la file d'attente pour tous les serveurs de publication et les publications (qui permettent les abonnements avec mise à jour en attente) associés à un serveur de distribution donné. Pour plus d’informations, consultez [Activer les abonnements de mise à jour pour les publications transactionnelles](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Activer les connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Sécurité et protection &#40;réplication&#41;](../../../relational-databases/replication/security/security-and-protection-replication.md)  
  
  
