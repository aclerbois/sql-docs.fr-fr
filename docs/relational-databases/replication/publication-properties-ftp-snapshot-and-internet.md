---
title: Propriétés de la publication, Instantané FTP et Internet | Microsoft Docs
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
- sql13.rep.newpubwizard.pubproperties.internetsynchronization.f1
ms.assetid: 8e0198c3-5e4e-418c-9920-78ccbbfc1323
caps.latest.revision: 25
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 80764484ec32e54ce9d48be659811a0c641b78d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publication-properties-ftp-snapshot-and-internet"></a>Propriétés de la publication, Instantané FTP et Internet
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette page vous permet de :  
  
-   définir les propriétés d'envoi de l'instantané via FTP (File Transfer Protocol). Pour plus d’informations, consultez [Transférer des instantanés via FTP](../../relational-databases/replication/transfer-snapshots-through-ftp.md). Pour utiliser FTP afin d'envoyer un instantané, vous devez définir un serveur FTP. Consultez la documentation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows pour plus d'informations.  
  
    > [!NOTE]  
    >  Les modifications des paramètres FTP nécessitent de générer un nouvel instantané.  
  
-   Définissez les propriétés de synchronisation Web de la réplication de fusion sur [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures qui permet de synchroniser les abonnements sur HTTPS (Secure Hypertext Transfer Protocol). Pour utiliser la synchronisation Web, vous devez configurer un serveur IIS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services). Pour plus d’informations, voir [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
## <a name="options"></a>Options  
 **Accéder aux fichiers d'instantanés via FTP**  
 Sélectionnez **Autoriser les abonnés à télécharger des fichiers d'instantanés via le protocole FTP (File Transfer Protocol)** et spécifiez le **nom du serveur FTP**, le **numéro de port**, **le chemin d'accès depuis le dossier racine FTP**, la **connexion**et le **mot de passe**pour permettre aux abonnés d'utiliser FTP pour l'envoi d'instantanés.  
  
 Cette option permet aux abonnés d'utiliser FTP pour extraire les fichiers d'instantanés, mais elle ne les oblige pas à le faire. Si vous sélectionnez cette option, l'Assistant Nouvel abonnement amène par défaut les abonnés à extraire les fichiers d'instantanés via FTP. Pour changer ce paramètre, utilisez la boîte de dialogue **Propriétés de l'abonnement** . Si vous autorisez les abonnés à accéder aux fichiers d'instantanés via FTP, définissez le dossier FTP comme emplacement de stockage des fichiers d'instantanés dans la page **Instantané** de la boîte de dialogue **Propriétés de la publication** . Dans ce cas, l'Agent d'instantané met à jour automatiquement les fichiers du dossier FTP lorsqu'un nouvel instantané est généré. Si l'emplacement ne correspond pas au dossier FTP, vous devez mettre à jour les fichiers manuellement lorsqu'un nouvel instantané est généré. Pour plus d’informations, consultez [Remettre un instantané via FTP](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md).  
  
 **Synchronisation Web**  
 Réplication de fusion uniquement. Sélectionnez **Autoriser les abonnés à se synchroniser en se connectant à un serveur Web**et définissez une adresse de serveur Web pour permettre aux abonnés d'utiliser la synchronisation Web. Le serveur web doit utiliser le protocole SSL (Secure Sockets Layer) et l’adresse web doit être complète, par exemple `https://server.domain.com/synchronize`. Pour plus d’informations, consultez [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Créer et appliquer l’instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
