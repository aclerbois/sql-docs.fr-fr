---
title: rrRenderingError- Erreur Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- rrRenderingError
ms.assetid: 0751efc3-b81b-44ee-8aac-8560f86ca322
caps.latest.revision: 26
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 099e0669f8c68dea2cde8b59098917bd0dc3ef60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rrrenderingerror---reporting-services-error"></a>rrRenderingError - Erreur Reporting Services
    
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID d'événement|rrRenderingError|  
|Source de l'événement|Microsoft.ReportingServices.Diagnostics.Utilities.ErrorStrings.resources.Strings|  
|Composant|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Texte du message|Une erreur s'est produite lors du rendu du rapport. (rrRenderingError) %1|  
  
## <a name="explanation"></a>Explication  
 Ce message s'affiche lorsque [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne peut pas restituer ou exporter le rapport.  
  
 Un message indiquant que la taille n'est pas prise en charge est généralement dû à une taille de page RDL non valide spécifiée. Spécifiez une taille de page RDL valide puis réessayez.  
  
 Un message indiquant qu'une mesure de taille RDL n'est pas prise en charge est en général provoqué par la spécification d'un type d'unité non pris en charge. Les types d'unités valides sont cm, in, mm, pc et pt. Spécifiez un type d'unité valide puis réessayez.  
  
 Un message indiquant qu'une taille négative n'est pas prise en charge est en général provoqué par la spécification d'une mesure négative pour la taille de la page, par exemple -5 cm. Spécifiez un nombre positif comme taille de page puis réessayez.  
  
 Un message indiquant qu'une taille RDL hors limites a été spécifiée est en général provoqué par une mesure de taille de page qui dépasse la taille de marge de page valide. Spécifiez une mesure pour la taille de page comprise entre les tailles de marge de page valides.  
  
 Un message indiquant qu'une couleur spécifiée n'est pas prise en charge est généralement dû à une couleur non valide spécifiée dans l'élément RDL. Choisissez une couleur prise en charge par l'élément RDL puis réessayez.  
  
 Un message indiquant que l'étiquette d'action est facultative uniquement lors de l'utilisation d'une action unique et que l'ajout de plusieurs actions requiert des étiquettes pour chaque action est en général provoqué par la spécification d'une étiquette d'action non valide. Spécifiez une étiquette d'action valide pour chaque action.  
  
 Un message indiquant que l'argument de style doit être d'un type spécifique est en général provoqué par la spécification d'une valeur de style incorrecte pour le type de données. La spécification RDL identifie les types valides que vous pouvez utiliser dans les valeurs de style d'éléments RDL différents. Par exemple, "2pt" est une valeur de style incorrecte pour la couleur d'arrière-plan et "true" est une valeur incorrecte de hauteur. Spécifiez une valeur correcte puis réessayez.  
  
 Un message indiquant que le style de bordure n'est pas pris en charge est généralement dû à la spécification d'un style de bordure non valide. Spécifiez un style de bordure pris en charge puis réessayez.  
  
 Un message indiquant que le type MIME de l'image n'est pas pris en charge est en général provoqué par la spécification d'un type MIME non valide pour un élément de rapport de type image. Spécifiez un type MIME pris en charge pour l'élément de rapport puis réessayez.  
  
 Un message indiquant que le nombre de lignes dépasse le nombre maximal de lignes possibles par feuille est en général dû au dépassement de ce nombre de lignes dans une feuille de calcul Excel. Excel prend en charge un maximum de 65 000 lignes.  
  
 Un message indiquant que le nombre de colonnes dépasse le nombre maximal de colonnes possibles par feuille est en général dû au dépassement de ce nombre de colonnes dans une feuille de calcul Excel.  
  
## <a name="user-action"></a>Action de l'utilisateur  
  
## <a name="internal-only"></a>Interne uniquement  
  
