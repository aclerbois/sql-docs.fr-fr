---
title: Lecture des données dans une table (tutoriel) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- reading data in a table
ms.assetid: 532232c9-3d41-45cd-9150-de67a1cbfcf5
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b6ddabf394394952a730522a610a802d8cfd4569
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-4---reading-the-data-in-a-table"></a>Leçon 1-4 - Lecture des données dans une table
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
Utilisez l'instruction SELECT pour lire les données dans une table. L'instruction SELECT est une des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] les plus importantes et dont la syntaxe comporte beaucoup de variations. Pour ce didacticiel, vous allez travailler avec cinq versions simples.  
  
### <a name="to-read-the-data-in-a-table"></a>Pour lire les données dans une table  
  
1.  Tapez et exécutez les instructions suivantes pour lire les données dans la table `Products` .  
  
    ```  
    -- The basic syntax for reading data from a single table  
    SELECT ProductID, ProductName, Price, ProductDescription  
        FROM dbo.Products  
    GO  
  
    ```  
  
2.  Vous pouvez utiliser un astérisque pour sélectionner toutes les colonnes de la table. Cette opération s'utilise souvent dans les requêtes ad hoc. Vous devez fournir la liste de la colonne dans votre code permanent pour que l'instruction retourne les colonnes prédites, même si une nouvelle colonne est ajoutée à la table ultérieurement.  
  
    ```  
    -- Returns all columns in the table  
    -- Does not use the optional schema, dbo  
    SELECT * FROM Products  
    GO  
  
    ```  
  
3.  Vous pouvez omettre les colonnes que vous ne souhaitez pas retourner. Les colonnes seront retournées dans leur ordre d'apparition.  
  
    ```  
    -- Returns only two of the columns from the table  
    SELECT ProductName, Price  
        FROM dbo.Products  
    GO  
  
    ```  
  
4.  Utilisez une clause `WHERE` pour limiter les lignes retournées à l'utilisateur.  
  
    ```  
    -- Returns only two of the records in the table  
    SELECT ProductID, ProductName, Price, ProductDescription  
        FROM dbo.Products  
        WHERE ProductID < 60  
    GO  
  
    ```  
  
5.  Vous pouvez travailler avec les valeurs des colonnes à mesure qu'elles sont retournées. L'exemple suivant effectue une opération mathématique sur la colonne `Price` . Les colonnes ayant été modifiées de cette manière n'ont pas de nom sauf si vous en fournissez un à l'aide du mot clé `AS` .  
  
    ```  
    -- Returns ProductName and the Price including a 7% tax  
    -- Provides the name CustomerPays for the calculated column  
    SELECT ProductName, Price * 1.07 AS CustomerPays  
        FROM dbo.Products  
    GO  
    ```  
  
## <a name="functions-that-are-useful-in-a-select-statement"></a>Fonctions utiles dans une instruction SELECT  
Pour des informations sur certaines fonctions que vous pouvez utiliser pour travailler avec des données dans les instructions SELECT, consultez les rubriques suivantes :  
  
|||  
|-|-|  
|[Fonctions de chaîne &#40;Transact-SQL&#41;](../t-sql/functions/string-functions-transact-sql.md)|[Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)|  
|[Fonctions mathématiques &#40;Transact-SQL&#41;](../t-sql/functions/mathematical-functions-transact-sql.md)|[Fonctions texte et image &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/b9c70488-1bf5-4068-a003-e548ccbc5199)|  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Résumé : Création des objets de base de données](../t-sql/lesson-1-5-summary-creating-database-objects.md)  
  
## <a name="see-also"></a> Voir aussi  
[SELECT &#40;Transact-SQL&#41;](../t-sql/queries/select-transact-sql.md)  
  
  
  
