---
title: Modifier une procédure stockée | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stored-Procs
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- modifying stored procedures
- editing stored procedures
- stored procedures [SQL Server], modifying
ms.assetid: 13396239-6100-48ce-aa34-461358d99c92
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6f7bdabb6ae916b777f4df6c8965e29a0a4fca63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-a-stored-procedure"></a>Modifier une procédure stockée
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
    
##  <a name="Top"></a> Cette rubrique explique comment modifier une procédure stockée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Avant de commencer :**  [Limitations et restrictions](#Restrictions), [Sécurité](#Security)  
  
-   **Pour modifier une procédure, à l’aide de :**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] pour les transformer en procédures stockées CLR et inversement.  
  
 Si la procédure précédente a été créée avec les options WITH ENCRYPTION ou WITH RECOMPILE, ces options sont activées seulement si elles figurent dans l'instruction ALTER PROCEDURE.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l'autorisation ALTER PROCEDURE sur la procédure.  
  
##  <a name="Procedures"></a> Pour modifier une procédure stockée  
 Vous pouvez utiliser l'un des éléments suivants :  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier une procédure dans Management Studio**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, développez la base de données à laquelle appartient la procédure, puis développez **Programmabilité**.  
  
3.  Développez **Procédures stockées**, cliquez avec le bouton droit sur la procédure à modifier, puis cliquez sur **Modifier**.  
  
4.  Modifiez le texte de la procédure stockée.  
  
5.  Pour tester la syntaxe, dans le menu **Requête** , cliquez sur **Analyser**.  
  
6.  Pour enregistrer les modifications apportées à la procédure, dans le menu **Requête** , cliquez sur **Exécuter**.  
  
7.  Pour enregistrer la procédure mise à jour en tant que script [!INCLUDE[tsql](../../includes/tsql-md.md)] , dans le menu **Fichier** , cliquez sur **Enregistrer sous**. Acceptez le nom de fichier ou remplacez-le par un autre nom, puis cliquez sur **Enregistrer**.  
  
> [!IMPORTANT]  
>  Validez toutes les entrées utilisateur. Ne concaténez pas les entrées utilisateur avant de les avoir validées. N'exécutez jamais une commande élaborée à partir d'une entrée utilisateur non validée.  
  
###  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier une procédure dans l'Éditeur de requête**  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, développez la base de données à laquelle appartient la procédure. Sinon, dans la barre d'outils, sélectionnez une base de données dans la liste des bases de données disponibles. Pour cet exemple, sélectionnez la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
3.  Dans le menu **Fichier** , cliquez sur **Nouvelle requête**.  
  
4.  Copiez et collez l'exemple suivant dans l'éditeur de requête. L'exemple crée la procédure `uspVendorAllInfo` qui retourne le nom de tous les fournisseurs dans la base de données [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , les produits qu'ils vendent, leurs conditions de crédit et leur disponibilité.  
  
    ```  
  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
  
    ```  
  
5.  Dans le menu **Fichier** , cliquez sur **Nouvelle requête**.  
  
6.  Copiez et collez l'exemple suivant dans l'éditeur de requête. L'exemple modifie la procédure `uspVendorAllInfo` . La clause EXECUTE AS CALLER est supprimée et le corps de la procédure est modifié pour qu'elle retourne uniquement les fournisseurs qui proposent le produit spécifié. Les fonctions `LEFT` et `CASE` personnalisent l'affichage du jeu de résultats.  
  
    ```  
    ALTER PROCEDURE Purchasing.uspVendorAllInfo  
        @Product varchar(25)   
    AS  
        SET NOCOUNT ON;  
        SELECT LEFT(v.Name, 25) AS Vendor, LEFT(p.Name, 25) AS 'Product name',   
        'Rating' = CASE v.CreditRating   
            WHEN 1 THEN 'Superior'  
            WHEN 2 THEN 'Excellent'  
            WHEN 3 THEN 'Above average'  
            WHEN 4 THEN 'Average'  
            WHEN 5 THEN 'Below average'  
            ELSE 'No rating'  
            END  
        , Availability = CASE v.ActiveFlag  
            WHEN 1 THEN 'Yes'  
            ELSE 'No'  
            END  
        FROM Purchasing.Vendor AS v   
        INNER JOIN Purchasing.ProductVendor AS pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product AS p   
          ON pv.ProductID = p.ProductID   
        WHERE p.Name LIKE @Product  
        ORDER BY v.Name ASC;  
    GO  
  
    ```  
  
7.  Pour enregistrer les modifications apportées à la procédure, dans le menu **Requête** , cliquez sur **Exécuter**.  
  
8.  Pour enregistrer la procédure mise à jour en tant que script [!INCLUDE[tsql](../../includes/tsql-md.md)] , dans le menu **Fichier** , cliquez sur **Enregistrer sous**. Acceptez le nom de fichier ou remplacez-le par un autre nom, puis cliquez sur **Enregistrer**.  
  
9. Pour exécuter la procédure stockée modifiée, exécutez l'exemple suivant.  
  
    ```  
    EXEC Purchasing.uspVendorAllInfo N'LL Crankarm';  
    GO  
  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)  
  
  
