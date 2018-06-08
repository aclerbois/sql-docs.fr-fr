---
title: Définir l’étendue de synchronisation (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6f4a11e6-6151-47be-a43f-e3dbf6c0e737
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 00b6638849391c64686afd9f9c070bffeb39e539
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-synchronization-scope-report-builder-and-ssrs"></a>Définir l'étendue de synchronisation (Générateur de rapports et SSRS)
  Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , les indicateurs transmettent les valeurs de données en assurant une synchronisation dans la plage des valeurs d’indicateurs comprises dans une étendue spécifiée.   
    
  Par défaut, l'étendue est le conteneur parent de l'indicateur, tel que la table ou matrice qui contient l'indicateur. Vous pouvez modifier la synchronisation de l'indicateur selon la disposition de votre rapport. Par exemple, si une région de données, comme une table, a un groupe de lignes, vous pouvez spécifier ce groupe comme étendue de l'indicateur. L'indicateur peut également omettre la synchronisation.  
  
 Les options telles que les unités de mesure peuvent être définies à l'aide d'expressions. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
 Pour obtenir des informations générales sur le fonctionnement et la définition de l’étendue dans les rapports, consultez [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="to-change-the-synchronization-scope-of-an-indicator"></a>Pour modifier l'étendue de synchronisation d'un indicateur  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Dans la liste **Étendue de synchronisation** , cliquez sur l’étendue à appliquer.  
  
     L’option **(Aucune)** , qui supprime l’étendue de synchronisation de l’indicateur, est toujours disponible. D'autres options dépendent de la disposition de votre rapport.  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit l’étendue.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
