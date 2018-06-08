---
title: Mise en forme des pointeurs sur une jauge (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 2fdf670a-5237-48fe-813d-97657c5c77d2
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 6b74271c98941233d712410a43b31959595119eb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="formatting-pointers-on-a-gauge-report-builder-and-ssrs"></a>Mise en forme des pointeurs sur une jauge (Générateur de rapports et SSRS)
 Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , le pointeur de jauge indique la valeur actuelle de la jauge.   
   
 Par défaut, lorsqu'un champ est ajouté, les valeurs contenues dans le champ sont agrégées en une valeur représentée par le pointeur sur la jauge. Vous pouvez ajouter plusieurs pointeurs à la jauge pour indiquer plusieurs valeurs sur la même échelle ou ajouter plusieurs échelles et un pointeur pour chaque échelle ajoutée. Après avoir ajouté un champ à une jauge, vous devez définir les valeurs maximale et minimale sur l'échelle correspondante pour donner le contexte de la valeur du pointeur. Vous avez également la possibilité de définir des valeurs minimale et maximale sur une plage qui représente une zone critique sur l'échelle.  
  
 Vous pouvez définir des propriétés d’apparence pour le pointeur en cliquant avec le bouton droit sur le pointeur et en sélectionnant **Propriétés du pointeur radial** ou **Propriétés du pointeur linéaire**. Chaque pointeur de jauge comporte le même jeu de propriétés. Il existe également des propriétés d'apparence uniques propres à chaque type de jauge :  
  
-   Sur une jauge radiale, vous pouvez spécifier un pointeur de type aiguille et l'extrémité de fin d'une aiguille.  
  
-   Sur une jauge linéaire, vous pouvez spécifier un pointeur de type thermomètre, qui est une variation du pointeur de type barre. Le pointeur de type thermomètre vous permet de spécifier la forme du bulbe.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="HowPointer"></a> Comment le pointeur est associé aux données  
 Par défaut, lorsqu'une jauge est ajoutée, elle contient un pointeur sans champ associé. C'est ce que l'on appelle un pointeur vide. Il affichera la valeur zéro jusqu'à ce qu'un champ soit ajouté au volet des données. Lorsque vous ajoutez un champ au volet des données, le pointeur est associé à ce champ. Si vous supprimez un champ du volet des données, le pointeur associé à ce champ est également supprimé.  
  
 Une fois les données ajoutées, quand vous cliquez avec le bouton droit sur le pointeur, vous obtenez les options **Effacer la valeur du pointeur** et **Supprimer le pointeur** . L'option **Effacer la valeur du pointeur** supprime le champ associé à la jauge, mais le pointeur apparaît toujours sur la jauge. L'option **Supprimer le pointeur** supprime le champ de la jauge et le pointeur de la vue. Si vous ajoutez à nouveau un champ à la jauge, le pointeur par défaut réapparaît. Lorsque vous affectez à la propriété **Masqué** du pointeur la valeur **True**, le pointeur n’est pas masqué sur l’aire de conception, mais il est masqué au moment de l’exécution.  
  
##  <a name="DisplayingMultiple"></a> Affichage de plusieurs pointeurs sur la jauge  
 Vous pouvez ajouter plusieurs pointeurs à la jauge pour représenter plusieurs valeurs sur la même échelle. Cela peut être utile pour afficher une valeur basse et une valeur haute en même temps. Pour spécifier plusieurs pointeurs sur la jauge pour la même échelle, cliquez avec le bouton droit n’importe où à l’intérieur de la jauge et cliquez sur **Ajouter un pointeur** dans le menu contextuel. Vous pouvez également ajouter une échelle en cliquant avec le bouton droit n’importe où dans la jauge et en cliquant sur **Ajouter une échelle**. Vous pouvez ensuite ajouter un nouveau pointeur, qui sera automatiquement associé à la dernière échelle.  
  
 Lorsque des pointeurs se chevauchent, l'ordre de dessin des pointeurs est déterminé par l'ordre dans lequel ils sont ajoutés à la jauge. Vous ne pouvez pas modifier l'ordre des pointeurs en modifiant l'ordre des champs dans le volet des données. Pour modifier l’ordre de dessin de plusieurs pointeurs, ouvrez le volet Propriétés et cliquez sur **Pointeurs (…)**. Modifiez l'ordre des pointeurs dans la collection de pointeurs.  
  
##  <a name="SettingGradients"></a> Définition des dégradés sur la base d'une aiguille  
 Vous pouvez spécifier la base d'une aiguille qui peut être dessinée au-dessus ou au-dessous du pointeur sur une jauge radiale uniquement. Tous les styles de base d'aiguille sont dessinés à l'aide de dégradés intégrés qui ne peuvent pas être modifiés. La seule exception est le style **RoundedDark** , qui vous permet de spécifier une couleur et un style de dégradé.  
  
##  <a name="SettingSnappingInterval"></a> Définition d'un intervalle d'alignement  
 Un intervalle d'alignement définit le multiple auquel les valeurs sont arrondies. Par défaut, la jauge pointera sur la valeur exacte du champ que vous avez spécifiée dans le volet des données. Toutefois, vous pouvez arrondir la valeur exacte à la valeur supérieure ou inférieure afin que le pointeur s'aligne sur un intervalle prédéfini. Par exemple, si la valeur sur votre jauge est 34,2 et que vous spécifiez un intervalle d'alignement de 5, le pointeur de jauge pointera sur 35. Si la valeur sur votre jauge est 31,2 et que vous spécifiez un intervalle d'alignement de 5, le pointeur de jauge pointera sur 30. Pour plus d’informations, consultez [Définir un intervalle d’alignement sur une jauge (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/0ece7297-6e2f-47fb-835d-b9e9cce53fe2).  
  
##  <a name="SpecifyingImage"></a> Spécification d'une image en tant que pointeur sur une jauge radiale  
 En plus de la liste intégrée de styles de pointeur, vous pouvez choisir une image comme pointeur. Cela fonctionne très bien lorsque vous utilisez une image pour remplacer un style de pointeur de type aiguille existant. L'image est superposée au pointeur, mais toutes les fonctionnalités du pointeur sont conservées. Les options de couleur et de dégradé ne sont pas applicables lorsqu'une image est utilisée comme pointeur.  
  
 Si l'image du pointeur a une forme irrégulière, vous devez définir la couleur comme transparente afin de masquer les zones de votre image qui ne doivent pas apparaître sur la jauge. Lorsque vous définissez une couleur transparente, la jauge transpose l'image sur votre pointeur existant et rogne l'image afin que seule la forme du pointeur apparaisse. La jauge recadre l'image en fonction de la taille de votre pointeur. Lorsque vous spécifiez une image comme pointeur, tout pointeur ajouté ultérieurement sur la jauge est dessiné sous l'image. C'est pourquoi il est préférable de ne pas spécifier d'image pour le pointeur s'il y a plusieurs pointeurs sur la jauge. Pour plus d’informations, consultez [Spécifier une image en tant que pointeur dans une jauge (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/9d73b3c3-a068-4868-a2be-0cd261b6e92b).  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme des échelles sur une jauge &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-scales-on-a-gauge-report-builder-and-ssrs.md)   
 [Mise en forme de plages sur une jauge &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-ranges-on-a-gauge-report-builder-and-ssrs.md)   
 [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
  
