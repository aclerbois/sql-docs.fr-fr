---
title: Remplissage d’une table avec des données hiérarchiques existantes | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
helpviewer_keywords:
- HierarchyID
ms.assetid: fd943d84-dbe6-4a05-912b-c88164998d80
caps.latest.revision: 23
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5d69702ab9c87a41a87b1c9ead6dd8c567f197c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-2---populating-a-table-with-existing-hierarchical-data"></a>Leçon 1-2 : remplissage d’une table avec des données hiérarchiques existantes
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]
Cette tâche crée une table et la remplit avec les données de la table **EmployeeDemo** . Les étapes de cette tâche sont les suivantes :  
  
-   Créez une table qui contient une colonne **hierarchyid** . Cette colonne pourrait remplacer les colonnes **EmployeeID** et **ManagerID** existantes. Toutefois, vous conserverez ces colonnes. Cela s'explique par le fait que les applications existantes peuvent faire référence à ces colonnes. De même, cela peut vous aider à comprendre les données après le transfert. La définition de table spécifie que **OrgNode** est la clé primaire, ce qui exige que la colonne contienne des valeurs uniques. L’index cluster sur la colonne **OrgNode** stockera la date dans la séquence **OrgNode** .  
  
-   Créez une table temporaire utilisée pour effectuer le suivi du nombre d'employés dont chaque responsable est le supérieur direct.  
  
-   Remplissez la nouvelle table en utilisant les données de la table **EmployeeDemo** .  
  
### <a name="to-create-a-new-table-named-neworg"></a>Pour créer une table nommée NewOrg  
  
-   Dans une fenêtre de l’Éditeur de requête, exécutez le code suivant pour créer une table nommée **HumanResources.NewOrg**:  
  
    ```  
    CREATE TABLE HumanResources.NewOrg  
    (  
      OrgNode hierarchyid,  
      EmployeeID int,  
      LoginID nvarchar(50),  
      ManagerID int  
    CONSTRAINT PK_NewOrg_OrgNode  
      PRIMARY KEY CLUSTERED (OrgNode)  
    );  
    GO  
    ```  
  
### <a name="to-create-a-temporary-table-named-children"></a>Pour créer une table temporaire nommée #Children  
  
1.  Créez une table temporaire nommée **#Children** avec une colonne nommée **Num** qui contiendra le nombre d’enfants pour chaque nœud :  
  
    ```  
    CREATE TABLE #Children   
       (  
        EmployeeID int,  
        ManagerID int,  
        Num int  
    );  
    GO  
    ```  
  
2.  Ajoutez un index qui accélérera considérablement la requête qui remplit la table **NewOrg** :  
  
    ```  
    CREATE CLUSTERED INDEX tmpind ON #Children(ManagerID, EmployeeID);  
    GO  
    ```  
  
### <a name="to-populate-the-neworg-table"></a>Pour remplir la table NewOrg  
  
1.  Les requêtes récursives interdisent les sous-requêtes avec agrégats. À la place, remplissez la table **#Children** avec le code suivant, qui utilise la méthode [ROW_NUMBER()](../../t-sql/functions/row-number-transact-sql.md) pour remplir la colonne **Num** :  
  
    ```  
    INSERT #Children (EmployeeID, ManagerID, Num)  
    SELECT EmployeeID, ManagerID,  
      ROW_NUMBER() OVER (PARTITION BY ManagerID ORDER BY ManagerID)   
    FROM HumanResources.EmployeeDemo  
    GO 
    ```  
  
2.  Examinez la table **#Children** . Notez la façon dont la colonne **Num** contient des numéros séquentiels pour chaque responsable.  
  
    ```  
    SELECT * FROM #Children ORDER BY ManagerID, Num  
    GO  
  
    ```  
  
    [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

    ```
    EmployeeID  ManagerID   Num
    1   NULL    1
    2   1   1
    16  1   2
    25  1   3
    234 1   4
    263 1   5
    273 1   6
    3   2   1
    4   3   1
    5   3   2
    6   3   3
    7   3   4
    ```


3.  Remplissez la table **NewOrg** . Utilisez les méthodes GetRoot et ToString pour concaténer les valeurs **Num** au format **hierarchyid** , puis mettez à jour la colonne **OrgNode** avec les valeurs hiérarchiques résultantes :  
  
    ```  
    WITH paths(path, EmployeeID)   
    AS (  
    -- This section provides the value for the root of the hierarchy  
    SELECT hierarchyid::GetRoot() AS OrgNode, EmployeeID   
    FROM #Children AS C   
    WHERE ManagerID IS NULL   

    UNION ALL   
    -- This section provides values for all nodes except the root  
    SELECT   
    CAST(p.path.ToString() + CAST(C.Num AS varchar(30)) + '/' AS hierarchyid),   
    C.EmployeeID  
    FROM #Children AS C   
    JOIN paths AS p   
       ON C.ManagerID = P.EmployeeID   
    )  
    INSERT HumanResources.NewOrg (OrgNode, O.EmployeeID, O.LoginID, O.ManagerID)  
    SELECT P.path, O.EmployeeID, O.LoginID, O.ManagerID  
    FROM HumanResources.EmployeeDemo AS O   
    JOIN Paths AS P   
       ON O.EmployeeID = P.EmployeeID  
    GO 
    ```  
  
4.  Une colonne **hierarchyid** est plus compréhensible quand vous la convertissez au format caractère. Vérifiez les données de la table **NewOrg** en exécutant le code suivant, qui contient deux représentations de la colonne **OrgNode** :  
  
    ```  
    SELECT OrgNode.ToString() AS LogicalNode, *   
    FROM HumanResources.NewOrg   
    ORDER BY LogicalNode;  
    GO  
    ```  
  
    La colonne **LogicalNode** convertit la colonne **hierarchyid** en format texte plus lisible qui représente la hiérarchie. Dans les tâches restantes, vous utiliserez la méthode `ToString()` pour afficher le format logique des colonnes **hierarchyid** .  
  
5.  Supprimez la table temporaire, qui n'est plus nécessaire :  
  
    ```  
    DROP TABLE #Children  
    GO  
    ```  
  
La tâche suivante créera des index pour prendre en charge la structure hiérarchique.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Optimisation de la table NewOrg](../../relational-databases/tables/lesson-1-3-optimizing-the-neworg-table.md)  
  
  
  
