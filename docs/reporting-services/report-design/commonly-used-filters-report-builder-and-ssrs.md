---
title: Filtres couramment utilisés (Générateur de rapports et SSRS) | Microsoft Docs
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
helpviewer_keywords:
- multivalued parameters [Reporting Services]
- single-valued parameters [Reporting Services]
- parameters [Reporting Services], multivalued
- valid values [Reporting Services]
ms.assetid: cb70d0cd-707b-4de5-b39f-e4eb57d316aa
caps.latest.revision: 36
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 4b1dee78e6edf4137d1fa50f2f81492989edd752
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="commonly-used-filters-report-builder-and-ssrs"></a>Filtres couramment utilisés (Générateur de rapports et SSRS)
  Pour créer un filtre, vous devez spécifier une ou plusieurs équations de filtre. Une équation de filtre comprend une expression, un type de données, un opérateur et une valeur. Cette rubrique donne des exemples de filtres couramment utilisés.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="filter-examples"></a>Exemples de filtres  
 Le tableau ci-dessous donne des exemples d'équations de filtre qui utilisent différents types de données et différent opérateurs. L'étendue de la comparaison est déterminée par l'élément de rapport pour lequel un filtre est défini. Par exemple, pour un filtre défini sur un dataset, **10 principaux %** représente les 10 premiers pour cent des valeurs dans le dataset ; pour un filtre défini sur un groupe, **10 principaux %** représente les 10 premiers pour cent des valeurs dans le groupe.  
  
|Expression simple|Type de données|Opérateur|Valeur|Description|  
|-----------------------|---------------|--------------|-----------|-----------------|  
|`[SUM(Quantity)]`|**Integer**|**>**|`7`|Inclut des valeurs de données supérieures à 7.|  
|`[SUM(Quantity)]`|**Integer**|**N supérieurs**|`10`|Inclut les 10 premières valeurs de données.|  
|`[SUM(Quantity)]`|**Integer**|**% supérieurs**|`20`|Inclut les 20 premiers pour cent des valeurs de données.|  
|`[Sales]`|**Texte**|**>**|`=CDec(100)`|Inclut toutes les valeurs de type System.Decimal (types de données money SQL) supérieures à 100 $.|  
|`[OrderDate]`|**DateTime**|**>**|`2088-01-01`|Inclut toutes les dates du 1er janvier 2008 à la date du jour.|  
|`[OrderDate]`|**DateTime**|**BETWEEN**|`2008-01-01`<br /><br /> `2008-02-01`|Inclut les dates à partir du 1er janvier 2008 jusqu'au 1er février 2008 compris.|  
|`[Territory]`|**Texte**|**LIKE**|`*east`|Tous les noms de secteurs qui se terminent par « est ».|  
|`[Territory]`|**Texte**|**LIKE**|`%o%th*`|Tous les secteurs dont le nom commence par Nord et Sud.|  
|`=LEFT(Fields!Subcat.Value,1)`|**Texte**|**IN**|`B, C, T`|Toutes les valeurs de sous-catégorie commençant par les lettres B, C ou T.|  
  
## <a name="examples-with-report-parameters"></a>Exemples avec des paramètres de rapport  
 Le tableau suivant fournit des exemples d'expressions de filtre comprenant une référence de paramètre à valeur unique ou à valeurs multiples.  
  
|Type de paramètre|Expression (de filtre)|Opérateur|Valeur|Type de données|  
|--------------------|---------------------------|--------------|-----------|---------------|  
|Valeur unique|`[EmployeeID]`|=|`[@EmployeeID]`|Entier|  
|Valeurs multiples|`[EmployeeID]`|IN|`[@EmployeeID]`|Entier|  
  
## <a name="see-also"></a> Voir aussi  
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)  
  
  
