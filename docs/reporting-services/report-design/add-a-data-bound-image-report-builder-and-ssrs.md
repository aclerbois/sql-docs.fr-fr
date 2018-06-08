---
title: Ajouter une image liée à des données (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: df4c38d4-bfcc-41c4-aa6d-952ca6fd7a2e
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 7a0891c5131b15e727db4664618753253bd605dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-data-bound-image-report-builder-and-ssrs"></a>Ajouter une image liée à des données (Générateur de rapports et SSRS)
  Un rapport peut inclure une référence à une image stockée dans une base de données. Cette image est appelée *image liée aux données*. Les images qui jouxtent les noms de produits dans une liste de produits sont des exemples d'images liées à des données.  
  
 L'ajout d'une image liée à des données à un en-tête de page ou pied de page requiert des étapes supplémentaires. Pour plus d’informations, consultez [En-têtes et pieds de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/page-headers-and-footers-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-add-a-data-bound-image"></a>Pour ajouter une image liée à des données  
  
1.  En mode de conception de rapport, créez une table avec une connexion à la source de données et un dataset avec un champ qui contient des données binaires de type image. Pour plus d’informations, consultez [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md).  
  
2.  Insérez une colonne dans votre table. Pour plus d’informations, consultez [Insérer ou supprimer une colonne &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md).  
  
3.  Dans le menu **Insérer** , cliquez sur **Image**, puis cliquez sur la ligne de données de la nouvelle colonne.  
  
4.  Dans la page Général de la boîte de dialogue **Propriétés de l’image** , tapez un nom dans la zone de texte **Nom** ou acceptez le nom par défaut.  
  
5.  (Facultatif) Dans la zone de texte **Info-bulle** , tapez le texte à afficher quand l’utilisateur pointe sur l’image dans le rapport rendu au format HTML.  
  
6.  Dans **Sélectionner la source de l’image**, sélectionnez **Base de données**.  
  
7.  Dans **Utiliser ce champ**, sélectionnez le champ qui contient des images dans votre rapport.  
  
8.  Dans **Utiliser ce type MIME**, sélectionnez le type MIME ou le format de fichier de l’image, par exemple bmp.  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Un espace réservé à l'image apparaît sur l'aire de conception du rapport.  
  
## <a name="see-also"></a> Voir aussi  
 [Images &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md)   
 [Incorporer une image dans un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/embed-an-image-in-a-report-report-builder-and-ssrs.md)   
 [Ajouter une image externe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-external-image-report-builder-and-ssrs.md)   
 [Boîte de dialogue Propriétés de l’image, Général &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/c2218b93-f7fe-46ef-995f-d7dadf9752ec)  
  
  
