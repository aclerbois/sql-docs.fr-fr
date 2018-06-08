---
title: Interrogation d’une table hiérarchique à l’aide de méthodes hiérarchiques | Microsoft Docs
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
ms.assetid: 3b4f7dae-65b5-4d8d-8641-87aba9aa692d
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ac9261cbe8d084875af36f4b0261dc24d05da5d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-3---querying-a-hierarchical-table-using-hierarchy-methods"></a>Leçon 2-3 : Interrogation d’une table hiérarchique à l’aide de méthodes hiérarchiques
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]
Maintenant que la table HumanResources.EmployeeOrg est entièrement remplie, cette tâche vous indiquera comment interroger la hiérarchie à l'aide de certaines des méthodes hiérarchiques.  
  
### <a name="to-find-subordinate-nodes"></a>Pour rechercher des nœuds subordonnés  
  
1.  Sariya a un employé subordonné. Pour rechercher les subordonnés de Sariya, exécutez la requête suivante qui utilise la méthode [IsDescendantOf](../../t-sql/data-types/isdescendantof-database-engine.md) :  
  
    ```  
    DECLARE @CurrentEmployee hierarchyid  
  
    SELECT @CurrentEmployee = OrgNode  
    FROM HumanResources.EmployeeOrg  
    WHERE EmployeeID = 46 ;  
  
    SELECT *  
    FROM HumanResources.EmployeeOrg  
    WHERE OrgNode.IsDescendantOf(@CurrentEmployee ) = 1 ;  
    ```  
  
    Le résultat répertorie Sariya et Wanida. Sariya est répertoriée car elle est la descendante au niveau 0. Wanida est la descendante au niveau 1.  
  
2.  Vous pouvez également créer des requêtes sur ces informations à l’aide de la méthode [GetAncestor](../../t-sql/data-types/getancestor-database-engine.md) . `GetAncestor` prend un argument pour le niveau que vous tentez de retourner. Étant donné que Wanida est un niveau plus bas que Sariya, utilisez `GetAncestor(1)` comme indiqué dans le code suivant :  
  
    ```  
    DECLARE @CurrentEmployee hierarchyid  
  
    SELECT @CurrentEmployee = OrgNode  
    FROM HumanResources.EmployeeOrg  
    WHERE EmployeeID = 46 ;  
  
    SELECT OrgNode.ToString() AS Text_OrgNode, *  
    FROM HumanResources.EmployeeOrg  
    WHERE OrgNode.GetAncestor(1) = @CurrentEmployee  
    ```  
  
    Cette fois-ci, le résultat répertorie uniquement Wanida.  
  
3.  Modifiez maintenant `@CurrentEmployee` sur David (EmployeeID 6) et le niveau sur 2. Exécutez le code suivant pour retourner également Wanida :  
  
    ```  
    DECLARE @CurrentEmployee hierarchyid  
  
    SELECT @CurrentEmployee = OrgNode  
    FROM HumanResources.EmployeeOrg  
    WHERE EmployeeID = 6 ;  
  
    SELECT OrgNode.ToString() AS Text_OrgNode, *  
    FROM HumanResources.EmployeeOrg  
    WHERE OrgNode.GetAncestor(2) = @CurrentEmployee  
    ```  
  
    Cette fois-ci, le résultat répertorie également Mary, deux niveaux plus bas, dont David est également le supérieur.  
  
### <a name="to-use-getroot-and-getlevel"></a>Pour utiliser GetRoot et GetLevel  
  
1.  À mesure que la hiérarchie s'agrandit, il devient plus difficile de déterminer l'emplacement des membres dans la hiérarchie. La méthode [GetLevel](../../t-sql/data-types/getlevel-database-engine.md) permet de déterminer le nombre de niveaux sous chaque ligne dans la hiérarchie. Exécutez le code suivant pour consulter les niveaux de toutes les lignes :  
  
    ```  
    SELECT OrgNode.ToString() AS Text_OrgNode,   
    OrgNode.GetLevel() AS EmpLevel, *  
    FROM HumanResources.EmployeeOrg ;  
    GO  
  
    ```  
  
2.  La méthode [GetRoot](../../t-sql/data-types/getroot-database-engine.md) permet de rechercher le nœud racine dans la hiérarchie. Le code suivant retourne la ligne unique qui est la racine :  
  
    ```  
    SELECT OrgNode.ToString() AS Text_OrgNode, *  
    FROM HumanResources.EmployeeOrg  
    WHERE OrgNode = hierarchyid::GetRoot() ;  
    GO  
  
    ```  
  
La tâche suivante réorganisera la hiérarchie.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Réorganisation de données dans une table hiérarchique à l'aide de méthodes hiérarchiques](../../relational-databases/tables/lesson-2-4-reordering-data-in-a-hierarchical-table-using-hierarchical-methods.md)  
  
  
  
