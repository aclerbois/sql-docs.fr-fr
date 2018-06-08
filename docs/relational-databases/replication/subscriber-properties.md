---
title: Propriétés de l’abonné | Microsoft Docs
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
- sql13.rep.configdistwizard.subscribers.f1
helpviewer_keywords:
- Subscriber Properties dialog box
ms.assetid: 32aa0347-64e4-4aa4-ac57-6bd3e5d52070
caps.latest.revision: 22
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 5321e8a5770a9b15a50a5a80b998fa0f8ac4bdaa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="subscriber-properties"></a>Propriétés de l'abonné
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Propriétés de l'Abonné** contient des informations relatives aux versions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutées par l'Abonné et antérieures à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="options"></a>Options  
 **Connexion de l'agent à l'Abonné**  
 Contexte dans lequel l'Agent de distribution et l'Agent de fusion se connectent à l'Abonné à partir du serveur de distribution. Ceci ne s'applique qu'aux versions antérieures à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 Choisissez l'option **Imiter le compte de processus de l'Agent** afin d'établir des connexions à l'Abonné grâce au contexte du compte de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le Serveur de distribution, ou bien choisissez **Authentification SQL Server**et saisissez alors une valeur pour chacun des champs **Connexion** et **Mot de passe**. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande de sélectionner plutôt l'option **Imiter le compte de processus de l'Agent**.  
  
 Pour [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, les informations de connexion sont spécifiées pour chaque abonnement dans l'Assistant Nouvel abonnement et vous pouvez les modifier dans la boîte de dialogue **Propriétés de l'abonnement** .  
  
 **Planifications des agents par défaut**  
 Planification par défaut utilisée dans l'Assistant Nouvel abonnement pour les Abonnés qui exécutent des versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)].  
  
 **Divers**  
 Inclut des informations relatives à l'abonné et au type d'abonné.  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Référence des propriétés &#40;réplication&#41;](../../relational-databases/replication/properties-reference-replication.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
