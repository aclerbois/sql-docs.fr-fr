---
title: Surveiller les composants SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e8f1b16b-ea40-4e12-886c-967ebda4e6e4
caps.latest.revision: 8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 5ded81f3d0010a1b34cd4f5e858fd535a54abbfb
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="monitor-sql-server-components"></a>Surveiller les composants SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La surveillance est importante car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit un service dans un environnement dynamique. Les données dans l'application sont fluctuantes. Le type d'accès requis par les utilisateurs peut changer. Le mode de connexion des utilisateurs change. Les types des applications accédant à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent même changer, mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère automatiquement les ressources de niveau système, telles que la mémoire et l’espace disque, pour minimiser les paramétrages manuels nécessaires au niveau système. La surveillance permet aux administrateurs d'identifier les tendances de performances afin de déterminer si des modifications s'imposent.  
  
 Pour surveiller efficacement un composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
1.  Déterminer vos objectifs en matière de surveillance.  
  
2.  Sélectionner l'outil approprié.  
  
3.  Identifier les composants à surveiller.  
  
4.  Sélectionner les éléments de mesure pour ces composants.  
  
5.  Surveiller le serveur.  
  
6.  Analyser les données.  
  
 Chacune de ces étapes est décrite ci-après.  
  
## <a name="determine-your-monitoring-goals"></a>Déterminer vos objectifs en matière de surveillance  
 Pour surveiller efficacement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez identifier clairement les motifs de surveillance. Ces motifs peuvent être les suivants :  
  
-   Établir un niveau de référence des performances.  
  
-   Identifier les fluctuations de performances dans le temps.  
  
-   Diagnostiquer des problèmes de performances spécifiques.  
  
-   Identifier les composants ou processus à optimiser.  
  
-   Comparer les effets de différentes applications clientes sur les performances.  
  
-   Auditer l'activité des utilisateurs.  
  
-   Tester un serveur sous différentes charges.  
  
-   Tester l'architecture d'une base de données.  
  
-   Tester les programmes de maintenance.  
  
-   Tester les plans de sauvegarde et de restauration.  
  
-   Déterminer le moment où il convient de modifier votre configuration matérielle.  
  
## <a name="select-the-appropriate-tool"></a>Sélectionner l'outil approprié  
 Une fois que vous avez identifié les motifs de la surveillance, vous devez sélectionner les outils correspondant à ce type de surveillance. Le système d'exploitation Windows et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportent un jeu complet d'outils permettant de surveiller les serveurs dans des environnements riches en transactions. Ces outils révèlent clairement la condition d'une instance du moteur de base de données SQL Server ou d'une instance de SQL Server Analysis Services.  
  
 Windows fournit les outils suivants pour la surveillance d'applications s'exécutant sur un serveur :  
  
-   Moniteur système, qui permet de collecter et d'afficher des données en temps réel sur des activités, telles que l'utilisation de la mémoire, du disque et du processeur ;  
  
-   Journaux et alertes de performance ;  
  
-   Gestionnaire des tâches  
  
 Pour plus d'informations sur les outils Windows ou Windows Server, consultez la documentation Windows.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit les outils suivants pour la surveillance des composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Trace SQL  
  
-   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]  
  
-   Distributed Replay Utility  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Moniteur d'activité  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Plan d'exécution de requêtes graphique  
  
-   Procédures stockées  
  
-   Commandes DBCC (Database Console Commands)  
  
-   Fonctions intégrées  
  
-   Indicateurs de trace  
  
 Pour plus d’informations sur les outils de surveillance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Outils d’analyse et de paramétrage des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md).  
  
## <a name="identify-the-components-to-monitor"></a>Identifier les composants à surveiller  
 La troisième étape dans la surveillance d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consiste à identifier les composants à surveiller. Par exemple, si vous utilisez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour tracer un serveur, vous définir la trace de sorte à collecter des données concernant des événements spécifiques. Vous pouvez également exclure des événements qui ne s'appliquent pas à votre situation.  
  
## <a name="select-metrics-for-monitored-components"></a>Sélectionner les éléments de mesure pour les composants surveillés  
 Après l'identification des composants à surveiller, déterminez les éléments de mesure à utiliser pour la surveillance. Par exemple, après avoir sélectionné les événements à inclure dans une trace, vous pouvez choisir d'inclure uniquement des données spécifiques concernant ces événements. La limitation de la trace aux données pertinentes permet de réduire la quantité de ressources système requise pour effectuer le suivi.  
  
## <a name="monitor-the-server"></a>Surveiller le serveur  
 Pour surveiller le serveur, exécutez l'outil de surveillance que vous avez configuré pour collecter des données. Par exemple, après avoir défini une trace, vous pouvez l’exécuter pour recueillir des données concernant les événements qui se sont produits sur le serveur.  
  
## <a name="analyze-the-data"></a>Analyser les données  
 Une fois le suivi terminé, analysez les données pour vérifier si vous avez atteint votre objectif de surveillance. Si ce n’est pas le cas, modifiez les composants ou les éléments de mesure utilisés pour surveiller le serveur.  
  
 Le processus de capture de données d’événement et de leur exploitation est décrit ci-dessous.  
  
1.  Appliquer des filtres pour limiter les données d'événement recueillies.  
  
     Le fait de limiter les données d'événement permet de s'attacher uniquement aux événements pertinents par rapport au scénario de surveillance en place. Par exemple, si vous souhaitez surveiller les requêtes lentes, vous pouvez utiliser un filtre afin de ne vous intéresser qu’aux requêtes dont l'exécution par l'application sur une base de données particulière prend plus de 30 secondes. Pour plus d’informations, consultez [Définir un filtre de trace &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/set-a-trace-filter-transact-sql.md) et [Filtrer des événements dans une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md).  
  
2.  Surveiller (capturer) les événements.  
  
     Dès qu'elle est activée, la surveillance active capture des données à partir de l'application, de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou du système d'exploitation spécifié. Par exemple, lorsque l'activité du disque est analysée à l'aide du Moniteur système, ce dernier capture les données d'événement, notamment les lectures et les écritures sur le disque et les affiche sur l'écran. Pour plus d’informations, consultez [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md).  
  
3.  Enregistrer les données d'événement capturées.  
  
     L'enregistrement des données d'événement capturées vous permet de les analyser ultérieurement, voire de les relire avec Distributed Replay Utility ou le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Les données d'événement capturées sont enregistrées dans un fichier pouvant être rechargé dans l'outil qui l'a créé à l'origine pour analyse. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] permet d’enregistrer les données d’événement dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'enregistrement des données d'événement capturées est essentiel lors de la création d'un niveau de référence des performances. Les données du niveau de référence des performances sont enregistrées et utilisées lors de la comparaison des données d'événement récemment capturées afin de déterminer si les performances sont optimales. Pour plus d’informations, consultez [Modèles et autorisations du générateur de SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md).  
  
4.  Créer des modèles de trace contenant les paramètres spécifiés pour capturer les événements.  
  
     Ces modèles contiennent des spécifications concernant les événements proprement dits, les données d’événement et les filtres utilisés pour la capture de données. Ils permettent de surveiller ultérieurement un ensemble spécifique d'événements sans avoir à redéfinir les événements, les données d'événement et les filtres. Par exemple, si vous voulez souvent surveiller le nombre d'interblocages et les utilisateurs impliqués dans ces interblocages, vous pouvez créer un fichier définissant ces événements, données d'événement et filtres d'événement, puis enregistrer le modèle et appliquer le filtre la prochaine fois que vous voudrez surveiller les interblocages. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] utilise les modèles de trace à cet effet. Pour plus d’informations, consultez [Définir les valeurs par défaut des définitions de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md) et [Créer un modèle de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md).  
  
5.  Analyser les données d'événement capturées.  
  
     Les données d'événement capturées sont chargées dans l'application qui les a capturées afin d’être analysées. Par exemple, une trace capturée à partir de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] peut être rechargée dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] à des fins de consultation et d’analyse. Pour plus d’informations, consultez [Afficher et analyser des traces avec SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md).  
  
     L'analyse des données d'événement implique l'identification des événements et de leur cause. Ces informations vous permettent d'effectuer des modifications susceptibles d'améliorer les performances, telles que l'ajout de mémoire, la modification d'index, la correction de problèmes de code avec des procédures stockées et des instructions Transact-SQL, en fonction du type d'analyse effectuée. Par exemple, vous pouvez utiliser l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour analyser une trace capturée depuis [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] et créer des recommandations d'index en fonction des résultats.  
  
6.  Relire les données d'événement capturées.  
  
     La relecture d'événements permet d'établir une copie de test de l'environnement de base de données à partir duquel les données ont été capturées, puis de répéter les événements capturés tels qu'ils se sont initialement produits sur le système réel. Cette fonction n'est disponible qu'avec Distributed Replay Utility ou le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Ces événements peuvent être relus à la vitesse à laquelle ils se sont produits, aussi rapidement que possible (pour contraindre le système) ou, plus vraisemblablement, pas à pas (pour analyser le système après chaque événement). L'analyse des événements exacts dans un environnement de test empêche tout effet nuisible sur le système de production. Pour plus d’informations, consultez [Relire des traces](../../tools/sql-server-profiler/replay-traces.md).  
  
  
