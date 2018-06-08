---
title: Ajouter un tri interactif à un tableau ou une matrice (Générateur de rapports et SSRS) | Microsoft Docs
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
f1_keywords:
- "10121"
- sql13.rtp.rptdesigner.textboxproperties.intrctvsort.f1
ms.assetid: 05819637-729b-4cf6-82de-91a99f184ec6
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: bd4c0edc746c0bc72652343d986ecb1d9f34ce59
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-interactive-sort-to-a-table-or-matrix-report-builder-and-ssrs"></a>Ajouter un tri interactif à un tableau ou une matrice (Générateur de rapports et SSRS)
  Ajoutez des boutons de tri interactif pour permettre aux utilisateurs de modifier l'ordre de tri des lignes et des colonnes dans les tables et les matrices. Cette fonctionnalité est prise en charge uniquement dans les formats de rendu qui prennent en charge les interactions avec l'utilisateur, tels que le format HTML.  
  
 Lorsque vous créez un bouton de tri interactif, vous devez spécifier les éléments à trier, les éléments selon lesquels effectuer le tri et l'étendue d'application du tri. Par exemple, vous pouvez trier des lignes de détails par nom de client, des valeurs de groupe de sous-catégories dans un groupe de catégories par vente ou des valeurs de groupe de catégories et de sous-catégories combinées par total.  
  
 Lorsque vous affichez le rapport, les colonnes qui prennent en charge le tri interactif présentent des icônes fléchées qui varient en fonction de l'ordre de tri. La première fois que vous cliquez sur un bouton de tri interactif, les éléments sont triés par ordre croissant. Les clics suivants permettent de passer de l'ordre croissant à l'ordre décroissant et inversement.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="BackToTop"></a> Dans cet article  
 [Tri de lignes de détails dans une table sans groupe](#SortingDetailRows)  
  
 [Tri d'un groupe de lignes parent de niveau supérieur dans une table ou une matrice](#SortingTopLevelParent)  
  
 [Tri de groupes enfants ou de lignes de détails dans un groupe](#SortingChildGroups)  
  
 [Tri de lignes en fonction d'une expression de groupe complexe](#SortingMultipleRowGroups)  
  
 [Synchronisation de l'ordre de tri pour plusieurs régions de données](#SynchronizingSortOrder)  
  
##  <a name="SortingDetailRows"></a> Tri de lignes de détails dans une table sans groupe  
 Ajoutez un bouton de tri interactif à un en-tête de colonne pour permettre aux utilisateurs de cliquer sur ce dernier afin de trier les lignes de détails d'une table selon la valeur affichée dans cette colonne.  
  
#### <a name="to-add-an-interactive-sort-button-to-a-column-header-to-sort-the-table-by-value"></a>Pour ajouter un bouton de tri interactif à un en-tête de colonne afin de trier la table par valeur  
  
1.  En mode création de rapport, dans une table sans groupe, cliquez avec le bouton droit sur la zone de texte de l’en-tête de colonne auquel vous souhaitez ajouter un bouton de tri interactif, puis cliquez sur **Propriétés de la zone de texte**.  
  
2.  Cliquez sur **Tri interactif**.  
  
3.  Sélectionnez **Activer le tri interactif sur cette zone de texte**.  
  
4.  Dans **Choisir les éléments à trier**, cliquez sur **Lignes de détails**.  
  
5.  Dans **Trier par**, spécifiez une expression de tri. Dans la liste déroulante, sélectionnez le champ correspondant à la colonne pour laquelle vous définissez une action de tri (par exemple, pour un en-tête de colonne intitulé « Title », choisissez `[Title]`). La spécification d'une expression de tri est obligatoire.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
7.  Répétez les étapes 1 à 6 pour toutes les colonnes auxquelles vous souhaitez ajouter un bouton de tri interactif.  
  
 Pour vérifier l’action de tri, cliquez sur **Exécuter** afin d’afficher un aperçu du rapport, puis cliquez sur les boutons de tri interactifs.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début") [Retour au début](#BackToTop)  
  
##  <a name="SortingTopLevelParent"></a> Tri d’un groupe de lignes parent de niveau supérieur dans une table ou une matrice  
 Ajoutez un bouton de tri interactif à un en-tête de colonne pour permettre aux utilisateurs de cliquer sur ce dernier afin de trier les lignes de groupes parents d'une table ou d'une matrice selon la valeur affichée dans cette colonne. L'ordre des groupes enfants n'est pas modifié.  
  
#### <a name="to-add-an-interactive-sort-button-to-a-column-header-to-sort-groups"></a>Pour ajouter un bouton de tri interactif à un en-tête de colonne afin de trier les groupes  
  
1.  Dans une table ou une matrice en mode création de rapport, cliquez avec le bouton droit sur la zone de texte de l’en-tête de colonne du groupe auquel vous souhaitez ajouter un bouton de tri interactif, puis cliquez sur **Propriétés de la zone de texte**.  
  
2.  Cliquez sur **Tri interactif**.  
  
3.  Sélectionnez **Activer le tri interactif sur cette zone de texte**.  
  
4.  Dans **Choisir les éléments à trier**, cliquez sur **Groupes**.  
  
5.  Dans la liste déroulante, sélectionnez le nom du groupe à trier. Pour les groupes basés sur des expressions de groupe simples, le champ **Trier par** est renseigné avec l’expression de groupe.  
  
    > [!NOTE]  
    >  Pour les expressions de groupe complexes, affectez manuellement à l’expression **Trier par** une valeur identique à l’expression de groupe.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Pour vérifier l’action de tri, cliquez sur **Exécuter** afin d’afficher un aperçu du rapport, puis cliquez sur les boutons de tri interactifs.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début") [Retour au début](#BackToTop)  
  
##  <a name="SortingChildGroups"></a> Tri de groupes enfants ou de lignes de détails dans un groupe  
 Ajoutez un bouton de tri interactif à une ligne d'en-tête de groupe pour permettre aux utilisateurs de trier les valeurs d'un groupe enfant à partir d'un groupe parent ou de trier les lignes de détails du groupe enfant le plus profond.  
  
#### <a name="to-add-an-interactive-sort-button-to-a-text-box-in-a-group-row-header-to-sort-child-groups-or-detail-rows"></a>Pour ajouter un bouton de tri interactif à une zone de texte dans un en-tête de ligne de groupe pour trier des groupes enfants ou des lignes de détails  
  
1.  En mode création de rapport, cliquez avec le bouton droit sur la zone de texte de la ligne d’en-tête de groupe à laquelle vous souhaitez ajouter un bouton de tri interactif, puis cliquez sur **Propriétés de la zone de texte**.  
  
2.  Cliquez sur **Tri interactif**.  
  
3.  Sélectionnez **Activer le tri interactif sur cette zone de texte**.  
  
4.  Dans **Choisir les éléments à trier**, cliquez sur l’une des options suivantes :  
  
    -   **Détails** Cliquez sur **Détails** pour trier les lignes de détails. Dans la liste déroulante, sélectionnez le champ selon lequel effectuer le tri. Pour cette option, vous devez spécifier la valeur selon laquelle effectuer le tri.  
  
    -   **Groupes** Cliquez sur **Groupes** pour trier les valeurs de groupes enfants. Pour cette option, l’expression **Trier par** est automatiquement renseignée à partir de l’expression de groupe.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Pour vérifier l’action de tri, cliquez sur **Exécuter** afin d’afficher un aperçu du rapport, puis cliquez sur les boutons de tri interactifs.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début") [Retour au début](#BackToTop)  
  
##  <a name="SortingMultipleRowGroups"></a> Tri de lignes en fonction d'une expression de groupe complexe  
 Ajoutez un bouton de tri interactif à un en-tête de colonne pour permettre aux utilisateurs de cliquer sur l'en-tête de colonne afin de trier les groupes parents et enfants combinés. Pour ce faire, vous devez modifier l'expression de groupe afin qu'elle corresponde à un composite des deux groupes. Par exemple, supposons qu'une matrice affiche des totaux de stock d'un magasin pour des articles regroupés par couleur et par taille. Pour trier les lignes selon la combinaison de couleur et de taille, au lieu d'avoir un groupe séparé pour la couleur et un autre pour la taille, vous pouvez définir un groupe qui combine les deux critères. Pour plus d’informations sur la définition d’expressions de groupe, consultez [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md).  
  
 Dans la procédure suivante, les termes spécifient des zones de région de données de tableau matriciel. Pour plus d’informations, consultez [Zones de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-areas-report-builder-and-ssrs.md).  
  
 En règle générale, lorsque vous triez des lignes en fonction de plusieurs groupes, vous voulez pouvoir afficher les totaux des lignes triées, indépendamment des groupes de colonnes. Dans cette procédure, aucun groupe de colonnes n'est utilisé. Pour commencer, ajoutez une matrice et supprimez le groupe de colonnes par défaut. Vous pouvez également commencer par ajouter une table et supprimer le groupe de détails.  
  
#### <a name="to-add-an-interactive-sort-button-to-a-column-header-to-sort-multiple-groups"></a>Pour ajouter un bouton de tri interactif à un en-tête de colonne afin de trier plusieurs groupes  
  
1.  En mode création de rapport, ajoutez une matrice.  
  
2.  Faites glisser un champ numérique vers la cellule de données pour lier le dataset à la matrice.  
  
     Ensuite, vous devez créer un groupe avec une expression de groupe qui spécifie plusieurs champs, ainsi qu'un en-tête de groupe à utiliser pour l'affichage des valeurs de groupe.  
  
3.  Vérifiez que la matrice est sélectionnée dans l'aire de conception. Le volet Regroupement affiche un groupe de lignes et de colonnes par défaut.  
  
4.  Dans le volet Groupes de lignes, cliquez avec le bouton droit sur le groupe de lignes par défaut, puis cliquez sur **Modifier le groupe**. La boîte de dialogue **Propriétés du groupe** s’ouvre.  
  
5.  Dans **Nom**, remplacez le nom par défaut par un nom qui spécifie les différents groupes selon lesquels vous souhaitez effectuer un regroupement.  
  
6.  Dans **Expressions de groupe**, sous **Regrouper sur**, cliquez sur le bouton Expression (**fx**) pour ouvrir la boîte de dialogue **Expression** .  
  
7.  Tapez l'expression qui spécifie l'ensemble des champs selon lesquels vous souhaitez effectuer le regroupement. Par exemple, l'expression de groupe suivante combine un champ nommé Color et un champ nommé Size : `=Fields!Color.Value & Fields!Size.Value`.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Le groupe est désormais défini. Ensuite, faites glisser les champs à afficher vers le corps du tableau matriciel de la matrice. Ajoutez au corps du tableau matriciel les champs selon lesquels vous avez choisi d’effectuer le regroupement à l’étape 7, en veillant à insérer chaque champ dans sa propre colonne.  
  
     Dans ce scénario, la première colonne de la zone de groupes de lignes du tableau matriciel n’est pas nécessaire. Pour supprimer la colonne, cliquez avec le bouton droit sur son en-tête, puis cliquez sur **Supprimer les colonnes**. Une boîte de dialogue vous demande alors si les groupes associés doivent être supprimés. Cliquez sur **Non**. La zone de groupe de lignes est supprimée et seul le corps du tableau matriciel est conservé.  
  
     Vous devez maintenant supprimer le groupe de colonnes par défaut.  
  
9. Dans le volet Groupes de colonnes, cliquez avec le bouton droit sur le groupe de colonnes par défaut, puis cliquez sur **Supprimer le groupe**. Une boîte de dialogue vous invite alors à indiquer si la suppression doit s'appliquer au groupe ainsi qu'aux lignes et aux colonnes qui lui sont associées, ou uniquement au groupe. Cliquez sur **Supprimer le groupe uniquement**. Le groupe de colonnes est supprimé, ainsi que la zone de groupe de colonnes. Seul le corps du tableau matriciel est conservé.  
  
     Ensuite, vous devez ajouter un bouton de tri interactif à la zone de texte qui couvre la matrice.  
  
10. Cliquez dans la zone de texte de la première ligne, puis cliquez sur **Propriétés de la zone de texte**.  
  
11. Cliquez sur **Tri interactif**.  
  
12. Sélectionnez **Activer le tri interactif sur cette zone de texte**.  
  
13. Dans **Choisir les éléments à trier**, cliquez sur **Groupes**.  
  
14. Dans la liste déroulante, sélectionnez le nom du groupe que vous avez créé à l'étape 5. L’expression de groupe est automatiquement copiée dans la zone de texte **Trier par** .  
  
15. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Le bouton de tri est désormais ajouté à la zone de texte.  
  
16. (Facultatif) Vous pouvez supprimer des valeurs dupliquées dans les colonnes qui affichent des valeurs de groupe. Dans l'aire de conception du rapport, cliquez sur la zone de texte affichant la valeur dont vous souhaitez masquer les doublons. Dans le volet Propriétés, accédez à l’option **Masquer les doublons**puis, dans la liste déroulante, sélectionnez le nom du dataset lié à cette matrice.  
  
 Pour vérifier l’action de tri, cliquez sur **Exécuter** afin d’afficher un aperçu du rapport, puis cliquez sur le bouton de tri interactif. La matrice effectue un tri selon les valeurs combinées de l'expression de groupe, bien que chaque valeur individuelle s'affiche dans sa propre colonne.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début") [Retour au début](#BackToTop)  
  
##  <a name="SynchronizingSortOrder"></a> Synchronisation de l'ordre de tri pour plusieurs régions de données  
 Ajoutez un bouton de tri interactif qui permettra aux utilisateurs de trier plusieurs régions de données d'un simple clic. Lorsque vous créez un bouton de tri interactif, vous pouvez spécifier une synchronisation du tri pour plusieurs régions de données, selon le même dataset de rapport. Par exemple, un rapport peut inclure une matrice et un graphique qui présente les données sous forme graphique. Lorsqu'un utilisateur modifie l'ordre de tri des lignes dans la matrice, le graphique affiche automatiquement le même ordre de tri.  
  
 Pour synchroniser l'ordre de tri, vous devez utiliser des expressions de tri identiques pour les régions de données ou les groupes à trier, ainsi que définir une étendue de sorte que le tri soit un ancêtre mutuel des deux régions de données. L'ancêtre mutuel peut être le dataset auquel les deux régions de données sont liées ou une région de données conteneur dans laquelle les deux régions de données apparaissent. Par exemple, supposons qu'un rapport contienne à la fois une matrice et un graphique affichant des données du même dataset et contenues dans une liste. Pour synchroniser l'action de tri, vous devez spécifier le tri interactif sur une colonne de la matrice et définir l'étendue à la liste. Lorsque l'utilisateur trie la matrice, le graphique est également trié.  
  
#### <a name="to-synchronize-sort-order-with-a-chart-for-an-interactive-sort-button-on-a-matrix-data-region"></a>Pour synchroniser l'ordre de tri avec un graphique, pour un bouton de tri interactif, sur une région de données de matrice  
  
1.  En mode création de rapport, ajoutez une matrice au rapport.  
  
2.  Ajoutez un champ de dataset numérique à la cellule de données de la matrice, tel qu'un champ représentant la quantité ou les ventes.  
  
3.  Définissez un groupe de lignes. Par défaut, l'ordre de tri du groupe est défini sur la même expression que l'expression de groupe.  
  
4.  Ajoutez un graphique au rapport, un graphique à secteurs, par exemple.  
  
5.  Faites glisser le champ sélectionné à l’étape 2 vers la zone **Valeur** du volet **Données du graphique** .  
  
6.  Faites glisser vers la zone **Groupes de catégories** le champ selon lequel vous avez choisi d’effectuer le regroupement.  
  
     L'expression du groupe de lignes de la matrice doit être identique à celle du groupe d'abscisses du graphique.  
  
7.  Cliquez avec le bouton droit sur le groupe de catégories, puis cliquez sur **Propriétés du groupe de catégories**.  
  
8.  Cliquez sur **Tri**.  
  
9. Cliquez sur **Ajouter**. Une nouvelle ligne de tri est ajoutée à la grille d'options de tri.  
  
10. Dans Trier par, dans la liste déroulante, sélectionnez le même champ que celui choisi à l'étape 6 pour effectuer le regroupement.  
  
11. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
12. Dans la matrice, cliquez avec le bouton droit sur la zone de texte de l’en-tête de colonne auquel vous souhaitez ajouter un bouton de tri interactif, puis cliquez sur **Propriétés de la zone de texte**.  
  
13. Cliquez sur **Tri interactif**.  
  
14. Sélectionnez **Activer le tri interactif sur cette zone de texte**.  
  
15. Dans **Choisir les éléments à trier**, cliquez sur **Groupes**.  
  
16. Dans la liste déroulante sous **Groupes**, sélectionnez le nom du groupe à trier. L’expression de groupe pour ce groupe est automatiquement affectée à la valeur **Trier par** .  
  
17. Sélectionnez **Appliquer également ce tri aux autres groupes et régions de données dans**. Dans la zone de texte, tapez le nom du dataset, par exemple « SalesData ».  
  
18. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Pour vérifier l’action de tri, cliquez sur **Exécuter** afin d’afficher un aperçu du rapport, puis cliquez sur le bouton de tri interactif. La matrice effectue un tri selon les valeurs combinées de l'expression de groupe, bien que chaque valeur individuelle s'affiche dans sa propre colonne.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début") [Retour au début](#BackToTop)  
  
## <a name="see-also"></a> Voir aussi  
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Tri interactif &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-report-builder-and-ssrs.md)   
 [Trier des données dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md)   
 [Exploration de la souplesse d’une région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/exploring-the-flexibility-of-a-tablix-data-region-report-builder-and-ssrs.md)  
  
  
