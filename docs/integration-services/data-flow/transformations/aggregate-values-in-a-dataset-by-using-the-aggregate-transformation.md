---
title: Agréger les valeurs dans un dataset à l’aide de la transformation d’agrégation | Microsoft Docs
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
helpviewer_keywords:
- Aggregate transformation [Integration Services]
- aggregate values [Integration Services]
- datasets [Integration Services], aggregate values
ms.assetid: 01b81c0f-d5e0-483b-81b2-73800a6945ac
caps.latest.revision: 48
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9d5ca8eb521cc0148aa036a6670863ae88d60c97
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="aggregate-values-in-a-dataset-by-using-the-aggregate-transformation"></a>Agréger les valeurs dans un dataset à l'aide de la transformation d'agrégation
  Pour pouvoir ajouter et configurer une transformation d'agrégation, le package doit inclure au moins une tâche de flux de données et une source.  
  
### <a name="to-aggregate-values-in-a-dataset"></a>Pour agréger les valeurs dans un dataset  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** , puis dans la **Boîte à outils**, faites glisser la transformation d’agrégation sur la surface de dessin.  
  
4.  Connectez la transformation d'agrégation au flux de données en faisant glisser un connecteur à partir de la source ou de la transformation précédente vers la transformation d'agrégation.  
  
5.  Double-cliquez sur la transformation.  
  
6.  Dans la boîte de dialogue **Éditeur de transformation d’agrégation** , cliquez sur l’onglet **Agrégations** .  
  
7.  Dans la liste **Colonnes d’entrée disponibles** , cochez la case des colonnes dont vous voulez agréger les valeurs. Les colonnes sélectionnées s'affichent dans la table.  
  
    > [!NOTE]  
    >  Vous pouvez sélectionner une colonne plusieurs fois afin de lui appliquer plusieurs transformations. Pour identifier les agrégations de manière unique, un numéro est ajouté au nom par défaut de l'alias de sortie de la colonne.  
  
8.  Si vous le souhaitez, modifiez la valeur des colonnes **Alias de sortie** .  
  
9. Pour modifier l’opération d’agrégation par défaut, **Group by**, sélectionnez une autre opération dans la liste **Opération** .  
  
10. Pour modifier la comparaison par défaut, sélectionnez les indicateurs de comparaison individuels répertoriés dans la colonne **Indicateurs de comparaison** . Par défaut, une comparaison ignore la casse, les caractères de type Kana, les caractères de non-espacement et la largeur des caractères.  
  
11. Si vous le souhaitez, pour l’agrégation **Count distinct** , indiquez le nombre exact de valeurs distinctes dans la colonne **Nombre de clés distinctes** ou sélectionnez un nombre approximatif dans la colonne **Échelle de nombre des valeurs distinctes** .  
  
    > [!NOTE]  
    >  La fourniture du nombre de valeurs distinctes, exact ou approximatif, optimise les performances, car la transformation peut préallouer la quantité de mémoire appropriée pour effectuer son travail.  
  
12. Si vous le souhaitez, cliquez sur **Avancé** et mettez à jour le nom de la sortie de la transformation d’agrégation. Si les agrégations contiennent une opération **Group By** , vous pouvez sélectionner un nombre approximatif de valeurs de clés de groupement dans la colonne **Redéfinir le nombre de clés** ou indiquer un nombre exact de valeurs de clés de groupement dans la colonne **Clés** .  
  
    > [!NOTE]  
    >  La fourniture du nombre de valeurs distinctes, exact ou approximatif, optimise les performances, car la transformation peut préallouer la quantité de mémoire appropriée pour effectuer son travail.  
  
    > [!NOTE]  
    >  Les options **Redéfinir le nombre de clés** et **Clés** s’excluent mutuellement. Si vous tapez des valeurs dans les deux colonnes, la valeur la plus grande de la colonne **Redéfinir le nombre de clés** ou **Clés** est utilisée.  
  
13. Si vous le souhaitez, cliquez sur l’onglet **Avancé** et définissez les attributs qui s’appliquent à l’optimisation de toutes les opérations réalisées par la transformation d’agrégation.  
  
14. Cliquez sur **OK**.  
  
15. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a> Voir aussi  
 [Transformation d'agrégation](../../../integration-services/data-flow/transformations/aggregate-transformation.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [tâche de flux de données](../../../integration-services/control-flow/data-flow-task.md)  
  
  
