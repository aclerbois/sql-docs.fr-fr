---
title: Vue d’ensemble des schémas et données multidimensionnels | Documents Microsoft
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
- multidimensional schemas and data
ms.assetid: ce37fa06-c581-4d80-9a9b-c3aa66408909
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e7dad7be35de7e15ae560f56c3ad51f7222be53b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="overview-of-multidimensional-schemas-and-data"></a>Vue d’ensemble des schémas et données multidimensionnels
## <a name="understanding-multidimensional-schemas"></a>Présentation des schémas multidimensionnels  
 L’objet de métadonnées central dans ADO MD est la *cube*, qui se compose d’un ensemble structuré de dimensions associées, hiérarchies, niveaux et membres.  
  
 A *dimension* est une catégorie indépendante de données à partir de votre base de données multidimensionnelle à partir de vos entités métier. Une dimension contient généralement des éléments à utiliser comme critère de requête pour les mesures de la base de données.  
  
 A *hiérarchie* est un chemin d’accès d’agrégation d’une dimension. Une dimension peut avoir plusieurs niveaux de granularité, qui ont des relations parent-enfant. Une hiérarchie définit la relation entre ces niveaux.  
  
 A *niveau* est une étape d’agrégation dans une hiérarchie. Pour les dimensions avec plusieurs couches d’informations, chaque couche est un niveau.  
  
 A *membre* est un élément de données dans une dimension. En règle générale, vous créez une légende ou décrivez une mesure de la base de données à l’aide de membres.  
  
 Les cubes sont représentés par [CubeDef](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md) objets dans ADO MD. Dimensions, hiérarchies, niveaux et membres sont également représentés par leurs objets ADO MD : [Dimension](../../../ado/reference/ado-md-api/dimension-object-ado-md.md), [hiérarchie](../../../ado/reference/ado-md-api/hierarchy-object-ado-md.md), [niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md), et [ Membre](../../../ado/reference/ado-md-api/member-object-ado-md.md).  
  
### <a name="dimensions"></a>Dimensions  
 Les dimensions d’un cube dépendent de vos entités commerciales et les types de données à modéliser dans la base de données. En règle générale, chaque dimension est un mécanisme de sélection des données ou d’un point d’entrée indépendante.  
  
 Par exemple, un cube contenant des données de ventes possède les cinq dimensions suivantes : vendeur, géographie, heure, produits et mesures. La dimension de mesures contient des valeurs de données de ventes réelles, tandis que les autres dimensions constituent des manières de classer et regrouper les valeurs de données de ventes.  
  
 La dimension Geography comprend l’ensemble de membres suivant :  
  
```  
{All, North America, Europe, Canada, USA, UK, Germany, Canada-West,  
Canada-East, USA-NW, USA-SW, USA-NE, USA-SE, England, Scotland,   
Wales,Ireland, Germany-North, Germany-South, Ottawa, Toronto,   
Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston,   
Shreveport, Miami, Boston, New York, London, Dover, Glasgow,   
Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin,   
Hamburg, Munich, Stuttgart}  
```  
  
### <a name="hierarchies"></a>Hierarchies  
 Hiérarchies définissent les manières dans lequel les niveaux d’une dimension peuvent être « remontées » ou regroupées. Une dimension peut avoir plusieurs hiérarchies. Il existe une hiérarchie naturelle dans la dimension Geography :  
  
### <a name="levels"></a>Levels  
 Dans la dimension Geography, exemple dans l’illustration précédente, chaque zone représente un niveau dans la hiérarchie.  
  
 Chaque niveau a un jeu de membres, comme suit :  
  
-   Le monde `= {All}`  
  
-   Continents `= {North America, Europe}`  
  
-   Pays `= {Canada, USA, UK, Germany}`  
  
-   Régions `= {Canada-East, Canada-West, USA-NE, USA-NW, USA-SE, USA-SW, England, Ireland, Scotland, Wales, Germany-North, Germany-South}`  
  
-   Villes `= {Ottawa, Toronto, Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston, Shreveport, Miami, Boston, New York, London, Dover, Glasgow, Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin, Hamburg, Munich, Stuttgart}`  
  
### <a name="members"></a>Membres  
 Membres de niveau feuille d’une hiérarchie ont pas d’enfants, et membres au niveau racine ont aucun parent. Tous les autres membres ont au moins un parent et au moins un enfant. Par exemple, un parcours partiels de l’arborescence de la hiérarchie dans la dimension Geography génère les relations parent-enfant suivantes :  
  
-   `{All} (parent of) {Europe, North America}`  
  
-   `{North America} (parent of) {Canada, USA}`  
  
-   `{USA} (parent of) {USA-NE, USA-NW, USA-SE, USA-SW}`  
  
-   `{USA-NW} (parent of) {Boise, Seattle}`  
  
 Les membres peuvent être consolidées dans une ou plusieurs hiérarchies par dimension. Envisagez d’une dimension de temps où il existe deux façons de regrouper au niveau de l’année à partir du niveau de jours :  
  
 Cet exemple illustre également une autre caractéristique : certains membres du niveau semaine de la hiérarchie année-semaine n’apparaissent pas dans n’importe quel niveau de la hiérarchie année-trimestre. Par conséquent, une hiérarchie ne devez pas inclure tous les membres d’une dimension.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../../ado/reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionnel) (ADO MD)](../../../ado/guide/multidimensional/ado-multidimensional-ado-md.md)   
 [Programmation avec ADO MD](../../../ado/guide/multidimensional/programming-with-ado-md.md)   
 [À l’aide d’ADO avec ADO MD](../../../ado/guide/multidimensional/using-ado-with-ado-md.md)   
 [Utilisation de données multidimensionnelles](../../../ado/guide/multidimensional/working-with-multidimensional-data.md)
