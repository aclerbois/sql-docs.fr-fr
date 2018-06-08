---
title: Modes de SQL (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: d840ee51-b863-4e77-84aa-37d3f094bfed
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 748348f5f9d2f97d359e585c77646bf5f5fd9ffa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-modes-mysqltosql"></a>Modes de SQL (MySQLToSQL)
SSMA pour MySQL peut fonctionner dans différents Modes SQL et applique ces modes différemment pour les différents clients.  
  
Modes de définissent la syntaxe SQL qui MySQL doit prendre en charge et le type de validation des données vérifie qu’il doit effectuer. Cela rend plus facile à utiliser MySQL dans différents environnements et d’utiliser MySQL avec SQL Server.  
  
## <a name="sql-modes-grid"></a>Grille de Modes SQL :  
  
-   Grille de Modes SQL au niveau racine contient les colonnes suivantes : **nom du Mode SQL**, **chargé les Modes SQL**, et **efficace Modes SQL**.  
  
-   Grille de Modes SQL au niveau des bases de données catégorie, base de données, Table catégorie, catégorie d’instructions, catégories de vues, table, vue, fonctions, procédures, UDF et niveau de l’objet événement contient les colonnes suivantes : **nom du Mode SQL**, **héritée les Modes SQL**, et **efficace Modes SQL**.  
  
-   Grille de Modes SQL au niveau de la procédure stockée, fonction de stockée et déclencheur contient les colonnes suivantes : **nom du Mode SQL**, **les Modes SQL d’origine**, et **efficace Modes SQL**.  
  
> [!NOTE]  
> Modes de groupe seront affichées dans gras, sous la colonne « Nom du Mode SQL ».  
  
## <a name="loaded-sql-modes"></a>Modes SQL chargé  
Voici les Modes SQL, qui sont définies au niveau de la session ou racine. Les Modes SQL une fois chargés dans la base de données cible ne peut pas être modifiés ou édités.  
  
## <a name="inherited-sql-modes"></a>Modes SQL héritées  
Voici les Modes SQL, qui sont héritées à partir du nœud Parent correspondant.  
  
À l’exception de la catégorie de fonctions, procédures catégorie, catégorie d’événements et déclencheurs, ces Modes SQL sont présents à tous les niveaux (objet de base de données, Table catégorie, catégorie d’instructions, vues catégorie, table, vue, fonctions, procédures, UDF et événements).  
  
> [!NOTE]  
> En sélectionnant le **hériter du Parent** case à cocher, héritée les Modes SQL peuvent être héritée à partir du nœud parent. Par défaut, cette case à cocher est sélectionnée.  
  
## <a name="original-sql-modes"></a>Modes SQL d’origine  
Voici les Modes SQL présent au niveau uniquement fonction, procédure et déclencheur.  
  
> [!NOTE]  
> En sélectionnant le **utiliser l’original** vérifier zone, les Modes SQL qui ont été initialement utilisé dans la fonction correspondante ou de procédure ou un déclencheur peut être utilisé. Par défaut, cette case à cocher est sélectionnée.  
  
## <a name="effective-sql-modes"></a>Modes SQL efficace  
Les Modes SQL effective peut être définis à différents niveaux de la manière suivante :  
  
-   **Au niveau de la session :**  
  
    1.  Tous les Modes SQL chargé peut être appelés, « Modes efficace de SQL ».  
  
    2.  À ce niveau, les modes SQL effectives peuvent être directement et explicitement modifiées.  
  
    3.  Le Mode de SQL efficace qui est défini explicitement ne sont pas reflétée comme Mode de SQL chargé et pour finir, est appliqué à l’objet.  
  
-   **Au niveau de la fonction ou procédure ou un déclencheur :**  
  
    1.  Tous les Modes SQL d’origine peuvent être appelées, « Modes efficace de SQL ».  
  
    2.  À ce niveau, le mode SQL effectif peut être explicitement modifié uniquement lorsque le **utiliser l’original** case à cocher est désactivée.  
  
    3.  Le Mode de SQL efficace qui est défini explicitement ne sont pas reflétée en tant que le Mode SQL d’origine et pour finir, est appliqué à l’objet.  
  
-   **Au niveau des nœuds autres que de la fonction ou procédure ou un déclencheur de niveau :**  
  
    1.  Tous les Modes SQL hérités peuvent être appelées, « Modes efficace de SQL ».  
  
    2.  À ce niveau, le mode SQL effectif peut être explicitement modifié uniquement lorsque le **hériter du Parent** case à cocher est désactivée.  
  
    3.  Le Mode de SQL efficace qui est défini explicitement ne sont pas reflétée comme Mode de SQL héritées et pour finir, est appliqué à l’objet.  
  
