---
title: Ajouter une action d’extraction à un rapport (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 153729c4-d01e-4629-b78f-0cfd5a7f83da
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 0af8a88162eabd7058d81ef72586da8309cf36be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-drillthrough-action-on-a-report-report-builder-and-ssrs"></a>Ajouter une action d'extraction à un rapport (Générateur de rapports et SSRS)
  Le rapport qui s’ouvre quand vous cliquez sur le lien dans le rapport principal est appelé *rapport d’extraction*. Ce lien d'extraction autorise une action d'extraction.  
  
 Les rapports d'extraction doivent être publiés sur le même serveur de rapports que le rapport principal, mais pas nécessairement dans le même dossier. Vous pouvez ajouter un lien d’extraction à tout élément disposant d’une propriété **Action** , tel qu’une zone de texte, une image ou des points de données dans un graphique.  
  
 Pour ajouter un lien d'extraction à un rapport, vous devez d'abord créer le rapport d'extraction auquel le rapport principal sera lié. Un rapport d'extraction comporte généralement les détails relatifs à un élément contenu dans le rapport de synthèse d'origine ; en outre, il comporte souvent des paramètres qui filtrent le rapport d'extraction en fonction de paramètres qui lui sont passés à partir du rapport principal. Pour plus d’informations sur la création du rapport d’extraction, consultez [Rapports d’extraction &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drillthrough-reports-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-add-a-drillthrough-action"></a>Pour ajouter une action d'extraction  
  
1.  En mode Conception, cliquez avec le bouton droit sur la zone de texte, l’image ou le graphique auquel vous voulez ajouter un lien, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés** de l’élément, cliquez sur **Action**.  
  
3.  Sélectionnez **Atteindre le rapport**. Des sections supplémentaires apparaissent dans la boîte de dialogue pour cette option.  
  
4.  Dans **Spécifier un rapport**, cliquez sur **Parcourir** pour localiser le rapport auquel vous souhaitez accéder, ou tapez le nom du rapport. Vous pouvez également cliquer sur le bouton d’expression (**fx**) pour créer une expression pour le nom du rapport.  
  
     Le chemin d'accès au rapport d'extraction présente un format différent selon que vous êtes en mode natif ou en mode intégré SharePoint. Si vous utilisez la fonctionnalité Parcourir pour accéder au rapport, un chemin d'accès au format approprié est fourni. Pour plus d’informations, consultez [Spécification de chemins d’accès à des éléments externes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specifying-paths-to-external-items-report-builder-and-ssrs.md).  
  
     Si vous devez spécifier des paramètres pour le rapport d'extraction, effectuez l'étape suivante.  
  
5.  Dans **Utiliser ces paramètres pour exécuter le rapport**, cliquez sur **Ajouter**. Une nouvelle ligne est ajoutée à la grille des paramètres.  
  
    -   Dans la zone de texte **Nom** , cliquez sur la liste déroulante ou tapez le nom du paramètre de rapport dans le rapport d’extraction.  
  
        > [!NOTE]  
        >  Les noms dans la liste des paramètres doivent correspondre de façon exacte aux paramètres du rapport cible. Par exemple, les noms de paramètres doivent avoir une casse identique. Si les noms ne correspondent pas ou si un paramètre attendu ne figure pas dans la liste, la génération du rapport d'extraction échoue.  
  
    -   Dans **Valeur**, tapez ou sélectionnez la valeur à transmettre au paramètre dans le rapport d’extraction.  
  
        > [!NOTE]  
        >  Ces valeurs peuvent contenir une expression qui correspond à une valeur à passer au paramètre de rapport. Les expressions de la liste de valeurs incluent la liste des champs du rapport en cours.  
  
     Pour plus d’informations sur la façon de masquer des paramètres dans les rapports, consultez [Ajouter, modifier ou supprimer un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md).  
  
6.  (Facultatif) Pour les zones de texte, il est judicieux d’indiquer à l’utilisateur que le texte est un lien en changeant la couleur et l’effet du texte sous l’onglet **Accueil** du ruban.  
  
7.  Pour tester le lien, exécutez le rapport et cliquez sur l'élément de rapport sur lequel vous avez défini ce lien.  
  
## <a name="see-also"></a> Voir aussi  
 [Boîte de dialogue Propriétés relatives aux actions &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/2c5d915b-4f97-42cf-b8f1-49ca3ff3d0f9)   
 [Mise en forme des points de données sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [Afficher des info-bulles dans une série &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
  
