---
title: Projets de qualité des données (DQS) | Microsoft Docs
ms.custom: ''
ms.date: 10/01/2012
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a43fc9c0-19b6-414a-8661-4c7c55e0c03e
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0464fb86b6ac1da08d8421026859539beecd7b91
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-quality-projects-dqs"></a>Projets de qualité des données (DQS)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Un projet de qualité des données dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) est un moyen d'utiliser une base de connaissances pour améliorer la qualité des données sources en effectuant des activités de *nettoyage des données* et de *correspondance de données* , puis en exportant les données résultantes dans une base de données SQL Server ou un fichier .csv. Vous pouvez créer un projet de qualité des données comme un projet de nettoyage ou un projet de correspondance pour effectuer les activités respectives. Les projets de nettoyage et de correspondance peuvent être exécutés avec la même base de connaissances, car la connaissance pour le nettoyage et la correspondance des données peut être générée dans la même base de connaissances.  
  
 Un projet de qualité des données présente les avantages suivants :  
  
-   Il vous permet d'effectuer le nettoyage des données sur vos données sources à l'aide de la connaissance dans une base de connaissances DQS.  
  
-   Il vous permet d'effectuer la correspondance de données sur vos données sources à l'aide de la stratégie de correspondance dans une base de connaissances.  
  
-   Il fournit un Assistant pour vous guider au cours des activités de nettoyage et de correspondance, et exporte les données selon votre sélection dans une base de données SQL Server ou un fichier .csv. Le gestionnaire de données peut utiliser le projet de qualité des données pour exécuter et contrôler les étapes de correspondance de données et de nettoyage interactives/assistées par ordinateur.  
  
##  <a name="Cleansing"></a> Projet de qualité des données : activité de nettoyage  
 Un projet de qualité des données de nettoyage vous permet de nettoyer vos données sources selon une base de connaissances. L'activité de nettoyage des données dans DQS est un processus en deux étapes :  
  
1.  Un processus de nettoyage des données *assisté par ordinateur* qui analyse les données sources par rapport à la connaissance dans la base de connaissances, et propose des modifications. Les données traitées sont classées (suggéré, nouveau, non valide, corrigé et correct) par DQS, puis affichées à l'intention de l'utilisateur pour un traitement ultérieur.  
  
2.  Un processus de nettoyage *interactif* qui permet au gestionnaire de données d'approuver, de rejeter ou de modifier les données proposées par le processus de nettoyage des données assisté par ordinateur.  
  
 Pour plus d'informations sur l'activité de nettoyage dans un projet de qualité des données, consultez [Data Cleansing](../data-quality-services/data-cleansing.md).  
  
##  <a name="Matching"></a> Projet de qualité des données : activité de correspondance  
 Un projet de qualité des données de correspondance vous permet d'effectuer l'activité de correspondance en fonction de la stratégie de correspondance dans une base de connaissances pour éviter la duplication de données en identifiant les correspondances exactes et approximatives, et en vous permettant ainsi de supprimer des données en double. Il est recommandé de nettoyer vos données avant d'exécuter la correspondance sur celles-ci. Pour cela :  
  
1.  Créez un projet de qualité des données, sélectionnez l'activité **Nettoyage** , effectuez l'activité de nettoyage de données sur vos données sources, puis exportez-les vers une table dans une base de données SQL Server.  
  
2.  Créez un autre projet de qualité des données à l'aide d'une base de connaissances qui contient une stratégie de correspondance, sélectionnez l'activité **Correspondance** puis, dans la page **Mapper** , sélectionnez la base de données et la table où vous avez exporté les données nettoyées à l'étape 1.  
  
3.  Effectuez l'activité de correspondance sur les données nettoyées.  
  
 Pour plus d'informations sur l'activité de correspondance dans un projet de qualité des données, consultez [Data Matching](../data-quality-services/data-matching.md).  
  
##  <a name="ProfilingNotification"></a> Profilage des données et notifications  
 Tout en exécutant les activités de nettoyage et de correspondance dans un projet de qualité de données, vous pouvez afficher des statistiques et des informations en temps réel sur les données que DQS est en train de traiter. Le profilage des données vous permet d'évaluer l'efficacité des processus de nettoyage et de correspondance, et vous pouvez éventuellement déterminer jusqu'à quel point le nettoyage ou la correspondance des données peuvent améliorer la qualité des données. Le profilage DQS fournit deux dimensions de qualité des données : l' *exhaustivité* (dans quelle mesure les données sont présentes) et la *précision* (dans quelle mesure les données peuvent être utilisées pour l'usage prévu). De plus, selon les informations de profilage des données, les notifications sont affichées à l'utilisateur sur les mesures qui peuvent être prises pour améliorer les opérations de nettoyage et de correspondance des données. Pour plus d'informations sur le profilage des données et les notifications, consultez [Data Profiling and Notifications in DQS](../data-quality-services/data-profiling-and-notifications-in-dqs.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Décrit comment créer un projet de qualité des données.|[Créer un projet de qualité des données](../data-quality-services/create-a-data-quality-project.md)|  
|Décrit comment ouvrir, déverrouiller, renommer et supprimer un projet de qualité des données.|[Ouvrir, déverrouiller, renommer et supprimer un projet de qualité des données](open-unlock-rename-and-delete-a-data-quality-project.md)|  
|Décrit comment ouvrir un projet Integration Services dans [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)].|[Ouvrir des projets Integration Services dans Data Quality Client](../data-quality-services/open-integration-services-projects-in-data-quality-client.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Bases de connaissances et domaines DQS](../data-quality-services/dqs-knowledge-bases-and-domains.md)  
  
  
