---
title: 'Leçon 11 : Créer des Partitions | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d738649ea357b172975505ff7993b56181ce0b4f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-10-create-partitions"></a>Leçon 10 : Créer des Partitions
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans cette leçon, vous allez créer les partitions pour diviser la table FactInternetSales en parties logiques plus petites qui peuvent être traitées (actualisée) indépendamment d’autres partitions. Par défaut, chaque table que vous incluez dans votre modèle a une partition qui comprend toutes les lignes et colonnes de la table. Pour la table FactInternetSales, nous souhaitons diviser les données par année ; une seule partition pour chacune des cinq années de la table. Chaque partition peut ensuite être traitée indépendamment. Pour plus d’informations, consultez [Partitions](../analysis-services/tabular-models/partitions-ssas-tabular.md).  
  
Durée estimée pour effectuer cette leçon : **15 minutes**  
  
## <a name="prerequisites"></a>Conditions préalables  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 9 : créer des hiérarchies](../analysis-services/lesson-9-create-hierarchies.md).  
  
## <a name="create-partitions"></a>Créer des partitions  
  
#### <a name="to-create-partitions-in-the-factinternetsales-table"></a>Pour créer des partitions dans la table FactInternetSales  
  
1.  Dans l’Explorateur de modèles tabulaires, développez **Tables**, avec le bouton droit **FactInternetSales** > **Partitions**.  
  
2.  Dans la boîte de dialogue Gestionnaire de Partition, cliquez sur **copie**.  
  
3.  Dans **nom de la Partition**, remplacez le nom par **FactInternetSales2010**.  
  
    > [!TIP]  
    > Notez que les noms de colonnes dans la fenêtre d’aperçu de la Table affichent les colonnes incluses dans la table de modèle (activée) avec les noms de colonnes de la source. Cela est dû au fait que la fenêtre d'aperçu de la table affiche les colonnes de la table source, pas de la table modèle.  
  
4.  Sélectionnez le **SQL** bouton juste au-dessus du côté droit de la fenêtre d’aperçu pour ouvrir l’éditeur de l’instruction SQL.  
  
    Étant donné que vous souhaitez que la partition inclue uniquement les lignes appartenant à une certaine période, vous devez inclure une clause WHERE. Vous pouvez créer une clause WHERE uniquement à l'aide d'une instruction SQL.  
  
5.  Dans le **instruction SQL** champ, remplacez l’instruction existante par copier-coller l’instruction suivante :  
  
    ```  
    SELECT   
    [dbo].[FactInternetSales].[ProductKey],  
    [dbo].[FactInternetSales].[CustomerKey],  
    [dbo].[FactInternetSales].[PromotionKey],  
    [dbo].[FactInternetSales].[CurrencyKey],  
    [dbo].[FactInternetSales].[SalesTerritoryKey],  
    [dbo].[FactInternetSales].[SalesOrderNumber],  
    [dbo].[FactInternetSales].[SalesOrderLineNumber],  
    [dbo].[FactInternetSales].[RevisionNumber],  
    [dbo].[FactInternetSales].[OrderQuantity],  
    [dbo].[FactInternetSales].[UnitPrice],  
    [dbo].[FactInternetSales].[ExtendedAmount],  
    [dbo].[FactInternetSales].[UnitPriceDiscountPct],  
    [dbo].[FactInternetSales].[DiscountAmount],  
    [dbo].[FactInternetSales].[ProductStandardCost],  
    [dbo].[FactInternetSales].[TotalProductCost],  
    [dbo].[FactInternetSales].[SalesAmount],  
    [dbo].[FactInternetSales].[TaxAmt],  
    [dbo].[FactInternetSales].[Freight],  
    [dbo].[FactInternetSales].[CarrierTrackingNumber],  
    [dbo].[FactInternetSales].[CustomerPONumber],  
    [dbo].[FactInternetSales].[OrderDate],  
    [dbo].[FactInternetSales].[DueDate],  
    [dbo].[FactInternetSales].[ShipDate]   
    FROM [dbo].[FactInternetSales]  
    WHERE (([OrderDate] >= N'2010-01-01 00:00:00') AND ([OrderDate] < N'2011-01-01 00:00:00'))  
    ```  
  
    Cette instruction spécifie que la partition doit inclure toutes les données des lignes où OrderDate correspond à l’année calendaire 2010, comme spécifié dans la clause WHERE.  
  
6.  Cliquez sur **Valider**.  
  
  
#### <a name="to-create-a-partition-for-the-2011-year"></a>Pour créer une partition pour l’année 2011  
  
1.  Dans la liste des partitions, cliquez sur le **FactInternetSales2010** vous venez de créer la partition, puis cliquez sur **copie**.  
  
2.  Dans **nom de la Partition**, type **FactInternetSales2011**.  
  
3.  Dans l’instruction SQL, pour que la partition inclue uniquement les lignes pour l’année 2011, remplacez la clause WHERE par :  
  
    ```  
    WHERE (([OrderDate] >= N'2011-01-01 00:00:00') AND ([OrderDate] < N'2012-01-01 00:00:00'))  
    ```  
  
#### <a name="to-create-a-partition-for-the-2012-year"></a>Pour créer une partition pour l’année 2012  
  
- Suivez les étapes ci-dessus, à l’aide de la clause WHERE suivante. 
  
    ```  
    WHERE (([OrderDate] >= N'2012-01-01 00:00:00') AND ([OrderDate] < N'2013-01-01 00:00:00'))  
    ```  
  
#### <a name="to-create-a-partition-for-the-2013-year"></a>Pour créer une partition pour l’année 2013  
  
- Suivez les étapes ci-dessus, à l’aide de la clause WHERE suivante. 
  
    ```  
    WHERE (([OrderDate] >= N'2013-01-01 00:00:00') AND ([OrderDate] < N'2014-01-01 00:00:00'))  
    ```  
  
#### <a name="to-create-a-partition-for-the-2014-year"></a>Pour créer une partition pour l’année 2014  
  
- Suivez les étapes ci-dessus, à l’aide de la clause WHERE suivante. 
  
    ```  
    WHERE (([OrderDate] >= N'2014-01-01 00:00:00') AND ([OrderDate] < N'2015-01-01 00:00:00'))  
    ```  

## <a name="delete-the-factinternetsales-partition"></a>Supprimer la partition FactInternetSales
Maintenant que vous avez des partitions pour chaque année, vous pouvez supprimer la partition FactInternetSales. Cela empêche le chevauchement lors du choix de traiter toutes les lors du traitement de partitions.
#### <a name="to-delete-the-factinternetsales-partition"></a>Pour supprimer la partition FactInternetSales
-  Cliquez sur la partition FactInternetSales, puis cliquez sur **supprimer**.



## <a name="process-partitions"></a>Traiter les partitions  
Dans le Gestionnaire de Partition, notez le **traités dernière** colonne pour chacune des nouvelles partitions vous montre simplement créé ces partitions n’ont jamais été traitées. Lorsque vous créez de nouvelles partitions, vous devez exécuter une opération « Traiter les partitions » ou « Traiter la table » pour actualiser les données dans ces partitions.  
  
#### <a name="to-process-the-factinternetsales-partitions"></a>Pour traiter les partitions FactInternetSales  
  
1.  Cliquez sur **OK** pour fermer la boîte de dialogue Gestionnaire de Partition.  
  
2.  Cliquez sur le **FactInternetSales** de table, puis cliquez sur le **modèle** menu > **processus** > **traiter les Partitions**.  
  
3.  Dans la boîte de dialogue traiter les Partitions, vérifiez **Mode** a la valeur **traiter par défaut**.  
  
4.  Cochez la case dans la colonne **Traiter** pour chacune des cinq partitions que vous avez créées, puis cliquez sur **OK**.  

    ![en tant que-tabulaire-lesson10-processus-partitions](../analysis-services/media/as-tabular-lesson10-process-partitions.png)
  
    Si vous êtes invité à entrer des informations d’identification d’emprunt d’identité, entrez le nom d’utilisateur Windows et un mot de passe spécifié dans la leçon 2.  
  
    La boîte de dialogue **Traitement des données** apparaît et affiche les détails du traitement pour chaque partition. Notez qu'un nombre de lignes différent est transféré pour chaque partition. Cela est dû au fait que chaque partition contient uniquement les lignes de l'année spécifiée dans la clause WHERE dans l'instruction SQL. Quand le traitement est terminé, continuez et fermez la boîte de dialogue Traitement des données.  
  
    ![en tant que-tabulaire-lesson10-processus-terminé](../analysis-services/media/as-tabular-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a>Quelle est l’étape suivante ?
Accédez à la leçon suivante : [leçon 11 : créer des rôles](../analysis-services/lesson-11-create-roles.md). 
