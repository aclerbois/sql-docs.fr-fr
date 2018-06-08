---
title: Tri, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
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
- sql13.dts.designer.sorttrans.f1
- sql13.dts.designer.sorttransformation.f1
helpviewer_keywords:
- Sort transformation
- descending sorts
- ascending sorts
- sorting data [Integration Services]
- multiple sorts
- duplicate data [Integration Services]
ms.assetid: 728c9351-84a8-4a89-be4d-d50d4adc04e0
caps.latest.revision: 50
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a20feb9d15a24ea417a715816e3b437e13d0ca2c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sort-transformation"></a>transformation de tri
  La transformation de tri trie les données d'entrée dans l'ordre croissant ou décroissant et copie les données triées dans sa sortie. Vous pouvez appliquer plusieurs tris à une entrée ; chaque tri est identifié par un chiffre qui détermine l'ordre de tri. La colonne qui possède le plus petit nombre est triée en premier, puis la colonne de tri ayant le deuxième plus petit nombre, et ainsi de suite. Par exemple, si une colonne nommée **PaysRégion** a un ordre de tri égal à 1 et qu’une colonne nommée **Ville** a un ordre de tri égal à 2, la sortie est triée par pays/région puis par ville. Un nombre positif indique que le tri est croissant, tandis qu'un nombre négatif indique qu'il est décroissant. Les colonnes qui ne sont pas triées ont un ordre de tri égal à 0. Les colonnes qui ne sont pas sélectionnées pour le tri sont automatiquement copiées dans la sortie de la transformation avec les colonnes triées.  
  
 La transformation de tri comprend un ensemble d'options de comparaison qui permettent de définir la façon dont la transformation gère les données de chaîne dans une colonne. Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).  
  
> [!NOTE]  
>  La transformation de tri ne trie pas les GUID dans le même ordre que la clause ORDER BY dans Transact-SQL. Alors que la transformation de tri trie les GUID commençant par un numéro compris entre 0 et 9 avant les GUID commençant par une lettre comprise entre A et F, la clause ORDER BY, telle qu'elle est implémentée dans le [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)], les trie différemment. Pour plus d’informations, consultez [Clause ORDER BY &#40;Transact-SQL&#41;](../../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
 La transformation de tri peut également supprimer les doublons de lignes dans le cadre du tri. Les doublons de lignes sont des lignes possédant les mêmes valeurs de clé de tri. La valeur de clé de tri est générée en fonction des options de comparaison de chaînes en cours d'utilisation ; par conséquent, différentes chaînes littérales peuvent avoir les mêmes valeurs de clé de tri. Dans les colonnes d'entrée, la transformation identifie en tant que doublons les lignes qui ont des valeurs différentes mais la même clé de tri.  
  
 La transformation de tri inclut la propriété personnalisée **MaximumThreads**, qui peut être mise à jour par une expression de propriété pendant le chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41; ](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Utiliser des expressions de propriété dans les packages](../../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Cette transformation a une entrée et une sortie. Elle ne prend pas en charge les sorties d'erreur.  
  
## <a name="configuration-of-the-sort-transformation"></a>Configuration de la transformation de tri  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programme.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition des propriétés du composant, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="related-content"></a>Contenu associé  
 Exemple, [SortDeDuplicateDelimitedString Custom SSIS Component](http://go.microsoft.com/fwlink/?LinkId=220821), sur codeplex.com.  
  
## <a name="sort-transformation-editor"></a>Éditeur de transformation de tri
  Utilisez la boîte de dialogue **Éditeur de transformation de tri** pour sélectionner les colonnes à trier, définir l'ordre de tri et indiquer si les doublons sont supprimés.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Définissez les colonnes à trier en utilisant les cases à cocher.  
  
 **Nom**  
 Affiche le nom de chaque colonne d'entrée disponible.  
  
 **Relais**  
 Indique s'il faut inclure la colonne dans la sortie triée.  
  
 **Colonne d'entrée**  
 Sélectionnez dans la liste le nom d'une colonne d'entrée pour chaque ligne. Vos sélections se reflètent dans les sélections des cases à cocher de la table **Colonnes d'entrée disponibles** .  
  
 **Alias de sortie**  
 Permet de saisir un alias pour chaque colonne de sortie. Par défaut, il s'agit du nom de la colonne d'entrée ; vous pouvez néanmoins choisir un nom unique et descriptif.  
  
 **Type de tri**  
 Indiquez si vous voulez effectuer un tri croissant ou décroissant.  
  
 **Ordre de tri**  
 Indiquez l'ordre de tri des colonnes. Vous devez le faire manuellement pour chaque colonne.  
  
 **Indicateurs de comparaison**  
 Pour plus d’informations sur les options de comparaison de chaînes, consultez [Comparaison des données chaînes](../../../integration-services/data-flow/comparing-string-data.md).  
  
 **Supprimer les lignes avec des valeurs de tri en double**  
 Indiquez si la transformation copie les lignes en double dans la sortie de transformation ou crée une seule entrée pour tous les doublons en fonction des options de comparaison de chaînes définies.  
  
## <a name="see-also"></a> Voir aussi  
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
