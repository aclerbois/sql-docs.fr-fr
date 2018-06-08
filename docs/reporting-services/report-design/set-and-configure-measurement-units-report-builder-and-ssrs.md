---
title: Définir et configurer des unités de mesure (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: a15a96c3-7d2c-433e-a440-4ea051e967a9
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 86607db4b618c0d56ab7684f9d3223721b306858
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-and-configure-measurement-units-report-builder-and-ssrs"></a>Définir et configurer des unités de mesure (Générateur de rapports et SSRS)
  Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , les indicateurs utilisent une des deux unités de mesure : valeurs en pourcentage ou valeurs numériques.   
    
  Par défaut, les indicateurs sont configurés pour utiliser des pourcentages comme unité de mesure. Cela signifie que les valeurs d'indicateur affectées à chaque icône dans le jeu d'indicateurs sont déterminées par une plage de pourcentages. Les plages de pourcentages sont réparties de façon égale entre les icônes dans le jeu d'indicateurs. Chaque icône représente un état d'indicateur. Vous pouvez modifier les pourcentages pour chaque icône du jeu d'indicateurs en spécifiant différents pourcentages de début et de fin. Les indicateurs détectent également automatiquement les valeurs maximale et minimale des données.  
  
 Vous pouvez remplacer l'unité de mesure par une valeur numérique. Dans ce cas, vous ne spécifiez pas de valeur minimale ou maximale pour les données ; à la place, vous fournissez uniquement les valeurs de début et de fin pour chaque icône utilisée par l'indicateur.  
  
 Les options telles que les unités de mesure peuvent être définies à l'aide d'expressions. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
## <a name="to-use-the-numeric-state-measurement-unit"></a>Pour utiliser l'unité de mesure des états de type numérique  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Dans la liste **Unité de mesure des états** , cliquez sur **Numérique**.  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit la valeur de l’option.  
  
4.  Pour chaque icône du jeu d’indicateurs, mettez à jour les valeurs dans les zones de texte **Début** et **Fin** .  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit les valeurs des options **Début** et **Fin** .  
  
    > [!NOTE]  
    >  Les valeurs des zones de texte **Début** et **Fin** doivent être numériques.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-use-the-percentage-measurement-unit"></a>Pour utiliser l'unité de mesure en pourcentage  
  
1.  Cliquez avec le bouton droit sur l’indicateur à modifier, puis cliquez sur **Propriétés de l’indicateur**.  
  
2.  Cliquez sur **Valeur et états** dans le volet gauche.  
  
3.  Dans la liste **Unité de mesure des états** , cliquez sur **Pourcentage**.  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit la valeur de l’option.  
  
4.  Vous pouvez éventuellement modifier les options **Minimum** et **Maximum** pour utiliser des valeurs spécifiques au lieu de détecter automatiquement les valeurs minimale et maximale des données que l’indicateur utilise. La valeur de l’option **Minimum** doit être inférieure à celle de l’option **Maximum**.  
  
    > [!NOTE]  
    >  Si vous définissez explicitement les valeurs minimale et maximale, cette plage de valeurs est utilisée par l'indicateur indépendamment des valeurs minimale et maximale réelles des données. Cela signifie que les valeurs inférieures à la valeur minimale et celles supérieures à la valeur maximale sont exclues de l'évaluation qui détermine l'icône d'indicateur à afficher dans le rapport.  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit les valeurs de l’option.  
  
5.  Pour chaque icône du jeu d’indicateurs, mettez à jour les valeurs dans les zones de texte **Début** et **Fin** .  
  
     Cliquez éventuellement sur le bouton **Expression** (*fx*) pour modifier une expression qui définit les valeurs des options **Début** et **Fin** .  
  
    > [!NOTE]  
    >  Les valeurs des zones de texte **Début** et **Fin** doivent être numériques.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
