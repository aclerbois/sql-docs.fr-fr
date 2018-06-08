---
title: Créer des relations de clé étrangère | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], creating
ms.assetid: 867a54b8-5be4-46e6-9702-49ae6dabf67c
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 67760c658b27bcddff443a943c65d725e23f44e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-foreign-key-relationships"></a>Créer des relations de clé étrangère
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Créer des relations de clé étrangère](https://msdn.microsoft.com/en-US/library/ms189049(SQL.120).aspx).


  Cette rubrique explique comment créer des relations de clé étrangère dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous créez une relation entre deux tables lorsque vous voulez associer des lignes d'une table à des lignes appartenant à une autre table.    
     
##  <a name="BeforeYouBegin"></a> Avant de commencer ! Limites et restrictions
 
-   Une contrainte de clé étrangère ne doit pas forcément être liée seulement à une contrainte de clé primaire d'une autre table. Elle peut également être définie pour référencer les colonnes d'une contrainte UNIQUE d'une autre table.    
    
-   Lorsqu'une valeur différente de NULL est entrée dans la colonne d'une contrainte FOREIGN KEY, la valeur doit exister dans la colonne référencée. Dans le cas contraire, le système retourne un message d'erreur signalant une violation de clé étrangère. Pour vous assurer que toutes les valeurs d'une contrainte de clé étrangère composite sont vérifiées, spécifiez NOT NULL pour toutes les colonnes participant à la contrainte.    
    
-   Les contraintes FOREIGN KEY ne peuvent faire référence qu'à des tables au sein de la même base de données sur le même serveur. L'intégrité référentielle inter-base de données doit être implémentée via les déclencheurs. Pour plus d’informations, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).    
    
-   Les contraintes FOREIGN KEY peuvent faire référence à une autre colonne dans la même table. On appelle habituellement ce mécanisme « auto-référence ».    
    
-   Une contrainte FOREIGN KEY spécifiée au niveau de la colonne ne peut lister qu'une colonne de référence. Cette colonne doit avoir le même type de données que la colonne pour laquelle la contrainte est définie.    
    
-   Une contrainte FOREIGN KEY spécifiée au niveau de la table doit avoir le même nombre de colonnes de référence que le nombre de colonnes de la liste des colonnes de la contrainte. Le type de données de chaque colonne de référence doit également être identique à la colonne de référence correspondante dans la liste des colonnes.    
    
-   Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'a pas de limite prédéfinie du nombre de contraintes FOREIGN KEY qu'une table peut contenir et qui référencent d'autres tables ou du nombre de contraintes FOREIGN KEY possédées par d'autres tables qui font référence à une table spécifique. Cependant, le nombre réel de contraintes FOREIGN KEY qui peuvent être utilisées est limité par la configuration matérielle et par la conception de la base de données et de l'application.  Une table peut référencer au maximum 253 autres tables et colonnes en tant que clés étrangères (références sortantes). [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] fait passer de 253 à 10 000 le nombre limite des autres tables et colonnes pouvant référencer des colonnes dans une table unique (références entrantes).  (Cela nécessite au minimum le niveau de compatibilité 130). Cette augmentation est soumise aux restrictions suivantes :    
    
    -   Les références de clés étrangères supérieures à 253 sont prises en charge pour les opérations DELETE et UPDATE DML. Les opérations MERGE ne sont pas prises en charge.    
    
    -   Une table comportant une clé étrangère référencée vers elle-même est toujours limitée à 253 références de clés étrangères.    
    
    -   Les références de clés étrangères supérieures à 253 ne sont actuellement disponibles ni pour les index columnstore, ni pour les tables optimisées en mémoire, ni pour Stretch Database.    
    
-   Les contraintes FOREIGN KEY ne sont pas appliquées dans les tables temporaires.    
    
-   Si une clé étrangère est définie sur une colonne avec le type de données CLR défini par l'utilisateur, l'implémentation du type doit prendre en charge le tri binaire. Pour plus d’informations, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).    
    
-   Une colonne de type **varchar(max)** ne peut participer à une contrainte FOREIGN KEY que si la clé primaire qu’elle référence est également définie comme étant de type **varchar(max)**.    
    

    
##   <a name="permissions"></a>Autorisations    
 La création d'une nouvelle table avec une clé étrangère nécessite une autorisation CREATE TABLE dans la base de données et une autorisation ALTER pour le schéma dans lequel la table a été créée.    
    
 La création d'une clé étrangère dans une table existante nécessite l'autorisation ALTER sur la table.    
       
    
## <a name="create-a-foreign-key-relationship-in-table-designer"></a>Créer une relation de clé étrangère dans le Concepteur de tables 
####  <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio    
    
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table qui se trouve du côté clé étrangère de la relation et cliquez sur **Conception**.    
    
     La table s'ouvre dans le **Concepteur de tables**.    
    
2.  Dans le menu **Concepteur de tables** , cliquez sur **Relations**.    
    
3.  Dans la boîte de dialogue **Relations de clé étrangère** , cliquez sur **Ajouter**.    
    
     La relation s’affiche dans la liste **Relation sélectionnée** avec un nom fourni par le système au format FK_\<*nom_table*>_\<*nom_table*>, où *nom_table* est le nom de la table de clé étrangère.    
    
4.  Cliquez sur la relation dans la liste **Relation sélectionnée** .    
    
5.  Cliquez sur **Spécification de tables et colonnes** dans la grille affichée à droite et cliquez sur le bouton de sélection (**…**), à droite de la propriété.    
    
6.  Dans la liste déroulante **Clé primaire** de la boîte de dialogue **Tables et colonnes** , choisissez la table qui sera du côté clé primaire de la relation.    
    
7.  Dans la grille située au-dessous, choisissez les colonnes qui participent à la clé primaire de la table. Dans la cellule de la grille située à gauche de chaque colonne, choisissez la colonne clé étrangère correspondante dans la table de clé étrangère.    
    
     Le**Concepteur de tables** propose un nom pour la relation. Pour changer ce nom, modifiez le contenu de la zone de texte **Nom de la relation** .    
    
8.  Choisissez **OK** pour créer la relation.    
       
## <a name="create-a-foreign-key-in-a-new-table"></a>Créer une clé étrangère dans une nouvelle table  
####  <a name="using-transact-sql"></a>Utilisation de Transact-SQL   
    
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].    
    
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.    
    
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple crée une table et définit une contrainte de clé étrangère sur la colonne `TempID` qui fait référence à la colonne `SalesReasonID` dans la table `Sales.SalesReason` . Les clauses ON DELETE CASCADE et ON UPDATE CASCADE sont utilisées pour garantir que les modifications apportées à la table `Sales.SalesReason` sont automatiquement propagées dans la table `Sales.TempSalesReason` .    
    
    ```    
    USE AdventureWorks2012;    
    GO    
    CREATE TABLE Sales.TempSalesReason (TempID int NOT NULL, Name nvarchar(50),     
    CONSTRAINT PK_TempSales PRIMARY KEY NONCLUSTERED (TempID),     
    CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)     
        REFERENCES Sales.SalesReason (SalesReasonID)     
        ON DELETE CASCADE    
        ON UPDATE CASCADE    
    );GO    
    
    ```    
    
## <a name="create-a-foreign-key-in-an-existing-table"></a>Créer une clé étrangère dans une table existante 
#### <a name="using-transasct-sql"></a>Utilisation de Transact-SQL   
    
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].    
    
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.    
    
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple crée une clé étrangère sur la colonne `TempID` et référence la colonne `SalesReasonID` dans la table `Sales.SalesReason`.    
    
    ```    
    USE AdventureWorks2012;    
    GO    
    ALTER TABLE Sales.TempSalesReason     
    ADD CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)     
        REFERENCES Sales.SalesReason (SalesReasonID)     
        ON DELETE CASCADE    
        ON UPDATE CASCADE    
    ;    
    GO    
    
    ```    
    
     Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md), [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) et [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md).    
    
  
