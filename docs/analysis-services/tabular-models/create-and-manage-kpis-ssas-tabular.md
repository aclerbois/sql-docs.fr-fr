---
title: "Créer et gérer des indicateurs de performance clés | Documents Microsoft"
ms.custom: 
ms.date: 02/22/2018
ms.prod: analysis-services
ms.prod_service: analysis-services, azure-analysis-services
ms.service: 
ms.component: data-mining
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- sql13.asvs.bidtoolset.kpi.f1
ms.assetid: c96026c2-4394-4c3c-986b-4c95a4421900
caps.latest.revision: 
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: b16307f79552529c2afc0c1d298c68a85ff1f2a1
ms.sourcegitcommit: d8ab09ad99e9ec30875076acee2ed303d61049b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="create-and-manage-kpis"></a>Créer et gérer des indicateurs de performance clés 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Cet article décrit comment créer, modifier ou supprimer un KPI (indicateur de Performance clé) dans un modèle tabulaire. Pour créer un indicateur de performance clé (KPI), sélectionnez une mesure qui donne la valeur de base de l'indicateur de performance clé. Utilisez ensuite la boîte de dialogue Indicateur de performance clé pour sélectionner une seconde mesure ou une valeur absolue qui prend une valeur cible. Vous pourrez ensuite définir des seuils d'état qui mesurent les performances entre les mesures de base et cible.  
  
## <a name="tasks"></a>Tâches  
  
> [!IMPORTANT]  
>  Avant de créer un KPI, vous devez d'abord créer une mesure de base qui retourne une valeur. La mesure de base doit ensuite être étendue à un KPI. Comment créer des mesures sont décrits dans une autre rubrique, [créer et gérer des mesures](../../analysis-services/tabular-models/create-and-manage-measures-ssas-tabular.md). Un KPI nécessite également une valeur cible. Cette valeur peut être obtenue à partir d'une autre valeur prédéfinie ou d'une valeur absolue. Une fois que vous avez étendu une mesure de base à un KPI, vous pouvez sélectionner la valeur cible et définir les seuils d’état dans la boîte de dialogue Indicateur de performance clé.  
  
###  <a name="bkmk_create_KPI"></a> Pour créer un KPI  
  
1.  Dans la Grille de mesures, cliquez avec le bouton droit sur la mesure qui sert de mesure de base (valeur), puis cliquez sur **Créer un KPI**.  
  
2.  Dans la boîte de dialogue **Indicateur de performance clé** , dans **Définir la valeur cible**, sélectionnez l’une des options suivantes :  
  
     Sélectionnez **Mesure**, puis sélectionnez une mesure cible dans la zone de liste.  
  
     Sélectionnez **Valeur absolue**, puis entrez une valeur numérique.  
  
3.  Dans **Définir les seuils d’état**, cliquez sur les valeurs des seuils et faites-les glisser.  
  
4.  Dans **Sélectionner le style d’icône**, cliquez sur un type d’image.  
  
5.  Cliquez sur **Descriptions**, puis tapez une description du KPI, de la valeur, de l’état et de la cible.  
  
> [!TIP]  
>  Vous pouvez utiliser la fonctionnalité Analyser dans Excel pour tester votre indicateur de performance clé. Pour plus d’informations, consultez [analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md).  
  
###  <a name="bkmk_edit_KPI"></a> Pour modifier un KPI  
  
-   Dans la grille de mesures, cliquez avec le bouton droit sur la mesure qui sert de mesure de base (valeur) du KPI, puis cliquez sur **Modifier les paramètres du KPI**.  
  
###  <a name="bkmk_delete"></a> Pour supprimer un KPI et la mesure de base  
  
-   Dans la grille de mesures, cliquez avec le bouton droit sur la mesure qui sert de mesure de base (valeur) du KPI, puis cliquez sur **Supprimer**.  
  
###  <a name="bkmk_delete_KPI"></a> Pour supprimer un KPI, mais conserver la mesure de base  
  
-   Dans la grille de mesures, cliquez avec le bouton droit sur la mesure qui sert de mesure de base (valeur) du KPI, puis cliquez sur **Supprimer le KPI**.  
  
## <a name="alt-shortcuts"></a>Raccourcis ALT  
  
|Section de l'interface utilisateur|Combinaison de touches|  
|----------------|-----------------|  
|Mesure de base du KPI|Alt+B|  
|État du KPI|Alt+S|  
|Measure|Alt+M|  
|Valeur absolue|Alt+A|  
|Définir les seuils d’état|Alt+U|  
|Sélectionner le style d’icône|Alt+I|  
|Tendance|Alt+T|  
|Descriptions|Alt+D|  
|Tendance|Alt+T|  
  
## <a name="see-also"></a>Voir aussi  
 [KPIs](../../analysis-services/tabular-models/kpis-ssas-tabular.md)   
 [Mesures](../../analysis-services/tabular-models/measures-ssas-tabular.md)   
 [Créer et managemeasures](../../analysis-services/tabular-models/create-and-manage-measures-ssas-tabular.md)  
  
  
