---
title: Union totale, transformation | Microsoft Docs
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
- sql13.dts.designer.unionalltrans.f1
- sql13.dts.designer.unionalltransformation.f1
helpviewer_keywords:
- merging datasets [Integration Services]
- combining datasets
- Union All transformation
- datasets [Integration Services], merging
ms.assetid: 942e4b90-9c41-4e9c-a6f3-80b3afe57f2f
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 275a8228ecfa9168b56b694e7155baef0135aacc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="union-all-transformation"></a>transformation d'union totale
  La transformation d'union totale combine plusieurs entrées en une seule sortie. Par exemple, les sorties de cinq sources de fichier plat peuvent être des entrées de la transformation d'union totale et être combinées en une seule sortie.  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Les entrées de transformation sont ajoutées à la sortie de transformation une par une ; aucune réorganisation de lignes n'intervient. Si le package nécessite une sortie triée, vous devez utiliser la transformation de fusion au lieu de la transformation d'union totale.  
  
 La première entrée que vous connectez à la transformation d'union totale est l'entrée à partir de laquelle la transformation crée la sortie de transformation. Les colonnes des entrées que vous connectez ensuite à la transformation sont mappées avec les colonnes de la sortie de transformation.  
  
 Pour fusionner les entrées, vous mappez les colonnes des entrées avec les colonnes de la sortie. Une colonne d'au moins une entrée doit être mappée avec chaque colonne de sortie. Vous ne pouvez mapper deux colonnes que si leurs métadonnées correspondent. Par exemple, les colonnes mappées doivent avoir le même type de données.  
  
 Si les colonnes mappées contiennent des données de chaîne et que la longueur de la colonne de sortie est plus courte que celle de la colonne d'entrée, la longueur de la colonne de sortie est automatiquement augmentée de manière à contenir la colonne d'entrée. Les colonnes d'entrée non mappées avec des colonnes de sortie ont la valeur NULL dans les colonnes de sortie.  
  
 Cette transformation a plusieurs entrées et une sortie. Elle ne prend pas en charge de sortie d'erreur.  
  
## <a name="configuration-of-the-union-all-transformation"></a>Configuration de la transformation d'union totale  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir par programmation, consultez [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796).  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="union-all-transformation-editor"></a>Éditeur de transformation d'union totale
  La boîte de dialogue **Éditeur de transformation d'union totale** permet de fusionner plusieurs jeux de lignes d'entrée dans un seul jeu de lignes de sortie. En incluant la transformation d'union totale dans un flux de données, vous pouvez fusionner les données de plusieurs flux, créer des datasets complexes en imbriquant les transformations d'union totale et en refusionnant les lignes après avoir corrigé les erreurs contenues dans les données.  
  
### <a name="options"></a>Options  
 **Nom de colonne de sortie**  
 Permet de saisir un alias pour chaque colonne. Par défaut, il s'agit du nom de la colonne d'entrée provenant de la première entrée (de référence). Vous pouvez toutefois choisir un nom unique descriptif.  
  
 **Entrée 1 d'union totale**  
 Sélectionnez cette option dans la liste des colonnes d'entrée disponibles dans la première entrée (de référence). Les métadonnées des colonnes mappées doivent correspondre.  
  
 **Entrée n d'union totale**  
 Sélectionnez cette option dans la liste des colonnes d'entrée disponibles dans à partir de la deuxième entrée. Les métadonnées des colonnes mappées doivent correspondre.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Fusionner des données à l'aide de la transformation d'union totale](../../../integration-services/data-flow/transformations/merge-data-by-using-the-union-all-transformation.md)  
  
  
