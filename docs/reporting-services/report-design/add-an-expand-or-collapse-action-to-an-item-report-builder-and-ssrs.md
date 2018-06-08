---
title: Ajouter une action Développer ou Réduire à un élément (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 49f07ad6-242b-4861-8fc1-91ca78c36d6c
caps.latest.revision: 12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 727521df55b09dc4d28a1b79054c4ad8c411cb01
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-an-expand-or-collapse-action-to-an-item-report-builder-and-ssrs"></a>Ajouter une action Développer ou Réduire à un élément (Générateur de rapports et SSRS)
  Vous pouvez permettre à un utilisateur de développer ou de réduire interactivement des éléments de rapport, ou dans une table ou une matrice, de développer ou de réduire des lignes et des colonnes associées à un groupe. Pour autoriser les utilisateurs à développer ou réduire un élément, définissez les propriétés de visibilité de cet élément. La définition de la visibilité s'effectue dans une Visionneuse de rapports HTML et porte parfois le nom d'action d' *exploration* .  
  
 En mode création de rapport, vous spécifiez le nom de la zone de texte où vous souhaitez afficher les icônes de développement et de réduction. Dans le rapport rendu, la zone de texte affiche un signe plus (+) ou un signe moins (-) en plus de son contenu. Lorsque l'utilisateur clique sur la bascule, l'affichage de rapport est actualisé de manière à montrer ou masquer l'élément de rapport, en fonction des paramètres de visibilité actuels des éléments du rapport.  
  
 En règle générale, l'action de développement et réduction permet d'afficher uniquement les données de synthèse et fournit à l'utilisateur la possibilité de cliquer sur le signe plus afin d'afficher les données de détail. Par exemple, vous pouvez à la base masquer une table qui indique les valeurs d'un graphique ou masquer les groupes enfants d'une table avec des groupes de lignes et de colonnes imbriqués, comme dans un rapport d'extraction.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-add-expand-and-collapse-action-to-a-group"></a>Pour ajouter une action Développer/Réduire à un groupe  
  
1.  En mode création de rapport, cliquez sur le tableau ou la matrice pour les sélectionner. Le volet Regroupement affiche les groupes de lignes et de colonnes.  
  
     ![Volet de regroupement](../../reporting-services/report-design/media/groupingpane.png "Volet de regroupement")  
  
     Si le volet de regroupement ne s'affiche pas, cliquez sur le menu **Affichage** , puis cliquez sur **Regroupement**.  
  
2.  Cliquez avec le bouton droit n’importe où dans la barre de titre du volet de regroupement, puis cliquez sur **Avancé**. Le volet Regroupement bascule dans un autre mode pour afficher la structure d'affichage sous-jacente des lignes et des colonnes sur l'aire de conception.  
  
     ![Volet de regroupement avec menu Mode avancé](../../reporting-services/report-design/media/groupingpane-advancedmode.png "Volet de regroupement avec menu Mode avancé")  
  
3.  Dans le volet Groupe approprié, cliquez sur le nom du groupe de lignes ou du groupe de colonnes dont vous souhaitez masquer les lignes ou les colonnes qui y sont associées. Le groupe est sélectionné et le volet Propriétés affiche les propriétés **Membre du tableau matriciel** .  
  
    > [!NOTE]  
    >  Si vous ne voyez pas le volet Propriétés, cliquez sur **Affichage** sur le ruban, puis cliquez sur **Propriétés**.  
  
4.  Dans **Masqué**, choisissez l'une des options suivantes pour définir la visibilité de cet élément de rapport la première fois que vous exécutez un rapport :  
  
    -   Sélectionnez **False** pour afficher l'élément de rapport.  
  
    -   Sélectionnez **True** pour masquer l'élément de rapport.  
  
    -   Sélectionnez **\<Expression>** pour ouvrir la boîte de dialogue **Expression** et créer une expression qui est évaluée au moment de l’exécution pour déterminer la visibilité.  
  
5.  Dans **ToggleItem**, sélectionnez dans la liste déroulante le nom d’une zone de texte à laquelle ajouter l’image bascule.  
  
     Dans l'image suivante, le groupe de lignes Couleur est configuré de façon à permettre aux utilisateurs de développer et de réduire les lignes associées.  
  
     ![Configuration d’un groupe de lignes à développer](../../reporting-services/report-design/media/expandcollapse-confighiddentoggleitemwithnumbers.png "Configuration d’un groupe de lignes à développer")  
  
    > [!NOTE]  
    >  La zone de texte avec l'image bascule ne peut pas être le groupe de lignes ou de colonnes dont vous souhaitez masquer les lignes ou les colonnes qui y sont associées. Elle doit figurer dans le même groupe que l'élément qui est en train d'être masqué ou dans un groupe ancêtre. Par exemple, pour afficher ou masquer les lignes associées à un groupe enfant, sélectionnez une zone de texte dans une ligne associée au groupe parent.  
  
6.  Pour tester la bascule, exécutez le rapport et cliquez sur la zone de texte comportant l'image bascule. L'affichage du rapport est actualisé de manière à afficher des groupes de lignes et des groupes de colonnes avec leur visibilité basculée.  
  
     ![Exécution du rapport avec développement d’un groupe de lignes](../../reporting-services/report-design/media/expandcollapse-runreport-rowgroup.png "Exécution du rapport avec développement d’un groupe de lignes")  
  
### <a name="to-add-expand-and-collapse-action-to-a-report-item"></a>Pour ajouter une action Développer/Réduire à un élément de rapport  
  
1.  Dans la vue Création de rapport, cliquez avec le bouton droit sur l’élément de rapport à afficher ou à masquer, puis cliquez sur **Propriétés** de *\<élément_de_rapport>*. La boîte de dialogue **Propriétés** de *\<élément de rapport>* correspondant à l’élément de rapport s’ouvre.  
  
2.  Cliquez sur **Visibilité**.  
  
3.  Dans **Lors de la première exécution du rapport**, choisissez l'une des options suivantes pour définir la visibilité de cet élément de rapport la première fois vous exécutez un rapport :  
  
    -   Sélectionnez **Afficher** pour afficher l'élément de rapport.  
  
    -   Sélectionnez **Masquer** pour masquer l'élément de rapport.  
  
    -   Sélectionnez **Afficher ou masquer en fonction d'une expression** pour utiliser une expression évaluée au moment de l'exécution pour déterminer la visibilité. Cliquez sur (**fx**) pour ouvrir la boîte de dialogue **Expression** et créer une expression.  
  
        > [!NOTE]  
        >  Quand vous spécifiez une expression de visibilité, vous définissez la propriété Hidden de l’élément de rapport. L'expression prend la valeur **Boolean** **True** pour masquer l'élément, et la valeur **False** pour l'afficher.  
  
4.  Dans **L’affichage peut être activé ou désactivé par cet élément de rapport**, tapez ou sélectionnez dans la liste déroulante le nom d’une zone de texte du rapport où afficher une image bascule, par exemple Textbox1.  
  
     Dans l'image suivante, la table est configurée de manière à permettre aux utilisateurs de la développer et de la réduire. L'affichage de la table peut être activé et désactivé depuis la zone de texte Table de Produits.  
  
     ![Configurer une table rapport à développer](../../reporting-services/report-design/media/expandcollapse-reporttable.png "Configurer une table rapport à développer")  
  
    > [!NOTE]  
    >  La zone de texte que vous choisissez doit figurer dans l'étendue actuelle ou contenante de cet élément de rapport (jusqu'au corps du rapport inclus). Par exemple, pour afficher ou masquer un graphique, sélectionnez une zone de texte qui est dans la même étendue contenante que le graphique, par exemple le corps du rapport ou un rectangle. La zone de texte doit figurer dans la même hiérarchie de conteneurs ou à un niveau plus élevé.  
  
5.  Pour tester la bascule, exécutez le rapport et cliquez sur la zone de texte comportant l'image bascule. L'affichage du rapport est actualisé de manière à afficher les éléments du rapport avec leur visibilité basculée.  
  
     ![Exécution du rapport avec développement d’une table](../../reporting-services/report-design/media/expandcollapse-runreport-reporttable.png "Rapport en cours d’exécution avec développement d’une table")  
  
## <a name="see-also"></a> Voir aussi  
 [Action d’exploration &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drilldown-action-report-builder-and-ssrs.md)   
 [Masquer un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/hide-an-item-report-builder-and-ssrs.md)  
  
  
