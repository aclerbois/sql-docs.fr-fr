---
title: Profilage des données et notifications dans DQS | Microsoft Docs
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
ms.assetid: a778bb5b-8e35-4a7b-b04a-ae2b46dec21b
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: df1f8fd9391cf5c59522107f641b1d2d87672d04
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-profiling-and-notifications-in-dqs"></a>Profilage des données et notifications dans DQS

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Le profilage des données dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) est le processus d'analyse des données dans une source de données existante et d'affichage des statistiques sur les données dans des activités DQS. Il vous fournit des mesures automatisées de la qualité des données. Le profilage DQS est intégré à la gestion des connaissances DQS et aux projets de qualité des données. Il est dynamique et réglable. Le profilage a deux objectifs importants : d'abord vous guider au cours des processus de qualité des données et prendre en charge les décisions et, ensuite, évaluer l'efficacité des processus. Le processus de profilage DQS présente les avantages suivants :  
  
-   Le profilage fournit un aperçu de la qualité des données sources et vous aide à identifier des problèmes de qualité des données.  
  
-   Le profilage évalue l'efficacité des processus de qualité des données et vous guide dans la découverte des connaissances, le nettoyage des données, la stratégie de correspondance et le travail de correspondance.  
  
-   Le profilage vous présente les informations les plus pertinentes au moment le plus approprié.  
  
-   Le processus de profilage génère des notifications qui soulignent les statistiques ou les événements significatifs qui peuvent justifier une action. Dans de nombreux cas, les notifications DQS indiquent une condition et recommandent la mesure que vous pouvez prendre pour remédier à cette condition.  
  
 Le profilage vous permet d'utiliser Data Quality Services non seulement pour la découverte des connaissances, le nettoyage et la correspondance, mais aussi en tant qu'outil d'analyse. Vous pouvez créer une base de connaissances pour l'analyse et exécutez la découverte des connaissances à l'aide de cette base de connaissances pour déterminer à partir des statistiques de profilage si la base de connaissances répond à vos besoins de découverte, nettoyage et correspondance.  
  
##  <a name="How"></a> Fonctionnement du profilage  
 Le profilage ne mesure pas la qualité de la base de connaissances. Il mesure la qualité des données sources. Le profilage vous fournit des statistiques qui indiquent l'effet de l'opération spécifique que vous effectuez dans la gestion des connaissances ou un projet de qualité des données sur vos données sources. Le profilage est toujours dans le contexte de l'activité spécifique que vous effectuez. Vous pouvez cliquer sur l'onglet de profilage dans un écran pour afficher des données de profilage sans quitter l'étape de l'activité que vous effectuez. La table de profilage est remplie en temps réel lorsque le processus est exécuté, ce qui vous permet d'évaluer les tâches de qualité des données pendant que vous les effectuez. Vous pouvez déterminer si les données sources sont meilleures après le nettoyage ou la déduplication, et dans quelle mesure.  
  
 Tous les numéros de profilage font référence au nombre d'apparitions d'une valeur et, dans de nombreux cas, au pourcentage du total, à l'exception des mesures d'unicité. Les mesures d'unicité font référence au nombre absolu de valeurs, quel que soit le nombre d'apparitions de ces valeurs.  
  
 Le profilage fait partie de la solution pilotée par la connaissance DQS. Il fournit des informations sur une base de connaissances, la correspondance ou le processus de nettoyage de données basé sur le mappage entre les champs de sources de données et les domaines de base de connaissances. Le profilage est exécuté uniquement lorsque le mappage est terminé ; aucun profilage n'est effectué pendant l'étape de mappage de toute activité. Le profilage est toujours joint à une activité. Le processus de profilage est effectué sur les données qui sont mappées aux domaines, et non sur les données dans les domaines. Le profilage est intégré aux étapes suivantes des activités :  
  
-   Étapes **Découvrir** et **Gérer les valeurs du domaine** de l'activité de découverte des connaissances  
  
-   Étapes **Nettoyer** et **Gérer et afficher les résultats** de l'activité de nettoyage  
  
-   Étapes **Stratégie de correspondance** et **Résultats de correspondance** de l'activité de stratégie de correspondance  
  
-   Étapes **Correspondance** et **Exporter** de l'activité de correspondance  
  
 DQS ne fournit pas de statistiques de profilage pour l'activité de gestion de l'arborescence du domaine.  
  
##  <a name="Activity"></a> Profilage des données par activité  
 Le profilage DQS utilise des dimensions standard de qualité des données pour représenter la qualité des données : exhaustivité (dans quelle mesure les données sont présentes), précision (dans quelle mesure les données peuvent être utilisées pour l'usage prévu) et unicité (dans quelle mesure différentes valeurs représentent différentes entités). Par défaut, les valeurs Null et vides sont considérées comme manquantes, ou diminuent le pourcentage d'exhaustivité ; toutefois, vous pouvez également définir d'autres valeurs comme équivalentes aux valeurs Null, auquel cas elles sont également considérées comme manquantes.  
  
 Le profilage vous fournit les statistiques nécessaires pour évaluer vos processus, mais vous devez interpréter les statistiques. Saisissez la signification de ce que le profilage indique en examinant les statistiques colonne par colonne.  
  
 Les activités DQS ont différents ensembles de statistiques de profilage, comme suit :  
  
-   Seule l'activité de nettoyage a des statistiques de profilage pour la précision (en pourcentage par domaine). La précision est affectée par la validité, la cohérence, les erreurs de syntaxe et les règles de domaine.  
  
-   Seule l'activité de nettoyage a des statistiques de profilage pour les termes corrects, corrigés et suggérés dans la source, et les valeurs corrigées et suggérées par domaine (à la fois nombre et pourcentage).  
  
-   Les activités de nettoyage et de découverte des connaissances ont des statistiques de profilage pour la validité (nettoyage par enregistrement, découverte des connaissances par enregistrement et domaine). Les activités de stratégie de correspondance et de correspondance n'ont pas de statistiques pour la validité.  
  
-   L'activité de nettoyage n'a pas de statistiques de profilage pour l'unicité. Les activités de découverte des connaissances, de stratégie de correspondance et de correspondance ont des statistiques de profilage pour l'unicité en nombre et pourcentage pour la source et par domaine.  
  
 Pour plus d'informations sur les statistiques de profilage spécifiques liées à une activité, consultez les sections de profilage dans les rubriques suivantes :  
  
-   [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md)  
  
-   [Nettoyer des données à l’aide de la base de connaissances DQS &#40;interne&#41;](../data-quality-services/cleanse-data-using-dqs-internal-knowledge.md)  
  
-   [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md)  
  
-   [Exécuter un projet de correspondance](../data-quality-services/run-a-matching-project.md)  
  
##  <a name="Monitoring"></a> Profilage des données dans l'analyse des activités  
 Les informations de profilage pour les activités de découverte des connaissances, de stratégie de correspondance, de correspondance et de nettoyage sont disponibles non seulement dans les pages des activités dans Data Quality Client, mais également dans l'analyse des activités. L'analyse des activités vous fournit une vue d'ensemble des activités en cours et passées. Outre les propriétés et les processus de calcul connexes des activités, vous pouvez afficher les informations de profilage générées pour chaque activité à un emplacement. Vous sélectionnez une activité dans la table des activités pour afficher les résultats de profilage dans une table ci-dessous. Vous pouvez également exporter les résultats de profilage. Pour plus d’informations, consultez [DQS Administration](../data-quality-services/dqs-administration.md).  
  
##  <a name="Notifications"></a> Notifications  
 En plus de collecter et d'afficher des statistiques et des mesures importantes par le profilage, DQS génère des notifications (si l'option est activée) pour indiquer lorsque vous pouvez prendre une mesure en fonction des statistiques de profilage affichées. DQS utilise des notifications pour mettre en évidence des faits importants sur la source de données et afficher l'efficacité de l'activité actuelle par rapport au rôle pour lequel elle a été exécutée. Les notifications fournissent des conseils et des recommandations qui indiquent une condition et comment vous pouvez améliorer une activité de découverte des connaissances, nettoyage des données ou correspondance de données.  
  
 Une notification DQS est utilisée pour soulever une question qui peut vous intéresser, ou pour résoudre un problème potentiel. Vous agissez à la suite de la notification si elle est appropriée à vos besoins. Par exemple, supposons que DQS publie une notification lorsque le nettoyage de données ne produit aucune valeur corrigée ni suggérée alors que l'exhaustivité et la précision sont toutes deux de 100 %. Cette notification indique que l'exécution de l'activité peut ne pas être nécessaire. Toutefois, il vous appartient de choisir d'exécuter l'activité.  
  
 Une notification est indiquée par une info-bulle avec un point d'exclamation dans l'onglet **Profilage** . Les statistiques associées à la notification sont de couleur rouge pour indiquer la justification statistique pour la notification.  
  
 Vous pouvez activer (par défaut) ou désactiver les notifications dans l'onglet **Paramètres généraux** de la section **Administration** de la page d'accueil de Data Quality Client. Lorsque la notification est désactivée, les info-bulles ne sont pas affichées et les statistiques ne sont pas de couleur rouge. Les performances ne sont pas améliorées de façon significative en désactivant les notifications. Le profilage est toujours opérationnel si vous désactivez les notifications.  
  
 Pour connaître des conditions spécifiques associées aux notifications pour une activité, consultez ce qui suit :  
  
-   [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md)  
  
-   [Nettoyer des données à l’aide de la base de connaissances DQS &#40;interne&#41;](../data-quality-services/cleanse-data-using-dqs-internal-knowledge.md)  
  
-   [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md)  
  
-   [Exécuter un projet de correspondance](../data-quality-services/run-a-matching-project.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Explique comment activer ou désactiver les notifications dans DQS.|[Activer ou désactiver les notifications de profilage dans DQS](../data-quality-services/enable-or-disable-profiling-notifications-in-dqs.md)|  
  
  
