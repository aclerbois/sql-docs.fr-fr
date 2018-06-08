---
title: Écran d'accueil de Data Quality Client | Microsoft Docs
ms.custom: ''
ms.date: 02/29/2012
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dqs.clienthome.f1
ms.assetid: 7c6ec469-bc7d-4d19-8e21-11dcf8ade108
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fb235729da9f818e5c20cad4c180acd4220c0a17
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-quality-client-home-screen"></a>Écran d'accueil de Data Quality Client

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Utilisez cet écran pour accéder aux interfaces utilisateur de chacun des trois groupes de tâches principaux de [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) : gestion des bases de connaissances, projets de qualité des données et administration.  
  
## <a name="options"></a>Options  
  
### <a name="knowledge-base-management"></a>Gestion des bases de connaissances  
 Une base de connaissances DQS désigne un référentiel de métadonnées utilisé par DQS pour améliorer la qualité des données. Ces métadonnées sont créées à la fois par la plateforme DQS lors d'un processus de découverte des connaissances assisté par ordinateur et par le gestionnaire de données dans un processus interactif de gestion des domaines.  
  
 **Nouvelle Base de connaissances**  
 Démarrez le processus de la création d'une base de connaissances de toutes pièces ou basez-le sur les métadonnées d'une base de connaissances existante. Cette commande affiche une page dans laquelle vous pouvez identifier la base de connaissances, la baser sur une base de connaissances existante, sélectionner l'activité de base de connaissances souhaitée, puis créer la base de connaissances.  
  
 **Ouvrir la base de connaissances**  
 Ouvrez une base de connaissances pour gérer ses domaines, procéder à la découverte des connaissances ou créer une stratégie de correspondance. Un clic sur le bouton **Ouvrir la Base de connaissances** affiche la page **Ouvrir la Base de connaissances** qui présente la liste des bases de connaissances existantes avec leurs propriétés, leur état actuel et les détails relatifs à leurs domaines. Sélectionnez une base de connaissances et ouvrez-la depuis la page **Ouvrir la Base de connaissances**.  
  
 **Base de connaissances récente**  
 Dans la liste affichée à l'écran, ouvrez une base de connaissances qui a déjà été créée. Si elle n'est pas verrouillée, cliquez sur la flèche droite et sélectionnez l'activité que vous souhaitez lancer dans la base de connaissances (gestion de l'arborescence du domaine, découverte des connaissances ou stratégie de correspondance).  
  
 Vous pouvez ouvrir une base de connaissances verrouillée et la modifier uniquement si vous êtes à l'origine de son verrouillage. Dans ce cas, la base de connaissances s'ouvre dans l'état dans lequel elle se trouvait lors de sa fermeture, qui est indiqué entre parenthèses. Si une base de connaissances est verrouillée et que vous n'êtes pas à l'origine du verrouillage, vous pouvez uniquement l'ouvrir en lecture seule.  
  
### <a name="data-quality-projects"></a>Projets de qualité des données  
 Un projet de qualité des données désigne le processus dans lequel DQS procède au nettoyage et à la mise en correspondance des données par le biais d'une correction des données assistée par ordinateur et d'un nettoyage de données interactif.  
  
 **Nouveau projet de qualité des données**  
 Lancez le projet de création d'un nouveau projet. Cette commande affiche une page dans laquelle vous pouvez identifier le projet, l'associer à une base de connaissances, afficher les détails de la base de connaissances, sélectionner l'activité de projet souhaitée, puis créer le projet.  
  
 **Ouvrir un projet de qualité des données**  
 Ouvrez un projet afin de procéder au nettoyage ou à la mise en correspondance des données. Un clic sur le bouton **Ouvrir le projet de qualité des données** affiche la page **Ouvrir le projet de qualité des données** qui présente la liste des projets existants avec leurs propriétés, leur état actuel, la base de connaissances utilisée, ainsi que les détails des domaines et des règles de stratégies de correspondance. Sélectionnez un projet et ouvrez-le depuis la page **Ouvrir le projet de qualité des données**.  
  
 **Projet de qualité des données récent**  
 Dans la liste affichée à l'écran, sélectionnez un projet qui a déjà été créé. Vous pouvez ouvrir un projet verrouillé uniquement si vous êtes à l'origine du verrouillage. Dans ce cas, le projet s'ouvre dans l'état dans lequel il se trouvait lors de sa fermeture, qui est indiqué entre parenthèses. Si le projet est terminé, il s'ouvre à l'étape Exporter de l'activité.  
  
### <a name="administration"></a>Administration  
 L'administration de DQS vous permet d'analyser, de configurer et de gérer DQS.  
  
 **Analyse des activités**  
 Affiche une vue de l'état de toutes les activités (à la fois actuelles et historiques) en rapport avec le [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]connecté. Les types d'activités analysées incluent la gestion des connaissances, le projet de qualité des données et la correction des données SSIS.  
  
 **Configuration**  
 Affichez les propriétés de configuration des comptes de service de données de référence (par le biais de Windows Azure Marketplace et directement dans les services de données de référence), les paramètres généraux (nettoyage interactif, mise en correspondance et profilage) et les paramètres de gravité du journal.  
  
## <a name="see-also"></a> Voir aussi  
 [Bases de connaissances et domaines DQS](../data-quality-services/dqs-knowledge-bases-and-domains.md)   
 [Projets de qualité des données &#40;DQS&#41;](../data-quality-services/data-quality-projects-dqs.md)   
 [Administration de DQS](../data-quality-services/dqs-administration.md)  
  
  
