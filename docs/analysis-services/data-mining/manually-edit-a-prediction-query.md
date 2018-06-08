---
title: Modifier manuellement une requête de prédiction | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: aba181ab73ab730869eaa7930591cf21a947d20c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="manually-edit-a-prediction-query"></a>Modifier manuellement une requête de prédiction
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Après avoir créé une requête en utilisant le Générateur de requêtes de prédiction, vous pouvez modifier la requête en activant le mode Texte de la requête sous l’onglet **Prédiction de modèle d’exploration de données** du Concepteur d’exploration de données. Un éditeur de texte apparaît au bas de l'écran pour afficher la requête créée par le Générateur de requêtes.  
  
 Le basculement vers le mode Texte de la requête est utile pour effectuer des ajouts à la requête. Par exemple, vous pouvez ajouter une clause WHERE ou une clause ORDER BY.  
  
 Utilisez la grille dans le Générateur de requêtes de prédiction pour insérer le nom des objets et des colonnes, ainsi que pour configurer la syntaxe de fonctions de prédiction individuelles, puis basculez vers le mode de modification manuelle pour modifier des valeurs de paramètres.  
  
> [!NOTE]  
>  Si vous repassez du mode **Texte de la requête** au mode **Conception** , toutes les modifications apportées en mode **Texte de la requête** sont perdues.  
  
### <a name="modify-a-query"></a>Modifier une requête  
  
1.  Sous l’onglet **Prédiction de modèle d’exploration de données** du Concepteur d’exploration de données de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur **SQL**.  
  
     La grille au bas de l'écran est remplacée par un éditeur de texte qui contient la requête. Vous pouvez modifier la requête dans cet éditeur.  
  
2.  Pour exécuter la requête, dans le menu **Modèle d’exploration de données** , sélectionnez **Résultat**ou cliquez sur le bouton pour passer aux résultats de la requête.  
  
    > [!NOTE]  
    >  Si la requête que vous avez créée n'est pas valide, la fenêtre Résultats n'affiche ni erreur ni résultats. Cliquez sur le bouton **Conception** ou sélectionnez **Conception** ou **Requête** dans le menu **Modèle d’exploration de données** pour résoudre le problème et réexécuter la requête.  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Générateur de requête de prédiction & #40 ; exploration de données & #41 ;](http://msdn.microsoft.com/library/12900d49-db88-48bb-a5f4-0a9a172bc126)   
 [Leçon 6 : Création et utilisation des prédictions & #40 ; Didacticiel d’exploration de données de base de données & #41 ;](http://msdn.microsoft.com/library/b213cb58-2c40-4c89-b08b-d3c36a4afad3)  
  
  
