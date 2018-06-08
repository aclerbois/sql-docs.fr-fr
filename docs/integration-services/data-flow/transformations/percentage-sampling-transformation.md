---
title: Échantillonnage du pourcentage, transformation | Microsoft Docs
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
- sql13.dts.designer.percentagesamplingtrans.f1
- sql13.dts.designer.percentagesamplingtransformation.f1
helpviewer_keywords:
- testing mining models
- sampling seeds [Integration Services]
- data mining [Analysis Services], sample data sets
- Percentage Sampling transformation
- sample data sets [Integration Services]
- datasets [Integration Services], sample
- training mining models
ms.assetid: 59767e52-f732-4b3f-8602-be50d0a64ef2
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cbcde5a88ef2c1588ec0cd8495a7276a002f9ca5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="percentage-sampling-transformation"></a>transformation de l'échantillonnage du pourcentage
  La transformation de l'échantillonnage du pourcentage crée un échantillon d'ensemble de données en sélectionnant un pourcentage des lignes d'entrée de transformation. L'échantillon d'ensemble de données est une sélection aléatoire de lignes dans l'entrée de transformation, visant à rendre l'échantillon résultant représentatif de l'entrée.  
  
> [!NOTE]  
>  Outre le pourcentage spécifié, la transformation de l'échantillonnage du pourcentage utilise un algorithme pour déterminer si une ligne doit être incluse dans l'échantillon en sortie. Par conséquent, le nombre de lignes dans l'échantillon en sortie peut ne pas refléter exactement le pourcentage spécifié. Par exemple, la spécification de la valeur 10 % pour un ensemble de données d'entrée composé de 25 000 lignes peut ne pas générer un échantillon de 2 500 lignes ; celui-ci peut avoir un peu plus ou un peu moins de lignes.  
  
 La transformation de l'échantillonnage du pourcentage est spécialement utile pour l'exploration de données. Cette transformation vous permet de diviser de façon aléatoire un jeu de données en deux jeux de données : un pour mettre au point le modèle d'exploration de données, l'autre pour le tester.  
  
 La transformation de l'échantillonnage du pourcentage permet également de créer des échantillons d'ensembles de données en vue de développer des packages. Vous pouvez appliquer la transformation de l'échantillonnage du pourcentage à un flux de données afin de réduire uniformément la taille de l'ensemble de données tout en conservant les caractéristiques de ses données. Le package de test peut ensuite s'exécuter plus rapidement car il utilise un ensemble de données réduit mais représentatif.  
  
## <a name="configuration-the-percentage-sampling-transformation"></a>Configuration de la transformation de l'échantillonnage du pourcentage  
 Vous pouvez spécifier une valeur d'échantillonnage de départ afin de modifier le comportement du générateur de nombres aléatoires qu'utilise la transformation pour sélectionner les lignes. Si la même valeur d'échantillonnage de départ est utilisée, la transformation crée toujours le même échantillon en sortie. Si aucune valeur de départ n'est spécifiée, la transformation utilise le nombre de cycles du système d'exploitation pour créer le nombre aléatoire. Par conséquent, vous pouvez utiliser une valeur de départ standard pour vérifier les résultats de la transformation pendant le développement et le test d'un package, puis opter pour une valeur de départ aléatoire lorsque le package passe en production.  
  
 Cette transformation est similaire à la transformation d'échantillonnage de lignes, qui crée un échantillon d'ensemble de données en sélectionnant un nombre spécifié de lignes d'entrée. Pour plus d’informations, consultez [Transformation d’échantillonnage de lignes](../../../integration-services/data-flow/transformations/row-sampling-transformation.md).  
  
 La transformation d’échantillonnage par pourcentage inclut la propriété personnalisée **SamplingValue** . La propriété peut être mise à jour par une expression de propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41; ](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Expressions de propriété dans des packages](../../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 La transformation a une entrée et deux sorties. Elle ne prend pas en charge de sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="percentage-sampling-transformation-editor"></a>Éditeur de transformation de l'échantillonnage du pourcentage
  La boîte de dialogue **Éditeur de transformation de l'échantillonnage du pourcentage** permet de fractionner une partie d'une entrée en un exemple par le biais d'un certain pourcentage de lignes. Cette transformation divise l'entrée en deux sorties distinctes.  
  
### <a name="options"></a>Options  
 **Pourcentage de lignes**  
 Permet d'indiquer le pourcentage de lignes de l'entrée à utiliser comme exemple.  
  
 Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.  
  
 **Nom de sortie de l'exemple**  
 Fournissez un nom unique pour la sortie qui contiendra les lignes échantillonnées. Le nom fourni s'affichera dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] .  
  
 **Nom de sortie non sélectionnée**  
 Fournissez un nom unique pour la sortie qui contiendra les lignes exclues de l'échantillonnage. Le nom fourni s'affichera dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] .  
  
 **Utiliser la valeur de départ aléatoire suivante**  
 Définissez la valeur de départ d'échantillonnage du générateur de nombres aléatoires qu'utilise la transformation pour créer un échantillon. Ceci est recommandé uniquement pour le développement et les tests. La fonctionnalité de transformation utilise le nombre de cycles de Microsoft Windows si aucune valeur de départ aléatoire n'est mentionnée.  
  
  
