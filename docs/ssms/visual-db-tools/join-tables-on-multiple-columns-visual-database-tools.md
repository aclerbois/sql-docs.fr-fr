---
title: Joindre des tables sur plusieurs colonnes (Visual Database Tools) | Microsoft Docs
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
- multiple column joins
- joins [SQL Server], multiple columns
ms.assetid: 56a158bc-a42a-4b78-baad-4721d2d22cd3
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4aedfa08f99d7d876819b91e55a376ce83318c13
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="join-tables-on-multiple-columns-visual-database-tools"></a>Joindre des tables sur plusieurs colonnes (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Il est possible de joindre les tables sur la base de plusieurs colonnes. En d'autres termes, vous pouvez créer une requête qui fait correspondre les lignes de deux tables uniquement si elles satisfont à plusieurs conditions. Si la base de données contient une relation qui fait correspondre plusieurs colonnes clés étrangères d'une table à une clé primaire multicolonne de l'autre table, vous pouvez utiliser cette relation pour créer une jointure multicolonne. Pour plus d’informations, consultez [Joindre automatiquement des tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-automatically-visual-database-tools.md).  
  
Même si la base de données ne comprend aucune relation de clé étrangère multicolonne, il est toujours possible de créer la jointure manuellement.  
  
### <a name="to-manually-create-a-multicolumn-join"></a>Pour créer manuellement une jointure multicolonne  
  
1.  Ajoutez les tables à joindre dans le [volet Schéma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) .  
  
2.  Faites glisser le nom de la colonne de jointure de la fenêtre de la première table jusqu'à la colonne liée de la fenêtre de la seconde table. Une jointure ne peut être basée sur des colonnes de type text, ntext ou image.  
  
    > [!NOTE]  
    > En général, les colonnes de jointure doivent afficher des types de données identiques (ou compatibles). Par exemple, si la colonne de jointure de la première table est une date, il faut la lier à une colonne de dates de la seconde table. En revanche, si la première colonne de jointure est un entier, la colonne de jointure liée doit également comporter des données de type entier mais la longueur peut varier. Toutefois, il peut arriver que les conversions de types de données implicites joignent des colonnes apparemment incompatibles.  
    >   
    > Le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) ne vérifie pas les types de données des colonnes utilisées pour créer une jointure mais, quand vous exécutez la requête, la base de données affiche une erreur si les types de données sont incompatibles.  
  
3.  Faites glisser le nom de la seconde colonne de jointure de la fenêtre de la première table jusqu'à la colonne associée dans la fenêtre de la seconde table.  
  
4.  Répétez l'étape 3 pour chaque nouvelle paire de colonnes de jointure dans les deux tables.  
  
5.  Exécute la requête.  
  
## <a name="see-also"></a> Voir aussi  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
  
