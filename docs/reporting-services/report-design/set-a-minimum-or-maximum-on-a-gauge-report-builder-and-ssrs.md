---
title: Définir un minimum ou un maximum sur une jauge (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: b4c260c0-5a88-4f30-8977-eb5cc78fc146
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: b329bd864f471a1dae95c21798ceb9dd88963b6b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-a-minimum-or-maximum-on-a-gauge-report-builder-and-ssrs"></a>Définir un minimum ou un maximum sur une jauge (Générateur de rapports et SSRS)
  Contrairement aux graphiques dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , où plusieurs groupes sont définis, les jauges affichent une seule valeur. Étant donné que le Générateur de rapports et le Concepteur de rapports déterminent le contexte ou la précision relative de la valeur que vous essayez d’afficher sur la jauge, vous devez définir les valeurs minimale et maximale de l’échelle.   
    
  Par exemple, si vos valeurs de données sont comprises entre 0 et 10, vous pouvez définir le minimum sur 0 et le maximum sur 10. Les intervalles sont calculés automatiquement en fonction des valeurs spécifiées pour le minimum et le maximum. Par défaut, le minimum et le maximum sont définis sur 0 et 100, mais il s'agit d'une valeur arbitraire que vous devez modifier. L'application ne calcule pas votre valeur sous la forme d'un pourcentage.  
  
 Si votre plage de valeurs est large, par exemple de 0 à 10 000, envisagez d'utiliser un multiplicateur pour réduire le nombre de zéros sur la jauge. Le multiplicateur réduira uniquement l'échelle des nombres sur la jauge et pas la valeur proprement dite.  
  
 Vous pouvez utiliser des expressions pour définir les valeurs des options **Minimum** et **Maximum** . Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
## <a name="to-set-the-minimum-and-maximum-on-the-gauge"></a>Pour définir le minimum et le maximum sur la jauge  
  
1.  Cliquez avec le bouton droit sur l’échelle et sélectionnez **Propriétés de l’échelle**. La boîte de dialogue **Propriétés de l’échelle** s’affiche.  
  
2.  Dans **Général**, spécifiez une valeur pour **Minimum**. Par défaut, cette valeur est 0. Vous pouvez également cliquer sur le bouton **Expression** (*fx*) si vous souhaitez modifier l’expression qui définit la valeur de l’option.  
  
3.  Spécifiez une valeur pour **Maximum**. Par défaut, il s'agit de 100. Vous pouvez également cliquer sur le bouton **Expression** (*fx*) si vous souhaitez modifier l’expression qui définit la valeur de l’option.  
  
4.  (Facultatif) Si les valeurs minimale et maximale sont élevées, spécifiez une valeur pour l’option **Multiplier les étiquettes d’échelle par** . Pour spécifier un multiplicateur qui réduit votre échelle, utilisez un nombre décimal. Par exemple, si vous utilisez une échelle de 0 à 1 000, vous pouvez spécifier une valeur de multiplicateur de 0,01 pour réduire l'échelle afin d'afficher 0 à 10.  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme des échelles sur une jauge &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-scales-on-a-gauge-report-builder-and-ssrs.md)   
 [Mise en forme des pointeurs sur une jauge &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-pointers-on-a-gauge-report-builder-and-ssrs.md)   
 [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
  
