---
title: Ajouter des colonnes à une table | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 77be2e392add4811d011d85ebc1ee9efbf52191d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="add-columns-to-a-table"></a>Ajouter des colonnes à une table
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cet article décrit comment ajouter des colonnes à une table existante.  
  
## <a name="add-columns-from-the-datasource"></a>Ajouter des colonnes de la source de données  
 Lorsque vous utilisez l'Assistant Importation de table pour importer des données depuis une table de source de données, une table est créée dans le modèle qui inclut toutes les colonnes de la table source, ou si vous choisissez de filtrer certaines colonnes à l'aide de la fonctionnalité Afficher un aperçu et filtrer, seulement les colonnes et les données filtrées que vous sélectionnez. Vous pouvez aussi écrire une requête SQL qui spécifie uniquement certaines colonnes à importer. Vous pouvez toutefois déterminer ultérieurement si une table source possède des colonnes supplémentaires à ajouter à la table de modèle, ou si vous devez ajouter une colonne calculée avec des valeurs dérivées d'une formule DAX.  
  
 Si, par exemple, lorsque vous avez initialement effectué une importation à partir d'une source de données, vous avez utilisé la fonctionnalité Afficher un aperçu et filtrer dans l'Assistant Importation de table afin de sélectionner un nombre limité de colonnes de la table source, vous déterminez plus tard que vous devez ajouter une autre colonne qui existe au niveau de la table source, mais qui n'existe pas encore dans la table de modèle. Par exemple, une nouvelle colonne AdjustedProfit a été ajoutée à la table FactSales au niveau de la source de données, et vous souhaitez maintenant ajouter la même colonne AdjustedProfit et des données à la table Sales dans le modèle.  
  
 Dans ce cas, vous pouvez utiliser la boîte de dialogue Modifier les propriétés de la table pour sélectionner les colonnes de la table source et les ajouter à la table de modèle. La boîte de dialogue Modifier les propriétés de la table inclut la fenêtre d'aperçu de la table. La fenêtre d'aperçu de la table affiche la table à la source. Les colonnes qui sont déjà incluses dans la définition de la table de modèle sont déjà activées. Les colonnes qui ne sont pas déjà incluses dans la définition de la table de modèle ne sont pas activées. Vous pouvez ajouter des colonnes de la source à la définition de la table de modèle en sélectionnant la colonne et en cliquant sur OK. La fenêtre d'aperçu de la table dans la boîte de dialogue Modifier les propriétés de la table fournit les mêmes vue et fonctionnalités que la fenêtre d'aperçu de la table dans la page Afficher un aperçu et filtrer de l'Assistant Importation de table.  
  
> [!IMPORTANT]  
>  Lors de l'ajout d'une colonne à une table qui contient deux partitions ou plus, avant d'ajouter la colonne à la définition de table à l'aide de la boîte de dialogue Modifier les propriétés de la table, vous devez d'abord utiliser le gestionnaire de partitions pour ajouter la colonne à toutes les partitions définies. Après avoir ajouté la colonne aux partitions définies, vous pouvez ensuite ajouter la même colonne à la définition de table à l'aide de la boîte de dialogue Modifier les propriétés de la table.  
  
> [!NOTE]  
>  Si vous avez utilisé une requête SQL pour sélectionner des tables et des colonnes lorsque vous avez initialement utilisé l'Assistant Importation de table pour importer des données, vous devez de nouveau utiliser une requête SQL dans la boîte de dialogue Modifier les propriétés de la table pour ajouter des colonnes à la table de modèle.  
  
#### <a name="to-add-a-column-from-the-data-source-by-using-the-edit-table-properties-dialog-box"></a>Pour ajouter une colonne de la source de données à l'aide de la boîte de dialogue Modifier les propriétés de la table  
  
1.  Dans le générateur de modèles, après avoir cliqué dans la table à laquelle vous souhaitez ajouter une colonne, cliquez sur le menu **Table** , puis sur  **Propriétés de la table**.  
  
2.  Dans la boîte de dialogue **Modifier les propriétés de la table** , dans la fenêtre d’aperçu de la table, sélectionnez la colonne source à ajouter, puis cliquez sur OK. Les colonnes déjà incluses dans la définition de la table sont déjà activées.  
  
## <a name="add-a-calculated-column"></a>Ajouter une colonne calculée  
 Dans une colonne calculée, une formule DAX est utilisée pour définir une valeur pour chaque ligne. Par exemple, vous pouvez créer une colonne calculée avec une formule simple (=1) qui ajoute la valeur 1 à chaque ligne. Les colonnes calculées peuvent également avoir des formules plus complexes qui calculent des valeurs en fonction d'autres données dans le modèle. Les colonnes calculées sont couvertes plus en détail dans d'autres rubriques. Pour plus d’informations, consultez [Colonnes calculées](../../analysis-services/tabular-models/ssas-calculated-columns.md).  
  
#### <a name="to-create-a-calculated-column"></a>Pour créer une colonne calculée  
  
1.  Dans le générateur de modèles, dans la vue de données, sélectionnez la table à laquelle vous voulez ajouter une nouvelle colonne calculée vide, faites défiler vers la colonne la plus à droite ou cliquez sur le menu **Colonne** , puis sur **Ajouter une colonne**.  
  
     Pour intercaler une colonne entre deux colonnes existantes, cliquez avec le bouton droit sur une colonne existante, puis sélectionnez **Insérer une colonne**.  
  
2.  Dans la barre de formule, tapez une formule DAX pour ajouter des attributs pour chaque ligne.  
  
## <a name="add-a-blank-column"></a>Ajouter une colonne vide  
 Vous pouvez créer une colonne nommée vide dans une table de modèle. Les colonnes vides peuvent être utiles si vous souhaitez coller des données depuis une autre source. Gardez à l'esprit que les données collées sont stockées autrement que les données importées. Pour plus d’informations, consultez [copier et coller des données](../../analysis-services/tabular-models/ssas-import-data-copy-and-paste-data.md).  
  
#### <a name="to-create-a-named-blank-column"></a>Pour créer une colonne nommée vide  
  
1.  Dans le générateur de modèles, dans la vue de données, sélectionnez la table à laquelle vous voulez ajouter une colonne vide, faites défiler vers la colonne la plus à droite ou cliquez sur le menu **Colonne** , puis sur **Ajouter une colonne**.  
  
     Pour intercaler une colonne entre deux colonnes existantes, cliquez avec le bouton droit sur une colonne existante, puis sélectionnez **Insérer une colonne**.  
  
2.  Cliquez sur la cellule supérieure, puis tapez un nom et appuyez sur ENTRÉE.  
  
## <a name="see-also"></a>Voir aussi  
 [Modifier la boîte de dialogue Propriétés de table](http://msdn.microsoft.com/library/8d913e83-7246-44cc-8fc7-31729023c0d8)   
 [Changer des mappages de filtres de lignes, de tables ou de colonnes](../../analysis-services/tabular-models/change-table-column-or-row-filter-mappings-ssas-tabular.md)  
  
  
