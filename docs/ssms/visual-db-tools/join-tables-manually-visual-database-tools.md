---
title: Joindre manuellement des tables (Visual Database Tools) | Microsoft Docs
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
- manual joins [SQL Server]
- joins [SQL Server], manual
- joins [SQL Server], creating
ms.assetid: 9c785356-646b-4c87-82d4-25efd6051d9d
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 905c551cd4e9aa9e58900a58b9d9399a3c662a7c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="join-tables-manually-visual-database-tools"></a>Joindre manuellement des tables (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Quand vous ajoutez plusieurs tables à une requête, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) tente de les joindre en fonction de données communes ou d’informations enregistrées dans la base de données sur d’éventuelles liaisons entre les tables. Pour plus d’informations, consultez [Joindre automatiquement des tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-automatically-visual-database-tools.md). Néanmoins, si le Concepteur de requêtes et de vues n'a pas joint automatiquement les tables ou si vous voulez créer d'autres conditions de jointure entre les tables, vous pouvez joindre les tables manuellement.  
  
Vous pouvez créer des jointures basées sur des comparaisons entre deux colonnes, quelles qu'elles soient et non uniquement entre des colonnes qui contiennent les mêmes informations. Par exemple, si votre base de données contient deux tables, `titles` et `roysched`, vous pouvez comparer des valeurs de la colonne `ytd_sales` de la table `titles` à celles des colonnes `lorange` et `hirange` de la table `roysched` . La création d'une telle jointure vous permet de trouver des titres dont les ventes de l'année à ce jour figurent entre les plages inférieure et supérieure des paiements de droits d'auteur.  
  
> [!TIP]  
> Les jointures sont réalisées plus rapidement si les colonnes de la condition de jointure ont été indexées. Dans certains cas, une jointure appliquée à des colonnes non indexées peut se traduire par une exécution très lente de la requête.  
  
### <a name="to-manually-join-tables-or-table-structured-objects"></a>Pour joindre manuellement des tables ou des objets de type table  
  
1.  Ajoutez les objets à joindre au [volet Schéma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) .  
  
2.  Faites glisser le nom de la colonne de jointure de la première table ou objet de type table jusqu'à la colonne liée de la seconde table ou objet de type table. Une jointure ne peut être basée sur des colonnes de type **text**, **ntext**ou**image** .  
  
    > [!NOTE]  
    > Les colonnes de jointure doivent afficher des types de données identiques (ou compatibles). Par exemple, si la colonne de jointure de la première table est une date, il faut la lier à une colonne de dates de la seconde table. En revanche, si la première colonne de jointure est un entier, la colonne de jointure liée doit également comporter des données de type entier mais la longueur peut varier. Le Concepteur de requêtes et de vues ne vérifie pas les types de données des colonnes utilisées pour créer une jointure mais, lorsque vous exécutez la requête, la base de données affiche une erreur si les types de données sont incompatibles.  
  
3.  Le cas échéant, remplacez l'opérateur de jointure qui est, par défaut un signe égal (=). Pour plus d’informations, consultez [Modifier des opérateurs de jointure &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/modify-join-operators-visual-database-tools.md).  
  
Le Concepteur de requêtes et de vues ajoute une clause INNER JOIN à l’instruction SQL dans le [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md). Vous pouvez changer ce type de jointure en jointure externe. Pour plus d’informations, consultez [Créer des jointures externes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-outer-joins-visual-database-tools.md).  
  
## <a name="see-also"></a> Voir aussi  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
  
