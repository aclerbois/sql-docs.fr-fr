---
title: Création des vues et des procédures stockées | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
helpviewer_keywords:
- creating views and stored procedures
ms.assetid: 53a0426d-07d8-4b7c-aa21-22632753bad8
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9731a52a783239755b78ae4ae967fc8ba6e44f73
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-3---creating-views-and-stored-procedures"></a>Leçon 2-3 - Création des vues et des procédures stockées
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
Étant donné que Mary peut désormais accéder à la base de données **TestData** , vous pouvez envisager de créer des objets de base de données, tels qu'une vue et une procédure stockée, puis octroyer à Mary l'accès à ses objets. Une vue est une instruction SELECT stockée, et une procédure stockée correspond à une ou plusieurs instructions [!INCLUDE[tsql](../includes/tsql-md.md)] qui s'exécutent comme un traitement.  
  
Les vues peuvent être interrogées comme des tables et n'acceptent pas de paramètres. Les procédures stockées sont plus complexes que les vues. Elles peuvent contenir des paramètres d'entrée et de sortie et peuvent contenir des instructions pour contrôler le flux du code, comme les instructions IF et WHILE. Il est recommandé de faire appel aux procédures stockées pour coder toutes les actions répétitives dans la base de données  
  
Pour cet exemple, vous allez utiliser CREATE VIEW pour créer une vue qui sélectionne seulement deux des colonnes dans la table **Products** . Puis, vous allez utiliser l'instruction CREATE PROCEDURE pour créer une procédure stockée qui accepte un paramètre de prix et retourne uniquement les produits dont le prix est inférieur à la valeur de paramètre spécifiée.  
  
### <a name="to-create-a-view"></a>Pour créer une vue  
  
1.  Exécutez l'instruction suivante pour créer une vue très simple qui exécute une instruction SELECT et retourne les noms et les prix de nos produits à l'utilisateur.  
  
    ```  
    CREATE VIEW vw_Names  
       AS  
       SELECT ProductName, Price FROM Products;  
    GO  
  
    ```  
  
### <a name="test-the-view"></a>Tester la vue  
  
1.  Les vues sont traitées comme des tables. Utilisez une instruction `SELECT` pour accéder à une vue.  
  
    ```  
    SELECT * FROM vw_Names;  
    GO  
  
    ```  
  
### <a name="to-create-a-stored-procedure"></a>Pour créer une procédure stockée  
  
1.  L'instruction suivante crée un nom de procédure stockée `pr_Names`, accepte un paramètre d'entrée nommé `@VarPrice` du type de données `money`. La procédure stockée imprime l'instruction `Products less than` concaténée avec le paramètre d'entrée dont le type de données `money` est remplacé par un type de données character `varchar(10)` . Puis, la procédure exécute une instruction `SELECT` sur la vue et passe le paramètre d'entrée dans le cadre de la clause `WHERE` . Cette opération retourne tous les produits dont le prix est inférieur à la valeur du paramètre d'entrée.  
  
    ```  
    CREATE PROCEDURE pr_Names @VarPrice money  
       AS  
       BEGIN  
          -- The print statement returns text to the user  
          PRINT 'Products less than ' + CAST(@VarPrice AS varchar(10));  
          -- A second statement starts here  
          SELECT ProductName, Price FROM vw_Names  
                WHERE Price < @varPrice;  
       END  
    GO  
  
    ```  
  
### <a name="test-the-stored-procedure"></a>Tester la procédure stockée  
  
1.  Pour tester la procédure stockée, tapez et exécutez l'instruction suivante. La procédure doit retourner les noms des deux produits entrés dans la table `Products` à la leçon 1 avec un prix inférieur à `10.00`.  
  
    ```  
    EXECUTE pr_Names 10.00;  
    GO  
  
    ```  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Octroi de l'accès à un objet de base de données](../t-sql/lesson-2-4-granting-access-to-a-database-object.md)  
  
## <a name="see-also"></a> Voir aussi  
[CREATE VIEW &#40;Transact-SQL&#41;](../t-sql/statements/create-view-transact-sql.md)  
[CREATE PROCEDURE &#40;Transact-SQL&#41;](../t-sql/statements/create-procedure-transact-sql.md)  
  
  
  
