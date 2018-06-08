---
title: Parcourir un modèle à l’aide de la visionneuse d’arborescence de contenu générique Microsoft | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f44c1cb014ca9abcd6122f6db6ee0e33407b7d1d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="browse-a-model-using-the-microsoft-generic-content-tree-viewer"></a>Explorer un modèle à l'aide de la visionneuse de l'arborescence de contenu générique Microsoft
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Generic Mining Model Content Viewer fournit des informations détaillées sur les séquences identifiées par l’algorithme d’exploration de données et donne aussi accès aux différentes statistiques générées pendant le processus d’analyse. Les informations varient en quantité et en type selon l'algorithme utilisé, mais elles peuvent inclure les catégories suivantes :  
  
-   Des segments de données et leurs caractéristiques.  
  
-   Des statistiques descriptives sur chaque de groupe ou sur le jeu entier de données.  
  
-   Le nombre de branches ou de nœuds enfants dans une arborescence.  
  
-   Des calculs, tels que la variance et la moyenne, pour un cluster ou un jeu entier de données.  
  
 Ces informations peuvent vous aider à mieux comprendre les résultats de votre analyse. Vous pouvez aussi identifier comment ajuster votre modèle puis effectuer un nouvel apprentissage, ou bien décider d'effectuer un nouvel apprentissage en utilisant un algorithme différent.  
  
## <a name="viewing-mining-model-content"></a>Affichage du contenu d'un modèle d'exploration de données  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Generic Content Viewer affiche les colonnes, les règles, les propriétés, les attributs, les nœuds et tout autre contenu de *l’ensemble de lignes du schéma de contenu* du modèle d’exploration de données. L'ensemble de lignes de schéma du contenu est une infrastructure générique permettant de présenter des informations détaillées sur le contenu d'un modèle d'exploration de données.  
  
 Ces informations détaillées sont contenues dans une table HTML qui représente les modèles, les clusters ou les arborescences du modèle en tant que nœuds. Vous pouvez cliquer sur chaque nœud et le développer pour obtenir plus de détails, tels que les formules ou le nombre de valeurs distinctes pour un attribut numérique. Vous pouvez également explorer les relations parent/enfant entre les nœuds.  
  
 Pour plus d’informations sur la signification générale des termes utilisés dans le contenu du modèle d’exploration de données, consultez [Contenu du modèle d’exploration &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md). Cette rubrique contient également des liens vers des informations relatives au contenu des modèles d'exploration de données pour des types de modèles spécifiques. Étant donné que chaque type de modèle d'exploration de données contient des informations qui sont très spécifiques à l'algorithme et aux modèles se trouvant dans les données, nous vous recommandons de consulter la rubrique de références techniques pour chaque type de modèle afin de comprendre parfaitement les détails de chaque type de modèle.  
  
## <a name="querying-mining-model-content"></a>Interrogation du contenu d'un modèle d'exploration de données  
 Vous pouvez également accéder aux informations fournies par la visionneuse de l’arborescence de contenu générique [!INCLUDE[msCoName](../../includes/msconame-md.md)] en interrogeant le modèle d’exploration de données. Vous pouvez créer des requêtes sur le contenu du modèle d'exploration de données en utilisant des instructions DMX (Data Mining Extensions). Par exemple, dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez exécuter une requête de contenu en utilisant l’instruction DMX suivante :  
  
```  
SELECT * FROM [<mining model name>].CONTENT  
```  
  
 Pour plus d’informations, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Visionneuse de l’arborescence de contenu générique Microsoft & #40 ; exploration de données & #41 ;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c)   
 [Algorithmes d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)  
  
  
