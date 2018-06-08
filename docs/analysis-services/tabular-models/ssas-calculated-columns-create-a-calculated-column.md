---
title: Créer une colonne calculée | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 25121b53ce050da38a6d19d0c458585acf7a9abd
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-calculated-column"></a>Créer une colonne calculée
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les colonnes calculées permettent d'ajouter de nouvelles données à votre modèle. Au lieu de coller ou importer des valeurs dans la colonne, vous créez une formule DAX qui définit les valeurs de niveau de ligne de la colonne. Les valeurs de chaque ligne d'une colonne calculée sont calculées et remplies lorsque vous créez une formule non valide puis cliquez sur ENTRÉE. La colonne calculée peut ensuite être ajoutée à une application de création de rapports ou d'analyse, comme toute autre colonne de données. Cet article décrit comment créer une nouvelle colonne calculée à l’aide de la barre de formule DAX dans le Générateur de modèles.  
  
#### <a name="to-create-a-new-calculated-column"></a>Pour créer une nouvelle colonne calculée  
  
1.  Dans le générateur de modèles, dans la vue de données, sélectionnez la table à laquelle vous souhaitez ajouter une colonne calculée, cliquez sur le menu **Colonne** , puis sur **Ajouter une colonne**.  
  
     **Ajouter une colonne** est mis en surbrillance dans la colonne la plus à droite vide, et le curseur se place dans la barre de formule.  
  
     Pour intercaler une colonne entre deux colonnes existantes, cliquez avec le bouton droit sur une colonne existante, puis sélectionnez **Insérer une colonne**.  
  
2.  Dans la barre de formule, effectuez l'une des opérations suivantes :  
  
    -   Tapez un signe égal suivi d'une formule.  
  
    -   Tapez un signe égal, suivi d'une fonction DAX, d'arguments et de paramètres, comme requis par la fonction.  
  
    -   Cliquez sur le bouton de fonction (**fx**) puis, dans la boîte de dialogue **Insérer une fonction** , sélectionnez une catégorie et une fonction, puis cliquez sur **OK**. Dans la barre de formule, tapez les arguments et les paramètres restants, comme requis par la fonction.  
  
3.  Appuyez sur Entrée pour accepter la formule.  
  
     La formule remplit la colonne tout entière et une valeur est calculée pour chaque ligne.  
  
> [!TIP]  
>  Vous pouvez utiliser la saisie semi-automatique des formules DAX au milieu d'une formule existante avec les fonctions imbriquées. Le texte immédiatement avant le point d'insertion est utilisé pour afficher des valeurs dans la liste déroulante, et tout le texte après le point d'insertion reste inchangé.  
  
## <a name="see-also"></a>Voir aussi  
 [Colonnes calculées](../../analysis-services/tabular-models/ssas-calculated-columns.md)   
 [Mesures](../../analysis-services/tabular-models/measures-ssas-tabular.md)  
  
  
