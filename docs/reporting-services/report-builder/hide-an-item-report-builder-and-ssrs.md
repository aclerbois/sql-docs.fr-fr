---
title: Masquer un élément (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.shared.visibility.f1
- "10503"
ms.assetid: 9d78f8de-959b-456f-8947-687fa6e2ba91
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 496efbdb099f6e9611b4b6d9be5a3a303ad82411
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hide-an-item-report-builder-and-ssrs"></a>Masquer un élément (Générateur de rapports et SSRS)
  Définissez la visibilité d'un élément de rapport lorsque vous souhaitez masquer de façon conditionnelle un élément en fonction d'un paramètre de rapport ou d'une autre expression que vous spécifiez.  
  
 Vous pouvez également concevoir un rapport afin de permettre aux utilisateurs d'activer ou de désactiver la visibilité des éléments de rapport en fonction de clics sur des zones de texte dans le rapport, par exemple pour un rapport d'exploration. Pour plus d’informations, consultez [Ajouter une action Développer ou Réduire à un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-expand-or-collapse-action-to-an-item-report-builder-and-ssrs.md).  
  
 Les procédures suivantes décrivent comment afficher ou masquer un élément de rapport dans un rapport rendu en fonction d'une constante ou d'une expression.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-hide-a-report-item"></a>Pour masquer un élément de rapport  
  
1.  En mode création de rapport, cliquez avec le bouton droit sur l’élément de rapport et ouvrez sa page **Propriétés** .  
  
    > [!NOTE]  
    >  Pour sélectionner l’intégralité d’une région de données de table ou de matrice, cliquez dans la région de données pour la sélectionner, cliquez avec le bouton droit sur une ligne, une colonne ou une poignée d’angle, puis cliquez sur **Propriétés du tableau matriciel**.  
  
2.  Cliquez sur **Visibilité**.  
  
3.  Dans **Lors de la première exécution du rapport**, spécifiez s’il faut masquer ou non l’élément quand le rapport est affiché pour la première fois :  
  
    -   Pour afficher l’élément, cliquez sur **Afficher**.  
  
    -   Pour masquer l’élément, cliquez sur **Masquer**.  
  
    -   Pour spécifier une expression qui est évaluée au moment de l’exécution, cliquez sur **Afficher ou masquer en fonction d’une expression**. Tapez l’expression ou cliquez sur le bouton Expression (**fx**) pour créer l’expression dans la boîte de dialogue **Expression** .  
  
        > [!NOTE]  
        >  Quand vous spécifiez une expression de visibilité, vous définissez la propriété Hidden de l’élément de rapport, comment indiqué dans l’image suivante. L'expression évaluée affiche l'élément de rapport lorsque la valeur est False et le masque lorsque la valeur est True.   
        > ![Boîte de dialogue et propriété masquée Properties_Visibility](../../reporting-services/report-builder/media/hiddenproperty-propertiesvisibility.png "Boîte de dialogue et propriété masquée Properties_Visibility")  
  
4.  Cliquez deux fois sur **OK** .  
  
### <a name="to-hide-static-rows-in-a-table-matrix-or-list"></a>Pour masquer des lignes statiques dans une table, une matrice ou une liste  
  
1.  En mode création de rapport, cliquez sur la table, la matrice ou la liste afin d'afficher les handles de ligne et de colonne.  
  
2.  Cliquez avec le bouton droit sur le handle de ligne, puis cliquez sur **Visibilité de ligne**. La boîte de dialogue **Visibilité de ligne** s’ouvre.  
  
3.  Pour définir la visibilité, effectuez les étapes 3 et 4 de la première procédure.  
  
### <a name="to-hide-static-columns-in-a-table-matrix-or-list"></a>Pour masquer des colonnes statiques dans une table, une matrice ou une liste  
  
1.  En mode Conception, sélectionnez la table, la matrice ou la liste afin d'afficher les handles de ligne et de colonne.  
  
2.  Cliquez avec le bouton droit sur le handle de colonne, puis cliquez sur **Visibilité de la colonne**.  
  
3.  Dans la boîte de dialogue **Visibilité de la colonne** , effectuez les étapes 3 et 4 de la première procédure.  
  
## <a name="see-also"></a> Voir aussi  
 [Action d’exploration &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drilldown-action-report-builder-and-ssrs.md)   
 [Ajouter une action Développer ou Réduire à un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-expand-or-collapse-action-to-an-item-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
  
  
