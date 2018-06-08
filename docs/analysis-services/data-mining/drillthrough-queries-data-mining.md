---
title: Les requêtes d’extraction (exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e8b3afda19fbbf084223d0f597fe2893dce17049
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="drillthrough-queries-data-mining"></a>Requêtes d'extraction (exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une *requête d'extraction* vous permet de récupérer les détails des cas ou des données de structure sous-jacents, en envoyant une requête au modèle d'exploration de données. L'extraction est utile si vous souhaitez consulter les cas utilisés pour l'apprentissage du modèle, par opposition à ceux utilisés pour tester le modèle, ou si vous souhaitez consulter des détails supplémentaires à partir des données de cas.  
  
 L'exploration de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit deux options différentes pour l'extraction :  
  
-   Extraction des **cas du modèle**  
  
     L'extraction des cas du modèle est utilisée lorsque vous souhaitez partir d'un schéma spécifique dans le modèle, tel qu'un cluster ou une branche d'un arbre de décision, et afficher des détails sur les cas individuels.  
  
-   Extraction des **cas de structure**  
  
     L'extraction des cas de structure est utilisée lorsque la structure contient des informations qui peuvent ne pas être disponibles dans le modèle. Par exemple, vous n'utiliserez pas les informations de contact client dans un modèle de clustering, même si les données ont été incluses dans la structure. Toutefois, après avoir créé le modèle, vous pouvez souhaiter extraire les informations de contact pour les clients regroupés dans un cluster donné.  
  
 Cette section fournit des exemples de la manière de créer ces requêtes.  
  
 [Utilisation de l'extraction dans le concepteur d'exploration de données](#bkmk_Designer)  
  
 [Création de requêtes d'extraction à l'aide de DMX](#bkmk_DMX)  
  
 [Considérations sur l'utilisation de l'extraction](#bkmk_Considerations)  
  
-   [Problèmes de sécurité](#bkmk_Security)  
  
-   [Limitations](#bkmk_Limits)  
  
##  <a name="bkmk_Designer"></a> Utilisation de l'extraction dans le concepteur d'exploration de données  
 Si un modèle d'exploration de données a été configuré pour autoriser l'extraction, et si vous disposez des autorisations appropriées, lorsque vous parcourez le modèle, vous pouvez cliquer sur un nœud dans la visionneuse appropriée et récupérer des informations détaillées sur les cas de ce nœud particulier.  
  
 [Extraire des données de cas à partir d’un modèle d’exploration de données](../../analysis-services/data-mining/drill-through-to-case-data-from-a-mining-model.md).  
  
 Si les cas d'apprentissage ont été mis en cache lorsque vous avez traité la structure d'exploration de données et si vous disposez des autorisations nécessaires, vous pouvez retourner des informations des cas des modèles et de la structure d'exploration de données, y compris les colonnes non incluses dans le modèle d'exploration de données.  
  
##  <a name="bkmk_DMX"></a> Création de requêtes d'extraction à l'aide de DMX  
 Vous pouvez extraire des données de cas en créant une requête DMX, si vous disposez des autorisations sur le modèle ou sur la structure. Pour des exemples de syntaxe pour la création de requêtes d'extraction dans DMX, consultez la rubrique suivante :  
  
 [Créer des requêtes d’extraction à l’aide de DMX](../../analysis-services/data-mining/create-drillthrough-queries-using-dmx.md)  
  
##  <a name="bkmk_Considerations"></a> Considérations sur l'utilisation de l'extraction  
  
-   Si vous utilisez l'Assistant Exploration de données, l'option d'activation de l'extraction vers les cas de modèles figure sur la dernière page de l'Assistant. L'extraction est désactivée par défaut. Pour plus d’informations, consultez [Fin de l’Assistant &#40;Assistant Exploration de données&#41;](http://msdn.microsoft.com/library/6aef1548-35eb-42fd-ae87-63650a79eda1).  
  
-   Vous pouvez ajouter la capacité d'extraire un modèle existant d'exploration de données, mais si vous procédez ainsi, le modèle doit être retraité avant que vous ne puissiez extraire les données.  
  
-   Le principe de l'extraction consiste à extraire des informations sur les cas d'apprentissage mis en cache lorsque vous avez traité la structure d'exploration de données. Par conséquent, si vous avez effacé les données en cache après avoir traité la structure en modifiant la propriété <xref:Microsoft.AnalysisServices.MiningStructureCacheMode> en **ClearAfterProcessing**, l’extraction ne fonctionne pas. Pour activer l’extraction vers des colonnes de structure, vous devez modifier la propriété <xref:Microsoft.AnalysisServices.MiningStructureCacheMode> en **KeepTrainingCases** , puis retraiter la structure.  
  
-   Si, contrairement au modèle d'exploration de données, la structure d'exploration de données ne vous autorise pas à extraire les données sous-jacentes, vous pouvez n'afficher les informations que des cas de modèle, mais non de la structure d'exploration de données.  
  
###  <a name="bkmk_Security"></a> Problèmes de sécurité pour l'extraction  
 Si vous souhaitez extraire les cas de structure du modèle, vous devez vérifier que la propriété [AllowDrillThrough](../../analysis-services/scripting/properties/allowdrillthrough-element-assl.md) de la structure d'exploration de données et du modèle d'exploration de données possède la valeur **True**. De plus, vous devez être membre d'un rôle ayant les autorisations d'extraction sur la structure et le modèle. Pour plus d’informations sur la façon de créer des rôles, consultez [Concepteur de rôle &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/e8ba42db-0565-4d68-b3ab-0c63d8d07192). consultez.  
  
 Les autorisations d'extraction sont définies séparément sur la structure et le modèle. L'autorisation de modèle permet d'effectuer une extraction à partir du modèle, même si vous n'avez pas d'autorisations sur la structure. Les autorisations d’extraction sur la structure permettent en outre d’inclure des colonnes de structure dans les requêtes d’extraction à partir du modèle, à l’aide de la fonction [StructureColumn &#40;DMX&#41;](../../dmx/structurecolumn-dmx.md).  
  
> [!NOTE]  
>  Si vous activez l'extraction à la fois sur la structure d'exploration de données et le modèle d'exploration de données, tout utilisateur membre d'un rôle ayant les autorisations d'extraction sur le modèle d'exploration de données peut également consulter les colonnes de la structure d'exploration de données, même si ces colonnes ne sont pas incluses dans le modèle d'exploration de données. Par conséquent, afin de protéger les informations sensibles, vous devez configurer la vue de la source de données de façon à masquer les informations personnelles et à n'autoriser l'accès en extraction sur la structure d'exploration de données qu'en cas de nécessité.  
  
###  <a name="bkmk_Limits"></a> Limitations sur l'extraction  
  
-   Les limitations suivantes s'appliquent aux opérations d'extraction sur un modèle, selon l'algorithme utilisé pour créer le modèle :  
  
|Nom de l'algorithme|Problème|  
|--------------------|-----------|  
|Algorithme MNB (Microsoft Naive Bayes)|Non pris en charge. Ces algorithmes n'assignent pas de cas aux nœuds spécifiques du contenu.|  
|Algorithme MNN (Microsoft Neural Network)|Non pris en charge. Ces algorithmes n'assignent pas de cas aux nœuds spécifiques du contenu.|  
|Algorithme MLR (Microsoft Logistic Regression)|Non pris en charge. Ces algorithmes n'assignent pas de cas aux nœuds spécifiques du contenu.|  
|Algorithme MLR (Microsoft Linear Regression)|Pris en charge. Toutefois, comme le modèle crée un nœud unique, **All**, l'extraction retourne tous les cas d'apprentissage pour le modèle. Si le jeu d'apprentissage est volumineux, le chargement des résultats peut durer plusieurs minutes.|  
|Algorithme MTS (Microsoft Time Series)|Pris en charge. Toutefois, vous ne pouvez pas extraire les données de structure ou de cas en utilisant la **Visionneuse de modèle d'exploration de données** dans le Concepteur de modèle d'exploration de données. Vous devez créer à la place une requête DMX.<br /><br /> De même, vous ne pouvez pas extraire des nœuds spécifiques ni écrire une requête DMX pour récupérer les cas de nœuds spécifiques d'un modèle de série chronologique. Vous pouvez récupérer les données de cas depuis le modèle ou la structure en utilisant d'autres critères, comme les valeurs de date ou d'attribut.<br /><br /> Vous pouvez également retourner les dates des cas du modèle, à l’aide de la fonction [Lag &#40;DMX&#41;](../../dmx/lag-dmx.md).<br /><br /> Si vous souhaitez consulter les détails des nœuds ARTXP et ARIMA créés par l’algorithme MTS (Microsoft Time Series), vous pouvez utiliser la [Visionneuse de l’arborescence de contenu générique Microsoft &#40;exploration de données&#41;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).|  
  
##  <a name="bkmk_Tasks"></a> Tâches associées  
 Utilisez les liens suivants pour utiliser l'extraction dans des scénarios spécifiques.  
  
|Tâche|Lien|  
|----------|----------|  
|Procédure qui décrit l'utilisation de l'extraction dans le Concepteur d'exploration de données|[Extraction des données de cas à partir d’un modèle d’exploration de données](../../analysis-services/data-mining/drill-through-to-case-data-from-a-mining-model.md)|  
|Pour modifier un modèle d'exploration de données existant afin d'autoriser l'extraction|[Activer l’extraction pour un modèle d’exploration de données](../../analysis-services/data-mining/enable-drillthrough-for-a-mining-model.md)|  
|Activation de l'extraction sur une structure d'exploration de données à l'aide de la clause DMX WITH DRILLTHROUGH|[CRÉER UNE STRUCTURE D’EXPLORATION DE DONNÉES & #40 ; DMX & #41 ;](../../dmx/create-mining-structure-dmx.md)|  
|Pour plus d'informations sur l'assignation d'autorisations qui s'appliquent à l'extraction sur des structures d'exploration de données et des modèles d'exploration de données|[Accorder des autorisations sur les structures d’exploration de données et modèles & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-permissions-on-data-mining-structures-and-models-analysis-services.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)   
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)  
  
  
