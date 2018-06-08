---
title: Mettre en forme du texte dans une zone de texte (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: df0794b5-96b0-4034-bd17-1be7f31e29db
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 2325ba67df6795e10faab50c51ffaaa7536f6721
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="format-text-in-a-text-box-report-builder-and-ssrs"></a>Mettre en forme du texte dans une zone de texte (Générateur de rapports et SSRS)
  Vous pouvez mettre en forme indépendamment toute partie du texte dans une zone de texte, et mélanger le texte de l'espace réservé à du texte statique dans une zone de texte. Cette possibilité de mélanger les formats et d'ajouter le texte d'espace réservé vous permet de créer des publipostages ou des modèles de texte dans votre rapport. Toute expression peut être définie et mise en forme séparément à l'aide d'un espace réservé.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-combine-multiple-formats-in-a-text-box"></a>Pour associer plusieurs formats dans une zone de texte  
  
1.  Sous l'onglet **Insérer** , cliquez sur **Zone de texte**. Cliquez sur l'aire de conception, puis faites glisser la souris pour créer une zone de la taille voulue.  
  
2.  Dans la zone de texte, sélectionnez le texte à mettre en forme.  
  
3.  Cliquez avec le bouton droit sur le texte sélectionné, puis cliquez sur **Propriétés du texte**.  
  
4.  Définissez les options de mise en forme. Par exemple, sous l’onglet **Général** :  
  
    -   **Info-bulle** Tapez du texte ou une expression qui se transforme en info-bulle. L'info-bulle apparaît lorsque l'utilisateur place le pointeur sur l'élément dans un rapport.  
  
    -   **Type de balise** Sélectionnez une option pour indiquer de quelle façon rendre le texte sélectionné :  
  
         **Texte brut** Affichez le texte sélectionné au format texte simple. Le format HTML sera traité comme texte littéral.  
  
         **HTML**  Affichez le texte sélectionné au format HTML. Si la valeur d'expression de l'espace réservé contient des balises HTML valides, ces balises seront rendues au format HTML. Pour plus d’informations, consultez [Importation de données HTML dans un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/importing-html-into-a-report-report-builder-and-ssrs.md).  
  
5.  Cliquez sur **OK**.  
  
6.  Répétez les étapes 2 à 5 pour tout texte supplémentaire que vous souhaitez mettre en forme.  
  
### <a name="to-format-text-and-placeholders-differently-in-the-same-text-box"></a>Pour mettre en forme différemment texte et espaces réservés dans la même zone de texte  
  
1.  Sous l’onglet **Insérer** , cliquez sur **Liste**. Cliquez sur l'aire de conception, puis faites glisser la souris pour créer une zone de la taille voulue. La boîte de dialogue **Propriétés du dataset** s'ouvre. Vous pouvez utiliser un dataset partagé ou un dataset incorporé dans votre rapport. Pour plus d’informations, cliquez sur [Boîte de dialogue Propriétés du dataset, Requête &#40;Générateur de rapports&#41;](../../reporting-services/report-data/dataset-properties-dialog-box-query-report-builder.md) ou [Boîte de dialogue Propriétés du dataset, Requête](http://msdn.microsoft.com/library/1fa34a4b-7de0-4e92-99fa-bc28a206773f).  
  
2.  Sous l'onglet **Insérer** , cliquez sur **Zone de texte**. Cliquez dans la liste, puis faites glisser la souris pour créer une zone de la taille voulue.  
  
3.  Tapez une étiquette dans la zone de texte, par exemple, **Mon champ**:.  
  
4.  Faites glisser un champ de votre dataset dans la zone de texte. Un espace réservé est créé pour votre champ.  
  
5.  Pour appliquer une mise en forme simple, sélectionnez le texte de l’espace réservé, puis cliquez sur l’une des options de mise en forme dans le groupe **Police** sous l’onglet **Accueil** . Par exemple, cliquez sur le bouton **Gras**.  
  
     Pour afficher davantage d’options de mise en forme, cliquez avec le bouton droit sur le texte de l’espace réservé, puis cliquez sur **Propriétés de l’espace réservé**.  
  
6.  Cliquez sur **OK**. En mode création de rapport, la zone de texte doit contenir «**Mon champ**: [*NomChamp*] », *NomChamp* étant le nom de votre champ.  
  
7.  Cliquez sur **Exécuter**.  
  
 La liste se répète une fois pour chaque valeur du champ, et l’espace réservé *NomChamp* est automatiquement remplacé par la valeur de ce champ dans le dataset.  
  
## <a name="see-also"></a> Voir aussi  
 [Zones de texte &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/text-boxes-report-builder-and-ssrs.md)   
 [Mise en forme du texte et des espaces réservés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Ajouter du code HTML à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-html-into-a-report-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Mise en forme des nombres et des dates &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md)   
 [Boîte de dialogue Propriétés de l’espace réservé, Général &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/7a867736-a3b0-4b5a-b3e5-fe7c8d7618a8)  
  
  
