---
title: Créer un rapport en escalier (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 5933c4f0-c713-4ecb-b521-ff46c9c63fff
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 7644c7d01ea8d12864cf402543f031f137aed383
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-stepped-report-report-builder-and-ssrs"></a>Créer un rapport en escalier (Générateur de rapports et SSRS)
Un rapport en escalier est un type de rapport paginé  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui affiche des lignes de détails ou des groupes enfants mis en retrait sous un groupe parent dans la même colonne, comme l’illustre l’exemple ci-dessous :  
  
 ![Rendu de rapport en escalier](../../reporting-services/report-design/media/steppedreportrendered.gif "Rendu de rapport en escalier")  
  
 Les rapports traditionnels sous forme de table placent le groupe parent dans une colonne adjacente dans le rapport. La nouvelle région de données de tableau matriciel vous permet d’ajouter un groupe et des lignes de détails ou des groupes enfants à la même colonne. Pour différencier les lignes de groupe des lignes de détails ou de groupes enfants, vous pouvez appliquer une mise en forme telle qu'une couleur de police ou vous pouvez mettre en retrait les lignes de détails.  
  
 Les procédures de cette rubrique vous expliquent comment créer manuellement un rapport en escalier, mais vous pouvez également utiliser l'Assistant Nouveau tableau ou nouvelle matrice. Il fournit la mise en page pour les rapports en escalier, ce qui en facilite la création. Après avoir exécuté l'Assistant, vous pouvez améliorer le rapport.  
  
> [!NOTE]  
>  L'Assistant est disponible uniquement dans le Générateur de rapports.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-create-a-stepped-report"></a>Pour créer un rapport en escalier  
  
1.  Créez un rapport de table. Par exemple, insérez une région de données de tableau matriciel et ajoutez des champs à la ligne de données.  
  
2.  Ajoutez un groupe parent à votre rapport.  
  
    1.  Cliquez n'importe où dans la table pour la sélectionner. Le volet de regroupement affiche le groupe de détails dans le volet Groupes de lignes.  
  
    2.  Dans le volet de regroupement, cliquez avec le bouton droit sur le groupe de détails, pointez sur **Ajouter un groupe**, puis cliquez sur **Groupe parent**.  
  
    3.  Dans la boîte de dialogue **Groupe de tableau matriciel** , indiquez un nom pour le groupe et tapez ou sélectionnez une expression de groupe dans la liste déroulante. La liste déroulante affiche les expressions de champ simples qui sont disponibles dans le volet des données de rapport. Par exemple, [PostalCode] est une expression de champ simple pour le champ PostalCode d'un dataset.  
  
    4.  Sélectionnez **Ajouter l’en-tête du groupe**. Cette option ajoute une ligne statique au-dessus du groupe pour l'étiquette et les totaux du groupe. De la même manière, vous pouvez sélectionner **Ajouter le pied de page du groupe** pour ajouter une ligne statique au-dessous du groupe. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Vous disposez maintenant d'un rapport tabulaire de base. Lorsqu'il est rendu, une colonne s'affiche avec la valeur d'instance de groupe, et une ou plusieurs colonnes avec les données de détail groupées. L'illustration suivante montre l'apparence de la région de données sur l'aire de conception.  
  
     ![Région de données de table avec groupe](../../reporting-services/report-design/media/tabledataregionwithgroup.gif "Région de données de table avec groupe")  
  
     L'illustration suivante montre l'apparence de la région de données rendue lorsque vous affichez le rapport.  
  
     ![Rendu de rapport groupé](../../reporting-services/report-design/media/tablereportrendered.gif "Rendu de rapport groupé")  
  
3.  Pour un rapport par palier, vous n'avez pas besoin de la première colonne qui affiche l'instance de groupe. À la place, copiez la valeur de la cellule d'en-tête de groupe, supprimez la colonne de groupe et collez la valeur dans la première zone de texte de la ligne d'en-tête de groupe. Pour supprimer la colonne de groupe, cliquez avec le bouton droit sur la colonne de groupe ou sur la cellule, puis cliquez sur **Supprimer les colonnes**. L'illustration suivante montre l'apparence de la région de données sur l'aire de conception.  
  
     ![Région de données avec ligne d’en-tête de groupe](../../reporting-services/report-design/media/tabledataregiongroupheader.gif "Région de données avec ligne d’en-tête de groupe")  
  
4.  Pour mettre en retrait les lignes de détails sous la ligne d'en-tête de groupe dans la même colonne, modifiez la marge intérieure de la cellule de données de détail.  
  
    1.  Sélectionnez la cellule avec le champ de détail que vous souhaitez mettre en retrait. Les propriétés de la zone de texte de cette cellule s'affichent dans le volet Propriétés.  
  
    2.  Dans le volet Propriétés, sous **Alignement**, développez les propriétés pour **Marge intérieure**.  
  
    3.  Pour **Gauche**, tapez une nouvelle valeur de marge intérieure, telle que **0,5 in**. La marge intérieure met en retrait le texte de la cellule selon la valeur que vous avez spécifiée. La valeur par défaut de la marge intérieure est égale à 2 points. Les valeurs valides pour les propriétés Padding sont zéro ou une valeur positive, suivis d'un indicateur de taille.  
  
         Les indicateurs de taille sont :  
  
        |||  
        |-|-|  
        |**in**|Pouces (1 pouce = 2,54 centimètres)|  
        |**cm**|Centimètres|  
        |**mm**|Millimètres|  
        |**pt**|Points (1 point = 1/72 pouce)|  
        |**pc**|Picas (1 pica = 12 points)|  
  
     Votre région de données doit ressembler à l'exemple suivant :  
  
     ![Région de données pour un rapport par palier](../../reporting-services/report-design/media/steppedreportdataregion.gif "Région de données pour un rapport par palier")  
  
     **Région de données pour une mise en page de rapport en escalier**  
  
     Sous l’onglet **Accueil** , cliquez sur **Exécuter**. Le rapport affiche le groupe avec des niveaux en retrait pour les valeurs des groupes enfants.  
  
## <a name="to-create-a-stepped-report-with-multiple-groups"></a>Pour créer un rapport par palier avec plusieurs groupes  
  
1.  Créez un rapport selon les indications de la procédure précédente.  
  
2.  Ajoutez des groupes supplémentaires à votre rapport.  
  
    1.  Dans le volet Groupes de lignes, cliquez avec le bouton droit sur le groupe, cliquez sur **Ajouter un groupe**, puis choisissez le type du groupe à ajouter.  
  
        > [!NOTE]  
        >  Il existe plusieurs façons d'ajouter des groupes à une région de données. Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md).  
  
    2.  Dans la boîte de dialogue **Groupe de tableaux matriciels** , tapez un nom.  
  
    3.  Dans **Expression Groupe**, tapez une expression ou sélectionnez le champ de dataset sur lequel effectuer le regroupement. Pour créer une expression, cliquez sur le bouton d’expression (**fx**) pour ouvrir la boîte de dialogue **Expression** .  
  
    4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Modifiez la marge intérieure de la cellule qui affiche les données du groupe.  
  
## <a name="see-also"></a> Voir aussi  
 [En-têtes et pieds de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/page-headers-and-footers-report-builder-and-ssrs.md)   
 [Mise en forme des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)   
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md)   
 [Matrices &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
