---
title: 'Leçon 5 : Créer des relations | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 36993a468a6997ff8de40da542deac00b25b18b4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-4-create-relationships"></a>Leçon 4 : Créer des relations
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans cette leçon, vous vérifiez les relations qui ont été créées automatiquement lorsque vous avez importé des données et ajouter de nouvelles relations entre les différentes tables. Une relation est une connexion entre deux tables qui établit le mode de corrélation des données dans les deux tables. Par exemple, la table DimProduct et la table DimProductSubcategory ont une relation basée sur le fait que chaque produit appartient à une sous-catégorie. Pour plus d’informations, consultez [relations](../analysis-services/tabular-models/relationships-ssas-tabular.md).
  
Durée estimée pour effectuer cette leçon : **10 minutes**  
  
## <a name="prerequisites"></a>Conditions préalables  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 3 : marquer en tant que Table de dates](../analysis-services/lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Examiner les relations existantes et ajouter de nouvelles relations  
Lorsque vous avez importé des données à l’aide de l’Assistant Importation de Table, que vous avez obtenu sept tables de la base de données AdventureWorksDW. En règle générale, lorsque vous importez des données à partir d’une source relationnelle, les relations existantes sont importées automatiquement avec les données. Toutefois, avant de poursuivre la création de votre modèle, vous devez vérifier que les relations entre les tables ont été créées correctement. Pour ce didacticiel, vous allez également ajouter trois relations.  
  
#### <a name="to-review-existing-relationships"></a>Pour vérifier les relations existantes  
  
1.  Cliquez sur le **modèle** menu > **vue de modèle** > **vue de diagramme**.  

    Le concepteur de modèles apparaît maintenant dans la vue de diagramme, c'est un format graphique affichant toutes les tables que vous avez importées et les lignes les reliant. Les lignes entre les tables indiquent les relations qui ont été créées automatiquement lorsque vous avez importé les données.
    
    ![en tant que-tabulaire-lesson4-diagramme](../analysis-services/media/as-tabular-lesson4-diagram.png)
  
    Utilisez les contrôles de la minicarte en bas à droite du concepteur de modèles pour ajuster la vue et inclure autant de tables que possible. Vous pouvez également cliquer et faire glisser des tables à différents emplacements, en rapprochant les tables autant que possible, ou en les plaçant dans un ordre précis. Le déplacement des tables n'affecte pas les relations qui existent déjà entre elles. Pour afficher toutes les colonnes d'une table donnée, cliquez et faites glisser le bord de la table pour l'agrandir ou la réduire.  
  
2.  Cliquez sur la ligne pleine entre la **DimCustomer** table et la **DimGeography** table. La ligne pleine entre ces deux tables indique que cette relation est active, c.-à-d. qu'elle est utilisée par défaut lors du calcul des formules DAX.  
  
    Notez le **GeographyKey** colonne dans la **DimCustomer** table et la **GeographyKey** colonne dans la **DimGeography** apparaissent maintenant chacune dans une zone de table. Cela indique qu'il s'agit des colonnes utilisées dans la relation. Les propriétés de la relation apparaissent maintenant aussi dans la fenêtre **Propriétés** .  
  
    > [!TIP]  
    > Outre l’utilisation du Générateur de modèles dans la vue de diagramme, vous pouvez également utiliser la boîte de dialogue Gérer les relations pour afficher les relations entre toutes les tables dans un format de table. Avec le bouton droit **relations** dans l’Explorateur de modèles tabulaires, puis cliquez sur **gérer les relations**. La boîte de dialogue Gérer les relations montre les relations qui ont été créées automatiquement lorsque vous avez importé des données.  
  
3.  Utilisez le Concepteur de modèle dans la vue de diagramme ou la boîte de dialogue Gérer les relations, pour vérifier les relations suivantes ont été créées lorsque chacune des tables ont été importées à partir de la base de données AdventureWorksDW :  
  
    |Actif|Table|Table de recherche associée|  
    |----------|---------|------------------------|  
    |Oui|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Oui|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Oui|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Oui|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Oui|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Si une des relations dans le tableau ci-dessus sont manquante, vérifiez que votre modèle inclut les tables suivantes : DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory et FactInternetSales. Si des tables provenant de la même connexion de source de données sont importées à des moments différents, aucune relation entre ces tables ne sera créée et les relations devront être créées manuellement.  

### <a name="take-a-closer-look"></a>Prendre une présentation détaillée
Dans la vue de diagramme, vous remarquerez une flèche, un astérisque et un nombre de lignes qui affichent la relation entre les tables.

![en tant que tableau-lesson4-ligne](../analysis-services/media/as-tabular-lesson4-line.png)

La flèche indique la direction du filtrage, que l’astérisque indique cette table est le côté « plusieurs » de la cardinalité de la relation, et le 1 illustre que cette table est le côté « un » de la relation. Si vous avez besoin modifier une relation ; par exemple, modifiez la direction du filtrage de la relation ou cardinalité, double-cliquez sur la ligne de relation dans la vue de diagramme pour ouvrir la boîte de dialogue Modifier la relation.

![en tant que-tabulaire-lesson4-modifier](../analysis-services/media/as-tabular-lesson4-edit.png)

Très probablement, vous devrez jamais modifier une relation. Ces fonctionnalités sont destinées à la modélisation des données avancées et sortent du cadre de ce didacticiel. Pour plus d’informations, consultez [bidirectionnelles entre les filtres pour les modèles tabulaires dans SQL Server 2016 Analysis Services](../analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services.md).

Dans certains cas, vous devrez peut-être créer des relations supplémentaires entre les tables dans votre modèle pour prendre en charge certaines logiques métiers. Pour ce didacticiel, vous devez créer trois relations supplémentaires entre la table FactInternetSales et la table DimDate.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Pour ajouter de nouvelles relations entre des tables  
  
1.  Dans le Générateur de modèles, dans le **FactInternetSales** table, cliquez et maintenez sur le **OrderDate** colonne, puis faites glisser le curseur à la **Date** colonne dans la **DimDate** de table et relâchez.  

    Une ligne pleine apparaît et indique que vous avez créé une relation active entre la **OrderDate** colonne dans la **Internet Sales** table et la **Date** colonne dans la **Date** table. 
  
      ![en tant que-tabulaire-lesson4-nouveau](../analysis-services/media/as-tabular-lesson4-new.png) 
  
    > [!NOTE]  
    > Lorsque vous créez des relations, la direction de la cardinalité et de filtre entre la table primaire et de la table de recherche associée est automatiquement sélectionnée.  
  
2.  Dans le **FactInternetSales** table, cliquez et maintenez sur le **DueDate** colonne, puis faites glisser le curseur à la **Date** colonne dans la **DimDate** de table et relâchez.  
  
    Une ligne pointillée apparaît indique que vous avez créé une relation inactive entre la **DueDate** colonne dans la **FactInternetSales** table et la **Date** colonne dans la **DimDate** table. Vous pouvez avoir plusieurs relations entre les tables, mais une seule relation peut être active à la fois.  
  
3.  Enfin, créez une relation de plus ; dans le **FactInternetSales** table, cliquez et maintenez sur le **ShipDate** colonne, puis faites glisser le curseur à la **Date** colonne dans la **DimDate** de table et relâchez.  
    
     ![en tant que-tabulaire-lesson4-newinactive](../analysis-services/media/as-tabular-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?
Accédez à la leçon suivante : [leçon 5 : créer des colonnes calculées](../analysis-services/lesson-5-create-calculated-columns.md).
  
  
  
