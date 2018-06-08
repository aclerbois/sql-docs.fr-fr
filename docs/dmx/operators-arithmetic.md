---
title: Opérateurs arithmétiques (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- DMX
helpviewer_keywords:
- arithmetic operators
ms.assetid: befe4f0c-e5dd-4ae1-b88e-6ac7aab2181a
caps.latest.revision: 17
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 10894ee5f3ab7d0a7defecf324a297b4bd3ad6ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="operators---arithmetic"></a>Opérateurs - arithmétique
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Vous pouvez utiliser des opérateurs arithmétiques dans les Extensions DMX (Data Mining) pour les calculs arithmétiques de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], y compris l’addition, soustraction, multiplication et division.  
  
 Le tableau ci-dessous identifie les opérateurs arithmétiques pris en charge par DMX.  
  
|Opérateur| Description|  
|--------------|-----------------|  
|[+ &#40;Ajouter&#41; &#40;DMX&#41;](../dmx/add-dmx.md)|Additionne deux nombres.|  
|[- &#40;Soustraire&#41; &#40;DMX&#41;](../dmx/subtract-dmx.md)|Soustrait un nombre d'un autre.|  
|[&#42;&#40;Multiplier&#41; &#40;DMX&#41;](../dmx/multiply-dmx.md)|Multiplie un nombre par un autre.|  
|[&#40;Diviser&#41; &#40;DMX&#41;](../dmx/divide-dmx.md)|Divise un nombre par un autre.|  
  
 Les règles suivantes déterminent l'ordre de priorité des opérateurs arithmétiques dans une expression DMX :  
  
-   Lorsqu'une expression contient plusieurs opérateurs arithmétiques, la multiplication et la division sont effectuées en premier, avant la soustraction et l'addition.  
  
-   Lorsque tous les opérateurs arithmétiques dans une expression ont le même niveau de priorité, l’ordre d’exécution est de gauche à droite.  
  
-   Les expressions entre parenthèses ont priorité sur toutes les autres opérations.  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Expressions &#40;DMX&#41;](../dmx/expressions-dmx.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Opérateurs &#40;DMX&#41;](../dmx/operators-dmx.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
