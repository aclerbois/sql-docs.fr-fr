---
title: Ajouter, déplacer ou supprimer une table, une matrice ou une liste (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 4b97c470-cde0-4bb1-a46e-5f5f5553feaa
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 989e39e411129bef0af9b1af14548286927c2655
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-move-or-delete-a-table-matrix-or-list-report-builder-and-ssrs"></a>Ajouter, déplacer ou supprimer une table, une matrice ou une liste (Générateur de rapports et SSRS)
  Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , une région de données affiche les données d’un dataset de rapport. Les tables, matrices, listes, graphiques et jauges sont des régions de données. Pour imbriquer une région de données dans une autre, ajoutez une à une chaque région de données, puis faites glisser la région de données enfant dans la région de données parente.  
  
 Le moyen le plus simple d'ajouter une région de données de type tableau ou matrice à votre rapport est d'exécuter l'Assistant Nouveau tableau ou nouvelle matrice. Ces Assistants vous aideront à sélectionner une connexion à une source de données, à organiser les champs et à choisir une disposition et un style. Les Assistants sont uniquement disponibles dans le Générateur de rapports.  
  
## <a name="to-add-a-table-or-matrix-to-a-report-by-using-the-new-table-or-new-matrix-wizard"></a>Pour ajouter une table ou une matrice à un rapport à l'aide de l'Assistant Table ou Matrice  
  
1.  Sous l’onglet **Insérer** , cliquez sur **Tableau** ou **Matrice**, puis sur **Assistant Tableau** ou **Assistant Matrice**.  
  
2.  Suivez les étapes de l’Assistant **Nouveau tableau** ou **Nouvelle matrice** .  
  
3.  Sous l’onglet **Accueil** , cliquez sur **Exécuter** pour visualiser le rapport rendu.  
  
4.  Sous l’onglet **Exécuter** , cliquez sur **Conception** pour continuer à utiliser le rapport.  
  
## <a name="to-add-a-data-region"></a>Pour ajouter une région de données  
  
1.  Sur le **ruban**, dans le groupe **Régions de données** , cliquez sur la région de données à ajouter.  
  
2.  Cliquez sur l'aire de conception, puis faites glisser la souris pour créer une zone de la taille voulue pour la région de données.  
  
3.  Faites glisser un champ de dataset de rapport à partir du volet des données de rapportvers une cellule de la région de données. La région de données est maintenant liée à des données du dataset de rapport.  
  
## <a name="to-select-a-data-region"></a>Pour sélectionner une région de données  
  
-   Dans le cas d'une région de données de tableau matriciel, cliquez avec le bouton droit sur la poignée d'angle. Dans le cas d'une région de données de graphique ou jauge, cliquez dans la région de données.  
  
     Une poignée de sélection et huit poignées de redimensionnement apparaissent.  
  
     Dans le cas d’une région de données imbriquée, cliquez dedans avec le bouton droit, cliquez sur **Sélectionner**, puis sélectionnez l’élément de rapport voulu. Pour vérifier quel élément de rapport est sélectionné, utilisez le volet Propriétés. Le nom de l'élément sélectionné dans l'aire de conception apparaît dans la barre d'outils du volet Propriétés.  
  
## <a name="to-move-a-data-region"></a>Pour déplacer une région de données  
  
-   Pour déplacer une région de données, cliquez sur la poignée de sélection de la région de données et faites-la glisser. Utilisez les lignes d'alignement pour l'aligner aux éléments de rapport existants.  
  
     Si la règle n’est pas visible, cliquez sur l’onglet Affichage et sélectionnez l’option **Règle** .  
  
     Vous pouvez aussi utiliser les touches de direction pour déplacer la région de données sélectionnée sur l'aire de conception.  
  
## <a name="to-delete-a-data-region"></a>Pour supprimer une région de données  
  
-   Sélectionnez la région de données, cliquez dedans avec le bouton droit, puis cliquez sur **Supprimer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
