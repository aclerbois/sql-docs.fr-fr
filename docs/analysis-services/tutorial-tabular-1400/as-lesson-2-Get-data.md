---
title: 'Leçon du didacticiel Analysis Services 2 : obtenir les données | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9adeebfbd3c49761adb816a7d28472a61ffca5cc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="get-data"></a>Obtenir des données

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous utilisez **obtenir des données** pour vous connecter à la base de données exemple AdventureWorksDW, sélectionner des données, aperçu et filtrer et d’importer dans votre espace de travail modèle.  
  
En utilisant obtenir des données, vous pouvez importer des données à partir d’un large éventail de sources. Données peuvent également être interrogées à l’aide d’une expression de formule Power Query M ou un [une expression de requête SQL native](../tabular-models/ssas-import-query.md).

> [!NOTE]
> Tâches et des images dans ce didacticiel illustrent la connexion à une base de données AdventureWorksDW2014 sur un serveur local. Dans certains cas, une base de données AdventureWorksDW sur Azure SQL Data Warehouse peut afficher des objets différents ; Toutefois, ils sont fondamentalement les mêmes.
  
Durée estimée pour effectuer cette leçon : **10 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 1 : créer un projet de modèle tabulaire](../tutorial-tabular-1400/as-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Créer une connexion  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw-database"></a>Pour créer une connexion à la base de données AdventureWorksDW  
  
1.  Dans **l’Explorateur de modèles tabulaires**, avec le bouton droit **des Sources de données** > **importer à partir de la Source de données**.  
  
    Cette opération lance **obtenir des données**, qui vous guide tout au long de la connexion à une source de données. Si vous ne voyez pas dans l’Explorateur de modèles tabulaires, **l’Explorateur de solutions**, double-cliquez sur **Model.bim** pour ouvrir le modèle dans le concepteur. 
    
    ![as-lesson2-getdata](../tutorial-tabular-1400/media/as-lesson2-getdata.png)
  
2.  Dans le groupe de données, cliquez sur **base de données** > **base de données SQL Server** > **connexion**.  
  
3.  Dans le **base de données SQL Server** boîte de dialogue, dans **Server**, tapez le nom du serveur où vous avez installé la base de données AdventureWorksDW, puis cliquez sur **connexion**.  

4.  Lorsque vous êtes invité à entrer les informations d’identification, vous devez spécifier les informations d’identification Qu'analysis Services utilise pour se connecter à la source de données lors de l’importation et le traitement des données. Dans **le Mode d’emprunt d’identité**, sélectionnez **emprunter l’identité d’un compte**, puis entrez les informations d’identification, puis cliquez sur **connexion**. Il est recommandé de qu'utiliser un compte dans lequel le mot de passe n’expire jamais.

    ![Leçon 2-compte d’identification](../tutorial-tabular-1400/media/as-lesson2-account.png)
  
    > [!NOTE]  
    > L'utilisation d'un compte d'utilisateur et d'un mot de passe Windows est la méthode la plus sûre pour se connecter à une source de données.
  
5.  Dans le navigateur, sélectionnez le **AdventureWorksDW** de base de données, puis cliquez sur **OK**. Cela crée la connexion à la base de données. 
  
6.  Dans le navigateur, sélectionnez la case à cocher pour les tables suivantes : **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, et **FactInternetSales**.  

    ![comme-lesson2-sélectionnez-des tables](../tutorial-tabular-1400/media/as-lesson2-select-tables.png)
  
Après avoir cliqué sur OK, l’éditeur de requête s’ouvre. Dans la section suivante, vous sélectionnez uniquement les données que vous souhaitez importer.

  
## <a name="filter-the-table-data"></a>Filtrer les données de table  

Tables dans la base de données exemple AdventureWorksDW contiennent des données qui n’est pas nécessaires d’inclure dans votre modèle. Lorsque cela est possible, que vous souhaitez filtrer les données inutiles pour économiser l’espace mémoire utilisé par le modèle. Vous filtrez les colonnes de tables afin de n'être pas importés dans la base de données de l’espace de travail ou de la base de données du modèle après que qu’il a été déployé. 
  
#### <a name="to-filter-the-table-data-before-importing"></a>Pour filtrer les données de table avant l’importation  
  
1.  Dans l’éditeur de requête, sélectionnez le **DimCustomer** table. Une vue de la table DimCustomer dans la source de données (votre base de données exemple AdventureWorksDW) s’affiche. 
  
2.  Sélection multiple (Ctrl + clic) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, puis avec le bouton droit, puis cliquez sur **supprimer les colonnes**. 

    ![as-lesson2-remove-columns](../tutorial-tabular-1400/media/as-lesson2-remove-columns.png)
  
    Étant donné que les valeurs de ces colonnes ne sont pas appropriées à l'analyse des ventes sur Internet, il est inutile de les importer. Colonnes inutiles diminuera votre modèle plus petites et plus efficace.  

    > [!TIP]
    > Si vous commettez une erreur, vous pouvez sauvegarder en supprimant une étape dans **étapes appliquées**.   
    
    ![as-lesson2-remove-columns](../tutorial-tabular-1400/media/as-lesson2-remove-step.png)

  
4.  Filtrez les autres tables en supprimant les colonnes suivantes dans chaque table :  
    
    **DimDate**
    
      |Colonne|  
      |--------|  
      |**DateKey**|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Colonne|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Colonne|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Colonne|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Colonne|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      Aucune colonne supprimée.
  
## <a name="Import"></a>Import the selected tables and column data  

Maintenant que vous avez prévisualisé et filtré les données inutiles, vous pouvez importer les données que vous ne souhaitez pas que les autres. L'Assistant importe les données de la table avec toutes les relations entre les tables. Nouvelles tables et colonnes sont créées dans le modèle et les données filtrées n’est pas importé.  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a>Pour importer les tables sélectionnées et les données de colonne  
  
1.  Vérifiez vos sélections. Si tout semble OK, cliquez sur **importation**. La boîte de dialogue de traitement de données affiche l’état des données importées à partir de votre source de données dans votre base de données de l’espace de travail.
  
    ![as-lesson2-success](../tutorial-tabular-1400/media/as-lesson2-success.png) 
  
2.  Cliquez sur **Fermer**.  

  
## <a name="save-your-model-project"></a>Enregistrer votre projet de modèle  

Il est important de sauvegarder fréquemment votre projet de modèle.  
  
#### <a name="to-save-the-model-project"></a>Pour enregistrer le projet de modèle  
  
-   Click **Fichier** > **Enregistrer tout**.  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?

[Leçon 3 : Marquer en tant que Table de dates](../tutorial-tabular-1400/as-lesson-3-mark-as-date-table.md).

  
  
