---
title: Tables imbriquées (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 35444ae17ac4a11bd0321e70631f45d84273e0af
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="nested-tables-analysis-services---data-mining"></a>Tables imbriquées (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], les données doivent être fournies à un algorithme d’exploration de données sous la forme d’une série de cas inclus dans une table de cas. Toutefois, tous les cas ne peuvent pas être décrits par une ligne de données unique. Par exemple, un cas peut être dérivé de deux tables : une qui contient des informations sur les clients et une autre qui contient les achats des clients. Un client unique présent dans la table des informations sur les clients peut avoir plusieurs articles dans la table des achats des clients, ce qui rend difficile la description des données à l'aide d'une seule ligne. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]Fournit une méthode unique pour gérer de tels cas, à l’aide de *tables imbriquées*. Le concept d'une table imbriquée est illustré dans la figure ci-dessous.  
  
 ![Deux tables combinées à l’aide d’une table imbriquée](../../analysis-services/data-mining/media/nested-tables.gif "deux tables combinées à l’aide d’une table imbriquée")  
  
 Dans ce diagramme, la première table, qui correspond à la table parent, contient des informations sur les clients et associe un identificateur unique à chaque client. La deuxième table, la table enfant, contient les achats de chaque client. Les achats figurant dans la table enfant sont liés à la table parent par l’identificateur unique, à savoir la colonne **CustomerKey** . La troisième table du diagramme montre les deux tables associées.  
  
 Une table imbriquée est représentée dans la table de cas sous la forme d’une colonne spéciale dont le type de données est **TABLE**. Pour toute ligne de cas particulière, ce type de colonne contient des lignes sélectionnées à partir de la table enfant qui se rapportent à la table parent.  
  
 Les données d'une table imbriquée peuvent être utilisées pour les prédictions ou en entrée, ou pour les deux. Par exemple, vous pouvez avoir deux colonnes de table imbriquée dans un modèle : une colonne de table imbriquée peut contenir une liste des produits achetés par un client, l'autre colonne contenant des informations sur les passe-temps favoris du client, informations obtenues à partir d'une étude. Dans ce scénario, vous pouvez utiliser les passe-temps favoris du client comme entrée pour analyser son comportement d'achat et prédire ses achats probables.  
  
## <a name="joining-case-tables-and-nested-tables"></a>Jointure de tables de cas et de tables imbriquées  
 Pour créer une table imbriquée, les deux tables sources doivent contenir une relation définie afin que les éléments d'une table puissent être liés à l'autre table. Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez définir cette relation dans la vue de source de données.  
  
> [!NOTE]  
>  Le champ **CustomerKey** est la clé relationnelle utilisée pour lier la table de cas et la table imbriquée dans la définition de la vue de source de données, et pour établir la relation des colonnes dans la structure d’exploration de données. Toutefois, il est déconseillé d'utiliser cette clé relationnelle dans les modèles d'exploration de données reposant sur cette structure. En général, il vaut mieux omettre la colonne clé relationnelle du modèle d'exploration de données si elle sert uniquement à joindre les tables et qu'elle ne fournit pas d'informations utiles dans le cadre de l'analyse.  
  
 Vous pouvez créer des tables imbriquées par programmation en utilisant le langage DMX (Data Mining Extensions) ou des objets AMO (Analysis Management Objects), ou en utilisant l'Assistant Exploration de données et le Concepteur d'exploration de données dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
## <a name="using-nested-table-columns-in-a-mining-model"></a>Utilisation de colonnes de table imbriquée dans un modèle d'exploration de données  
 Dans la table de cas, la clé est souvent un ID de client, un nom de produit ou une date dans une série, c'est-à-dire des données qui identifient de façon unique une ligne dans la table. . Toutefois, dans les tables imbriquées, la clé n'est pas en général la clé relationnelle (ou clé étrangère), mais plutôt la colonne qui représente l'attribut que vous modélisez.  
  
 Par exemple, si la table de cas contient des commandes et que la table imbriquée contient les articles de la commande, vous voudrez sans doute modéliser la relation entre les articles stockés dans la table imbriquée sur plusieurs commandes, qui sont stockées dans la table de cas. Par conséquent, bien que la table imbriquée **Items** soit jointe à la table de cas **Orders** par la clé relationnelle **OrderID**, vous ne devez pas utiliser **OrderID** comme clé de table imbriquée. Au lieu de cela, sélectionnez la colonne **Items** comme clé de table imbriquée, car cette colonne contient les données à modéliser. Dans la plupart des cas, vous pouvez ignorer en toute sécurité **OrderID** dans le modèle d’exploration de données, car la relation entre la table de cas et la table imbriquée a déjà été établie par la définition de la vue de source de données.  
  
 Lorsque vous choisissez une colonne à utiliser comme clé de table imbriquée, vous devez vous assurer que les valeurs dans cette colonne sont uniques pour chaque cas. Par exemple, si la table de cas représente des clients et que la table imbriquée représente des articles achetés par le client, vous devez vérifier qu'aucun article n'est répertorié plus d'une fois par client. Si un client a acheté le même article plusieurs fois, vous pouvez créer une vue différente avec une colonne qui agrège le nombre d'achats pour chaque produit unique.  
  
 La manière dont vous décidez de gérer les valeurs en double dans une table imbriquée dépend du modèle d'exploration de données que vous créez et du problème d'entreprise que vous tentez de résoudre. Dans certains scénarios, vous pouvez ne pas vous préoccuper du nombre de fois où un client a acheté un produit particulier mais plutôt de l'existence d'au moins un achat. Dans d'autres scénarios, la quantité et la séquence des achats peuvent être très importantes.  
  
 Si l'ordre des articles est important, vous pouvez avoir besoin d'une colonne supplémentaire qui indique la séquence. Quand vous utilisez l’algorithme Sequence Clustering pour créer un modèle, vous devez choisir une colonne *séquence clé* supplémentaire pour représenter l’ordre des articles. La colonne de séquence clé est un type spécial de clé imbriquée qui est utilisée uniquement dans les modèles Sequence Clustering et qui nécessite un type de données numérique unique. Par exemple, des entiers et des dates peuvent être utilisés comme colonne de séquence clé, mais toutes les valeurs de séquence doivent être uniques. Outre la colonne de séquence clé, un modèle Sequence Clustering possède également une clé de table imbriquée qui représente l'attribut modélisé, par exemple les produits achetés.  
  
### <a name="using-non-key-nested-columns-from-a-nested-table"></a>Utilisation de colonnes imbriquées non-clés à partir d'une table imbriquée  
 Après avoir défini la jointure entre la table de cas et la table imbriquée, puis choisi une colonne contenant des attributs intéressants et uniques à utiliser comme clé de table imbriquée, vous pouvez inclure d'autres colonnes de la table imbriquée à utiliser comme entrée au modèle. Toutes les colonnes de la table imbriquée peuvent être utilisées pour l'entrée, la prédiction et l'entrée, ou uniquement la prédiction.  
  
 Par exemple, si la table imbriquée contient les colonnes **Product**, **ProductQuantity**et **ProductPrice**, vous pouvez choisir **Product** comme clé de table imbriquée et ajouter **ProductQuantity** à la structure d’exploration de données pour une utilisation comme entrée.  
  
## <a name="filtering-nested-table-data"></a>Filtrage de données de table imbriquée  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous pouvez créer des filtres sur les données utilisées pour effectuer l'apprentissage d'un modèle d'exploration de données ou le tester. Un filtre peut être utilisé pour affecter la composition du modèle, ou pour tester le modèle sur un sous-ensemble de cas. Des filtres peuvent également être appliqués aux tables imbriquées. Toutefois, la syntaxe pouvant être utilisée avec les tables imbriquées présente certaines limites.  
  
 Souvent, lorsque vous appliquez un filtre à une table imbriquée, vous testez l'existence ou la non-existence d'un attribut. Par exemple, vous pouvez appliquer un filtre qui restreint les cas utilisés dans le modèle à ceux ayant une valeur spécifiée dans la table imbriquée. Ou, vous pouvez restreindre les cas utilisés dans le modèle aux clients qui n'ont pas acheté un article particulier.  
  
 Lorsque vous créez des filtres sur une table imbriquée, vous pouvez également utiliser des opérateurs tels que « supérieur à » et « inférieur à ». Par exemple, vous pouvez restreindre les cas utilisés dans le modèle aux clients qui ont acheté au moins n unités du produit cible. La capacité de filtrer les attributs de table imbriquée offre une grande souplesse dans la personnalisation des modèles.  
  
 Pour plus d’informations sur la création et l’utilisation de filtres de modèle, consultez [Filtres pour les modèles d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Les Structures d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)  
  
  
