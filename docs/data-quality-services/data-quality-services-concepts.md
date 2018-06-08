---
title: Concepts Data Quality Services | Microsoft Docs
ms.custom: ''
ms.date: 01/01/2012
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 837c71ee-48fa-4044-8744-2be9119aaa04
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0268596f0cbba206ed81dfb4f3af3a450c6f9c7f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-quality-services-concepts"></a>Concepts Data Quality Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique fournit un bref résumé des concepts [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) relatifs à la gestion des connaissances, aux projets de qualité des données et à l'administration de la qualité des données.  
  
##  <a name="Knowledge"></a> Concepts relatifs à la gestion des connaissances  
 La base de connaissances DQS est un référentiel de métadonnées créé par le gestionnaire de données ou un professionnel de l'informatique pour améliorer la qualité des données via des opérations de nettoyage et de mise en correspondance. La gestion des connaissances DQS inclut les processus utilisés pour créer et gérer la base de connaissances, de manière assistée par ordinateur et de manière interactive.  
  
 **Découverte des connaissances**  
  
 La découverte des connaissances est un processus assisté par ordinateur qui analyse des exemples de données tirés de votre organisation pour générer des connaissances. Lorsque vous obtenez les résultats de l'analyse, vous pouvez valider et améliorer les connaissances, puis les appliquer pour nettoyer, mettre en correspondance et profiler les données. Pour plus d’informations, consultez [DQS Knowledge Bases and Domains](../data-quality-services/dqs-knowledge-bases-and-domains.md).  
  
 **Gestion de l'arborescence du domaine**  
  
 Le processus de gestion de domaine vous permet de modifier ou augmenter les connaissances générées par le processus de découverte des connaissances. Vous pouvez modifier, mettre à jour et examiner de manière interactive les connaissances dans une base de connaissances. Une base de connaissances comprend des champs de données qui contiennent des valeurs de domaine et leur état, leurs règles de domaine, leurs relations à base de termes et leurs données de référence. Dans la gestion d'un domaine, vous pouvez modifier les propriétés du domaine, lui joindre des données de référence, gérer ses règles, gérer ses valeurs, écrire des relations de données et créer, supprimer, importer, ou exporter des domaines. Vous pouvez également utiliser des domaines composés qui agrègent plusieurs domaines. Pour plus d’informations, consultez [DQS Knowledge Bases and Domains](../data-quality-services/dqs-knowledge-bases-and-domains.md).  
  
 **Stratégie de correspondance**  
  
 Une stratégie de correspondance contient les règles de correspondance utilisées pour effectuer une déduplication de données. Le processus de stratégie de correspondance vous permet de créer des règles de correspondance, de les adapter précisément en fonction des résultats de la correspondance et du profilage des données, et d'ajouter la stratégie à la base de connaissances. Pour plus d’informations, consultez [Correspondance de données](../data-quality-services/data-matching.md).  
  
 **Reference Data Services**  
  
 Vous pouvez utiliser des données de référence pour valider, corriger et enrichir vos données, en exploitant les services de sociétés qui garantissent la qualité de leurs données de référence. Vous pouvez utiliser les services de la Place de marché Microsoft Azure pour vous connecter à des fournisseurs de données de référence, ou vous pouvez prendre directement contact avec un fournisseur. Pour plus d’informations, consultez [Reference Data Services in DQS](../data-quality-services/reference-data-services-in-dqs.md).  
  
 Pour plus d'informations sur la gestion des connaissances dans DQS, consultez [DQS Knowledge Bases and Domains](../data-quality-services/dqs-knowledge-bases-and-domains.md).  
  
##  <a name="Projects"></a> Concepts relatifs au projets de qualité des données  
 Le gestionnaire de données effectue des opérations de qualité des données (nettoyage et correspondance) à l'aide d'un projet de qualité des données dans l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] .  
  
 **Nettoyage de données**  
  
 Le nettoyage de données dans DQS s'effectue sur la base des connaissances dans une base de connaissances DQS. Le nettoyage de données dans DQS est un processus en deux étapes :  
  
-   **Nettoyage assisté par ordinateur**: DQS utilise les connaissances d'une base de connaissances sélectionnée pour le projet de nettoyage afin de proposer des corrections/suggestions aux valeurs d'une source de données.  
  
-   **Nettoyage interactif**: le gestionnaire de données ou un professionnel de l'informatique peut exécuter le processus de nettoyage interactif pour modifier ou augmenter les corrections aux données proposées par le processus de nettoyage de données assisté par ordinateur. Il utilise pour cela les niveaux de confiance et les statistiques identifiés par le processus de nettoyage de données, ou écrit manuellement ses propres modifications dans le projet.  
  
 Après le nettoyage de données, le gestionnaire peut exporter les données traitées vers une base de données SQL Server, un fichier .csv ou un fichier Excel. Pour plus d’informations, consultez [Data Cleansing](../data-quality-services/data-cleansing.md).  
  
 **Correspondance de données**  
  
 Le processus de correspondance permet au gestionnaire de données de comparer des données afin que celles similaires, mais légèrement différentes, puissent être alignées par un processus de déduplication. DQS effectue la déduplication en fonction des règles de correspondance contenues dans la base de connaissances ; le gestionnaire de données spécifie les paramètres du processus de correspondance dans un projet de qualité des données. Pour plus d’informations, consultez [Correspondance de données](../data-quality-services/data-matching.md).  
  
 **Profilage et notifications**  
  
 Le profilage des données fournit aux gestionnaires de données des statistiques et des informations en temps réel sur les données traitées par DQS dans le cadre des activité de nettoyage ou de correspondance pendant l'exécution d'un projet de qualité de données. Le profilage des données vous aide à évaluer l'efficacité des activités de nettoyage et de correspondance dans un projet de qualité des données, et les notifications suggèrent des mesures pouvant être prises pour améliorer ces activités. Pour plus d’informations, consultez [Data Profiling and Notifications in DQS](../data-quality-services/data-profiling-and-notifications-in-dqs.md).  
  
 Pour plus d’informations sur les projets de qualité des données dans DQS, consultez [Projets de qualité des données &#40;DQS&#41;](../data-quality-services/data-quality-projects-dqs.md).  
  
##  <a name="Admin"></a> Concepts relatifs à l'administration de la qualité des données  
 Un administrateur DQS peut effectuer diverses tâches d'administration à l'aide de l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] .  
  
 **Analyse des activités**  
  
 L'analyse des activités affiche l'état de chaque activité comprise dans une plage de données, fournit des données pour chaque activité et permet aux administrateurs DQS de contrôler une activité. Pour plus d’informations, consultez [Monitor DQS Activities](../data-quality-services/monitor-dqs-activities.md).  
  
 **Configuration**  
  
 L'option de configuration vous permet de :  
  
-   Configurer les paramètres de service des données de référence. Pour plus d’informations, consultez [Configure DQS to Use Reference Data](../data-quality-services/configure-dqs-to-use-reference-data.md).  
  
-   Définir les valeurs de seuil pour les activités de nettoyage et de correspondance. Pour plus d’informations, consultez [Configurer les valeurs de seuil pour le nettoyage et la correspondance](../data-quality-services/configure-threshold-values-for-cleansing-and-matching.md).  
  
-   Activer/désactiver les notifications de profilage Pour plus d’informations, consultez [Activer ou désactiver les notifications de profilage dans DQS](../data-quality-services/enable-or-disable-profiling-notifications-in-dqs.md).  
  
-   Configurer les niveaux de gravité des journaux DQS au niveau de l'activité ou au niveau du module (configuration plus avancée). Pour plus d’informations, consultez [Configure Severity Levels for DQS Log Files](../data-quality-services/configure-severity-levels-for-dqs-log-files.md).  
  
 **Sécurité relative au magasin d'objets blob distants (DQS)**  
  
 Vous pouvez utiliser des rôles dans le mécanisme de sécurité SQL Server pour sécuriser DQS. Il existe trois rôles DQS qui déterminent le niveau d'accès d'un utilisateur à l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] : dqs_administrator, dqs_kb_editor, et dqs_kb_operator. Vous ne pouvez pas accorder les rôles aux utilisateurs à l'aide de l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] ; cela s'effectue à l'aide de SQL Server Management Studio. Pour plus d’informations, consultez [DQS Security](../data-quality-services/dqs-security.md).  
  
 Pour plus d'informations sur l'administration DQS, consultez [DQS Administration](../data-quality-services/dqs-administration.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Data Quality Services](../data-quality-services/data-quality-services.md)  
  
  
