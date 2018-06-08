---
title: Réduire le bruit dans les stratégies d’utilisation du processeur (utilitaire SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.SWB.UE.ReduceNoise.F1
ms.assetid: 94bf4d93-c0ff-4869-bde7-80c24866092e
caps.latest.revision: 7
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: c83600be3b93928cdcdcac07b56308306287e659
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reduce-noise-in-cpu-utilization-policies-sql-server-utility"></a>Réduire le bruit dans les stratégies d'utilisation du processeur (Utilitaire SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez les stratégies suivantes pour limiter le bruit lors de la création de rapports, ainsi que les violations indésirables, dans les stratégies d'utilisation des ressources de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="how-frequently-should-processor-utilization-be-in-violation-before-it-is-reported-as-overutilized"></a>À quelle fréquence l'utilisation du processeur doit-elle se trouver en état de violation pour être signalée comme surexploitée ?  
 La période d'évaluation et le pourcentage de tolérance aux violations sont tous deux configurables à l'aide des paramètres de l'onglet **Stratégie** dans le nœud **Administration de l'utilitaire** de l'Explorateur de l'utilitaire. Pour modifier les stratégies, utilisez les contrôles Slider à droite des descriptions de stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   L'intervalle de collecte des données est de 15 minutes. Cette valeur n'est pas configurable.  
  
-   Le seuil supérieur par défaut de la stratégie d'utilisation du processeur est de 70 %. Les options sont comprises entre 0 % et 100 %.  
  
-   La période d'évaluation par défaut pour la surexploitation du processeur est d'1 heure. Les options sont comprises entre 1 heure et 1 semaine.  
  
-   Le pourcentage par défaut des points de données en violation avant que le processeur soit signalé comme surexploité est de 20 %. Les options sont comprises entre 0 % et 100 %.  
  
 Par exemple, 4 points de données seront collectés par heure, selon les valeurs par défaut, et le seuil de la stratégie est de 20 %. Donc par défaut, toute violation dans une période de collecte d'1 heure correspondra à 25 % des 4 points de données. Les valeurs par défaut signalent toute violation du seuil de la stratégie de surexploitation de l'UC.  
  
 Pour réduire le bruit généré par une violation unique, envisagez les options suivantes :  
  
-   Augmentez la période d'évaluation d'1 incrément de 6 heures. Une violation unique en 6 heures correspondrait à 1 point de données dans une taille de l'échantillon de 24. Dans ce cas, la stratégie tolère 4 violations du seuil de stratégie (16,7 % des points de données) en 6 heures, mais signale une surexploitation à partir de 5 violations (>20 % des points de données) dans une période de collecte de 6 heures.  
  
-   Augmentez le pourcentage de tolérance aux violations d'1 incrément de 30 %. Une violation unique en 1 heure correspondrait à 1 point de données dans une taille de l'échantillon de 4. Dans ce cas, la stratégie tolère 1 violation par heure, mais signale une surexploitation à partir de 2 violations (>30 % des points de données) dans une période de collecte d'1 heure.  
  
-   Augmentez les seuils de stratégie pour l'utilisation du processeur de l'instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de l'application de couche Données. Pour plus d’informations sur la modification des stratégies globales d’utilisation du processeur pour les instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou les applications de couche Données, consultez [Administration de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/3e5a00c3-8905-40f0-9ddc-d924df9c2f0d). Pour plus d’informations sur la modification des stratégies d’utilisation du processeur pour des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Détails de l’instance gérée &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/6e51b7bb-a733-4852-8c33-7f4dbdf931c2). Pour plus d’informations sur la modification des stratégies d’utilisation du processeur pour des applications de la couche Données spécifiques, consultez [Détails des applications de la couche Données déployées &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/79c41dd9-abcb-434e-9326-00a341d5c867).  
  
## <a name="how-frequently-should-processor-utilization-be-in-violation-before-it-is-reported-as-underutilized"></a>À quelle fréquence l'utilisation du processeur doit-elle se trouver en état de violation pour être signalée comme sous-exploitée ?  
 La période d'évaluation et le pourcentage de tolérance aux violations sont tous deux configurables à l'aide des paramètres de l'onglet **Stratégie** dans le nœud **Administration de l'utilitaire** de l'Explorateur de l'utilitaire. Pour modifier les stratégies, utilisez les contrôles Slider à droite des descriptions de stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   L'intervalle de collecte des données est de 15 minutes. Cette valeur n'est pas configurable.  
  
-   Le seuil inférieur par défaut de la stratégie d'utilisation du processeur est de 0 %. Les options sont comprises entre 0 % et 100 %.  
  
-   La période d'évaluation par défaut pour la sous-exploitation du processeur est d'1 semaine. Les options sont comprises entre 1 jour et 1 mois.  
  
-   Le pourcentage par défaut des points de données en violation avant que le processeur soit signalé comme sous-exploité est de 90 %. Les options sont comprises entre 0 % et 100 %.  
  
 Selon les valeurs par défaut, 672 points de données sont collectés par semaine, mais le seuil de stratégie est de 0 %. Donc par défaut, cette stratégie ne génère pas de violations de sous-exploitation du processeur. Pour plus d’informations sur la modification des stratégies globales d’utilisation du processeur pour les instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou les applications de couche Données, consultez [Administration de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/3e5a00c3-8905-40f0-9ddc-d924df9c2f0d). Pour plus d’informations sur la modification des stratégies d’utilisation du processeur pour des instances de SQL Server, consultez [Détails de l’instance gérée &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/6e51b7bb-a733-4852-8c33-7f4dbdf931c2). Pour plus d’informations sur la modification des stratégies d’utilisation du processeur pour des applications de la couche Données spécifiques, consultez [Détails des applications de la couche Données déployées &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/79c41dd9-abcb-434e-9326-00a341d5c867).  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/3e5a00c3-8905-40f0-9ddc-d924df9c2f0d)   
 [Surveiller des instances de SQL Server dans l'utilitaire SQL Server](../../relational-databases/manage/monitor-instances-of-sql-server-in-the-sql-server-utility.md)   
 [Modifier une définition de la stratégie de contrôle d’intégrité des ressources &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/modify-a-resource-health-policy-definition-sql-server-utility.md)   
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)  
  
  
