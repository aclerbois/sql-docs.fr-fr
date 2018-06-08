---
title: Documentation sur Analytics Platform System | Microsoft Docs
description: 'Microsoft Analytics Platform System (APS) est une plateforme de données conçue pour l’entreposage des données et l’analytique de Big Data. Cette plateforme offre les avantages suivants : intégration étroite des données, traitement très rapide des requêtes, stockage hautement extensible et maintenance simple pour vos solutions décisionnelles de bout en bout.'
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 26c59dc36d0ba3714dca4dbca10e8ec7b09b9235
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="microsoft-analytics-platform-system"></a>Microsoft Analytics Platform System  
Microsoft Analytics Platform System (APS) est une plateforme de données conçue pour l’entreposage des données et l’analytique de Big Data. Cette plateforme offre les avantages suivants : intégration étroite des données, traitement très rapide des requêtes, stockage hautement extensible et maintenance simple pour vos solutions décisionnelles de bout en bout.  
  
![Architecture de l’appliance](media/architecture-high-level.png "architecture de l’appliance")  
  
Analytics Platform System héberge SQL Server Parallel Data Warehouse (PDW), le logiciel qui exécute l’entrepôt de données MPP (Massively Parallel Processing).  
  
La technologie PolyBase combine des données PDW relationnelles à des données Hadoop provenant de différentes sources, comme Hortonworks sur Windows Server, Hortonworks sur Linux, Cloudera sur Linux, le stockage d’objets blob Microsoft Azure de HDInsight et HDInsight sur Analytics Platform System. Ces capacités d’intégration de données avancées, alliées à une intégration étroite d’outils de Business Intelligence, permettent à Analytics Platform System de retourner des données d’analyse intégrées qui aident les décideurs de votre entreprise à prendre des décisions avisées.  
  
Analytics Platform System est fourni pour votre centre de données sous forme d’appliance, avec le matériel et les logiciels préinstallés et préconfigurés de manière appropriée pour exécuter plusieurs charges de travail. Quand vous achetez Analytics Platform System, vous achetez des nœuds de calcul pour PDW et, éventuellement, des nœuds supplémentaires pour HDInsight en fonction des besoins de votre entreprise.  
  
Outre sa rapidité et son extensibilité, Analytics Platform System offre une redondance élevée et une haute disponibilité, ce qui en fait une plateforme fiable sur laquelle vous pouvez entreposer vos données métier les plus critiques. Analytics Platform System a été conçu dans un souci de simplicité pour une prise en main et une gestion faciles. La technologie PolyBase de PDW pour l’analyse des données Hadoop et l’intégration étroite d’outils de Business Intelligence en font une plateforme complète pour la création de solutions de bout en bout.  
  
  
## <a name="parallel-data-warehouse-software-designed-for-massively-parallel-processing"></a>Parallel Data Warehouse, un logiciel conçu pour le traitement MPP
  
Utilisez PDW comme principal composant d’entreposage des données relationnelles pour vos solutions décisionnelles de bout en bout. Grâce à la conception MPP (Massively Parallel Processing) de PDW, les requêtes s’exécutent en moyenne 50 fois plus vite que les entrepôts de données standard reposant sur des systèmes de gestion de base de données SMP (Symmetric Multi-Processing).  
  
> [!NOTE]  
> Avec cette vitesse d’exécution 50 fois plus rapide, les requêtes s’effectuent en quelques minutes au lieu de plusieurs heures, ou en quelques secondes au lieu de plusieurs minutes. Ce haut niveau de performance permet aux analystes de votre entreprise de fournir des résultats plus complets et plus rapides, et d’effectuer facilement des requêtes ad hoc ou des analyses plus détaillées des données. Votre entreprise peut alors prendre de meilleures décisions, plus rapidement.  
  
En plus d’améliorer les performances d’exécution des requêtes, PDW facilite les opérations suivantes :  
  
-   Augmentez la capacité de votre entrepôt de données de quelques téraoctets jusqu’à plus de six pétaoctets de données dans une seule appliance en ajoutant des « unités d’échelle » à votre système existant.  
  
-   Garantissez la disponibilité permanente de vos données grâce à la redondance élevée et à la haute disponibilité intégrées.  
  
-   Trouvez une solution aux nouveaux enjeux que constituent le chargement et la consolidation des données.  
  
-   Intégrez des données Hadoop avec des données relationnelles pour accélérer les analyses à l’aide de la technologie PolyBase de traitement hautement parallèle (MPP) de PDW.  
  
-   Utilisez des outils de Business Intelligence pour créer des solutions de bout en bout complètes.  

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les avantages de PDW, consultez le livre blanc [A Breakthrough Platform for Next-Generation Data Warehousing and Big Data Solutions](http://msdn.microsoft.com/library/dn520808.aspx) sur MSDN.  
  

