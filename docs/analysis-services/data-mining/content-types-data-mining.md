---
title: Types (exploration de données) de contenu | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0dcc5840467f039e78c0c4d4b75862bbf78a6a42
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="content-types-data-mining"></a>Types de contenu (Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous pouvez définir à la fois le type de données physique d’une colonne dans une structure d’exploration de données et un type de contenu logique pour la colonne quand elle est utilisée dans un modèle.  
  
 Le *type de données* détermine la façon dont les algorithmes traitent les données dans ces colonnes quand vous créez des modèles d’exploration de données. La définition du type de données d'une colonne donne les informations d'algorithme relatives au type de données des colonnes, ainsi que la façon de traiter les données. Chaque type de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge un ou plusieurs types de contenu pour l'exploration de données.  
  
 Le *type de contenu* décrit le comportement du contenu que contient la colonne. Par exemple, si le contenu d'une colonne se répète selon un intervalle de temps spécifique, tel que les jours de la semaine, vous pouvez définir le type de contenu de cette colonne comme étant cyclique.  
  
 Certains algorithmes nécessitent des types de données et des types de contenu spécifiques pour pouvoir fonctionner correctement. Par exemple, l'algorithme MNB (Microsoft Naive Bayes) ne peut pas utiliser de colonnes continues en tant qu'entrée, ni prédire de valeurs continues. Certains types de contenu, tels que la séquence clé, sont utilisés uniquement par un algorithme spécifique. Pour obtenir la liste des algorithmes et des types de contenu pris en charge par chacun, consultez [Algorithmes d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md).  
  
 La liste suivante décrit les types de contenu utilisés dans le cadre de l'exploration de données et identifie les types de données qui prennent en charge chacun de ces types.  
  
## <a name="discrete"></a>Discret  
 Le terme*Discret* signifie que la colonne contient un nombre fini de valeurs sans spectre entre les valeurs. Une colonne Sexe (homme/femme) est un exemple classique de colonne d'attributs discrète, en ce sens que les données représentent un nombre spécifique de catégories.  
  
 Les valeurs dans une colonne d'attributs discrète ne peuvent pas impliquer un classement, même si les valeurs sont numériques. De plus, même si les valeurs utilisées pour la colonne discrète sont numériques, le calcul de valeurs fractionnaires est impossible. Les indicatifs téléphoniques sont un bon exemple de données discrètes numériques.  
  
 Le type de contenu **Discret** est pris en charge par tous les types de données d’exploration de données.  
  
## <a name="continuous"></a>Continu  
 Le terme*continu* signifie que la colonne contient des valeurs qui représentent des données numériques sur une échelle qui autorise des valeurs temporaires. À la différence d'une colonne discrète, qui représente des données finies et dénombrables, une colonne continue représente des mesures évolutives et les données peuvent contenir un nombre infini de valeurs fractionnaires. Une colonne de températures est un exemple de colonne d'attributs continue.  
  
 Lorsqu'une colonne contient les données numériques continues, et que vous savez comment les données doivent être distribuées, vous pouvez potentiellement améliorer l'exactitude de l'analyse en spécifiant la distribution attendue des valeurs. Vous spécifiez la distribution des colonnes au niveau de la structure d'exploration de données. Ainsi, le paramètre s’applique à tous les modèles basés sur la structure. Pour plus d’informations, consultez [Distributions de colonnes &#40;exploration de données&#41;](../../analysis-services/data-mining/column-distributions-data-mining.md).  
  
 Le type de contenu **Continu** est pris en charge par les types de données suivants : **Date**, **Double**et **Long**.  
  
## <a name="discretized"></a>Discrétisé  
 La*discrétisation* est le processus consistant à mettre les valeurs d’un jeu continu de données dans des compartiments pour obtenir un nombre limité de valeurs possibles. Seules des données numériques peuvent être discrétisées.  
  
 Ainsi, le type de données *discrétisé* (Discretized) indique que la colonne contient des valeurs qui représentent des groupes, ou compartiments, de valeurs dérivés d’une colonne continue. Les compartiments sont traités comme des valeurs discrètes et ordonnées.  
  
 Vous pouvez discrétiser vos données manuellement pour vérifier que vous obtenez bien les compartiments désirés, ou vous pouvez utiliser les méthodes de discrétisation fournies dans SQL Server Analysis Services. Certains algorithmes effectuent automatiquement la discrétisation. Pour plus d’informations, consultez [Modifier la discrétisation d’une colonne dans un modèle d’exploration de données](../../analysis-services/data-mining/change-the-discretization-of-a-column-in-a-mining-model.md).  
  
 Le type de contenu **Discrétisé** est pris en charge par les types de données suivants : **Date**, **Double**, **Long**et **Text**.  
  
## <a name="key"></a>Key  
 Le type de contenu *clé* (Key) signifie que la colonne identifie de façon unique une ligne. Dans une table de cas, la colonne clé est généralement un identificateur numérique ou texte. Vous attribuez la valeur **key** au type de contenu pour indiquer que la colonne doit uniquement servir au suivi des enregistrements, et non à des fins d’analyse.  
  
 Les tables imbriquées ont également des clés, mais l'utilisation de la clé de table imbriquée diffère quelque peu. Vous attribuez la valeur **key** au type de contenu dans une table imbriquée si la colonne est l’attribut à analyser. Les valeurs dans la clé de table imbriquée doivent être uniques pour chaque cas, mais il peut y avoir des doublons sur tout le jeu de cas.  
  
 Par exemple, si vous analysez les produits qu’achètent les clients, vous attribuez la valeur key au type de contenu de la colonne **CustomerID** dans la table de cas et de la colonne **PurchasedProducts** dans la table imbriquée.  
  
> [!NOTE]  
>  Les tables imbriquées sont disponibles uniquement si vous utilisez les données d’une source de données externe ayant été définie comme vue de source de données Analysis Services.  
  
 Ce type de contenu est pris en charge par les types de données suivants : **Date**, **Double**, **Long**et **Text**.  
  
## <a name="key-sequence"></a>Séquence clé  
 Le type de contenu *séquence clé* (key sequence) ne peut être utilisé que dans des modèles Sequence Clustering. En attribuant la valeur **key sequence**au type de contenu, vous indiquez que la colonne contient des valeurs qui représentent une séquence d’événements. Les valeurs sont ordonnées, mais elles n'ont pas besoin d'être séparées par une distance égale.  
  
 Ce type de contenu est pris en charge par les types de données suivants : **Double**, **Long**, **Text**et **Date**.  
  
## <a name="key-time"></a>Temps clé  
 Le type de contenu *temps clé* (key time) ne peut être utilisé que dans les modèles de série chronologique. En attribuant la valeur **key time**au type de contenu, vous indiquez que les valeurs sont ordonnées et qu’elles représentent une échelle de temps.  
  
 Ce type de contenu est pris en charge par les types de données suivants : **Double**, **Long**et **Date**.  
  
## <a name="table"></a>Table  
 Le type de contenu *table* indique que la colonne contient une autre table de données, comprenant une ou plusieurs colonnes et une ou plusieurs lignes. Pour toute ligne particulière de la table de cas, cette colonne peut contenir plusieurs valeurs qui sont toutes associées à l'enregistrement de cas parent. Par exemple, si la table de cas principale contient une liste de clients, vous pouvez avoir plusieurs colonnes qui contiennent des tables imbriquées, telles qu’une colonne **ProductsPurchased** dans laquelle la table imbriquée répertorie les produits précédemment achetés par ce client, et une colonne **Hobbies** qui répertorie les centres d’intérêt du client.  
  
 Le type de données de cette colonne est toujours **Table**.  
  
## <a name="cyclical"></a>Cyclique  
 Le type de contenu *cyclique* (cyclical) signifie que la colonne contient des valeurs qui représentent un jeu ordonné cyclique. Par exemple, les jours numérotés de la semaine constituent un jeu ordonné cyclique car le jour numéro un suit le jour numéro sept.  
  
 Les colonnes cycliques sont considérées comme ordonnées et discrètes en termes de type de contenu.  
  
 Ce type de contenu est pris en charge par tous les types de données d’exploration de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Toutefois, la plupart des algorithmes considèrent les valeurs cycliques comme des valeurs discrètes et n'effectuent pas de traitement spécial.  
  
## <a name="ordered"></a>Ordonné  
 Le type de contenu *ordonné* (Ordered) indique également que la colonne contient des valeurs définissant une séquence ou un ordre. Toutefois, dans ce type de contenu, les valeurs utilisées pour le classement n'impliquent aucune relation de distance ou de grandeur entre les valeurs du jeu. Par exemple, si une colonne d'attributs ordonnée contient des informations sur des niveaux de compétence classés de un à cinq, ceci n'implique pas une relation de distance entre les niveaux de compétence ; un niveau de compétence de valeur cinq n'est pas forcément cinq fois meilleur qu'un niveau de compétence de valeur un.  
  
 Les colonnes d'attributs ordonnées sont considérées comme discrètes en termes de type de contenu.  
  
 Ce type de contenu est pris en charge par tous les types de données d’exploration de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Toutefois, la plupart des algorithmes considèrent les valeurs ordonnées comme des valeurs discrètes et n'effectuent pas de traitement spécial.  
  
## <a name="classified"></a>Classifié  
 En plus des types de contenu susmentionnés qui sont couramment utilisés avec tous les modèles, vous pouvez utiliser des colonnes classifiées pour définir les types de contenu de certains types de données. Pour plus d’informations sur les colonnes classifiées, consultez [Colonnes classifiées &#40;exploration de données&#41;](../../analysis-services/data-mining/classified-columns-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu des Types de & #40 ; DMX & #41 ;](../../dmx/content-types-dmx.md)   
 [Types de données & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/data-types-data-mining.md)   
 [Types de données & #40 ; DMX & #41 ;](../../dmx/data-types-dmx.md)   
 [Modifier les propriétés d’une Structure d’exploration de données](../../analysis-services/data-mining/change-the-properties-of-a-mining-structure.md)   
 [Colonnes de Structure d’exploration de données](../../analysis-services/data-mining/mining-structure-columns.md)  
  
  
