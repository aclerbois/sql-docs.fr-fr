---
title: Mise en cache proactive (Partitions) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4959473b120a3a8a0c289ff3cd8f91e89df44b86
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="partitions---proactive-caching"></a>Partitions - mise en cache Proactive
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La mise en cache proactive fournit la création et la gestion de la mise en cache MOLAP automatique pour les objets OLAP. Les cubes incorporent immédiatement les modifications apportées aux données dans la base de données, en fonction des notifications reçues de cette dernière. L'objectif de la mise en cache proactive est de fournir la performance MOLAP traditionnelle tout en conservant la rapidité et la facilité de gestion offertes par ROLAP.  
  
 Un objet <xref:Microsoft.AnalysisServices.ProactiveCaching> simple est composé d'une spécification temporelle et d'une notification de table. La spécification temporelle définit le délai de mise à jour du cache après qu'une notification de modification a été reçue. La notification de table définit le schéma de notification entre la table de données et l'objet <xref:Microsoft.AnalysisServices.ProactiveCaching>.  
  
 Le mode de stockage MOLAP (Multidimensional OLAP) assure le meilleur temps de réponse aux requêtes, mais au prix d'une certaine latence des données. Le mode de stockage ROLAP (Relational OLAP) en temps réel permet aux utilisateurs de parcourir immédiatement les modifications les plus récentes apportées à une source de données, mais au prix de performances nettement plus faibles que celles du mode de stockage MOLAP à cause de l'absence de synthèses précalculées des données et parce que le stockage relationnel n'est pas optimisé pour les requêtes de style OLAP. Si vous disposez d'applications dans lesquelles les utilisateurs doivent voir les données récentes et que vous souhaitez aussi bénéficier des avantages de performances du stockage MOLAP, SQL Server Analysis Services offre différentes options de mise en cache proactive pour répondre à ce besoin, plus particulièrement associées à l'utilisation des partitions. La mise en cache proactive est définie par partition et par dimension. Les options de mise en cache proactive offrent un compromis entre les performances améliorées du stockage MOLAP et le stockage ROLAP en temps réel, et permettent le traitement automatique des partitions lors de la modification des données sous-jacentes ou dans le cadre d'une planification.  
  
## <a name="proactive-caching-configuration-options"></a>Options de configuration de la mise en cache proactive  
 SQL Server Analysis Services offre plusieurs options de configuration de la mise en cache proactive qui vous permettent de maximiser les performances, de minimiser la latence et de planifier le traitement. Les fonctionnalités de mise en cache proactive simplifient le processus de gestion de l'obsolescence des données. Les paramètres de mise en cache proactive définissent la fréquence de la reconstruction de la structure OLAP multidimensionnelle, également appelée cache OLAP, et déterminent si le stockage MOLAP obsolète est interrogé pendant la reconstruction du cache ou de la source de données ROLAP sous-jacente. Ils précisent également si le cache est reconstruit suivant une planification ou en fonction des modifications survenues dans la base de données.  
  
### <a name="minimizing-latency"></a>Minimisation de la latence  
 Lorsque la mise en cache proactive est configurée pour minimiser la latence, les requêtes portant sur un objet OLAP sont soumises au stockage ROLAP ou MOLAP selon que des modifications de données ont eu lieu récemment ou non et selon la configuration de la mise en cache proactive. Le moteur d'interrogation dirige les requêtes vers les données sources du stockage MOLAP jusqu'à ce que des changements surviennent dans la source de données. Pour minimiser la latence, lorsqu'une source de données est modifiée, les objets MOLAP stockés dans le cache sont supprimés et les requêtes sont dirigées vers le stockage ROLAP pendant que les objets MOLAP sont reconstruits dans le cache. Une fois les objets MOLAP reconstruits et traités, les requêtes sont redirigées automatiquement vers le stockage MOLAP. L'actualisation du cache peut être très rapide pour une partition de petite taille, telle que la partition actuelle, qui peut être aussi réduite que celle du jour en cours.  
  
### <a name="maximizing-performance"></a>Maximisation des performances  
 Pour maximiser les performances tout en minimisant la latence, la mise en cache peut également être utilisée sans supprimer les objets MOLAP en cours. Les requêtes continuent alors de porter sur les objets MOLAP pendant que les données sont écrites et traitées dans un nouveau cache. Cette méthode offre de meilleures performances, mais il est possible que des requêtes retournent des données anciennes pendant la construction du nouveau cache.  
  
## <a name="see-also"></a>Voir aussi  
 [Stockage de dimension](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-storage.md)   
 [Définir le stockage des partitions & #40 ; Analysis Services - multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/set-partition-storage-analysis-services-multidimensional.md)  
  
  
