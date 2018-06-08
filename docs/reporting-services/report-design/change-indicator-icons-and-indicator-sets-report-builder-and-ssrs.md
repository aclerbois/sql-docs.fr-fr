---
title: Changer les icônes d’indicateur et jeux d’indicateurs (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 8a056adf-4473-473d-9b0c-314675af7bfd
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 132121a2dede7d632c05278d7a1692ca1fba23b2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-indicator-icons-and-indicator-sets-report-builder-and-ssrs"></a>Modifier les icônes d'indicateur et jeux d'indicateurs (Générateur de rapports et SSRS)
  Les jeux d’indicateurs préconfigurés que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit pour les rapports paginés peuvent ne pas toujours représenter efficacement vos données ni fonctionner correctement dans le rapport remis. Cette rubrique fournit des procédures permettant de modifier l'apparence des icônes d'indicateur et les jeux d'indicateurs pour inclure plus ou moins d'icônes d'indicateur, ou d'autres icônes d'indicateur.  
  
 Les options telles que les couleurs peuvent être définies à l'aide d'expressions. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
## <a name="to-change-the-color-of-an-indicator-icon"></a>Pour modifier la couleur d'une icône d'indicateur  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Cliquez sur la flèche vers le bas dans la colonne **Couleur** en regard de l’icône que vous souhaitez changer et cliquez sur la couleur à utiliser, **Aucune couleur**ou **Couleurs supplémentaires**.  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit la valeur de l’option **Couleur** .  
  
     Si vous avez cliqué sur **Couleurs supplémentaires**, la boîte de dialogue **Sélectionner une couleur** s’ouvre, dans laquelle vous pouvez choisir parmi un grand nombre de couleurs. Pour plus d’informations sur ses options, consultez [Boîte de dialogue Sélectionner une couleur &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/ac7089a3-5c7b-4f53-8348-180610e86da2). Cliquez sur **OK** pour fermer la boîte de dialogue **Sélectionner une couleur** .  
  
4.  Cliquez sur **OK**.  
  
## <a name="to-change-the-icon"></a>Pour modifier l'icône  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Cliquez sur la flèche vers le bas en regard de l'icône que vous souhaitez changer et sélectionnez une icône différente.  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit la valeur de l’option **Icône** .  
  
4.  Cliquez sur **OK**.  
  
## <a name="to-use-a-custom-image-as-an-indicator-icon"></a>Pour utiliser une image personnalisée comme icône d'indicateur  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Cliquez sur la flèche vers le bas en regard de l’icône que vous souhaitez changer et sélectionnez **Image**.  
  
4.  Dans la liste **Sélectionner la source de l’image** , cliquez sur **Externe**, **Rapport**ou **Base de données**.  
  
5.  Selon la source de l'image, procédez de l'une des manières suivantes :  
  
    -   Pour utiliser une image stockée hors du rapport, cliquez sur **Parcourir** et recherchez l’image. Le rapport inclura une référence à l'image.  
  
    -   Pour utiliser une image incorporée dans le rapport, cliquez sur **Importer** et recherchez l’image. L'image sera ajoutée à la définition de rapport et enregistrée avec celle-ci.  
  
    -   Pour utiliser une image qui est dans une base de données, dans la liste **Utiliser ce champ** , sélectionnez le champ de la liste, puis dans la liste **Utiliser ce type MIME** , sélectionnez le type MIME de l’image.  
  
6.  Cliquez sur **OK**.  
  
## <a name="to-add-an-icon-to-the-indicator-set"></a>Pour ajouter une icône au jeu d'indicateurs  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Cliquez sur **Ajouter**. Un indicateur est ajouté, à l’aide de l’icône par défaut et de l’option **Aucune couleur** .  
  
     Configurez l'indicateur pour utiliser l'icône et la couleur souhaitées. Les procédures plus haut dans cette rubrique décrivent les étapes de cette opération.  
  
4.  Cliquez sur **OK**.  
  
## <a name="to-delete-an-icon-to-the-indicator-set"></a>Pour supprimer une icône du jeu d'indicateurs  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Sélectionnez l’icône à supprimer, puis cliquez sur **Supprimer**.  
  
4.  Cliquez sur **OK**.  
  
## <a name="see-also"></a> Voir aussi  
 [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
