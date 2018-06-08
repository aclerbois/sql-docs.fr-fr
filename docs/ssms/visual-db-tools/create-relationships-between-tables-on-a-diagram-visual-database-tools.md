---
title: Créer des relations entre des tables sur un diagramme | Microsoft Docs
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
- diagrams [SQL Server], designing
ms.assetid: 28e9630c-dff4-46cc-bb0e-fe77998b6ac2
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 60e7e7c130924f674119f13d911aad1c0e342dae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-relationships-between-tables-on-a-diagram-visual-database-tools"></a>Créer des relations entre des tables sur un diagramme (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez créer des relations entre des colonnes dans différentes tables dans le Concepteur de diagrammes en faisant glisser les colonnes entre les tables.  
  
### <a name="to-create-a-relationship-graphically"></a>Pour créer une relation sous forme de graphique  
  
1.  Dans le Concepteur de diagrammes, cliquez sur le sélectionneur de ligne d'une ou de plusieurs colonnes de base de données que vous souhaitez associer à une colonne dans une autre table.  
  
2.  Faites glisser la ou les colonne(s) sélectionnée(s) vers la table associée.  
  
3.  Deux boîtes de dialogue s’ouvre : **Relation de clé étrangère** et **Tables et colonnes**, cette dernière apparaissant au premier plan.  
  
4.  Le**nom de relation** a une nom fourni par le système au format FK_*localtable*_*foreigntable*. Vous pouvez modifier cette valeur.  
  
5.  Vérifiez que la **table de clé primaire** spécifie la bonne table.  
  
6.  La grille affiche les colonnes locales et leurs colonnes étrangères correspondantes. Vous pouvez ajouter ou supprimer des colonnes de table ou modifier les mappages.  
  
7.  Choisissez **OK**.  
  
    La boîte de dialogue **Relation de clé étrangère** s’ouvre. La**Relation sélectionnée** affiche la relation créée.  
  
8.  Modifiez les propriétés de la relation dans la grille.  
  
9. Choisissez **OK** pour créer la relation.  
  
    Le Concepteur de base de données affiche une relation entre les colonnes que vous avez choisies.  
  
## <a name="see-also"></a> Voir aussi  
[Boîte de dialogue Tables et colonnes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/tables-and-columns-dialog-box-visual-database-tools.md)  
[Utilisation des contraintes (Visual Database Tools)](http://msdn.microsoft.com/en-us/637098af-2567-48f8-90f4-b41df059833e)  
[Utiliser des tables dans les schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-tables-in-database-diagram-visual-database-tools.md)  
  
