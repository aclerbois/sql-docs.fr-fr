---
title: Utilisation des indicateurs de performance clés dans Reporting Services | Microsoft Docs
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: a28cf500-6d47-4268-a248-04837e7a09eb
caps.latest.revision: 13
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 74c0cd02499ab10f2aeded7cc528c7db244cd2cb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-kpis-in-reporting-services"></a>Utilisation des indicateurs de performance clés dans Reporting Services

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)])

Un indicateur de performance clé (KPI) est un indice visuel qui représente la progression vers un objectif.  Les Indicateurs de performance clés sont utiles pour les équipes, les responsables et les entreprises pour évaluer rapidement l’état d’avancement par rapport à des objectifs mesurables.   
  
À l’aide des indicateurs de performance clés dans SQL Server 2016 Reporting Services, vous pouvez facilement visualiser les réponses aux questions suivantes :  
  
-   Suis-je en avance ou en retard ?  
  
-   Quelle est mon avance ou quel est mon retard ?  
  
-   Quelle est la quantité minimum de travail que j’ai terminé ?  
  
## <a name="creating-a-dataset"></a>Création d’un dataset  
Un indicateur de performance clé utilise uniquement la première ligne de données issue d’un dataset partagé. Assurez-vous que les données que vous souhaitez utiliser se trouvent sur cette première ligne. Pour créer un dataset partagé, vous pouvez utiliser le Générateur de rapports ou SQL Server Data Tools.  
  
> **Remarque**: le dataset ne doit pas nécessairement être dans le même dossier que l’indicateur de performance clé.  
  
## <a name="placement-of-kpis"></a>Positionnement des indicateurs de performance clés  
  
Les indicateurs de performance clés peuvent être créés dans n’importe quel dossier sur votre serveur de rapports.  Avant de créer un indicateur de performance clé, vous devez réfléchir au meilleur emplacement pour celui-ci. Il est préférable de le placer dans un dossier visible pour les utilisateurs, qui soit en même temps utile à d’autres rapports et indicateurs de performance clés.  
  
## <a name="adding-a-kpi"></a>Ajout d’un indicateur de performance clé  
  
Après avoir déterminé l’emplacement de votre indicateur de performance clé, accédez à ce dossier et sélectionnez **Nouveau** > **Indicateur de performance clé** dans le menu supérieur.  
  
![rsCreateKPI1](../reporting-services/media/rscreatekpi1.png)  
  
L’écran **Nouvel indicateur de performance clé** s’affiche.  
  
![rsCreateKPI2](../reporting-services/media/rscreatekpi2.png)  
  
Vous pouvez assigner des valeurs statiques ou utiliser les données issues d’un dataset partagé. Lorsque vous créez un indicateur de performance clé, il est rempli avec une série aléatoire de données manuelles.  
  
|Champ|Description|  
|---|---|  
|Format de la valeur|  Permet de modifier le format de la valeur affichée.|   
|Valeur|La valeur à afficher pour l’indicateur de performance clé.|  
|Objectif|Utilisé en comparaison à une valeur numérique et présenté sous forme de différence de pourcentage.|  
|État|Valeur numérique utilisée pour déterminer la couleur de vignette de l’indicateur de performance clé. Les valeurs valides sont 1 (vert), 0 (orange) et -1 (rouge).|  
|Ensemble de tendances|Valeurs numériques séparées par des virgules utilisées pour la visualisation de graphique. Ceci peut également être défini sur une colonne d’un dataset avec des valeurs qui représentent la tendance.|  
  
> **Avertissement**: bien que vous pouvez utiliser la valeur texte pour le champ **État** au moment de la conception, vous devez utiliser la valeur numérique en cas d’actualisation d’un dataset. Si vous actualisez un dataset avec la valeur texte, au lieu de la valeur numérique, cela peut endommager les indicateurs de performance clés sur votre serveur.  
  
> **Remarque**: les champs **Valeur**, **Objectif** et **État** peuvent uniquement choisir une valeur à partir de la première ligne du résultat d’un dataset. Toutefois, le champ **Ensemble de tendances** peut choisir la colonne qui reflète la tendance.  
  
Procédez comme suit pour utiliser les données d’un dataset partagé.  
  
1.  Modifiez la liste déroulante des champs de **Manuellement**ou **Ne pas définir**à **Champ de dataset**.  
  
    ![rsCreateKPI3](../reporting-services/media/rscreatekpi3.png)  
  
2.  Sélectionnez les **points de suspension (...)** dans la zone de données. L’écran **Choisir un dataset** s’affiche.  
  
    ![rsCreateKPI4](../reporting-services/media/rscreatekpi4.png)  
  
3.  Sélectionnez le dataset qui contient les données que vous souhaitez afficher.  
  
4.  Choisissez le champ que vous souhaitez utiliser. Sélectionnez **OK**.  
  
    ![rsCreateKPI5](../reporting-services/media/rscreatekpi5.png)  
  
5.  Modifiez **Format de la valeur** pour correspondre au format de la valeur. Dans cet exemple, la valeur est une devise.  
  
    ![rsCreateKPI6](../reporting-services/media/rscreatekpi6.png)  
  
6.  Sélectionnez **Appliquer**.  
  
    ![rsCreateKPI7](../reporting-services/media/rscreatekpi7.png)  
  
## <a name="removing-a-kpi"></a>Suppression d’un indicateur de performance clé  
  
Pour supprimer un indicateur de performance clé, vous pouvez effectuer les opérations suivantes.  
  
1.  Sélectionnez les **points de suspension (...)** de l’indicateur de performance clé à supprimer. Sélectionnez **Gérer**.  
  
    ![rsRemoveKPI1](../reporting-services/media/rsremovekpi1.png)  
  
2.  Sélectionnez **Supprimer**. Sélectionnez **Supprimer** à nouveau dans la boîte de dialogue de confirmation.  
  
    ![rsRemoveKPI2](../reporting-services/media/rsremovekpi2.png)  
  
## <a name="refreshing-a-kpi"></a>Actualisation d’un indicateur de performance clé  
  
Pour actualiser l’indicateur de performance clé, vous devez configurer une mise en cache pour le dataset partagé. Pour plus d’informations sur les plans d’actualisation du cache, consultez [Utiliser des datasets partagés](../reporting-services/work-with-shared-datasets-web-portal.md).  
  
## <a name="next-steps"></a>Étapes suivantes
  
[Portail Web](../reporting-services/web-portal-ssrs-native-mode.md)  
[Utiliser des datasets partagés](../reporting-services/work-with-shared-datasets-web-portal.md)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
