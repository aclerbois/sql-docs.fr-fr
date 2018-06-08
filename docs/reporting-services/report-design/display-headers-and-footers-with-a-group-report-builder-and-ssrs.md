---
title: Afficher des en-têtes et des pieds de page de groupe (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 8eb7d648-4df2-491a-96cb-99e55629d617
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 3068cf6494dfc790f2990f8e872dd9365e3594f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="display-headers-and-footers-with-a-group-report-builder-and-ssrs"></a>Afficher des en-têtes et des pieds de page de groupe (Générateur de rapports et SSRS)
  Vous pouvez mieux contrôler si une ligne statique, telle qu'un en-tête ou un pied de page de groupe, est rendue avec des lignes dynamiques qui sont associées à un groupe dans une région de données de tableau matriciel.  
  
 Pour répéter l'ensemble des en-têtes de lignes ou des en-têtes de colonnes sur plusieurs pages, vous pouvez définir des propriétés pour la région de données de tableau matriciel. Pour plus d’informations, consultez [Afficher des en-têtes de ligne et de colonne sur plusieurs pages (Générateur de rapports et SSRS)](display-row-and-column-headers-on-multiple-pages-report-builder-and-ssrs.md).  
  
 Pour contrôler le comportement de rendu des lignes et des colonnes dynamiques qui sont associées aux groupes imbriqués, ou des lignes et des colonnes statiques qui sont associées à des étiquettes ou des sous-totaux, vous devez définir des propriétés pour le membre du tableau matriciel. Un membre du tableau matriciel représente une ligne ou une colonne statique ou dynamique. Un membre statique se répète une fois. Par exemple, une ligne de total global est une ligne statique. Un membre dynamique se répète une fois pour chaque instance de groupe. Par exemple, une ligne associée à un groupe avec l'expression de groupe [Territory] se répète une fois pour chaque valeur unique de secteur. Pour plus d’informations sur les membres du tableau matriciel, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
 Vous pouvez sélectionner un membre du tableau matriciel dans le volet Regroupement et définir les propriétés **KeepWithGroup**, **KeepTogether**et **RepeatOnNewPage** dans le volet Propriétés. Utilisez **KeepWithGroup** pour afficher des en-têtes et des pieds de page de groupe sur la même page que le groupe. Utilisez **KeepTogether** pour afficher les membres statiques avec les lignes ou les colonnes d’un groupe. Utilisez **RepeatOnNewPage** pour répéter l’en-tête ou le pied de page de groupe sur chaque page qui affiche au moins une instance complète du membre de groupe de lignes désigné par la valeur **KeepWithGroup** . La propriété**RepeatOnNewPage** n’est pas prise en charge pour les membres de groupe de colonnes.  
  
> [!NOTE]  
>  **KeepWithGroup**, **KeepTogether**et **RepeatOnNewPage** sont des propriétés de membre de groupe que vous pouvez définir à l’aide du **mode Avancé** du volet de regroupement. Pour plus d’informations, consultez [Volet de regroupement &#40;Générateur de rapports&#41;](../../reporting-services/report-design/grouping-pane-report-builder.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-keep-a-static-row-with-a-set-of-dynamic-rows-associated-with-a-row-group"></a>Pour conserver une ligne statique avec un ensemble de lignes dynamiques associées à un groupe de lignes  
  
1.  Sur l’aire de conception, cliquez n’importe où dans la région de données de tableau matriciel pour la sélectionner. Le volet de regroupement affiche les groupes de lignes et de colonnes de la région de données.  
  
2.  Dans la partie droite du volet de regroupement, cliquez sur la flèche orientée vers le bas, puis sur **Mode avancé**. Le volet Groupes de lignes affiche les membres hiérarchiques statiques et dynamiques pour la hiérarchie des groupes de lignes.  
  
3.  Cliquez sur le membre statique qui correspond à l'en-tête ou au pied de page de ligne que vous souhaitez conserver avec les lignes de groupe. Le volet Propriétés affiche les propriétés du **membre du tableau matriciel** .  
  
4.  Dans le volet Propriétés, cliquez sur **KeepWithGroup**, puis choisissez l’une des valeurs suivantes dans la liste déroulante :  
  
    -   **Aucun** Sélectionnez cette option pour ne pas indiquer de préférence concernant la conservation de ce membre avec les membres du groupe de lignes sélectionné.  
  
    -   **Avant** Sélectionnez cette option pour garder ce membre avec les membres du groupe précédent. Vous pouvez l'utiliser pour les lignes de pied de page de groupe.  
  
    -   **Après** Sélectionnez cette option pour garder ce membre avec les membres du groupe suivant. Vous pouvez l'utiliser pour les lignes d'en-tête de groupe.  
  
5.  (Facultatif) Affichez l'aperçu du rapport. Dans la mesure du possible, le convertisseur de rapport garde ce membre avec les membres de groupe de lignes spécifiés.  
  
### <a name="to-keep-a-static-column-with-a-set-of-dynamic-columns-associated-with-a-column-group"></a>Pour conserver une colonne statique avec un ensemble de colonnes dynamiques associées à un groupe de colonnes  
  
1.  Sur l’aire de conception, cliquez n’importe où dans la région de données de tableau matriciel pour la sélectionner. Le volet de regroupement affiche les groupes de lignes et de colonnes de la région de données.  
  
2.  Dans la partie droite du volet de regroupement, cliquez sur la flèche orientée vers le bas, puis sur **Mode avancé**. Le volet Groupes de colonnes affiche les membres hiérarchiques statiques et dynamiques pour la hiérarchie des groupes de colonnes.  
  
3.  Cliquez sur le membre statique qui correspond à la colonne statique que vous souhaitez conserver avec les colonnes de groupe. Le volet Propriétés affiche les propriétés du **membre du tableau matriciel** .  
  
4.  Dans le volet Propriétés, cliquez sur **KeepWithGroup**, puis choisissez l’une des valeurs suivantes dans la liste déroulante :  
  
    -   **Aucun** Sélectionnez cette option pour ne pas indiquer de préférence concernant la conservation de ce membre avec les membres du groupe de colonnes sélectionné.  
  
    -   **Avant** Sélectionnez cette option pour garder ce membre avec les membres du groupe précédent. Vous pouvez l'utiliser pour les étiquettes de colonne qui s'affichent avant les membres de groupe de colonnes spécifiés.  
  
    -   **Après** Sélectionnez cette option pour garder ce membre avec les membres du groupe suivant. Vous pouvez l'utiliser pour les totaux de colonne qui s'affichent après les membres de groupe de colonnes spécifiés.  
  
5.  (Facultatif) Affichez l'aperçu du rapport. Dans la mesure du possible, le convertisseur de rapport garde ce membre avec les membres de groupe de colonnes spécifiés.  
  
## <a name="see-also"></a> Voir aussi  
 [Cellules, lignes et colonnes de région de données de tableau matriciel (Générateur de rapports et SSRS)](tablix-data-region-report-builder-and-ssrs.md)   
 
  
  
