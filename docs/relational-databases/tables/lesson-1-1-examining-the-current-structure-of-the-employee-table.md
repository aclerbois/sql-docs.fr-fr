---
title: Étude de la structure actuelle de la table Employee | Microsoft Docs
ms.custom: ''
ms.date: 03/27/2018
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
- examining the current structure of the employee
ms.assetid: d546a820-105a-417d-ac35-44a6d1d70ac6
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 620defd42e27b122a92ee145da6b4b14d1cbf996
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-1---examining-the-current-structure-of-the-employee-table"></a>Leçon 1-1 : étude de la structure actuelle de la table Employee
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]
L’exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] (ou ultérieur) contient une table **Employee** dans le schéma **HumanResources**. Afin d'éviter de modifier la table d'origine, cette étape effectue une copie de la table **Employee** , nommée **EmployeeDemo**. Pour simplifier l'exemple, vous ne copiez que cinq colonnes de la table d'origine. Vous interrogez ensuite la table **HumanResources.EmployeeDemo** pour vérifier comment sont structurées les données dans une table sans utiliser le type de données **hierarchyid** .  
  
### <a name="to-copy-the-employee-table"></a>Pour copier la table Employee  
  
1.  Dans une fenêtre de l'Éditeur de requête, exécutez le code suivant pour copier la structure et les données de la table **Employee** dans une nouvelle table nommée **EmployeeDemo**. Étant donné que la table d’origine utilise déjà hierarchyid, cette requête aplatit essentiellement la hiérarchie pour obtenir le nom du responsable de l’employé. Dans les parties suivantes de cette leçon, nous reconstruirons cette hiérarchie.
  
    ```  
    USE AdventureWorks ;  
    GO  
  
    SELECT emp.BusinessEntityID AS EmployeeID, emp.LoginID, 
      (SELECT  man.BusinessEntityID FROM HumanResources.Employee man 
            WHERE emp.OrganizationNode.GetAncestor(1)=man.OrganizationNode OR 
                (emp.OrganizationNode.GetAncestor(1) = 0x AND man.OrganizationNode IS NULL)) AS ManagerID
        , emp.JobTitle, emp.HireDate
    INTO HumanResources.EmployeeDemo   
    FROM HumanResources.Employee emp ;
    GO
    ```  
  
### <a name="to-examine-the-structure-and-data-of-the-employeedemo-table"></a>Pour examiner la structure et les données de la table EmployeeDemo  
  
-   Cette nouvelle table **EmployeeDemo** représente une table classique dans une base de données existante que vous pouvez souhaiter migrer vers une nouvelle structure. Dans une fenêtre de l'Éditeur de requête, exécutez le code suivant pour voir comment la table utilise une jointure réflexive pour afficher les relations employé/responsable :  
  
    ```  
    SELECT   
        Mgr.EmployeeID AS MgrID, Mgr.LoginID AS Manager,   
        Emp.EmployeeID AS E_ID, Emp.LoginID, Emp.JobTitle  
    FROM HumanResources.EmployeeDemo AS Emp  
    LEFT JOIN HumanResources.EmployeeDemo AS Mgr  
    ON Emp.ManagerID = Mgr.EmployeeID  
    ORDER BY MgrID, E_ID  
    ```  
  
    [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
    ```  
    MgrID Manager                 E_ID LoginID                  JobTitle  
    NULL    NULL    1   adventure-works\ken0    Chief Executive Officer
    1   adventure-works\ken0    2   adventure-works\terri0  Vice President of Engineering
    1   adventure-works\ken0    16  adventure-works\david0  Marketing Manager
    1   adventure-works\ken0    25  adventure-works\james1  Vice President of Production
    1   adventure-works\ken0    234 adventure-works\laura1  Chief Financial Officer
    1   adventure-works\ken0    263 adventure-works\jean0   Information Services Manager
    1   adventure-works\ken0    273 adventure-works\brian3  Vice President of Sales
    2   adventure-works\terri0  3   adventure-works\roberto0    Engineering Manager
    3   adventure-works\roberto0    4   adventure-works\rob0    Senior Tool Designer
    ...  
    ```  
  
    Les résultats se poursuivent pour un total de 290 lignes.  
  
Notez que le résultat de la clause **ORDER BY** a provoqué le regroupement des subordonnés directs de chaque niveau de gestion. Par exemple, les sept subordonnés directs de **MgrID** 1 (ken0) sont regroupés les uns à côté des autres. Il est possible, mais beaucoup plus difficile, de regrouper tous ceux dont **MgrID** 1 est le supérieur final.  
  
Dans la tâche suivante, nous créerons une table avec un type de données **hierarchyid** et déplacerons les données dans la nouvelle table.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Remplissage d'une table avec des données hiérarchiques existantes](../../relational-databases/tables/lesson-1-2-populating-a-table-with-existing-hierarchical-data.md)  
  
  
  
