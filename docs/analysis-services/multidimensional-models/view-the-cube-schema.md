---
title: Afficher le schéma de Cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 51f22eecc2cc892d65691b13edb9382f3134ddac
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="view-the-cube-schema"></a>Afficher le schéma de cube
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le volet **Vue de source de données** de l'onglet **Structure de cube** du **Concepteur de cube** affiche le schéma de cube. Le schéma est l'ensemble de tables à partir desquelles les mesures et les dimensions d'un cube sont dérivées. Chaque schéma de cube se compose d'une ou plusieurs tables de faits et d'une ou plusieurs tables de dimension sur lesquelles les mesures et les dimensions du cube sont basées.  
  
 Le volet **Vue de source de données** de l'onglet **Structure de cube** affiche un diagramme de la vue de source de données sur laquelle le cube est basé. Ce diagramme est un sous-ensemble du diagramme principal de la vue de source de données. Vous pouvez masquer et afficher les tables dans le volet **Vue de source de données** et afficher tous les diagrammes existants. Toutefois, vous ne pouvez pas apporter des modifications (telles que l'ajout de nouvelles relations ou requêtes nommées) au schéma sous-jacent. Pour apporter des modifications au schéma, utilisez le concepteur de vue de source de données.  
  
 Lorsque vous créez un cube, le diagramme affiché dans le volet **Vue de source de données** de l'onglet **Structure de cube** est initialement le même que le diagramme **Afficher toutes les tables** dans la vue de source de données du projet ou de la base de données. Vous pouvez remplacer ce diagramme par un diagramme existant dans la vue de source de données et effectuer des ajustements dans le volet **Vue de source de données** .  
  
 Lorsque vous travaillez avec le diagramme dans le **Concepteur de cube**, les commandes qui agissent sur l'onglet ou sur n'importe quel objet sélectionné dans l'onglet sont disponibles dans le menu **Vue de source de données** . Vous pouvez également cliquer avec le bouton droit sur l'arrière-plan du diagramme ou sur n'importe quel objet de ce diagramme afin d'utiliser les commandes qui agissent sur le diagramme ou l'objet sélectionné. Vous pouvez :  
  
-   Basculer entre le diagramme et les formats d'arborescence.  
  
-   Réorganiser, rechercher, afficher et masquer des tables.  
  
-   Afficher les noms conviviaux.  
  
-   Permuter les mises en page.  
  
-   Modifier l'agrandissement.  
  
-   Afficher les propriétés.  
  
 En outre, vous pouvez effectuer les actions répertoriées dans le tableau suivant :  
  
|Pour|Action|  
|--------|-------------|  
|Utilisez un diagramme de la vue de source de données du cube|Dans le menu **Vue de source de données** , pointez sur **Copier le schéma à partir de**, puis cliquez sur le diagramme de vue de source de données que vous souhaitez utiliser.<br /><br /> - ou -<br /><br /> Cliquez avec le bouton droit sur l’arrière-plan du volet **Vue de source de données** , pointez sur **Copier le schéma à partir de**, puis cliquez sur le diagramme dans la vue de source de données de votre choix. Cette méthode crée une copie distincte du diagramme ; par conséquent, toutes les modifications que vous apportez sur l'onglet **Générateur de cube** n'apparaissent pas dans le diagramme d'origine.|  
|Afficher uniquement les tables utilisées dans le cube|Dans le menu **Vue de source de données** , cliquez sur **Afficher seulement les tables utilisées**.<br /><br /> - ou -<br /><br /> Cliquez avec le bouton droit sur l’arrière-plan du volet **Vue de source de données** , puis sélectionnez **Afficher seulement les tables utilisées**.|  
|Modifier le schéma de la vue de source de données|Dans le menu **Vue de source de données** , cliquez sur **Modifier la vue de source de données**.<br /><br /> - ou -<br /><br /> Cliquez avec le bouton droit sur l’arrière-plan du volet **Vue de source de données** , puis sélectionnez **Modifier la vue de source de données**.|  
  
  
