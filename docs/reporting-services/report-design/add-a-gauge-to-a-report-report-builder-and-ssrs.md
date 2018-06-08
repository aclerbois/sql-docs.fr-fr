---
title: Ajouter une jauge à un rapport (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 45da4fef-2b02-40e1-977c-f8f80d87155e
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 5f9c8467cb8ea31ef490cd21ff51e675abe1fd17
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-gauge-to-a-report-report-builder-and-ssrs"></a>Ajouter une jauge à un rapport (Générateur de rapports et SSRS)
  Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , quand vous souhaitez synthétiser des données dans un format visuel, vous pouvez utiliser une région de données de type jauge. Après avoir ajouté une région de données de jauge à l’aire de conception, vous pouvez faire glisser les champs de dataset du rapport vers un volet des données sur la jauge.  
  
## <a name="to-add-a-gauge-to-your-report"></a>Pour ajouter une jauge à votre rapport  
  
1.  Créez un rapport ou ouvrez-en un qui existe déjà.  
  
2.  Sous l'onglet Insérer, double-cliquez sur Jauge. La boîte de dialogue **Sélectionner le type de jauge** s'ouvre.  
  
3.  Sélectionnez le type de jauge que vous souhaitez ajouter. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Contrairement aux graphiques, il n’existe que deux types de jauge : linéaire et radial. Les jauges disponibles dans la boîte de dialogue **Sélectionner le type de jauge** sont des modèles correspondant à ces deux types de jauges. C'est ce qui explique que vous ne pouvez pas modifier le type de jauge une fois que vous avez ajouté une jauge à votre rapport. Vous devez supprimer et rajouter une jauge pour en modifier le type.  
  
     Si le rapport ne possède pas de source de données ni de dataset, la boîte de dialogue **Propriétés de la source de données** s'ouvre et vous aide à en créer. Pour plus d’informations, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
     Si le rapport est associé à une source de données mais à aucun dataset, la boîte de dialogue **Propriétés du dataset** s'ouvre et vous aide à en créer un. Pour plus d’informations, consultez [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md).  
  
4.  Cliquez sur la jauge pour afficher le volet des données. Par défaut, une jauge dispose d'un pointeur qui correspond à une valeur. Vous pouvez ajouter des pointeurs supplémentaires.  
  
5.  Ajoutez un champ du dataset à la zone de dépôt de champ de données. Vous pouvez ajouter un champ uniquement. Si vous souhaitez afficher plusieurs champs, vous devez ajouter des pointeurs supplémentaires, un pour chaque champ.  
  
     Cliquez avec le bouton droit sur l’échelle de la jauge et sélectionnez **Propriétés de l’échelle**. Tapez la valeur **Minimum** et **Maximum** de l'échelle. Pour plus d’informations, consultez [Définir un minimum ou un maximum sur une jauge &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/set-a-minimum-or-maximum-on-a-gauge-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md)   
 [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
  
