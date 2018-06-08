---
title: Créer des sous-requêtes (Visual Database Tools) | Microsoft Docs
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
- search criteria [SQL Server], subqueries
- nested queries
- subqueries [SQL Server], SQL pane
ms.assetid: 34f6b9b4-ca3a-4a4f-9594-36e513f1c547
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3d3f45e5994251b28e5ecdd215c327f930ab2cd3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-subqueries-visual-database-tools"></a>Créer des sous-requêtes (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez utiliser les résultats d'une requête en tant que point de départ d'une autre requête. Vous pouvez utiliser les résultats d’une sous-requête en tant qu’instruction faisant appel à la fonction IN( ), à l’opérateur EXISTS ou à la clause FROM.  
  
Vous pouvez créer une sous-requête en l'introduisant directement dans le volet SQL ou copier une requête et la coller dans un autre.  
  
### <a name="to-define-a-subquery-in-the-sql-pane"></a>Pour définir une sous-requête dans le volet SQL  
  
1.  Créez la requête de base.  
  
2.  Dans le volet SQL, sélectionnez l’instruction SQL, puis utilisez la commande **Copier** pour déplacer la requête dans le Presse-papiers.  
  
3.  Créez la requête, puis utilisez la commande **Coller** pour coller la première requête dans la clause WHERE ou FROM de cette nouvelle requête.  
  
    Imaginez, par exemple, que vous soyez en présence de deux tables ( `products` et `suppliers`) et que vous souhaitiez créer une requête indiquant tous les produits de fournisseurs suédois. Créez la première requête relative à la table `suppliers` pour rechercher tous les fournisseurs suédois :  
  
    ```  
    SELECT supplier_id  
    FROM supplier  
    WHERE (country = 'Sweden')  
    ```  
  
    Utilisez la commande Copier pour copier cette requête dans le Presse-papiers. Créez la deuxième requête relative à la table `products` répertoriant toutes les informations nécessaires sur les produits :  
  
    ```  
    SELECT product_id, supplier_id, product_name  
    FROM products  
    ```  
  
    Dans le volet SQL, ajoutez une clause WHERE à la deuxième requête, puis collez la première requête à partir du Presse-papiers. Ajoutez des parenthèses autour de la première requête afin d'obtenir le résultat final suivant :  
  
    ```  
    SELECT product_id, supplier_id, product_name  
    FROM products  
    WHERE supplier_id IN  
       (SELECT supplier_id  
      FROM supplier  
      WHERE (country = 'Sweden'))  
    ```  
  
## <a name="see-also"></a> Voir aussi  
[Types de requêtes pris en charge &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/supported-query-types-visual-database-tools.md)  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
