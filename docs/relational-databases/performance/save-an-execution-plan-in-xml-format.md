---
title: Enregistrer un plan d’exécution au format XML | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XML query plans [SQL Server]
- opening execution plans
- XML Showplans [SQL Server]
- execution plans [SQL Server], saving
- saving execution plans
ms.assetid: c439e53b-56f3-4442-97c6-dabd48a203d8
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2d48e9e6902e3bd201b395aa817d3ec3f26ad222
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="save-an-execution-plan-in-xml-format"></a>Enregistrer un plan d'exécution au format XML
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour enregistrer un plan d'exécution en tant que fichier XML, puis pour l'ouvrir et le consulter.  
  
 Pour utiliser la fonctionnalité de plan d'exécution de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ou les options SET de Showplan XML, les utilisateurs doivent disposer des autorisations appropriées pour exécuter la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] pour laquelle un plan d'exécution est généré et doivent obtenir l'autorisation SHOWPLAN pour toutes les bases de données référencées par la requête.  
  
### <a name="to-save-a-query-plan-by-using-the-xml-showplan-set-options"></a>Pour enregistrer un plan de requête avec les options SET de Showplan XML  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , ouvrez un éditeur de requête et connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Activez [SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md) avec l’instruction suivante :  
  
    ```sql  
    SET SHOWPLAN_XML ON;  
    GO  
    ```  
  
     Pour activer [STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md), utilisez l’instruction suivante :  
  
    ```sql  
    SET STATISTICS XML ON;  
    GO  
    ```  
  
     > [!NOTE] 
     > SHOWPLAN_XML génère des informations de plan d'exécution de requête de compilation pour une requête, mais sans exécuter cette dernière. On emploie également le terme de « plan d’exécution **estimé** ». STATISTICS XML génère des informations de plan d’exécution de requête à l’exécution pour une requête, et exécute cette dernière. On emploie également le terme de « plan d’exécution **réel** ».  
  
3.  Exécuter une requête. Exemple :  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    SET SHOWPLAN_XML ON;  
    GO  
    -- Execute a query.  
    SELECT BusinessEntityID   
    FROM HumanResources.Employee  
    WHERE NationalIDNumber = '509647174';  
    GO  
    SET SHOWPLAN_XML OFF;  
    ```  
  
4.  Dans le volet **Résultats** , cliquez avec le bouton droit sur le **Plan d’exécution XML Microsoft SQL Server** contenant le plan de requête, puis cliquez sur **Enregistrer les résultats sous**.  
  
5.  Dans la boîte de dialogue **Enregistrer** **les résultats \<de la grille ou du texte>**, dans la zone **Type de fichier**, cliquez sur **Tous les fichiers (\*.\*)**.  
  
6.  Dans la boîte de dialogue **Nom de fichier**, fournissez un nom au format \<nom **>.sqlplan**, puis cliquez sur **Enregistrer**.  
  
### <a name="to-save-an-execution-plan-by-using-sql-server-management-studio-options"></a>Pour enregistrer un plan d'exécution avec les options de SQL Server Management Studio  
  
1.  Générez un plan d'exécution soit estimé soit réel au moyen de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour plus d’informations, consultez [Afficher le plan d’exécution estimé](../../relational-databases/performance/display-the-estimated-execution-plan.md) et [Afficher un plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md).  
  
2.  Sous l’onglet **Plan d’exécution** du volet de résultats, cliquez avec le bouton droit sur le plan d’exécution graphique, puis choisissez **Enregistrer le plan d’exécution en tant que**.  
  
     Vous pouvez aussi choisir **Enregistrer le plan d’exécution en tant que** dans le menu **Fichier**.  
  
3.  Dans la boîte de dialogue **Enregistrer sous**, assurez-vous que **Type de fichier** est défini à **Fichiers de plan d’exécution (\*.sqlplan)**.  
  
4.  Dans la boîte de dialogue **Nom de fichier**, fournissez un nom au format \<nom **>.sqlplan**, puis cliquez sur **Enregistrer**.  
  
### <a name="to-open-a-saved-xml-query-plan-in-sql-server-management-studio"></a>Pour ouvrir un plan de requête XML dans SQL Server Management Studio  
  
1.  Dans le menu **Fichier** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], choisissez **Ouvrir**, puis cliquez sur **Fichier**.  
  
2.  Dans la boîte de dialogue **Ouvrir un fichier**, définissez **Types de fichiers** à **Fichiers de plan d’exécution (\*.sqlplan)** pour produire une liste filtrée des fichiers de plan de requête XML enregistrés.  
  
3.  Sélectionnez le fichier de plan de requête XML que vous voulez consulter, puis cliquez sur **Ouvrir**.  
  
     En guise d’alternative, dans l’Explorateur Windows, double-cliquez sur un fichier avec l’extension **.sqlplan**. Le plan s'ouvre dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [SET SHOWPLAN_XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-xml-transact-sql.md)   
 [SET STATISTICS XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-xml-transact-sql.md)  
  
  
