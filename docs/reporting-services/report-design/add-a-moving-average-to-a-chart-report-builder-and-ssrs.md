---
title: Ajouter une moyenne mobile à un graphique (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 166cf9c1-0750-4866-8381-542e4fbfe65a
caps.latest.revision: 13
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: e9095171bd77895c679e426c90fbc447434c6333
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-moving-average-to-a-chart-report-builder-and-ssrs"></a>Ajouter une moyenne mobile à un graphique (Générateur de rapports et SSRS)
Une moyenne mobile est une moyenne des données de votre série, calculée sur une période de temps définie. Dans des rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , la moyenne mobile peut être indiquée sur le graphique pour identifier des tendances significatives.  

![didacticiel-histogramme-générateur-de-rapports](../../reporting-services/media/report-builder-column-chart-tutorial.png)
  
 La formule de moyenne mobile est l'indicateur de prix le plus populaire utilisé dans les analyses techniques. De nombreuses autres formules, y compris la moyenne, la médiane et l'écart type, peuvent également être dérivées d'une série sur le graphique. Lors de la spécification d'une moyenne mobile, chaque formule peut comporter un ou plusieurs paramètres qui doivent être spécifiés.  
 
 Le [Didacticiel : ajouter un histogramme à un rapport (Générateur de rapports)](Tutorial:%20Add%20a%20Column%20Chart%20to%20Your%20Report%20\(Report%20Builder\).md) vous guide pas à pas dans la procédure d’ajout d’une moyenne mobile à un graphique, si vous voulez essayer avec des exemples de données.
  
 Lorsqu'une formule de moyenne mobile est ajoutée en mode Création, la série de lignes ajoutée est uniquement un espace réservé visuel. Le graphique calculera les points de données de chaque formule pendant le traitement des rapports.  
  
 La prise en charge intégrée des courbes de tendance n’est pas disponible dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-calculated-moving-average-to-a-series-on-the-chart"></a>Pour ajouter une moyenne mobile calculée à une série sur le graphique  
  
1.  Cliquez avec le bouton droit sur un champ dans la zone **Valeurs** , puis cliquez sur **Ajouter une série calculée**. La boîte de dialogue **Propriétés de la série calculée** s'ouvre.  
  
2.  Sélectionnez l’option **Moyenne mobile** dans la liste déroulante **Formule** .  
  
3.  Spécifiez une valeur entière pour la **Période** qui représente la période de la moyenne mobile.  
  
    > [!NOTE]  
    >  La période est le nombre de jours utilisés pour calculer une moyenne mobile. Si les valeurs de date/d'heure ne sont pas spécifiées sur l'axe des abscisses, la période est représentée par le nombre de points de données utilisés pour calculer une moyenne mobile. S'il n'existe qu'un seul point de données, la formule de moyenne mobile n'effectue pas de calcul. La moyenne mobile est calculée en commençant par le deuxième point. Si vous spécifiez l'option **Démarrer à partir du premier point** , le graphique démarrera la moyenne mobile au niveau du premier point. S'il n'existe qu'un seul point de données, le point dans la moyenne mobile calculée sera identique au premier point dans votre série d'origine.  
  
## <a name="see-also"></a> Voir aussi  
* [Didacticiel : ajouter un histogramme à un rapport (Générateur de rapports)](Tutorial:%20Add%20a%20Column%20Chart%20to%20Your%20Report%20\(Report%20Builder\).md)
*  [Mise en forme d’un graphique (Générateur de rapports et SSRS)](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
*  [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
*  [Ajouter des points vides à un graphique (Générateur de rapports et SSRS)](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md)  
  
  
