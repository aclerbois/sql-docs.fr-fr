---
title: Insérer ou supprimer une colonne (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: e9db79e2-7e7d-4359-a706-cb746c94182a
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: b6fbf5e25a982f511e3451cec07b7414ffced559
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="insert-or-delete-a-column-report-builder-and-ssrs"></a>Insérer ou supprimer une colonne (Générateur de rapports et SSRS)
  Vous pouvez ajouter ou supprimer des colonnes dans une région de données de tableau matriciel dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Cette région peut être une table, une matrice ou une liste. Les procédures suivantes ne s'appliquent pas aux régions de données de graphique ou de jauge.  
  
 Dans une région de données de tableau matriciel, vous pouvez ajouter des colonnes associées à un groupe (à l'intérieur d'un groupe) ou non associées à un groupe (à l'extérieur d'un groupe). Une colonne qui se trouve à l'intérieur d'un groupe n'est utilisée qu'à une seule reprise pour chaque valeur de groupe unique. Par exemple, une colonne située à l'intérieur d'un groupe dont les données sont regroupées en colonnes contenant des noms de couleur n'est utilisée qu'une seule fois par nom de couleur distinct. Pour les groupes imbriqués, une colonne peut être à l'extérieur du groupe enfant mais à l'intérieur du groupe parent. Dans ce cas, la ligne est utilisée une fois pour chaque valeur unique dans le groupe parent.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-select-a-data-region-so-that-the-row-and-column-handles-appear"></a>Pour sélectionner une région de données de manière à afficher les poignées de ligne et de colonne  
  
-   En mode Conception, cliquez dans l'angle supérieur gauche de la région de données de tableau matriciel pour faire apparaître les poignées de colonne et de ligne au-dessus et en regard de cette région.  
  
     Pour plus d’informations sur les régions de données, consultez la page [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
## <a name="to-insert-a-column-in-a-selected-data-region"></a>Pour insérer une colonne dans une région de données sélectionnée  
  
-   Cliquez avec le bouton droit sur une poignée de colonne à l’emplacement où vous souhaitez insérer une colonne, cliquez sur **Insérer une colonne**, puis sur **Gauche** ou **Droite**.  
  
     -- ou --  
  
-   Cliquez avec le bouton droit sur une cellule de la région de données au niveau de laquelle vous souhaitez insérer une colonne, cliquez sur **Insérer une colonne**, puis sur **Gauche** ou **Droite**.  
  
## <a name="to-delete-a-column-from-a-selected-data-region"></a>Pour supprimer une colonne d'une région de données sélectionnée  
  
-   Sélectionnez la ou les colonnes que vous souhaitez supprimer, cliquez avec le bouton droit sur la poignée de l’une des colonnes sélectionnées, puis cliquez sur **Supprimer les colonnes**.  
  
     -- ou --  
  
-   Cliquez avec le bouton droit sur une cellule de la région de données au niveau de laquelle vous souhaitez supprimer une colonne, puis cliquez sur **Supprimer les colonnes**.  
  
## <a name="to-insert-a-column-in-a-group-in-a-selected-data-region"></a>Pour insérer une colonne dans un groupe d'une région de données sélectionnée  
  
-   Dans la zone de groupe de colonnes d’une région de données de tableau matriciel, cliquez avec le bouton droit sur une cellule de groupe de colonnes au niveau de laquelle vous souhaitez insérer une colonne, sélectionnez **Insérer une colonne**, puis cliquez sur **À gauche - En dehors du groupe**, **À gauche - Dans le groupe**, **À droite - Dans le groupe**ou **À droite - En dehors du groupe**.  
  
     Une colonne est ajoutée à l'intérieur ou à l'extérieur du groupe représenté par la cellule de groupe de colonnes sur laquelle vous avez cliqué.  
  
## <a name="to-delete-a-column-from-a-group-in-a-selected-data-region"></a>Pour supprimer une colonne d'un groupe d'une région de données sélectionnée  
  
-   Dans la zone de groupe de colonnes d’une région des données de tableau matriciel, cliquez avec le bouton droit sur une cellule de groupe de colonnes au niveau de laquelle vous souhaitez supprimer une colonne, puis sélectionnez **Supprimer les colonnes**.  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md)   
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md)   
 [Matrices &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)      
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
