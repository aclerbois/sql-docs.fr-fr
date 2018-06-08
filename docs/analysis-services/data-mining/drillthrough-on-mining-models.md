---
title: L’extraction sur les modèles d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2abd3430c1882148cd564c973ecf5824f3a55d4a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="drillthrough-on-mining-models"></a>Extraction sur des modèles d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  *L'extraction* désigne la capacité d'interroger un modèle d'exploration de données ou une structure d'exploration de données pour obtenir des informations détaillées qui ne sont pas exposées dans le modèle.  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]fournit deux options différentes pour l’extraction des données de cas. Vous pouvez extraire les cas utilisés pour générer les données ou les cas de la structure d'exploration de données.  
  
## <a name="drillthrough-to-model-cases-vs-drillthrough-to-structure"></a>Extraction dans des cas de modèle et dans la structure  
 L'extraction dans des **cas de modèle** est utile pour rechercher des informations supplémentaires sur les règles, les modèles ou les clusters dans un modèle. Par exemple, vous n'utiliseriez pas les informations de contact client pour une analyse dans un modèle de clustering, même si les données étaient disponibles, mais à l'aide de l'extraction, vous pouvez accéder à ces informations à partir du modèle.  
  
 Par opposition, **l'extraction dans les données de la structure** est conçue pour fournir un accès aux informations qui n'ont pas été mises à disposition dans le modèle. Par exemple, certaines colonnes de structure peuvent avoir été exclues d'un modèle, car le type de données était incompatible ou les données ne présentaient aucune utilité pour l'analyse.  
  
## <a name="enabling-drillthrough-on-a-model"></a>Activation de l'extraction sur un modèle  
 Pour utiliser l'extraction sur un modèle d'exploration de données, les conditions suivantes doivent être remplies :  
  
-   Il est possible de configurer l'extraction sur les cas du modèle uniquement et pas sur la structure d'exploration de données, mais pas vice versa.  En d'autres termes, l'extraction doit être activée sur le modèle d'exploration de données pour autoriser l'extraction à la structure d'exploration de données.  
  
-   L'extraction sur le modèle et la structure est désactivée par défaut. Si vous utilisez l'Assistant Exploration de données, l'option d'activation de l'extraction vers les cas de modèles figure sur la dernière page de l'Assistant.  
  
-   Vous pouvez ajouter la capacité d'extraire un modèle existant d'exploration de données, mais si vous procédez ainsi, le modèle doit être retraité avant que vous ne puissiez extraire les données.  
  
-   L'extraction ne fonctionnera pas à moins que le cache créé au cours du processus d'apprentissage n'ait été conservé. Pour plus d’informations sur les propriétés qui contrôlent la mise en cache, consultez [Extraction sur des structures d’exploration de données](../../analysis-services/data-mining/drillthrough-on-mining-structures.md).  
  
## <a name="models-that-support-drillthrough"></a>Modèles qui prennent en charge l'extraction  
 Si un modèle d'exploration de données a été configuré pour autoriser l'extraction, et si vous disposez des autorisations appropriées, lorsque vous parcourez le modèle, vous pouvez cliquer sur un nœud dans la visionneuse appropriée et récupérer des informations détaillées sur les cas de ce nœud particulier.  
  
 Les modèles ne prennent pas tous en charge l'extraction ; cela dépend de l'algorithme qui a été utilisé pour créer le modèle. Le tableau suivant répertorie les types de modèles qui ne prennent pas en charge l'extraction, ou qui la prennent en charge sous certaines conditions. Si le type de modèle n'est pas répertorié ici, l'extraction est prise en charge.  
  
|**Nom de l'algorithme**|**Prise en charge de l’extraction**|  
|------------------------|----------------------------------|  
|Algorithme MNB (Microsoft Naive Bayes)|Non pris en charge.<br /><br /> Ces algorithmes n'assignent pas de cas aux nœuds spécifiques du contenu.|  
|Algorithme MNN (Microsoft Neural Network)|Non pris en charge.<br /><br /> Ces algorithmes n'assignent pas de cas aux nœuds spécifiques du contenu.|  
|Algorithme MLR (Microsoft Logistic Regression)|Non pris en charge.<br /><br /> Ces algorithmes n'assignent pas de cas aux nœuds spécifiques du contenu.|  
|Algorithme MLR (Microsoft Linear Regression)|Pris en charge.<br /><br /> Toutefois, comme le modèle crée un nœud unique, **All**, l'extraction retourne tous les cas d'apprentissage pour le modèle. Si le jeu d'apprentissage est volumineux, le chargement des résultats peut durer plusieurs minutes.|  
|Algorithme MTS (Microsoft Time Series)|Pris en charge.<br /><br /> Toutefois, vous ne pouvez pas extraire les données de structure ou de cas en utilisant la **Visionneuse de modèle d'exploration de données** dans le Concepteur de modèle d'exploration de données. Vous devez créer à la place une requête DMX.<br /><br /> De même, vous ne pouvez pas extraire des nœuds spécifiques ni écrire une requête DMX pour récupérer les cas de nœuds spécifiques d'un modèle de série chronologique. Vous pouvez récupérer les données de cas depuis le modèle ou la structure en utilisant d'autres critères, comme les valeurs de date ou d'attribut.<br /><br /> Si vous voulez consulter les détails des nœuds ARTXP et ARIMA créés par l’algorithme MTS (Microsoft Time Series), il vous sera peut-être plus simple d’utiliser la [Visionneuse de l’arborescence de contenu générique Microsoft &#40;exploration de données&#41;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).|  
  
## <a name="related-tasks"></a>Tâches associées  
 Consultez les rubriques suivantes pour plus d'informations sur l'utilisation de l'extraction avec des modèles d'exploration de données.  
  
|Tâches|Liens|  
|-----------|-----------|  
|Utiliser l'extraction dans les visionneuses de modèle d'exploration de données|[Utiliser l’extraction des visionneuses de modèle](../../analysis-services/data-mining/use-drillthrough-from-the-model-viewers.md)|  
|Récupérer les données de cas pour un modèle à l'aide de l'extraction|[Extraction des données de cas à partir d’un modèle d’exploration de données](../../analysis-services/data-mining/drill-through-to-case-data-from-a-mining-model.md)|  
|Activer l'extraction sur un modèle d'exploration de données existant|[Activer l’extraction pour un modèle d’exploration de données](../../analysis-services/data-mining/enable-drillthrough-for-a-mining-model.md)|  
|Consulter les exemples de requêtes d'extraction pour les types de modèles spécifiques.|[Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)|  
|Activer l'extraction dans l'Assistant Modèle d'exploration de données|[Fin de l’Assistant &#40;Assistant Exploration de données&#41;](http://msdn.microsoft.com/library/6aef1548-35eb-42fd-ae87-63650a79eda1).|  
  
## <a name="see-also"></a>Voir aussi  
 [Extraction sur les Structures d’exploration de données](../../analysis-services/data-mining/drillthrough-on-mining-structures.md)  
  
  
