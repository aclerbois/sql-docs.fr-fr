---
title: Calculs | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 32c87ea7625e0e6894b9fd018eed8067873cf83f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="calculations"></a>calculs
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un calcul est une expression de MDX (Multidimensional Expressions) ou un script qui est utilisé pour définir un membre calculé, d’un jeu nommé ou d’une attribution d’étendue dans un cube [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les calculs vous permettent d'ajouter des objets définis non pas par les données du cube, mais par des expressions qui peuvent référencer d'autres parties du cube, d'autres cubes voire des informations à l'extérieur de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les calculs vous permettent d'étendre les capacités d'un cube, en renforçant la flexibilité et la puissance des applications de décisionnel. Pour plus d’informations sur les calculs de script, consultez [Introduction aux scripts MDX dans Microsoft SQL Server 2005](http://go.microsoft.com/fwlink/?LinkId=81892). Pour plus d’informations sur les problèmes de performances liés aux calculs et des requêtes MDX, consultez le [Guide des performances SQL Server 2005 Analysis Services](http://go.microsoft.com/fwlink/?LinkId=81621).  
  
## <a name="calculated-members"></a>Membres calculés  
 Un membre calculé est un membre dont la valeur est calculée au moment de l'exécution en utilisant une expression MDX (Multidimensional Expressions) que vous spécifiez lorsque vous définissez le membre calculé. Un membre calculé est disponible pour les applications de décisionnel comme n'importe quel autre membre. Les membres calculés n'augmentent pas la taille du cube, car seules les définitions sont stockées dans le cube ; les valeurs sont calculées dans la mémoire lorsqu'il faut répondre à une requête.  
  
 Les membres calculés peuvent être définis pour n'importe quelle dimension, y compris la dimension de mesures. Les membres calculés créés sur la dimension de mesures sont appelés mesures calculées.  
  
 Bien que les membres calculés reposent généralement sur les données qui existent déjà dans le cube, vous pouvez créer des expressions complexes en combinant les données avec des opérateurs arithmétiques, des nombres et des fonctions. Vous pouvez également utiliser des fonctions MDX, telles que LookupCube, pour accéder aux données d'autres cubes de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] inclut des bibliothèques de fonctions Visual Studio standardisées et vous permet d'utiliser des procédures stockées pour récupérer des données à partir d'autres sources que la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] active. Pour plus d’informations sur les procédures stockées, consultez [définition de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md).  
  
 Supposons, par exemple, que les cadres d'une société de transport veuillent déterminer les types de cargaisons les plus rentables à transporter, en fonction du bénéfice par unité de volume. Ils utilisent un cube Shipments qui contient les dimensions Cargo, Fleet et Time et les mesures Price_to_Ship, Cost_to_Ship et Volume_in_Cubic_Meters ; cependant, ce cube ne contient pas de mesure de rentabilité. Vous pouvez créer un membre calculé correspondant à une mesure appelée Profit_per_Cubic_Meter dans le cube en combinant les mesures existantes dans l'expression suivante :  
  
```  
([Measures].[Price_to_Ship] - [Measures].[Cost_to_Ship]) /  
[Measures].[Volume_in_Cubic_Meters]  
```  
  
 Une fois que vous avez créé le membre calculé, Profit_per_Cubic_Meter s'affichera avec les autres mesures au prochain affichage du cube Shipments.  
  
 Pour créer des membres calculés, utilisez le **calcul**onglet dans le Concepteur de Cube. Pour plus d’informations, consultez [créer des membres calculés](../../analysis-services/multidimensional-models/create-calculated-members.md)  
  
## <a name="named-sets"></a>Jeux nommés  
 Un jeu nommé est une expression contenant une instruction MDX CREATE SET qui retourne un jeu. L’expression MDX est enregistrée en tant que partie de la définition d’un cube dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Un jeu nommé est créé afin d'être réutilisé dans les requêtes MDX (Multidimensional Expressions). Un jeu nommé permet aux utilisateurs professionnels de simplifier leurs requêtes et d'utiliser un nom de jeu au lieu d'une expression de jeu pour les expressions de jeu complexes et fréquemment employées. **Rubrique connexe :** [créer des jeux nommés](../../analysis-services/multidimensional-models/create-named-sets.md)  
  
## <a name="script-commands"></a>Commandes de script  
 Une commande de script est un script MDX, intégré dans la définition du cube. Les commandes de script permettent d'effectuer quasiment toute action prise en charge par MDX sur un cube, telle que l'attribution d'étendue d'un calcul à une seule partie du cube. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], des scripts MDX peuvent s’appliquent à l’intégralité du cube ou à des sections spécifiques du cube, à des moments particuliers lors de l’exécution du script. La commande de script par défaut, l'instruction CALCULATE, remplit les cellules du cube avec des données agrégées en fonction de l'étendue par défaut.  
  
 L'étendue par défaut est le cube tout entier, mais vous pouvez définir une étendue plus limitée - que l'on appelle sous-cube - puis appliquer un script MDX seulement à cet espace particulier du cube. L'instruction SCOPE définit l'étendue de toutes les instructions et expressions MDX ultérieures dans le script de calcul jusqu'à ce que l'étendue soit arrêtée ou redéfinie. L'instruction THIS est ensuite utilisée pour appliquer une expression MDX à l'étendue actuelle. Vous pouvez utiliser l'instruction BACK_COLOR pour spécifier une couleur d'arrière-plan de cellule pour les cellules de l'étendue actuelle, afin de faciliter le débogage.  
  
 Par exemple, vous pouvez utiliser une commande de script pour attribuer des quotas de ventes à des employés en fonction du temps et des territoires de ventes en utilisant comme références les valeurs pondérées des ventes dans une période antérieure.  
  
## <a name="see-also"></a>Voir aussi  
 [Calculs dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)  
  
  
