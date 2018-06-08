---
title: Ajout d’attributs aux Dimensions | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 753aad68d1c6fc9fb6fe53efb3b73ae767b4570e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-2-3---adding-attributes-to-dimensions"></a>Leçon 2-3-Ajout d’attributs aux Dimensions
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

Une fois les dimensions définies, vous pouvez les remplir avec les attributs qui représentent chaque élément de données dans la dimension. Les attributs reposent généralement sur les champs d'une vue de source de données. Lorsque vous ajoutez des attributs à une dimension, vous pouvez inclure des champs de n'importe quelle table dans la vue de source de données.  
  
Dans cette tâche, vous utiliserez le Concepteur de dimensions pour ajouter des attributs aux dimensions Customer et Product. La dimension Customer inclut des attributs basés sur des champs des deux tables Customer et Geography.  
  
## <a name="adding-attributes-to-the-customer-dimension"></a>Ajout d'attributs à la dimension Customer  
  
#### <a name="to-add-attributes"></a>Pour ajouter des attributs  
  
1.  Ouvrez le Concepteur de dimensions pour la dimension Customer. Pour cela, double-cliquez sur la dimension **Customer** du nœud **Dimensions** de l’Explorateur de solutions.  
  
2.  Dans le volet **Attributs** , remarquez les attributs Customer Key et Geography Key créés par l'Assistant Cube.  
  
3.  Dans la barre d'outils de l'onglet **Structure de dimension** , assurez-vous que l'icône Zoom qui permet d'afficher les tables du volet **Vue de source de données** est définie avec un zoom de 100 %.  
  
4.  Faites glisser les colonnes suivantes de la table **Customer** du volet **Vue de source de données** vers le volet **Attributs** :  
  
    -   **BirthDate**  
  
    -   **MaritalStatus**  
  
    -   **Gender**  
  
    -   **EmailAddress**  
  
    -   **YearlyIncome**  
  
    -   **TotalChildren**  
  
    -   **NumberChildrenAtHome**  
  
    -   **EnglishEducation**  
  
    -   **EnglishOccupation**  
  
    -   **HouseOwnerFlag**  
  
    -   **NumberCarsOwned**  
  
    -   **Téléphone**  
  
    -   **DateFirstPurchase**  
  
    -   **CommuteDistance**  
  
5.  Faites glisser les colonnes suivantes de la table **Geography** du volet **Vue de source de données** vers le volet **Attributs** :  
  
    -   **Ville**  
  
    -   **StateProvinceName**  
  
    -   **EnglishCountryRegionName**  
  
    -   **PostalCode**  
  
6.  Dans le menu Fichier, cliquez sur **Enregistrer tout**.  
  
## <a name="adding-attributes-to-the-product-dimension"></a>Ajout d'attributs à la dimension Product  
  
#### <a name="to-add-attributes"></a>Pour ajouter des attributs  
  
1.  Ouvrez le Concepteur de dimensions pour la dimension Product. Double-cliquez sur la dimension **Product** dans l’Explorateur de solutions.  
  
2.  Dans le volet **Attributs** , remarquez que l'attribut Product Key été créé par l'Assistant Cube.  
  
3.  Dans la barre d'outils de l'onglet **Structure de dimension** , assurez-vous que l'icône Zoom qui permet d'afficher les tables du volet **Vue de source de données** est définie avec un zoom de 100 %.  
  
4.  Faites glisser les colonnes suivantes de la table **Product** du volet **Vue de source de données** vers le volet **Attributs** :  
  
    -   **StandardCost**  
  
    -   **Color**  
  
    -   **SafetyStockLevel**  
  
    -   **ReorderPoint**  
  
    -   **ListPrice**  
  
    -   **Taille**  
  
    -   **SizeRange**  
  
    -   **Weight**  
  
    -   **DaysToManufacture**  
  
    -   **ProductLine**  
  
    -   **DealerPrice**  
  
    -   **Classe**  
  
    -   **Style**  
  
    -   **ModelName**  
  
    -   **StartDate**  
  
    -   **EndDate**  
  
    -   **État**  
  
5.  Dans le menu Fichier, cliquez sur **Enregistrer tout**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Vérification des propriétés de la Dimension et de Cube](../analysis-services/lesson-2-4-reviewing-cube-and-dimension-properties.md)  
  
## <a name="see-also"></a>Voir aussi  
[Dimension Attribute Properties Reference](../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
  
