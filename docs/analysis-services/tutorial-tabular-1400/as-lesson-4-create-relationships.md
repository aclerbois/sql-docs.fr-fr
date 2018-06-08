---
title: 'Leçon du didacticiel Analysis Services 4 : créer des relations | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 564126e1de4a8019778e33718b48462f633ae232
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-relationships"></a>Créer des relations

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous vérifiez les relations qui ont été créées automatiquement lorsque vous avez importé des données et ajoutez de nouvelles relations entre les différentes tables. Une relation est une connexion entre deux tables qui établit le mode de corrélation des données dans les deux tables. Par exemple, la table DimProduct et la table DimProductSubcategory ont une relation basée sur le fait que chaque produit appartient à une sous-catégorie. Pour plus d’informations, consultez [relations](../tabular-models/relationships-ssas-tabular.md).
  
Durée estimée pour effectuer cette leçon : **10 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 3 : marquer en tant que Table de dates](../tutorial-tabular-1400/as-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Examiner les relations existantes et ajouter de nouvelles relations  

Lorsque vous avez importé des données à l’aide d’obtenir des données, que vous avez obtenu sept tables de la base de données AdventureWorksDW. En règle générale, lorsque vous importez des données à partir d’une source relationnelle, les relations existantes sont importées automatiquement avec les données. Dans l’ordre pour obtenir des données créer automatiquement des relations dans le modèle de données, il faut relations entre les tables de la source de données.

Avant de procéder à la création de votre modèle, vous devez vérifier les relations entre les tables ont été créées correctement. Pour ce didacticiel, vous permet également d’ajouter trois nouvelles relations.  

  
#### <a name="to-review-existing-relationships"></a>Pour vérifier les relations existantes  
  
1.  Cliquez sur le **modèle** menu > **vue de modèle** > **vue de diagramme**.  

    Le Générateur de modèles apparaît maintenant dans la vue de diagramme, un format graphique affichant toutes les tables vous importées et les lignes entre eux. Les lignes entre les tables indiquent les relations qui ont été créées automatiquement lorsque vous avez importé les données.
    
    ![en tant que diagramme de lesson4](../tutorial-tabular-1400/media/as-lesson4-diagram.png)
  
    > [!NOTE]
    > Si vous ne voyez pas toutes les relations entre les tables, il est probable ne signifie aucune relation entre ces tables à la source de données.

    Inclure autant de tables que possible à l’aide de contrôles de la minicarte dans le coin inférieur droit du Générateur de modèles. Vous pouvez également cliquer et faire glisser des tables à différents emplacements, en rapprochant les tables autant que possible, ou en les plaçant dans un ordre précis. Déplacement de tables n’affecte pas les relations entre les tables. Pour afficher toutes les colonnes dans une table particulière, cliquez et faites glisser sur le bord de la table pour développer ou réduire sa taille.  
  
2.  Cliquez sur la ligne pleine entre la **DimCustomer** table et la **DimGeography** table. La ligne pleine entre ces deux tables indique que cette relation est active, autrement dit, qu'il est utilisé par défaut lors du calcul des formules DAX.  
  
    Notez le **GeographyKey** colonne dans la **DimCustomer** table et la **GeographyKey** colonne dans la **DimGeography** apparaissent maintenant chacune dans une zone de table. Ces colonnes sont utilisées dans la relation. Les propriétés de la relation apparaissent maintenant aussi dans la fenêtre **Propriétés** .  
  
    > [!TIP]  
    > Vous pouvez également utiliser la boîte de dialogue Gérer les relations pour afficher les relations entre toutes les tables dans un format de table. Dans l’Explorateur de modèles tabulaires, cliquez sur **relations** > **gérer les relations**.
  
3.  Vérifier que les relations suivantes ont été créées lorsque chacune des tables ont été importées à partir de la base de données AdventureWorksDW :  
  
    |Actif|Table|Table de recherche associée|  
    |----------|---------|------------------------|  
    |Oui|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Oui|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Oui|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Oui|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Oui|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Si une des relations sont manquante, vérifiez que votre modèle inclut les tables suivantes : DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory et FactInternetSales. Si des tables à partir de la même connexion de source de données sont importées à des moments, toutes les relations entre les différents ces tables ne sont pas créées et doivent être créés manuellement. Si aucune relation n’apparaît pas, cela signifie qu'aucune relation n’est à la source de données. Vous pouvez les créer manuellement dans le modèle de données.

### <a name="take-a-closer-look"></a>Prendre une présentation détaillée

Dans la vue de diagramme, notez un numéro sur les lignes qui affichent la relation entre les tables, un astérisque et une flèche.

![en tant que ligne de lesson4](../tutorial-tabular-1400/media/as-lesson4-line.png)

La flèche indique la direction du filtre. L’astérisque indique que cette table est la *nombreux* côté de la cardinalité de la relation et l’autre contient cette table est la *un* côté de la relation. Si vous avez besoin modifier une relation ; par exemple, modifiez la direction du filtrage de la relation ou cardinalité, double-cliquez sur la ligne de relation pour ouvrir la boîte de dialogue Modifier la relation.

![en tant que lesson4-modifier](../tutorial-tabular-1400/media/as-lesson4-edit.png)

Ces fonctionnalités sont destinées à la modélisation des données avancées et sortent du cadre de ce didacticiel. Pour plus d’informations, consultez [bidirectionnelles entre les filtres pour les modèles tabulaires dans Analysis Services](../tabular-models/bi-directional-cross-filters-tabular-models-analysis-services.md).

Dans certains cas, vous devrez peut-être créer des relations supplémentaires entre les tables dans votre modèle pour prendre en charge certaines logiques métiers. Pour ce didacticiel, vous devez créer trois relations supplémentaires entre la table FactInternetSales et la table DimDate.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Pour ajouter de nouvelles relations entre des tables  
  
1.  Dans le Générateur de modèles, dans le **FactInternetSales** table, cliquez et maintenez la sélection le **OrderDate** colonne, puis faites glisser le curseur à la **Date** colonne dans la **DimDate** de table et relâchez.  

    Une ligne pleine apparaît et indique que vous avez créé une relation active entre la **OrderDate** colonne dans la **Internet Sales** table et le **Date** colonne dans la **Date** table. 
  
      ![en tant que-lesson4-nouveau](../tutorial-tabular-1400/media/as-lesson4-new.png) 
  
    > [!NOTE]  
    > Lorsque vous créez des relations, la direction de la cardinalité et de filtre entre la table primaire et de la table de recherche associée est automatiquement sélectionnée.  
  
2.  Dans le **FactInternetSales** table, cliquez et maintenez sur le **DueDate** colonne, puis faites glisser le curseur à la **Date** colonne dans la **DimDate** de table et relâchez.  
  
    Une ligne pointillée apparaît indique que vous avez créé une relation inactive entre la **DueDate** colonne dans la **FactInternetSales** table et le **Date** colonne dans la **DimDate** table. Vous pouvez avoir plusieurs relations entre les tables, mais une seule relation peut être active à la fois. Les relations inactives peuvent être effectuées pour effectuer des agrégations spéciale dans les expressions DAX personnalisées actives.  
  
3.  Enfin, créez une relation de plus. Dans le **FactInternetSales** table, cliquez et maintenez sur le **ShipDate** colonne, puis faites glisser le curseur à la **Date** colonne dans la **DimDate** de table et relâchez.  
    
     ![en tant que newinactive de lesson4](../tutorial-tabular-1400/media/as-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?

[Leçon 5 : Créer des colonnes calculées](../tutorial-tabular-1400/as-lesson-5-create-calculated-columns.md).
  
  
  
