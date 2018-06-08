---
title: Créer et gérer des hiérarchies | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c9282c3b28ca5998cc21d2906f06d50f862e49a9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-manage-hierarchies"></a>Créer et gérer des hiérarchies 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les hiérarchies peuvent être créées et gérées dans le générateur de modèles, dans la vue de diagramme. Pour afficher le concepteur de modèles dans la Vue de diagramme, dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur le menu **Modèle** , pointez sur **Vue du modèle**, puis cliquez sur **Vue de diagramme**.  
  
 Cet article comprend les tâches suivantes :  
  
-   [Créer une hiérarchie](#bkmk_create)  
  
-   [Modifier une hiérarchie](#bkmk_edit)  
  
-   [Supprimer une hiérarchie](#bkmk_delete)  
  
##  <a name="bkmk_create"></a> Créer une hiérarchie  
 Vous pouvez créer une hiérarchie en utilisant les colonnes et le menu contextuel de table. Lorsque vous créez une hiérarchie, un nouveau niveau parent s'affiche avec les colonnes sélectionnées en tant que niveaux enfants.  
  
#### <a name="to-create-a-hierarchy-from-the-context-menu"></a>Pour créer une hiérarchie à partir du menu contextuel  
  
1.  Dans le générateur de modèles (Vue de diagramme), dans une fenêtre de table, cliquez avec le bouton droit sur une colonne, puis sélectionnez **Créer une hiérarchie**.  
  
     Pour sélectionner plusieurs colonnes, cliquez sur chaque colonne, puis cliquez avec le bouton droit pour ouvrir le menu contextuel, puis sélectionnez **Créer une hiérarchie**.  
  
     Un niveau de hiérarchie parent est créé au bas de la fenêtre de table et les colonnes sélectionnées sont copiées sous la hiérarchie en tant que niveaux enfants.  
  
2.  Tapez un nom pour la hiérarchie.  
  
 Vous pouvez faire glisser des colonnes supplémentaires dans le niveau parent de votre hiérarchie, lequel copie les colonnes. Placez le niveau enfant à l'endroit où vous souhaitez qu'il apparaisse dans la hiérarchie.  
  
> [!NOTE]  
>  La commande Créer une hiérarchie est désactivée dans le menu contextuel si vous sélectionnez plusieurs mesures avec une ou plusieurs colonnes, ou si vous sélectionnez des colonnes à partir de plusieurs tables.  
  
##  <a name="bkmk_edit"></a> Modifier une hiérarchie  
 Vous pouvez renommer une hiérarchie, renommer un niveau enfant, modifier l'ordre des niveaux enfants, ajouter des colonnes supplémentaires en tant que niveaux enfants, supprimer un niveau enfant d'une hiérarchie, afficher le nom de la source d'un niveau enfant (le nom de la colonne) et masquer un niveau enfant s'il a le même nom que le niveau de hiérarchie parent.  
  
#### <a name="to-change-the-name-of-a-hierarchy-or-child-level"></a>Pour modifier le nom d'une hiérarchie ou d'un niveau enfant  
  
1.  Cliquez avec le bouton droit sur le niveau de hiérarchie parent ou sur un niveau enfant, puis sélectionnez **Renommer**.  
  
2.  Entrez le nouveau nom ou modifiez le nom existant.  
  
#### <a name="to-change-the-order-of-a-child-level-in-a-hierarchy"></a>Pour modifier l'ordre d'un niveau enfant dans une hiérarchie  
  
-   Cliquez et faites glisser un niveau enfant vers une nouvelle position dans la hiérarchie.  
  
-   Ou cliquez avec le bouton droit sur un niveau enfant de la hiérarchie, puis cliquez sur Monter pour déplacer le niveau plus haut dans la liste, ou sur Descendre pour déplacer le niveau plus bas dans la liste.  
  
-   Vous pouvez également cliquer sur un niveau enfant pour le sélectionner, puis appuyez sur Alt + Flèche haut pour déplacer le niveau vers le haut dans la liste, ou sur Alt + Flèche bas pour déplacer le niveau vers le bas.  
  
#### <a name="to-add-another-child-level-to-a-hierarchy"></a>Pour ajouter un autre niveau enfant à une hiérarchie  
  
-   Cliquez et faites glisser une colonne sur le niveau parent ou à un emplacement spécifique de la hiérarchie. La colonne est copiée en tant que niveau enfant de la hiérarchie.  
  
-   Vous pouvez également cliquer avec le bouton droit sur une colonne, pointer sur **Ajouter à la hiérarchie**, puis cliquer sur la hiérarchie.  
  
> [!NOTE]  
>  Vous pouvez ajouter une colonne masquée (une colonne qui n'apparaît pas dans les rapports) en tant que niveau enfant à la hiérarchie. Le niveau enfant n'est pas masqué.  
  
#### <a name="to-remove-a-child-level-from-a-hierarchy"></a>Pour supprimer un niveau enfant d'une hiérarchie  
  
-   Cliquez avec le bouton droit sur un niveau enfant, puis sélectionnez **Supprimer de la hiérarchie**.  
  
-   Ou bien, cliquez sur un niveau enfant, puis appuyez sur **Supprimer**.  
  
> [!NOTE]  
>  Si vous renommez un niveau de hiérarchie enfant, il ne porte plus le même nom que la colonne à partir de laquelle il a été copié. Utilisez la commande **Afficher le nom de la source** pour voir la colonne à partir de laquelle il a été copié.  
  
#### <a name="to-show-a-source-name"></a>Pour afficher un nom de source  
  
-   Cliquez avec le bouton droit sur un niveau enfant de la hiérarchie, puis sélectionnez **Afficher le nom de la source**. Le nom de la colonne à partir de laquelle il a été copié s'affiche.  
  
##  <a name="bkmk_delete"></a> Supprimer une hiérarchie  
  
#### <a name="to-delete-a-hierarchy-and-remove-its-child-levels"></a>Pour supprimer une hiérarchie et ses niveaux enfants  
  
-   Cliquez avec le bouton droit sur le niveau de hiérarchie parent, puis cliquez sur Supprimer la hiérarchie.  
  
-   Ou bien, cliquez sur le niveau de hiérarchie parent, puis appuyez sur Supprimer. Cela supprime également tous les niveaux enfants.  
  
## <a name="see-also"></a>Voir aussi  
 [Générateur de modèles tabulaires ](../../analysis-services/tabular-models/tabular-model-designer-ssas.md)   
 [Hiérarchies](../../analysis-services/tabular-models/hierarchies-ssas-tabular.md)   
 [Mesures](../../analysis-services/tabular-models/measures-ssas-tabular.md)  
  
  
