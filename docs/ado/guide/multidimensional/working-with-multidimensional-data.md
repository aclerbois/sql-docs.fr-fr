---
title: Utilisation des données multidimensionnelles | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional data [ADO]
ms.assetid: 84387746-aa3e-44fd-ad6c-a8214a6966dc
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1d7b5257bf0161c4064f1f25be5c223f46787e4f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-multidimensional-data"></a>Utilisation des données multidimensionnelles
A *ensemble de cellules* est le résultat d’une requête sur des données multidimensionnelles. Il se compose d’une collection d’axes, généralement pas plus de quatre axes et généralement deux ou trois. Un *axe* est une collection de membres à partir d’une ou plusieurs dimensions, qui est utilisée pour rechercher ou filtrer des valeurs spécifiques dans un cube.  
  
 A *position* est un point sur un axe. Pour un axe consistant en une seule dimension, ces positions sont un sous-ensemble des membres de dimension. Si un axe comprend plusieurs dimensions, chaque position est une entité composée, ce qui a *n* parties where *n* est le nombre de dimensions orientées le long de cet axe. Chaque partie de la position est un membre d’une dimension qui le composent.  
  
 Par exemple, si les dimensions géographie et produit à partir d’un cube contenant des données de ventes sont orientées le long de l’axe des x d’un ensemble de cellules, une position le long de cet axe peut contenir les membres « USA » et « Ordinateurs ». Dans cet exemple, la détermination de la position le long de l’axe des abscisses requiert que les membres de chaque dimension sont orientés le long de l’axe.  
  
 A *cellule* est un objet placé à l’intersection des coordonnées des axes. Chaque cellule dispose de plusieurs éléments d’information associé, notamment les données elles-mêmes, une chaîne mise en forme (la forme affichable des données de cellule) et la valeur ordinale de la cellule. (Chaque cellule est une valeur ordinale unique dans l’ensemble de cellules. La valeur ordinale de la première cellule de l’ensemble de cellules est égal à zéro, alors que la cellule la plus à gauche de la deuxième ligne d’un jeu de cellules avec huit colonnes aurait une valeur ordinale de huit.)  
  
 Par exemple, un cube possède les six dimensions suivantes (Notez que ce schéma de cube est légèrement différente de celui donné [vue d’ensemble des schémas et données multidimensionnels](../../../ado/guide/multidimensional/overview-of-multidimensional-schemas-and-data.md)) :  
  
-   Vendeur  
  
-   Géographie (hiérarchie naturelle) — Continents, pays, États, etc.  
  
-   Trimestres — Trimestres, mois, jours  
  
-   Années  
  
-   Mesures — Ventes, ChangementPourcentage, VentesBudget  
  
-   Produits (Products)  
  
 L’ensemble de cellules suivant représente des ventes pour 1991 pour tous les produits :  
  
> [!NOTE]
>  Les valeurs de cellule dans l’exemple peuvent être considérés comme paires classées de valeurs ordinales de position où le premier chiffre représente la position de l’axe des abscisses et le deuxième chiffre de la position de l’axe des y.  
  
 Les caractéristiques de cet ensemble de cellules sont les suivantes :  
  
-   Dimensions d’axe : trimestres, vendeur, géographie  
  
-   Filtrer des dimensions : mesures, années, produits  
  
-   Deux axes : colonne (x, ou axe 0) et ligne (y ou axe 1)  
  
-   axe des x : deux dimensions imbriquées, vendeur et géographie  
  
-   l’axe des y : dimension trimestres  
  
 L’axe des x contient deux dimensions imbriquées : vendeur et géographie. À partir de la géographie, quatre membres sont sélectionnés : Seattle, Boston, USA-South et Japan. Deux membres vendeur sont sélectionnés : Valentine et Nash. Il en résulte un total de huit positions sur cet axe (8 = 4 * 2).  
  
 Chaque coordonnée est représentée sous la forme d’une position avec deux membres : un à partir de la dimension vendeur et une autre à partir de la dimension Geography :  
  
```  
(Valentine, Seattle), (Valentine, Boston), (Valentine, USA_North),  
(Valentine, Japan), (Nash, Seattle), (Nash, Boston), (Nash, USA_North),  
(Nash, Japan)  
```  
  
 L’axe des y n'a qu’une seule dimension, qui contient les huit positions suivantes :  
  
```  
Jan, Feb, Mar, Qtr2, Qtr3, Oct, Nov, Dec  
```  
  
 Ensembles de cellules, les cellules, les axes et les positions sont tous représentées dans ADO MD par les objets correspondants : [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md), [cellule](../../../ado/reference/ado-md-api/cell-object-ado-md.md), [axe](../../../ado/reference/ado-md-api/axis-object-ado-md.md), et [Position](../../../ado/reference/ado-md-api/position-object-ado-md.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../../ado/reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionnel) (ADO MD)](../../../ado/guide/multidimensional/ado-multidimensional-ado-md.md)   
 [Vue d’ensemble des schémas et données multidimensionnels](../../../ado/guide/multidimensional/overview-of-multidimensional-schemas-and-data.md)   
 [Programmation avec ADO MD](../../../ado/guide/multidimensional/programming-with-ado-md.md)   
 [Utilisation d’ADO avec ADO MD](../../../ado/guide/multidimensional/using-ado-with-ado-md.md)
