---
title: Ajouter des lignes dans le volet de résultats (Visual Database Tools) | Microsoft Docs
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
helpviewer_keywords:
- View Designer, Results pane
- inserting rows
- Query Designer [SQL Server], Results pane
- Results pane
- adding rows
- row additions [SQL Server], Results pane
ms.assetid: 59891c84-3f54-4ab9-8b86-72c59627b480
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 077a48d38e12fc6ff4debb9f6b02a3df83923e9d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-new-rows-in-the-results-pane-visual-database-tools"></a>Ajouter des lignes dans le volet de résultats (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Pour ajouter de nouvelles données, vous pouvez soit les taper, soit les coller à partir d'un autre programme, tel que le Bloc-notes ou Excel. Une ligne collée doit avoir exactement le même nombre et les mêmes types de colonnes que la table dans laquelle vous effectuez le collage. Vous pouvez coller plusieurs lignes à la fois dans le volet Résultats.  
  
Pour plus d’informations sur la façon d’entrer des données, consultez [Règles relatives à la mise à jour des résultats &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/rules-for-updating-results-visual-database-tools.md).  
  
### <a name="to-add-a-new-data-row"></a>Pour ajouter une nouvelle ligne de données  
  
1.  Allez en bas du volet Résultats, où vous trouverez une ligne blanche pour ajouter une nouvelle ligne de données.  
  
    Les valeurs initiales de toutes les colonnes sont *NULL*.  
  
    > [!TIP]  
    > Pour accéder directement à la première ligne vide, utilisez la barre de navigation située au bas du volet Résultats.  
  
2.  Si vous collez des lignes depuis le Presse-papiers, pour sélectionner la nouvelle ligne, cliquez sur le bouton situé à sa gauche.  
  
    > [!NOTE]  
    > Si une ou plusieurs lignes que vous collez ne peuvent pas être validées dans la base de données, un message indiquant la ou les lignes qui n'ont pas pu être validées s'affiche.  
  
3.  Entrez les données de la nouvelle ligne. Si vous effectuez un collage, choisissez **Coller** dans le menu **Edition** .  
  
4.  Laissez cette ligne pour la valider dans la base de données.  
  
Si une erreur se produit lorsque vous enregistrez la ligne, le Concepteur de requêtes et de vues affiche un message, puis vous ramène à la ligne que vous avez modifiée. Ensuite, vous pouvez :  
  
-   Résoudre l'erreur par de nouvelles modifications de la ligne.  
  
-   Annuler la modification en appuyant sur ÉCHAP. Si vous appuyez sur ÉCHAP alors que vous êtes dans une cellule que vous avez modifiée, seules les modifications apportées à cette cellule sont annulées. Si vous appuyez sur ÉCHAP alors que vous êtes dans une cellule que vous n'avez pas modifiée, toutes les modifications de la ligne sont annulées.  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser des données du volet de résultats &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-data-in-the-results-pane-visual-database-tools.md)  
[Effectuer des opérations de base concernant les requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
  
