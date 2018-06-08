---
title: Définir l’orientation d’une zone de texte (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 64bd53f4-2f31-4732-8c2e-64c7b54b6972
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: bf2588b4e5c958a15eefd0c8745489005799a842
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-text-box-orientation-report-builder-and-ssrs"></a>Définir l'orientation d'une zone de texte (Générateur de rapports et SSRS)
Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , vous pouvez faire pivoter une zone de texte dans différentes directions :   
* Horizontalement   
* Verticalement (rotation de 90 degrés, avec le texte lu de haut en bas)  
* Rotation de 270 degrés (texte lu de bas en haut).   
  
Étant donné que la rotation concerne la zone de texte, et non le texte, elle s’applique à l’ensemble du texte dans la zone de texte. Vous ne pouvez pas spécifier de directions différentes pour certaines parties du texte. Dimensionnez manuellement la largeur de colonne et la hauteur de ligne pour les adapter au texte pivoté.  
  
 La propriété WritingMode, que vous utilisez pour spécifier l’orientation de texte, n’est pas disponible dans la boîte de dialogue **Propriétés de la zone de texte** . Elle se trouve dans le volet Propriétés, où vous devez la définir.   
  
## <a name="to-rotate-text"></a>Pour faire pivoter le texte  
  
1.  Créez un rapport ou ouvrez-en un qui existe déjà, puis [ajoutez une zone de texte](../../reporting-services/report-design/add-move-or-delete-a-text-box-report-builder-and-ssrs.md) à l’aire de conception.  
  
3.  Sélectionnez la zone de texte que vous souhaitez faire pivoter.  
  
2.  Si le volet Propriétés n’est pas ouvert, sous l’onglet **Affichage** , cochez la case **Propriétés** .  
  
4.  Dans le volet Propriétés, recherchez la propriété WritingMode et sélectionnez l’orientation de texte à appliquer à la zone de texte.  
  
    > [!NOTE]  
    >  Quand les propriétés du volet Propriétés sont organisées en catégories, WritingMode figure dans la catégorie **Localisation** .  
  
5.  Dans la zone de liste, sélectionnez **Horizontal**, **Vertical**ou **Rotate270**.  
  
## <a name="see-also"></a> Voir aussi  
 [Zones de texte &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/text-boxes-report-builder-and-ssrs.md)   
 [Didacticiel : mettre en forme du texte &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-format-text-report-builder.md)  
  
  
