---
title: Générer un script SQL (objets de réplication) | Microsoft Docs
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
- sql13.rep.generatesqlscript.f1
helpviewer_keywords:
- Generate SQL Script dialog box
ms.assetid: b7ccc34e-1c22-44b8-8eb5-f6423af3164e
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4922b5913ace9e72459e655e51c7de61ee79d9b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-sql-script-replication-objects"></a>Générer un script SQL (objets de réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Un script de réplication contient les procédures stockées système [!INCLUDE[tsql](../../includes/tsql-md.md)] nécessaires à l'implémentation de composants de réplication en script, tels qu'une publication ou un abonnement. Tous les composants de réplication dans une topologie doivent faire l'objet d'un script et s'intégrer dans un plan de récupération des données en cas de sinistre ; les scripts peuvent également être utilisés pour automatiser des tâches répétitives. La réplication propose deux boîtes de dialogue spécifiques à l'écriture de scripts mettant en œuvre des objets de réplication :  
  
-   **Générer un script SQL**, disponible à partir du menu contextuel du dossier **Replication** ainsi que de tous les sous-dossiers de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cette boîte de dialogue vous permet de générer des scripts d'objets de réplication sur une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Générer un script SQL\<nom_objet>**, disponible à partir du menu contextuel relatif aux publications et aux abonnements. Cette boîte de dialogue vous permet de générer des scripts d'objets individuels.  
  
 Ces boîtes de dialogue génèrent des scripts d'objets sur une instance unique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ils ne se connectent pas à d'autres instances pour permettre générer des scripts d'objets associés.  
  
## <a name="generate-sql-script-options"></a>Options de la boîte de dialogue Générer un script SQL  
 **Propriétés du serveur de distribution**  
 Permet de générer des scripts de procédures stockées pour : activer ou désactiver le serveur de distribution, ajouter ou supprimer des serveurs de publication associés au serveur de distribution, et créer ou supprimer la base de données de distribution.  
  
 **Publications dans les sources de données suivantes**  
 Permet de générer des scripts de procédures stockées pour : activer ou désactiver la publication, et créer ou supprimer des publications, des articles, des abonnements par extraction et des travaux de réplication.  
  
 **Abonnements dans les sources de données suivantes**  
 Permet de générer des scripts de procédures stockées afin de créer ou de supprimer des abonnements extraits et des travaux de réplication.  
  
 **Pour créer ou activer les composants** et **Pour supprimer ou désactiver les composants**  
 Permet d'indiquer si le script doit inclure des commandes de création ou de suppression d'un objet de réplication. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'utiliser la boîte de dialogue pour créer un ensemble de scripts assurant l'activation et la désactivation de composants.  
  
 **Travaux de réplication**  
 Permet de générer des scripts de travaux de réplication en complément des scripts d'appels de procédures stockées. Cette option n'est disponible que lors de la génération de scripts à partir d'un serveur de distribution.  
  
 Les procédures stockées de réplication créent les travaux nécessaires lors de leur exécution ; il n'est donc pas requis de choisir cette option. Il peut toutefois s'avérer utile d'obtenir un enregistrement des travaux créés dans le cas où un seul travail devait être recréé.  
  
## <a name="generate-sql-script-objectname-options"></a>Options de la boîte de dialogue Générer un script SQL \<nom_objet>  
 **Pour créer ou activer les composants** et **Pour supprimer ou désactiver les composants**  
 Permet d'indiquer si le script doit inclure des commandes de création ou de suppression d'un objet de réplication. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'utiliser la boîte de dialogue pour créer un ensemble de scripts d'activation et de désactivation de composants.  
  
 **Travaux de réplication**  
 Cette option n'est disponible qu'à partir de la boîte de dialogue **Générer un script SQL** .  
  
## <a name="see-also"></a> Voir aussi  
 [Création de scripts de réplication](../../relational-databases/replication/scripting-replication.md)  
  
  
