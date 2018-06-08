---
title: Administration de DQS | Microsoft Docs
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
helpviewer_keywords:
- dqs administration
- administration
- dqs,adminstration
ms.assetid: 9940ef5d-f6f6-4dec-9414-1077a4d7f12b
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8bd848acf0dc8c45c1aacf884219421a6309ecd5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dqs-administration"></a>administration de dqs

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) vous permet d'administrer et gérer les différentes activités de DQS effectuées sur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)], de configurer les propriétés de niveau serveur relatives aux activités de DQS, de configurer les paramètres de Reference Data Services et de configurer les paramètres de journalisation de DQS. Ces opérations sont effectuées par le biais de la fonctionnalité **Administration** de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]. Selon votre accès de sécurité (rôle) dans DQS, vous êtes autorisé ou non à accéder à certaines fonctionnalités de cette zone.  
  
 Outre ces activités d'administration, cette rubrique offre également des informations sur la sauvegarde et la restauration des bases de données DQS, une activité d'administration qui n'est pas effectuée à l'aide de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)].  
  
 La fonctionnalité d'administration de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] présente les avantages suivants :  
  
-   Permet aux gestionnaires de données d'analyser les différentes activités de DQS sur un [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] à partir de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)].  
  
-   Permet aux administrateurs de DQS d'analyser les activités de DQS sur un [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] à partir de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]et de *terminer* une activité en cours d'exécution ou d' *arrêter* un processus en cours d'exécution dans une activité, si nécessaire.  
  
-   Configurer les paramètres du service de données de référence tels que la configuration de la connectivité à Windows Azure Marketplace et la gestion des fournisseurs de services de données de référence tiers.  
  
-   Configurer les valeurs de seuil pour les activités de nettoyage et de mise en correspondance.  
  
-   Activer/désactiver les notifications dans [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)].  
  
-   Configurer la journalisation en fonction du niveau de gravité des événements.  
  
##  <a name="AdminUsingClent"></a> Activités administratives effectuées à l'aide de Data Quality Client  
 Ces activités sont effectuées à l'aide de la fonctionnalité **Administration** de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)].  
  
### <a name="activity-monitoring"></a>Analyse des activités  
 L'écran **Analyse des activités** de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] affiche des informations détaillées sur chaque activité effectuée sur un [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]. Cet écran est utilisé principalement par le gestionnaire de données pour procéder à une analyse approfondie de toutes les activités effectuées sur le [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] auquel l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] est connectée. Cet écran ne fournit pas de surveillance au niveau du système. En outre, cet écran permet aux administrateurs de DQS de contrôler une activité ou un processus d'une activité en terminant une activité en cours d'exécution ou en arrêtant un processus en cours d'exécution dans une activité, si nécessaire. Les données sont affichées pour la découverte des connaissances, la gestion des domaines, la stratégie de correspondance, le nettoyage, la mise en correspondance et le nettoyage basé sur SQL Server Integration Services (SSIS).  
  
### <a name="configuration"></a>Configuration  
 L'écran **Configuration** de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] permet à l'administrateur de DQS d'effectuer les opérations suivantes :  
  
-   **Données de référence**: configurez les fournisseurs de services de données de référence : Windows Azure Marketplace ou fournisseurs de services de données de référence directs. Après avoir installé les fournisseurs de services de données de référence, vous pouvez mapper un domaine/domaine composite aux données de référence pendant l'activité de gestion des domaines d'une base de connaissances, puis utiliser cette base de connaissances pour l'activité de nettoyage dans un projet de qualité des données. Elle vous permet également de spécifier les paramètres proxy de connexion à Internet pour l'utilisation de Windows Azure Marketplace.  
  
-   **Paramètres généraux**: spécifiez les valeurs de seuil pour le nettoyage et la mise en correspondance des données, et indiquez s'il faut activer les notifications pour le profilage dans [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]. Ces valeurs de seuil sont utilisées par DQS pendant les opérations de nettoyage et de mise en correspondance assistées par ordinateur dans un projet de qualité des données.  
  
-   **Paramètres de journal**: les fichiers journaux de DQS enregistrent les activités effectuées dans DQS et sont utiles pour le suivi des problèmes opérationnels pendant la maintenance et le dépannage. Vous pouvez filtrer les messages que vous souhaitez consigner dans le journal pour les différentes fonctionnalités de DQS (gestion de domaines, découverte des connaissances, nettoyage, mise en correspondance et services de données de référence) et les différents modules de DQS selon le niveau de gravité des événements.  
  
> [!NOTE]  
>  L'écran **Configuration** est accessible uniquement aux utilisateurs disposant du rôle dqs_administrator sur la base de données DQS_MAIN.  
  
##  <a name="AdminOutsideClient"></a> Activités administratives effectuées en dehors de Data Quality Client  
 Ces opérations sont effectuées en dehors de Data Quality Client :  
  
-   **Sauvegarder et restaurer des bases de données DQS**: les opérations de sauvegarde et de restauration des bases de données DQS sont identiques à celles utilisées pour toute base de données SQL Server, à l'exception de quelques spécificités applicables à DQS.  
  
-   **Détacher et attacher des bases de données DQS**: les étapes permettant d'attacher et de détacher des bases de données DQS sont identiques à celles utilisées pour toute base de données SQL Server, à l'exception de quelques spécificités applicables à DQS.  
  
 Pour plus d’informations, consultez [Manage DQS Databases](../data-quality-services/manage-dqs-databases.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Explique comment analyser les activités de DQS.|[Surveiller les activités DQS](../data-quality-services/monitor-dqs-activities.md)|  
|Explique comment configurer les paramètres des données de référence dans DQS.|[Configurer DQS pour utiliser des données de référence](../data-quality-services/configure-dqs-to-use-reference-data.md)|  
|Explique comment configurer les valeurs de seuil pour les opérations de nettoyage et de mise en correspondance.|[Configurer les valeurs de seuil pour le nettoyage et la correspondance](../data-quality-services/configure-threshold-values-for-cleansing-and-matching.md)|  
|Explique comment activer ou désactiver les notifications dans DQS.|[Activer ou désactiver les notifications de profilage dans DQS](../data-quality-services/enable-or-disable-profiling-notifications-in-dqs.md)|  
|Explique comment configurer la journalisation dans DQS selon le niveau de gravité des événements.|[Configurer les niveaux de gravité pour les fichiers journaux DQS](../data-quality-services/configure-severity-levels-for-dqs-log-files.md)|  
|Explique comment configurer les paramètres de journalisation avancés dans DQS.|[Configurer les paramètres avancés pour les fichiers journaux DQS](../data-quality-services/configure-advanced-settings-for-dqs-log-files.md)|  
|Explique comment sauvegarder et restaurer les bases de données DQS.|[Sauvegarde et restauration de bases de données DQS](../data-quality-services/backing-up-and-restoring-dqs-databases.md)|  
|Décrit comment détacher et attacher des bases de données DQS.|[Attachement et détachement de bases de données DQS](../data-quality-services/detaching-and-attaching-dqs-databases.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Services de données de référence dans DQS](../data-quality-services/reference-data-services-in-dqs.md)   
 [Gérer les fichiers journaux DQS](../data-quality-services/manage-dqs-log-files.md)   
 [Manage DQS Databases](../data-quality-services/manage-dqs-databases.md)  
  
  
