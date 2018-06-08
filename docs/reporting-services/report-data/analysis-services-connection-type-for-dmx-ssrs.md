---
title: Type de connexion Analysis Services pour DMX (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- parameters [Reporting Services], DMX
- Data Mining Prediction [Reporting Services]
- query view [Reporting Services]
- DMX [Reporting Services]
- design view [Reporting Services]
- data mining [Reporting Services]
- passing parameters [Reporting Services]
ms.assetid: 2de825e9-6d8a-4128-add0-da15dc6cea3e
caps.latest.revision: 64
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9293053b1a3a0043b3573c1efc55f4a825ce78a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="analysis-services-connection-type-for-dmx-ssrs"></a>Type de connexion Analysis Services pour DMX (SSRS)
  Quand vous créez un dataset à l’aide d’une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , le Concepteur de rapports affiche le Concepteur de requêtes MDX (Multidimensional Expression) s’il détecte un cube valide. Si aucun cube n'est détecté, mais qu'un modèle d'exploration de données est disponible, le Concepteur de rapports affiche le Concepteur de requêtes DMX (Data Mining Extensions). Pour basculer entre les concepteurs MDX et DMX, cliquez sur le bouton **Type de commande DMX** (![Basculer vers la vue langage de requête DMX](../../reporting-services/report-data/media/rsqdicon-commandtypedmx.gif "Basculer vers la vue langage de requête DMX")) dans la barre d’outils. Utilisez le Concepteur de requêtes DMX pour créer de manière interactive une requête DMX à l'aide d'éléments graphiques. Pour utiliser le Concepteur de requêtes DMX, la source de données que vous spécifiez doit déjà avoir un modèle d'exploration de données qui fournit les données. Les résultats de requête sont convertis en un jeu de lignes à deux dimensions qui sera utilisé dans le rapport.  
  
> [!NOTE]  
>  Vous devez former le modèle avant de concevoir le rapport. Pour plus d’informations, consultez [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md).  
  
## <a name="design-mode"></a>Mode Création  
 Le Concepteur de requêtes DMX s'ouvre en mode Création. Le mode Création comprend une zone de conception graphique permettant de sélectionner un modèle d'exploration de données et une table d'entrée, ainsi qu'une grille utilisée pour spécifier la requête de prédiction. Il existe deux autres modes dans le Concepteur de requêtes DMX : le mode Requête et le mode Résultat. En mode Requête, la grille du mode Création est remplacée par un volet Requête, qui vous permet de taper des requêtes DMX. En mode Résultat, le jeu de résultats retourné par la requête apparaît dans une grille de données.  
  
 Pour changer de mode dans le Concepteur de requêtes DMX, cliquez avec le bouton droit dans la zone de conception, puis sélectionnez **Création** **Requête**ou **Résultat**. Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes DMX Analysis Services](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md) et [Récupérer des données d’un modèle d’exploration de données &#40;DMX&#41; &#40;SSRS&#41;](../../reporting-services/report-data/retrieve-data-from-a-data-mining-model-dmx-ssrs.md).  
  
## <a name="designing-a-prediction-query"></a>Conception d'une requête de prédiction  
 Le volet Création de requête du mode Création contient deux fenêtres : **Modèle d’exploration de données** et **Sélectionner une ou plusieurs tables d’entrée**. La fenêtre **Modèle d’exploration de données** vous permet de sélectionner le modèle d’exploration de données à utiliser dans la requête. La fenêtre **Sélectionner une ou plusieurs tables d’entrée** vous permet de sélectionner la table sur laquelle baser les prévisions. Si vous souhaitez utiliser une requête singleton au lieu d’une table d’entrée, cliquez avec le bouton droit dans le volet Conception de requête, puis sélectionnez **Requête singleton**. La fenêtre **Sélectionner une ou plusieurs tables d’entrée** est remplacée par une fenêtre **Entrée de requête singleton** .  
  
 En mode Création, faites glisser les champs des fenêtres **Modèle d’exploration de données** et **Sélectionner une ou plusieurs tables d’entrée** vers la colonne **Champ** du volet Grille. Vous pouvez également remplir les colonnes restantes pour spécifier un alias, afficher le champ dans les résultats, regrouper des champs et spécifier un opérateur pour restreindre la valeur de champ à un critère ou argument donné. Si vous utilisez le mode Requête, générez la requête DMX en faisant glisser des champs vers le volet Requête.  
  
## <a name="using-parameters"></a>Utilisation des paramètres  
 Vous pouvez transmettre des paramètres de rapport à un paramètre de requête DMX. Pour ce faire, vous devez ajouter un paramètre à la requête DMX, définir les paramètres de requête dans la boîte de dialogue **Paramètres de la requête** , puis modifier les paramètres de rapport correspondants. Pour définir un paramètre de requête, cliquez sur le bouton **Paramètres de la requête** (![Icône de la boîte de dialogue Paramètres de la requête](../../reporting-services/report-data/media/iconqueryparameter.gif "Icône de la boîte de dialogue Paramètres de la requête")) dans la barre d’outils. Pour obtenir des instructions sur la définition des paramètres dans une requête DMX, consultez [Définir des paramètres dans le Concepteur de requêtes MDX pour Analysis Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/define-parameters-in-the-mdx-query-designer-for-analysis-services.md).  
  
 Pour plus d’informations sur la façon de gérer la relation entre les paramètres de rapport et les paramètres de requête, consultez [Associer un paramètre de requête à un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/associate-a-query-parameter-with-a-report-parameter-report-builder-and-ssrs.md). Pour plus d’informations sur les paramètres, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md)   
 [Outils de création de requêtes &#40;SSRS&#41;](../../reporting-services/report-data/query-design-tools-ssrs.md)   
 [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
  
  
