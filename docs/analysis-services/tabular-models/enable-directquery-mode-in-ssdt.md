---
title: Activer le mode DirectQuery dans SSDT | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1d2a1ced9638a48dc02729c0f224b883974a7dde
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="enable-directquery-mode-in-ssdt"></a>Activer le mode DirectQuery dans SSDT
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Dans cette rubrique, nous allons vous expliquer comment activer le mode DirectQuery pour un projet de modèle tabulaire dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
Quand vous activez le mode DirectQuery pour un modèle tabulaire que vous concevez dans SSDT, voici ce qu’il se produit :
-   les fonctionnalités incompatibles avec le mode DirectQuery sont désactivées ;  
-   le modèle existant est validé ; des avertissements s’affichent en présence de fonctionnalités non compatibles avec le mode DirectQuery ;  
-   si des données ont été importées avant l’activation du mode DirectQuery, le cache du modèle de travail est vidé.  
  
### <a name="enable-directquery"></a>Activer DirectQuery  
  
Dans SSDT, dans le volet **Propriétés** du fichier **Model.bim** , modifiez la propriété **Mode DirectQuery**en la définissant sur **Activé**.  

![Activer le mode DirectQuery dans SSDT](../../analysis-services/tabular-models/media/enable-directquery-mode-in-ssdt.png)
  
Si votre modèle comporte déjà une connexion à une source de données et des données existantes, vous êtes invité à entrer les informations d’identification de base de données utilisées pour se connecter à la base de données relationnelle. Toutes les données déjà présentes dans le modèle sont supprimées du cache en mémoire.  
  
Si votre modèle est finalisé en tout ou partie avant l’activation du mode DirectQuery, il se peut que vous obteniez des erreurs concernant les fonctionnalités incompatibles. Dans Visual Studio, ouvrez la **Liste d’erreurs** et résolvez tous les problèmes qui empêcheraient le modèle de basculer en mode DirectQuery.  


### <a name="whats-next"></a>Quelle est l’étape suivante ? 
Vous pouvez maintenant importer des données à l’aide de l’Assistant Importation de table pour obtenir des métadonnées pour le modèle. Vous obtiendrez non pas des lignes de données, mais des tables, des colonnes et des relations qui serviront de base à votre modèle. 

Vous pouvez créer un exemple de partition pour chaque table et ajouter des exemples de données de façon à vérifier le comportement du modèle au fur et à mesure de sa création. Les exemples de données ajoutés sont utilisés dans **Analyser pour Excel** ou dans d’autres outils clients pouvant se connecter à la base de données de l’espace de travail. Pour plus d’informations, consultez [Ajouter des exemples de données à un modèle DirectQuery en mode Création](../../analysis-services/tabular-models/add-sample-data-to-a-directquery-model-in-design-mode.md) .  
  
> [!TIP]  
    >  Vous pouvez toujours afficher un petit ensemble de lignes intégré pour chaque table, même en mode DirectQuery sur un modèle vide Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], cliquez sur **Table** > **Propriétés de la table** pour afficher le dataset de 50 lignes.  
  
  
## <a name="see-also"></a>Voir aussi  
[Activer le mode DirectQuery dans SSMS](../../analysis-services/tabular-models/enable-directquery-mode-in-ssms.md)

[Ajouter des exemples de données à un modèle DirectQuery en mode Création](../../analysis-services/tabular-models/add-sample-data-to-a-directquery-model-in-design-mode.md)
  
