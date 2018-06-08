---
title: Mettre en cache les datasets partagés (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4acb1bbe-1c04-4979-b893-dc1b1c5039b6
caps.latest.revision: 6
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1190043f72b862e83a8ef39d6a9f0b1940c896ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cache-shared-datasets-ssrs"></a>Mettre en cache les datasets partagés (SSRS)
  Les résultats de la requête pour un dataset partagé peuvent être copiés vers un cache afin de fournir des données cohérentes pour plusieurs rapports et améliorer le temps de réponse pour la requête de dataset. Comme pour les rapports, vous pouvez configurer un dataset partagé à mettre en cache lors de la première utilisation ou en spécifiant une planification.  
  
 Un dataset partagé peut être inclus dans plusieurs rapports ou dans le cadre de définitions de composant. En mettant en cache le dataset partagé, vous fournissez un jeu cohérent de données pour tous les rapports qui l'utilisent, et vous réduisez également le nombre d'exécution de la requête de dataset par rapport à la source de données externe.  
  
 La liste suivante fournit des exemples de situations dans lesquelles il convient de mettre en cache un dataset partagé :  
  
-   La requête met longtemps à s'exécuter.  
  
-   La requête accepte des paramètres, mais la plupart du temps, le nombre de combinaisons de paramètres est réduit. Chaque combinaison crée des résultats de requête mis en cache.  
  
-   La requête s'exécute à des heures prédictibles du jour, de la semaine ou du mois.  
  
-   La requête s'exécute comme le résultat d'une référence de dataset partagé dans un rapport remis via la messagerie électronique, où un grand nombre de personnes sont susceptibles de cliquer sur le lien dans une courte plage horaire.  
  
 La liste suivante fournit des exemples de situations dans lesquelles il ne convient pas de mettre en cache un dataset partagé :  
  
-   Les résultats de la requête doivent toujours inclure les données les plus récentes.  
  
-   La requête s'exécute rapidement.  
  
-   La requête s'exécute peu souvent.  
  
-   La requête accepte des paramètres, le nombre de combinaisons de paramètres est élevé, et aucune combinaison n'est plus probable qu'une autre.  
  
-   La source de données sur laquelle est basé le dataset partagé a une Invite ou des informations d'identification intégrées de Windows.  
  
-   Le filtre de dataset partagé ou la requête contient une expression avec une référence à la collection globale Utilisateur.  
  
 Si un utilisateur choisit des valeurs de paramètre de rapport qui diffèrent des valeurs par défaut spécifiées pour le jeu de résultats mis en cache, la requête de dataset s'exécute activement et les résultats mis en cache ne sont pas utilisés pour cette requête.  
  
## <a name="caching-shared-datasets"></a>Mise en cache des datasets partagés  
 Pour activer la mise en cache pour un dataset partagé, vous devez sélectionner l'option de cache sur le dataset partagé. Une fois la mise en cache activée, les résultats de la requête pour un dataset partagé sont copiés vers le cache lors de la première utilisation. Si le dataset partagé a des paramètres, chaque combinaison de paramètres crée une entrée dans le cache.  
  
 Pendant que les résultats de la requête pour une combinaison de paramètres spécifique sont dans le cache, chaque rapport lancé pour le traitement et qui inclut une référence au dataset partagé avec ces valeurs de paramètre utilisera les données en mémoire cache.  
  
 Vous pouvez spécifier la durée pendant laquelle conserver les données dans le cache avant leur expiration. Pour plus d’informations, consultez [Page Mise en cache, datasets partagés &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/eac372e9-d2a1-48a8-bbe5-09d101df16ea).  
  
## <a name="preloading-the-cache"></a>Préchargement du cache  
 Vous pouvez précharger le cache en créant un plan d'actualisation du cache. Avec un plan d'actualisation, vous pouvez spécifier la fréquence d'actualisation du cache à l'aide d'une planification spécifique par élément ou d'une planification partagée. Pour éviter qu'il y ait plusieurs entrées du cache pour le même élément, la planification que vous spécifiez doit permettre suffisamment de temps pour le traitement des requêtes sur la source de données externe. Par exemple, si la requête prend 20 minutes pour s'exécuter, la planification d'actualisation doit être supérieure à 20 minutes. Pour plus d'informations, consultez [Schedules](../../reporting-services/subscriptions/schedules.md).  
  
 Pour créer un plan d'actualisation du cache pour un dataset partagé, les conditions suivantes s'appliquent.  
  
-   Le dataset partagé doit être activé pour la mise en cache.  
  
-   La source de données partagée dont dépend le dataset partagé ne peut pas utiliser une Invite ou des informations d'identification intégrées de Windows.  
  
-   Si le dataset partagé accepte des paramètres, vous devez spécifier des valeurs statiques par défaut pour chaque paramètre qui n'est pas marqué en lecture seule. Les paramètres en lecture seule utiliseront toujours la valeur par défaut. Pour mettre en cache un dataset partagé pour plusieurs combinaisons de paramètres, vous devez créer un plan d'actualisation du cache distinct pour chaque combinaison de valeurs. Les paramètres ne peuvent pas contenir de références à d'autres datasets.  
  
-   Chaque plan d'actualisation du cache est associé à un seul dataset partagé ou rapport.  
  
-   Vous devez avoir des autorisations ReadPolicy et UpdatePolicy sur le dataset partagé.  
  
 Les plans d'actualisation du cache s'appliquent aux datasets partagés et aux rapports. Pour plus d’informations, consultez [Options d’actualisation du cache &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/227da40c-6bd2-48ec-aa9c-50ce6c1ca3a6).  
  
## <a name="conditions-that-cause-cache-expiration"></a>Conditions entraînant l'expiration du cache  
 Les conditions suivantes peuvent provoquer le fait qu'un cache de dataset partagé devienne non valide.  
  
-   Une condition de planification expire. Le cache dépasse le délai d'attente imparti ou atteint l'heure d'expiration.  
  
-   Une planification partagée est supprimée.  
  
-   Modifications apportées à une planification partagée. Les planifications partagées peuvent être suspendues, ce qui affecte également l'expiration d'un cache.  
  
-   La définition de la requête pour le dataset partagé change.  
  
-   Les informations d'identification pour la source de données partagée dont dépend le dataset partagé changent.  
  
-   Les options de cache pour le dataset partagé changent.  
  
-   Les valeurs par défaut pour les paramètres en lecture seule pour le dataset partagé changent.  
  
-   Les filtres qui font partie de la définition du dataset partagé changent.  
  
-   Le dataset partagé est supprimé du serveur de rapports. Lorsqu'un dataset partagé est supprimé, les copies mises en cache associées et les plans d'actualisation du cache sont également supprimés.  
  
 Les mises à jour apportées aux plans d'actualisation du cache pour les datasets partagés n'affectent pas les rapports qui sont déjà traités. La mise à jour d'un plan d'actualisation du cache affecte uniquement les futurs lancements de rapports qui font référence au dataset partagé.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer des datasets partagés](../../reporting-services/report-data/manage-shared-datasets.md)  
  
  
