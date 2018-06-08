---
title: 'Leçon supplémentaire de Analysis Services tutorial : les hiérarchies irrégulières | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bc5a2164576e2e6142d8835dad6f6c114b7a9c5b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="supplemental-lesson---ragged-hierarchies"></a>Leçon supplémentaire - hiérarchies irrégulières

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon supplémentaire, vous résolvez un problème courant lors de glissement sur des hiérarchies qui contiennent des valeurs vides (membres) à différents niveaux. Par exemple, une organisation où un responsable hiérarchique a service et des non cadres en tant que collaborateurs directs. Ou bien, des hiérarchies composée de pays-région-City, où certaines villes n’ont pas un département, telles que Washington D.C., cité du Vatican ou parent. Lorsqu’une hiérarchie possède des membres vides, il est souvent descend à des niveaux différents ou irrégulières.

![as-Lesson-Detail-Ragged-Hierarchies-table](../tutorial-tabular-1400/media/as-lesson-detail-ragged-hierarchies-table.png)

Les modèles tabulaires au niveau de compatibilité 1400 possèdent un autre **masquer les membres** propriété pour les hiérarchies. Le **par défaut** paramètre qu’il n’y aucun membre vide n’importe quel niveau. Le **masquer les membres vides** paramètre exclut vides à partir de la hiérarchie lorsque ajouté à un tableau croisé dynamique ou un rapport.  
  
Durée estimée pour effectuer cette leçon : **20 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  
Cet article de la leçon supplémentaire fait partie d’un didacticiel de modélisation tabulaire. Avant d’effectuer les tâches de cette leçon supplémentaire, vous devez avoir terminé toutes les leçons précédentes ou avoir un projet de modèle d’exemple Adventure Works Internet Sales terminé. 

Si vous avez créé le projet AW Internet Sales dans le cadre de ce didacticiel, votre modèle ne contient pas encore toutes les données ou les hiérarchies irrégulières. Pour effectuer cette leçon supplémentaire, vous devez d’abord créer le problème en ajoutant des tables supplémentaires, créez des relations, les colonnes calculées, une mesure et une hiérarchie d’organisation. Cette partie prend environ 15 minutes. Ensuite, vous devez résoudre en quelques minutes.  

## <a name="add-tables-and-objects"></a>Ajouter des tables et des objets
  
### <a name="to-add-new-tables-to-your-model"></a>Pour ajouter de nouvelles tables à votre modèle
  
1.  Dans l’Explorateur de modèles tabulaires, développez **des Sources de données**, puis cliquez sur votre connexion > **importer de nouvelles Tables**.
  
2.  Dans le navigateur, sélectionnez **DimEmployee** et **FactResellerSales**, puis cliquez sur **OK**.

3.  Dans l’éditeur de requête, cliquez sur **importation**

4.  Créez les éléments suivants [relations](../tutorial-tabular-1400/as-lesson-4-create-relationships.md):

    | tableau 1           | Colonne       | Direction du filtrage   | tableau 2     | Colonne      | Actif |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Par défaut            | DimDate     | Date        | Oui    |
    | FactResellerSales | DueDate      | Par défaut            | DimDate     | Date        | non     |
    | FactResellerSales | ShipDateKey  | Par défaut            | DimDate     | Date        | non     |
    | FactResellerSales | ProductKey   | Par défaut            | DimProduct  | ProductKey  | Oui    |
    | FactResellerSales | EmployeeKey  | Pour les deux Tables | DimEmployee | EmployeeKey | Oui    |

5. Dans le **DimEmployee** table, créez les éléments suivants [des colonnes calculées](../tutorial-tabular-1400/as-lesson-5-create-calculated-columns.md): 

    **Chemin d'accès** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **Nom complet** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Niveau 1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Level2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],2,1)) 
    ```

    **Level3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],3,1)) 
    ```

    **Level4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],4,1)) 
    ```

    **Level5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],5,1)) 
    ```

6.  Dans le **DimEmployee** table, créez un [hiérarchie](../tutorial-tabular-1400/as-lesson-9-create-hierarchies.md) nommé **organisation**. Ajouter les colonnes suivantes dans l’ordre : **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.

7.  Dans le **FactResellerSales** table, créez les éléments suivants [mesure](../tutorial-tabular-1400/as-lesson-6-create-measures.md):

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  Utilisez [analyser dans Excel](../tutorial-tabular-1400/as-lesson-12-analyze-in-excel.md) pour ouvrir Excel et créer automatiquement un tableau croisé dynamique.

9.  Dans **PivotTable Fields**, ajouter le **organisation** hiérarchie à partir de la **DimEmployee** table **lignes**et le  **ResellerTotalSales** à partir de la **FactResellerSales** table **valeurs**.

    ![as-Lesson-Detail-Ragged-Hierarchies-PivotTable](../tutorial-tabular-1400/media/as-lesson-detail-ragged-hierarchies-pivottable.png)

    Comme vous pouvez le voir dans le tableau croisé dynamique, la hiérarchie affiche les lignes qui sont décalés. Il existe plusieurs lignes dans laquelle les membres vides sont affichés.

## <a name="to-fix-the-ragged-hierarchy-by-setting-the-hide-members-property"></a>Pour corriger la hiérarchie irrégulière en définissant les masquer les membres de propriété

1.  Dans **l’Explorateur de modèles tabulaires**, développez **Tables** > **DimEmployee** > **hiérarchies**  >  **Organisation**.

2.  Dans **propriétés** > **masquer les membres**, sélectionnez **masquer les membres vides**. 

    ![as-Lesson-Detail-Ragged-Hierarchies-hidemembers](../tutorial-tabular-1400/media/as-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Dans Excel, actualisez le tableau croisé dynamique. 

    ![as-Lesson-Detail-Ragged-Hierarchies-PivotTable-Refresh](../tutorial-tabular-1400/media/as-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Qui se présente désormais mieux !

## <a name="see-also"></a>Voir aussi   
[Leçon 9 : Créer des hiérarchies](../tutorial-tabular-1400/as-lesson-9-create-hierarchies.md)  
[Leçon supplémentaire - sécurité dynamique](../tutorial-tabular-1400/as-supplemental-lesson-dynamic-security.md)  
[Leçon supplémentaire - les lignes de détails](../tutorial-tabular-1400/as-supplemental-lesson-detail-rows.md)  
