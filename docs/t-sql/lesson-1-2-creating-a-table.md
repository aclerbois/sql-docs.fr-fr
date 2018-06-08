---
title: Création d’une table (tutoriel) | Microsoft Docs
ms.custom: ''
ms.date: 04/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- creating tables
ms.assetid: 653f2dd3-36a2-4bd5-8703-71a57d244661
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 97aa24c9a3a320577073cf42ca5dd8cb6cbf08d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-2---creating-a-table"></a>Leçon 1-2 - Création d’une table
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

Pour créer une table, vous devez fournir un nom pour la table et les noms et les types de données de chaque colonne dans la table. Il est aussi recommandé d'indiquer si les valeurs Null sont autorisées dans chaque colonne. Pour créer une table, vous devez avoir les autorisations `CREATE TABLE` et `ALTER SCHEMA` sur le schéma qui contiendra la table. Le rôle fixe de base de données `db_ddladmin` dispose de ces autorisations.  
  
La plupart des tables possèdent une clé primaire constituée d'une ou plusieurs colonnes de la table. Une clé primaire est toujours unique. Le [!INCLUDE[ssDE](../includes/ssde-md.md)] applique la restriction qui veut que les valeurs de clé primaire ne peuvent pas être répétées dans la table.  
  
Pour obtenir une liste des types de données et des liens contenant une description individuelle, consultez [Types de données &#40;Transact-SQL&#41;](../t-sql/data-types/data-types-transact-sql.md).  
  
> [!NOTE]  
> Vous pouvez configurer le [!INCLUDE[ssDE](../includes/ssde-md.md)] pour qu’il respecte la casse ou non. Si vous installez le [!INCLUDE[ssDE](../includes/ssde-md.md)] pour qu’il respecte la casse, les noms des objets doivent toujours avoir la même casse. Par exemple, une table nommée OrderData est différente d'une table nommée ORDERDATA. Si le [!INCLUDE[ssDE](../includes/ssde-md.md)] ne respecte pas la casse, ces deux noms de tables sont considérés comme une seule et même table, et ce nom ne peut être utilisé qu’une fois.  
  
### <a name="to-create-a-database-to-contain-the-new-table"></a>Pour créer une base de données qui contient la nouvelle table  
  
-   Entrez le code suivant dans une fenêtre de l'Éditeur de requête.  
  
    ```  
    USE master;  
    GO  
  
    --Delete the TestData database if it exists.  
    IF EXISTS(SELECT * from sys.databases WHERE name='TestData')  
    BEGIN  
        DROP DATABASE TestData;  
    END  
  
    --Create a new database called TestData.  
    CREATE DATABASE TestData;  
    Press the F5 key to execute the code and create the database.  
    ```  
  
### <a name="switch-the-query-editor-connection-to-the-testdata-database"></a>Passer la connexion de l'Éditeur de requête à la base de données TestData  
  
-   Dans une fenêtre Éditeur de requêtes, tapez et exécutez le code suivant pour modifier votre connexion à la base de données `TestData` .  
  
    ```  
    USE TestData  
    GO  
    ```  
  
### <a name="to-create-a-table"></a>Pour créer une table  
  
-   Dans une fenêtre Éditeur de requêtes, tapez et exécutez le code suivant pour créer une simple table nommée `Products`. Les colonnes de la table sont nommées `ProductID`, `ProductName`, `Price`, et `ProductDescription`. La colonne `ProductID` est la clé primaire de la table. `int`, `varchar(25)`, `money`et `text` sont tous des types de données. Seules les colonnes `Price` et `ProductionDescription` peuvent n'avoir aucune données lors de l'insertion ou de la modification d'une ligne. Cette instruction contient un élément facultatif (`dbo.`) appelé un schéma. Le schéma est l'objet de base de données qui est propriétaire de la table. Si vous êtes administrateur, `dbo` est le schéma par défaut. `dbo` représente le propriétaire de la base de données.  
  
    ```  
    CREATE TABLE dbo.Products  
       (ProductID int PRIMARY KEY NOT NULL,  
        ProductName varchar(25) NOT NULL,  
        Price money NULL,  
        ProductDescription text NULL)  
    GO  
    ```  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Insertion et mise à jour des données dans une table &#40;Didacticiel&#41;](../t-sql/lesson-1-3-inserting-and-updating-data-in-a-table.md)  
  
## <a name="see-also"></a> Voir aussi  
[CREATE TABLE &#40;Transact-SQL&#41;](../t-sql/statements/create-table-transact-sql.md)  
  
  
  

