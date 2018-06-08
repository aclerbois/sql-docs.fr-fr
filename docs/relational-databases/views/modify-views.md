---
title: Modifier des vues | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], renaming
- views [SQL Server], modifying
- modifying views
- renaming views
ms.assetid: 2d3c14dc-43e5-4324-b8fb-f2692d330b16
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e4a1df9dc02b3566315436ec8c82cdd466cf25fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-views"></a>Modifier des vues
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]
  Après avoir défini une vue, vous pouvez modifier sa définition dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] sans la supprimer ni être obligé de la recréer à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour modifier une vue, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Modifier une vue n'affecte aucun des objets qui en dépendent, tels que les procédures stockées ou les déclencheurs, à moins que la définition de la vue ne change de telle manière qu'un objet sous-jacent ne soit plus valide.  
  
-   Si la vue actuellement utilisée est modifiée en utilisant ALTER VIEW, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pose un verrou de schéma exclusif sur la vue. Lorsque le verrou est attribué (et qu'il n'y a aucun utilisateur actif de la vue), le [!INCLUDE[ssDE](../../includes/ssde-md.md)] supprime toutes les copies de la vue du cache de procédure. Les plans existants qui font référence à la vue restent dans le cache, mais ils sont recompilés lorsqu'ils sont invoqués.  
  
-   L"instruction ALTER VIEW peut être appliquée à des vues indexées, mais elle supprime de manière inconditionnelle tous les index de la vue.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Pour exécuter l'instruction ALTER VIEW, il est nécessaire de disposer au minimum de l'autorisation ALTER sur OBJECT.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-a-view"></a>Pour modifier une vue  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) en regard de la base de données dans laquelle votre vue se trouve, puis cliquez sur le signe plus (+) en regard du dossier **Vues** .  
  
2.  Cliquez avec le bouton droit sur la vue à modifier, puis sélectionnez **Conception**.  
  
3.  Dans le volet Diagramme du Concepteur de requêtes, apportez les modifications à la vue à l'aide d'une ou de plusieurs des méthodes suivantes :  
  
    1.  Activez ou désactivez les cases à cocher des éléments à ajouter ou supprimer.  
  
    2.  Cliquez avec le bouton droit dans le volet Schéma, sélectionnez **Ajouter une table…**, puis les colonnes supplémentaires à ajouter à la vue dans la boîte de dialogue **Ajouter une table** .  
  
    3.  Cliquez avec le bouton droit sur la barre de titre de la table que vous souhaitez supprimer et sélectionnez **Supprimer**.  
  
4.  Dans le menu **Fichier**, cliquez sur **Enregistrer***nom_vue*.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-modify-a-view"></a>Pour modifier une vue  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple commence par créer une vue, puis la modifie à l'aide de ALTER VIEW. Une clause WHERE est ajoutée à la définition de la vue.  
  
    ```  
    USE AdventureWorks2012 ;  
    GO  
    -- Create a view.  
    CREATE VIEW HumanResources.EmployeeHireDate  
    AS  
    SELECT p.FirstName, p.LastName, e.HireDate  
    FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
    ON e.BusinessEntityID = p.BusinessEntityID ;   
  
    -- Modify the view by adding a WHERE clause to limit the rows returned.  
    ALTER VIEW HumanResources.EmployeeHireDate  
    AS  
    SELECT p.FirstName, p.LastName, e.HireDate  
    FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
    ON e.BusinessEntityID = p.BusinessEntityID  
    WHERE HireDate < CONVERT(DATETIME,'20020101',101) ;   
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md).  
  
  
