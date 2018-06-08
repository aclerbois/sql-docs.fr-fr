---
title: Fusion, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.mergetrans.f1
- sql13.dts.designer.mergetransformation.f1
helpviewer_keywords:
- merging datasets [Integration Services]
- merging data [Integration Services]
- Merge transformation
- combining datasets
- datasets [Integration Services], merging
ms.assetid: cff8690c-07ac-46a0-aab5-20bd4848c677
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 49e73b34167e7eb1b494995adce976ecf2601b22
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="merge-transformation"></a>transformation de fusion
  La transformation de fusion combine deux datasets triés en un seul datasets. Les lignes de chaque ensemble de données sont insérées dans la sortie en fonction des valeurs de leurs colonnes clés.  
  
 L'intégration de la transformation de fusion dans un flux de données permet de réaliser les tâches suivantes :  
  
-   Fusionner des données de deux sources de données, telles que des tables et des fichiers.  
  
-   Créer des ensembles de données complexes en imbriquant des transformations de fusion.  
  
-   Fusionner des lignes à nouveau après avoir corrigé les erreurs affectant les données.  
  
 La transformation de fusion est similaire aux transformations d'union totale. Utilisez la transformation d'union totale au lieu de la transformation de fusion dans les situations suivantes :  
  
-   Les entrées de transformation ne sont pas triées.  
  
-   La sortie combinée n'a pas besoin d'être triée.  
  
-   La transformation a au moins trois entrées.  
  
## <a name="input-requirements"></a>Spécifications relatives aux entrées  
 La transformation de fusion requiert des données triées pour ses entrées. Pour plus d’informations sur cette spécification importante, consultez [Trier des données pour les transformations de fusion et de jointure de fusion](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).  
  
 La transformation de fusion requiert également que les colonnes fusionnées dans ses entrées aient des métadonnées correspondantes. Par exemple, vous ne pouvez pas fusionner une colonne de type de données numérique avec une colonne de type de données caractère. Si les données sont du type de données chaîne, la colonne de la deuxième entrée doit avoir une longueur inférieure ou égale à celle de la colonne de la première entrée avec laquelle elle est fusionnée.  
  
 Dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] , l’interface utilisateur de la transformation de fusion mappe automatiquement les colonnes qui ont les mêmes métadonnées. Vous pouvez ensuite mapper manuellement les colonnes ayant des types de données compatibles.  
  
 Cette transformation a deux entrées et une sortie. Elle ne prend pas en charge de sortie d'erreur.  
  
## <a name="configuration-of-the-merge-transformation"></a>Configuration de la transformation de fusion  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programme.  
  
 Pour plus d'informations sur les propriétés définissables par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d'informations sur la définition des propriétés, consultez les rubriques suivantes :  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
-   [Trier des données pour les transformations de fusion et de jointure de fusion](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="merge-transformation-editor"></a>Éditeur de transformation de fusion
  Utilisez **l’Éditeur de transformation de fusion** pour définir des colonnes dans deux datasets triés à fusionner.  
  
> [!IMPORTANT]  
>  La transformation de fusion requiert des données triées pour ses entrées. Pour plus d’informations sur cette spécification importante, consultez [Trier des données pour les transformations de fusion et de jointure de fusion](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).  
  
### <a name="options"></a>Options  
 **Nom de colonne de sortie**  
 Spécifiez le nom de la colonne de sortie.  
  
 **Entrée de fusion 1**  
 Sélectionnez la colonne à fusionner sous la forme Entrée de fusion 1.  
  
 **Entrée de fusion 2**  
 Sélectionnez la colonne à fusionner sous la forme Entrée de fusion 2.  
  
## <a name="see-also"></a> Voir aussi  
 [Transformation de jointure de fusion](../../../integration-services/data-flow/transformations/merge-join-transformation.md)   
 [Union All Transformation](../../../integration-services/data-flow/transformations/union-all-transformation.md)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
