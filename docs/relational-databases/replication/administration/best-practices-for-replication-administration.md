---
title: Bonnes pratiques en matière d’administration de la réplication| Microsoft Docs
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
- administering replication, best practices
- replication [SQL Server], administering
ms.assetid: 850e8a87-b34c-4934-afb5-a1104f118ba8
caps.latest.revision: 17
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 00683ffe3901a02f2996f638ed7ec231acf3f98d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="best-practices-for-replication-administration"></a>Bonnes pratiques en matière d’administration de la réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Après avoir configuré la réplication, il est important de comprendre en quoi consiste l'administration d'une topologie de réplication. Cette rubrique fournit des indications de base sur les bonnes pratiques dans un certain nombre de domaines, avec des liens sur chaque domaine pour plus d'informations. En plus de suivre les indications suivantes sur les bonnes pratiques présentées dans cette rubrique, il est conseillé de lire la rubrique du forum aux questions afin de vous familiariser avec les questions et les problèmes les plus courants : [Questions fréquentes (FAQ) pour les administrateurs de réplication](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md).  
  
 Les indications sur les bonnes pratiques sont scindées en deux domaines :  
  
-   Les informations suivantes couvrent les méthodes conseillées devant être implémentées pour toutes les topologies de réplication :  
  
    -   Développer et tester une stratégie de sauvegarde et de restauration.  
  
    -   Créer un script de la topologie de réplication.  
  
    -   Créer des seuils et des alertes.  
  
    -   Analyser la topologie de réplication.  
  
    -   Établir des lignes de base de performance et ajuster au besoin la réplication.  
  
-   Les informations suivantes couvrent les méthodes conseillées devant être envisagées, mais n'étant pas nécessairement requises pour votre topologie :  
  
    -   Valider périodiquement les données.  
  
    -   Ajuster les paramètres de l'Agent par le biais des profils.  
  
    -   Ajuster les périodes de rétention de publication et de distribution.  
  
    -   Comprendre comment modifier les propriétés des articles et de la publication si les conditions requises par l'application sont modifiées.  
  
    -   Comprendre comment effectuer des modifications de schémas si les conditions requises par l'application sont modifiées.  
  
## <a name="develop-and-test-a-backup-and-restore-strategy"></a>Développer et tester une stratégie de sauvegarde et de restauration  
 Toutes les bases de données doivent être sauvegardées régulièrement, et la possibilité de restaurer ces sauvegardes doit être testé périodiquement ; il en est de même pour les bases de données répliquées. Les bases de données suivantes doivent être sauvegardées régulièrement :  
  
-   Base de données de publication  
  
-   Base de données de distribution  
  
-   Bases de données d'abonnement  
  
-   Les bases de données**msdb** et **master** sur le serveur de publication, sur le serveur de distribution et sur tous les abonnés  
  
 Les bases de données répliquées nécessitent une attention toute particulière en ce qui concerne la sauvegarde et la restauration de données. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données répliquées](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).  
  
## <a name="script-the-replication-topology"></a>Créer un script de la topologie de réplication  
 Tous les composants de réplication dans une topologie doivent faire l'objet d'un script et s'intégrer dans un plan de récupération des données en cas de sinistre ; les scripts peuvent également être utilisés pour automatiser des tâches répétitives. Un script contient les procédures stockées système [!INCLUDE[tsql](../../../includes/tsql-md.md)] nécessaires à l'implémentation du ou des composants faisant l'objet d'un script, comme une publication ou un abonnement. Il est possible de créer des scripts à l'aide d'un Assistant (comme l'Assistant Nouvelle publication) ou dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] après avoir créé un composant. Vous pouvez afficher, modifier et exécuter le script à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou **sqlcmd**. Les scripts peuvent être stockés avec les fichiers de sauvegarde dans le cas où la topologie de réplication doit être reconfigurée. Pour plus d’informations, voir [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
 Le script doit être exécuté à nouveau sur un composant si des modifications de propriétés sont apportées. Si vous utilisez des procédures stockées personnalisées avec la réplication transactionnelle, une copie de chaque procédure doit être stockée avec les scripts ; la copie doit être mise à jour si la procédure change (les procédures sont généralement mises à jour suite à des modifications de schéma ou à des modifications des conditions requises par l'application). Pour plus d’informations sur les procédures personnalisées, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
## <a name="establish-performance-baselines-and-tune-replication-if-necessary"></a>Établir des lignes de base de performance et ajuster au besoin la réplication  
 Avant de configurer la réplication, il est recommandé de vous familiariser avec les facteurs affectant les performances de la réplication :  
  
-   Serveur et matériel réseau  
  
-   Conception de la base de données  
  
-   Configuration du serveur de distribution  
  
-   Conception et options de la publication  
  
-   Conception et utilisation du filtrage  
  
-   Options d'abonnement  
  
-   Options d'instantané  
  
-   Paramètres de l'Agent  
  
-   Maintenance  
  
 Une fois la réplication configurée, il est recommandé de développer une ligne de base de performance, ce qui vous permettra de déterminer comment se comporte la réplication avec une charge de travail standard pour vos applications et votre topologie. Utilisez le Moniteur de réplication et le Moniteur système pour déterminer les nombres standard pour les cinq dimensions de performance de réplication suivantes :  
  
-   Latence : temps nécessaire à la propagation d'une modification de données entre les nœuds d'une topologie de réplication.  
  
-   Débit : volume des activités de réplication (mesuré en termes de commandes transmises sur une période donnée) que le système est en mesure d'assurer dans le temps.  
  
-   Concurrence : nombre de processus de réplication qui peuvent être exécutés simultanément sur un système.  
  
-   Durée de synchronisation : temps nécessaire à l'exécution d'une synchronisation donnée.  
  
-   Consommation de ressources : ressources matérielles et réseau utilisées par le traitement de la réplication.  
  
 La latence et le débit sont les deux facteurs les plus importants pour la réplication transactionnelle dans la mesure où les systèmes fondés sur ce type de réplication exigent généralement une latence faible et un débit élevé. La simultanéité et la durée de synchronisation sont plus appropriées à la réplication de fusion, car les systèmes basés sur la réplication de fusion comportent souvent de nombreux abonnés, et un serveur de publication peut avoir un nombre de synchronisations simultanées significatif avec ces abonnés.  
  
 Une fois que vous avez établi les nombres de la ligne de base, définissez les seuils dans le Moniteur de réplication. Pour plus d’informations, consultez [Définir des seuils et des avertissements dans le moniteur de réplication](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md) et [Utiliser les alertes pour les événements des agents de réplication](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md). Si vous rencontrez un problème de performance, il est recommandé de parcourir les suggestions dans les rubriques d'amélioration des performances répertoriées ci-dessus et d'appliquer les modifications dans les zones affectant les problèmes que vous rencontrez.  
  
## <a name="create-thresholds-and-alerts"></a>Créer des seuils et des alertes  
 Le Moniteur de réplication vous permet de définir un certain nombre de seuils liés à l'état et aux performances. Il est recommandé de définir les seuils appropriés pour votre topologie. Si un seuil est atteint, un avertissement s'affiche et une alerte peut éventuellement être envoyée sur un compte de messagerie, une radiomessagerie ou un autre périphérique. Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
 En plus des alertes pouvant être liées à des seuils d'analyse, la réplication fournit plusieurs alertes prédéfinies qui répondent aux actions de l'Agent de réplication. Ces alertes permettent de tenir un administrateur informé de l'état de la topologie de réplication. Il est recommandé de parcourir la rubrique décrivant les alertes et d'utiliser celles qui correspondent à vos besoins d'administration (vous avez également la possibilité de créer des alertes supplémentaires si nécessaire). Pour plus d’informations, consultez [Utiliser les alertes pour les événements des agents de réplication](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md).  
  
## <a name="monitor-the-replication-topology"></a>Analyser la topologie de réplication  
 Une fois la topologie de réplication en place et les seuils et alertes configurées, il est recommandé d'analyser régulièrement la réplication. L'analyse d'une topologie de réplication est un aspect important du déploiement de la réplication. L'activité de la réplication étant distribuée, il est essentiel de faire le suivi de cette activité et des états à travers tous les ordinateurs impliqués dans la réplication. Les outils suivants peuvent être utilisés pour surveiller la réplication :  
  
-   Le Moniteur de réplication est l'outil le plus important d'analyse de réplication, il vous permet d'analyser la santé globale d'une topologie de réplication. Pour plus d’informations, voir [Monitoring Replication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md).  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)] et les Replication Management Objects fournissent des interfaces pour l'analyse de réplication. Pour plus d’informations, voir [Monitoring Replication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md).  
  
-   Le Moniteur système peut également s'avérer très utile pour l'analyse des performances de la réplication. Pour plus d’informations, voir [Monitoring Replication with System Monitor](../../../relational-databases/replication/monitor/monitoring-replication-with-system-monitor.md).  
  
## <a name="validate-data-periodically"></a>Valider périodiquement les données  
 La validation n'est pas requise par la réplication, elle est néanmoins recommandée pour valider périodiquement la réplication transactionnelle et la réplication de fusion. La validation vous permet de vérifier que les données sur l'Abonné correspondent à celles du serveur de publication. Une validation réussie indique qu'à un moment donné, tous les modifications provenant du serveur de publication ont été répliquées sur l'Abonné (et de l'Abonné sur le serveur de publication si les mises à jour sont prises en charge sur l'Abonné) et que les deux bases de données sont synchronisées.  
  
 Il est recommandé d'effectuer la validation en fonction de la planification de sauvegarde de la base de données de publication. Par exemple, si la base de données de publication est sauvegardée intégralement une fois par semaine, la validation peut être exécutée une fois par semaine quand la sauvegarde est terminée. Pour plus d’informations, consultez [Valider des données répliquées](../../../relational-databases/replication/validate-replicated-data.md).  
  
## <a name="use-agent-profiles-to-change-agent-parameters-if-necessary"></a>Utiliser les profils de l'Agent pour modifier les paramètres de l'Agent si nécessaire  
 Les profils de l'Agent représentent une méthode pratique de configuration des paramètres de l'Agent de réplication. Les paramètres peuvent également être spécifiés sur la ligne de commande de l'Agent, mais il est généralement plus judicieux d'utiliser un profil de l'Agent prédéfini ou de créer un nouveau profil si vous devez modifier la valeur d'un paramètre. Par exemple, si vous utilisez la réplication de fusion et qu'un abonné passe d'une connexion haut débit à une connexion d'accès à distance, envisagez d'utiliser le profil de **liaison lente** pour l'Agent de fusion ; l'ensemble des paramètres de ce profil est mieux adapté à la liaison de communications lentes. Pour plus d'informations, voir [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
## <a name="adjust-publication-and-distribution-retention-periods-if-necessary"></a>Ajuster les périodes de rétention de publication et de distribution si nécessaire  
 La réplication transactionnelle et la réplication de fusion se servent de périodes de rétention pour déterminer, respectivement, la durée de stockage des transactions dans la base de données de distribution, et la fréquence de synchronisation d'un abonnement. Il est recommandé d'utiliser d'abord les paramètres par défaut, mais pour analyser votre topologie, un réglage des paramètres est nécessaire. Par exemple, dans le cas de la réplication de fusion, la période de rétention de la publication (qui est de 14 jours par défaut) détermine la durée de stockage des métadonnées dans les tables système. Si les abonnements synchronisent toujours tous les cinq jours, envisagez d'ajuster le paramètre à un nombre de jours moins élevé ce qui permettra de réduire les métadonnées et d'offrir de meilleures performances. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="understand-how-to-modify-publications-if-application-requirements-change"></a>Comprendre comment modifier les publications si les conditions requises par l'application sont modifiées  
 Une fois que vous avez créé une publication, il sera peut-être nécessaire d'ajouter ou de supprimer des articles, ou de modifier les propriétés de la publication ou des articles. La plupart des modifications sont autorisées après qu'une publication ait été créée, mais dans certains cas, il est nécessaire de générer un nouvel instantané pour une publication et/ou de réinitialiser les abonnements à la publication. Pour plus d’informations, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) et [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
## <a name="understand-how-to-make-schema-changes-if-application-requirements-change"></a>Comprendre comment effectuer des modifications de schémas si les conditions requises par l'application sont modifiées  
 Dans certains cas, les modifications de schémas sont nécessaires une fois qu'une application est en fonctionnement. Dans une topologie de réplication, ces modifications doivent le plus souvent être propagées à tous les abonnés. La réplication prend en charge une grande variété de modifications de schéma pour les objets publiés. Lorsque vous effectuez l'une des modifications de schémas qui suit sur l'objet publié approprié sur un serveur de publication [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cette modification est propagée par défaut sur tous les Abonnés [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   ALTER TABLE  
  
-   ALTER VIEW  
  
-   ALTER PROCEDURE  
  
-   ALTER FUNCTION  
  
-   ALTER TRIGGER  
  
 Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Administration &#40;réplication&#41;](../../../relational-databases/replication/administration/administration-replication.md)  
  
  
