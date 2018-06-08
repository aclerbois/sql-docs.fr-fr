---
title: 'Leçon 4 : Marquer en tant que Table de dates | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 77f29250621485f5606a0bf33615e8d15eb7d80b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-3-mark-as-date-table"></a>Leçon 3 : Marquer en tant que Table de dates
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans la lecon 2 : Ajouter des données, vous avez importé une table de dimension nommée DimDate. Alors que dans votre modèle, cette table est nommée DimDate, il peut également être appelé une *table de dates*, car elle contient des données de date et d’heure.  
  
Chaque fois que vous utilisez les fonctions time intelligence DAX dans les calculs, comme vous allez effectuer lorsque vous créez des mesures un peu plus tard, vous devez spécifier les propriétés de table date, qui incluent un *table de dates* et un identificateur unique *colonne de Date* dans cette table.
  
Dans cette leçon, vous devez marquer la table DimDate comme le *table de dates* et la colonne de Date (dans la table Date) en tant que le *colonne de Date* (identificateur unique).  

Avant de nous marquer la table de dates et une colonne de date, nous devons faire un peu housekeeping pour faciliter la compréhension de notre modèle. Vous remarquerez que dans la table DimDate une colonne nommée **FullDateAlternateKey**. Il contient une ligne pour chaque jour de l’année civile incluse dans la table. Nous utiliserons cette colonne beaucoup dans les formules de mesure et dans les rapports. Toutefois, FullDateAlternateKey n’est pas vraiment un bon identificateur pour cette colonne. Nous allons renommer **Date**, rendant ainsi plus faciles à identifier et à inclure dans les formules. Chaque fois que possible, il est judicieux de renommer des objets tels que des tables et des colonnes pour les rendre plus facilement repérable dans les applications telles que Power BI et Excel de création de rapports. 
  
Durée estimée pour effectuer cette leçon : **3 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 2 : ajouter des données](../analysis-services/lesson-2-add-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Pour renommer la colonne FullDateAlternateKey

1.  Dans le Générateur de modèles, cliquez sur le **DimDate** table.

2.  Double-cliquez sur l’en-tête pour le **FullDateAlternateKey** colonne, puis renommez-le à **Date**.

  
### <a name="to-set-mark-as-date-table"></a>Pour définir Marquer en tant que table de dates  
  
1.  Sélectionnez la colonne **Date** , puis dans la fenêtre **Propriétés** , sous **Type de données**, vérifiez que  **Date** est sélectionné.  
  
2.  Cliquez sur le menu **Table** , cliquez sur **Date**, puis sur **Marquer comme Table de Date**.  
  
3.  Dans la boîte de dialogue **Marquer comme Table de Date** , dans la zone de liste **Date** , sélectionnez la colonne **Date** comme identificateur unique. Il sera généralement être sélectionné par défaut. Cliquez sur **OK**. 

    ![en tant que tableau-lesson3-date-table](../analysis-services/media/as-tabular-lesson3-date-table.png)
  

## <a name="whats-next"></a>Quelle est l’étape suivante ?
Accédez à la leçon suivante : [leçon 4 : créer des relations](../analysis-services/lesson-4-create-relationships.md).
  
