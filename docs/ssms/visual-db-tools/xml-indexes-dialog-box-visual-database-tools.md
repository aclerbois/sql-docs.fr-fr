---
title: Index XML, boîte de dialogue (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.xmlindexes
ms.assetid: eef38310-4498-4ccc-bb77-5bbd1c7cc477
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 00757208c128ef00ea58c5139b6edc8461621307
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-indexes-dialog-box-visual-database-tools"></a>Boîte de dialogue Index XML (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Utilisez la boîte de dialogue **Index XML** pour créer des index pour les colonnes du type de données XML qui ne peuvent pas être indexées à l’aide de la boîte de dialogue **Index/Clés** . Chaque colonne XML peut posséder plusieurs index XML, mais le premier index créé (principal) est utilisé comme base pour les autres (secondaires). Si vous supprimez l'index XML principal, les index secondaires sont également supprimés.  
  
## <a name="options"></a>Options  
**Index XML sélectionné**  
Répertorie les index XML existants. Sélectionnez cette option pour afficher ses propriétés dans la partie droite de la grille. Si la liste est vide, aucune propriété n'est définie pour la table.  
  
**Ajouter**  
Crée un nouvel index XML.  
  
**Supprimer**  
Supprime l’index XML sélectionné dans la liste **Index XML sélectionné** . Lorsque vous supprimez l'index XML principal, vous êtes averti que cette opération supprimera également tout index secondaire, et que vous pouvez soit continuer, soit annuler l'action.  
  
**Catégorie Général**  
Développée, elle affiche les champs des propriétés **Colonnes**, **Est primaire**et **Type**.  
  
**Colonnes**  
Indique que cet index est trié en ordre croissant.  
  
**Est primaire**  
Indique s'il s'agit de l'index principal. Le premier index XML créé sur la colonne est utilisé comme base pour les autres.  
  
**Nom de la référence primaire**  
Affiche le nom de l'index principal s'il s'agit d'un index secondaire. Disponible uniquement s'il s'agit d'un index secondaire.  
  
**Type secondaire**  
Affiche le type de l'index secondaire. Disponible uniquement s'il s'agit d'un index secondaire.  
  
**Type**  
Indique qu'il s'agit d'un index XML.  
  
**Catégorie Identité**  
Développée, elle affiche les champs de propriétés de **Nom** et **Description** .  
  
**Nom**  
Indique le nom de l'index XML. Lorsqu'un nouvel index ou une nouvelle clé sont créés, ils obtiennent un nom par défaut basé sur la table affichée dans la fenêtre active du Concepteur de tables. Vous pouvez modifier le nom à tout moment.  
  
**Description**  
Décrivez l’index. Pour écrire une description plus détaillée, cliquez sur **Description** , puis sur le bouton de sélection **(...)** qui apparaît à droite du champ de propriété. Vous obtiendrez une zone d'écriture plus large.  
  
**Catégorie Concepteur de tables**  
Développée, elle affiche des informations sur les propriétés de cet index XML.  
  
**Spécification du remplissage**  
Développée, elle affiche des informations sur les options **Taux de remplissage** et **Index Pad**.  
  
**Taux de remplissage**  
Spécifie le pourcentage de la page d'index que le système peut remplir. Lorsqu'une page est pleine, le système doit la fractionner au cas où de nouvelles données seraient ajoutées, ce qui affaiblit les performances.  
  
-   La valeur 100 signifie que les pages seront remplies et occuperont un espace de stockage minimal, mais c'est la solution la moins efficace. Ce paramètre ne doit être utilisé que lorsqu'aucune modification ne sera apportée aux données, par exemple dans un tableau en lecture seule.  
  
-   Une valeur inférieure laisse davantage d'espace vide sur les ensembles de données, ce qui réduit le besoin de les fractionner à mesure que la taille des index augmente. Néanmoins, l'espace de stockage requis est plus important. Ce paramètre est plus approprié si vous avez l'intention de modifier les données contenues dans la table.  
  
**Index Pad**  
Donne aux pages de cet index le même pourcentage d’espace libre (de remplissage) que celui spécifié dans **Taux de remplissage**.  
  
**Est désactivé**  
Spécifie si cet index est désactivé. Les index désactivés ne prennent pas en charge les recherches, et ne sont pas mis à jour lorsque de nouveaux éléments sont ajoutés à la table. Vous pouvez améliorer les performances pour les insertions et les mises à jour en bloc en désactivant un index.  
  
**Verrouillage de page autorisé**  
Spécifier si le verrouillage au niveau des pages est autorisé dans cet index. L'autorisation ou non du verrouillage au niveau de la page affecte les performances de la base de données.  
  
**Recalculer les statistiques**  
Calcul de nouvelles statistiques au moment de la création de l'index. Le calcul de ces nouvelles statistiques ralentit la génération des index, mais améliore généralement les performances des requêtes.  
  
**Verrouillage de ligne autorisé**  
Spécifier si le verrouillage au niveau des lignes est autorisé dans cet index. L'autorisation ou non du verrouillage au niveau de la ligne affecte les performances de la base de données.  
  
## <a name="see-also"></a> Voir aussi  
[Créer des index XML](http://msdn.microsoft.com/en-us/6ecac598-355d-4408-baf7-1b2e8d4cf7c1)  
  
