---
title: Propriétés du serveur de publication - Serveur de distribution | Microsoft Docs
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
- sql13.rep.configdistwizard.distpubproperties.f1
helpviewer_keywords:
- Publisher Properties dialog box
ms.assetid: ab6ada76-0f99-43fe-b524-baac7b1bc483
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9aba1da54540bec767755238707c5c18b933fd91
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publisher-properties---distributor"></a>Propriétés du serveur de publication - Serveur de distribution
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Propriétés du serveur de publication** permet d'afficher et de modifier les propriétés associées à la relation existant entre le serveur de publication et son serveur de distribution.  
  
## <a name="options"></a>Options  
 **Connexion de l'agent au serveur de publication**  
 Permet d'ajouter le contexte selon lequel les agents suivants établissent les connexions du serveur de distribution au serveur de publication :  
  
-   l'Agent de lecture de la file d'attente propre aux publications transactionnelles autorisant les abonnements de mise à jour dans la file d'attente ;  
  
-   l'Agent d'instantané et l'Agent de lecture du journal pour les publications Oracle.  
  
 Choisissez l'option **Imiter le compte de processus de l'Agent** afin d'établir des connexions au serveur de publication grâce au contexte du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows fourni et sous lequel ces agents doivent s'exécuter, ou bien choisissez **Authentification SQL Server**et saisissez alors une valeur pour chacun des champs **Connexion** et **Mot de passe**. Nous vous recommandons de sélectionner plutôt l'option **Imiter le compte de processus de l'Agent**. Pour plus d’informations sur la sécurité de l’agent, consultez [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 Les comptes Windows sous lesquels ces agents s'exécutent sont précisés dans l'Assistant Nouvelle publication. Ces comptes peuvent être modifiés de l'une des façons suivantes :  
  
-   dans la boîte de dialogue **Propriétés du serveur de distribution** , en ce qui concerne l'Agent de lecture de la file d'attente ;  
  
-   dans la boîte de dialogue **Propriétés du serveur de publication** , dans le cas de l'Agent d'instantané et de l'Agent de lecture du journal.  
  
 **Divers**  
 Les propriétés **Type de serveur de publication** et **Nom de la base de données de distribution** sont des propriétés en lecture seule. La propriété **Dossier d'instantanés par défaut** est cependant modifiable. Pour plus d’informations sur le dossier d’instantanés, consultez [Sécuriser le dossier d’instantanés](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Référence des propriétés &#40;réplication&#41;](../../relational-databases/replication/properties-reference-replication.md)  
  
  
