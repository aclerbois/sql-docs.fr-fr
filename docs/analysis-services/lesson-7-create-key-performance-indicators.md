---
title: 'Leçon 8 : Créer des indicateurs de Performance clés | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6e56d4a533caaf95077eb06fabb5fd0bc0c42b07
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-7-create-key-performance-indicators"></a>Leçon 7 : Créer des indicateurs de Performance clés
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Au cours de cette leçon, vous allez créer des indicateurs de performance clés (KPI). Les KPI évaluent la performance d’une valeur, définie par une mesure de *base* , par rapport à une valeur *cible* , également définie par une mesure ou par une valeur absolue. Dans les applications clientes de création de rapports, les indicateurs de performance clés offrent aux professionnels un moyen d'obtenir rapidement et aisément un récapitulatif d'un succès commercial ou d'identifier les tendances. Pour plus d’informations, consultez [indicateurs de performance clés](../analysis-services/tabular-models/kpis-ssas-tabular.md).  
  
Durée estimée pour effectuer cette leçon : **15 minutes**  
  
## <a name="prerequisites"></a>Conditions préalables  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [Leçon 6 : créer des mesures](../analysis-services/lesson-6-create-measures.md).   
  
## <a name="create-key-performance-indicators"></a>Créer des indicateurs de performance clé (KPI)  
  
#### <a name="to-create-an-internetcurrentquartersalesperformance-kpi"></a>Pour créer un KPI InternetCurrentQuarterSalesPerformance  
  
1.  Dans le Générateur de modèles, cliquez sur le **FactInternetSales** table (onglet).  
  
2.  Dans la grille de mesures, cliquez sur une cellule vide.  
  
3.  Dans la barre de formule au-dessus de la table, tapez la formule suivante : 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=IFERROR([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    Cette mesure servira de mesure de base à l'indicateur de performance clé.  
  
4.  Avec le bouton droit **InternetCurrentQuarterSalesPerformance** > **créer un KPI**.   
  
5.  Dans la boîte de dialogue indicateur de Performance clé (KPI) dans **cible** sélectionnez **valeur absolue**, puis tapez **1.1**.  
  
7.  Dans le secteur gauche (bas) du curseur, tapez **1**, puis dans le secteur droit (élevé) du curseur, tapez **1.07**.  
  
8.  Dans **Sélectionner le style d’icône**, sélectionnez le type d’icône losange (rouge), triangle (jaune) et cercle (vert).
  
    ![en tant que-tabulaire-lesson7-indicateur de performance clé](../analysis-services/media/as-tabular-lesson7-kpi.png)
    
    > [!TIP]  
    > Notez l’extensible **Descriptions** étiquette sous les styles d’icônes disponibles. Cela permet d’entrer des descriptions pour les différents éléments de l’indicateur de performance clé pour les rendre plus identifiable dans les applications clientes.  
  
9. Cliquez sur **OK** pour terminer le KPI.  
  
    Dans la grille de mesures, remarquez l’icône en regard du **InternetCurrentQuarterSalesPerformance** mesure. Cette icône indique que cette mesure sert de valeur de base à un indicateur de performance clé.  
  
#### <a name="to-create-an-internetcurrentquartermarginperformance-kpi"></a>Pour créer un KPI InternetCurrentQuarterMarginPerformance  
  
1.  Dans la grille de mesures pour le **FactInternetSales** , cliquez sur une cellule vide.  
  
2.  Dans la barre de formule au-dessus de la table, tapez la formule suivante :  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  Avec le bouton droit **InternetCurrentQuarterMarginPerformance** > **créer un KPI**.  
  
4.  Dans la boîte de dialogue indicateur de Performance clé (KPI) dans **cible** sélectionnez **valeur absolue**, puis tapez **1.25**.   
  
5.  Dans **Définir les seuils d’état**, faites glisser le secteur gauche (bas) du curseur jusqu’à ce que le champ affiche **0.8**, puis faites glisser le secteur droit (élevé) du curseur jusqu’à ce que le champ affiche **1.03**.  
  
6.  Dans **Sélectionner le style d’icône**, sélectionnez le type d’icône losange (rouge), triangle (jaune) et cercle (vert), puis cliquez sur **OK**.  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?
Accédez à la leçon suivante : [leçon 8 : créer des Perspectives](../analysis-services/lesson-8-create-perspectives.md).
  
  
