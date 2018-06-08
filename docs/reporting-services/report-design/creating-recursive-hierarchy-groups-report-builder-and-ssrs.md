---
title: Création de groupes de hiérarchies récursives (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 06eccab6-4089-46e8-a84f-5bf3bbe0c23b
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: f5c87c8116faca9612dffce45ea744069024bb9b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-recursive-hierarchy-groups-report-builder-and-ssrs"></a>Création de groupes de hiérarchies récursives (Générateur de rapports et SSRS)
Pour afficher des données récursives dans des rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] où la relation entre le parent et l’enfant est représentée par des champs du dataset), définissez l’expression du groupe de régions de données en fonction du champ enfant et la propriété Parent en fonction du champ parent.  
  
 L'affichage de données hiérarchiques est couramment utilisé pour les groupes de hiérarchies récursives, par exemple des employés dans un graphique d'organisation. Le dataset inclut une liste d'employés et les directeurs, où les noms de directeurs apparaissent également dans la liste des employés.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="creating-recursive-hierarchies"></a>Création de hiérarchies récursives  
 Pour créer une hiérarchie récursive dans une région de données de tableau matriciel, vous devez définir l’expression de groupe en fonction du champ qui spécifie les données enfants et la propriété Parent du groupe en fonction du champ qui spécifie les données parents. Par exemple, pour un dataset qui comprend des champs pour l’ID d’employé et l’ID de responsables où les employés rendent des comptent aux responsables, affectez l’ID d’employé à l’expression de groupe et l’ID de responsable à la propriété Parent.  
  
 Un groupe défini en tant que hiérarchie récursive (c’est-à-dire un groupe qui utilise la propriété Parent) ne peut comporter qu’une seule expression de groupe. Vous pouvez utiliser la fonction **Level** dans la définition de la marge intérieure de la zone de texte pour appliquer un retrait aux noms des employés en fonction du niveau que ceux-ci occupent dans la hiérarchie.  
  
 Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md) et [Créer un groupe de hiérarchies récursives &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-recursive-hierarchy-group-report-builder-and-ssrs.md).  
  
### <a name="aggregate-functions-that-support-recursion"></a>Fonctions d'agrégation qui prennent en charge la récursivité  
 Vous pouvez utiliser les fonctions d’agrégation Reporting Services qui acceptent le paramètre *Recursive* pour calculer les données de synthèse d’une hiérarchie récursive. Les fonctions suivantes acceptent **Recursive** comme paramètre : [Sum](../../reporting-services/report-design/report-builder-functions-sum-function.md), [Avg](../../reporting-services/report-design/report-builder-functions-avg-function.md), [Count](../../reporting-services/report-design/report-builder-functions-count-function.md), [CountDistinct](../../reporting-services/report-design/report-builder-functions-countdistinct-function.md), [CountRows](../../reporting-services/report-design/report-builder-functions-countrows-function.md), [Max](../../reporting-services/report-design/report-builder-functions-max-function.md), [Min](../../reporting-services/report-design/report-builder-functions-min-function.md), [StDev](../../reporting-services/report-design/report-builder-functions-stdev-function.md), [StDevP](../../reporting-services/report-design/report-builder-functions-stdevp-function.md), [Sum](../../reporting-services/report-design/report-builder-functions-sum-function.md), [Var](../../reporting-services/report-design/report-builder-functions-var-function.md)et [VarP](../../reporting-services/report-design/report-builder-functions-varp-function.md). Pour plus d’informations, consultez [Informations de référence sur les fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Informations de référence sur les fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md)   
 [Matrices &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)    
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
