---
title: Numéros séquentiels | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sequence-numbers
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- sequence number object, overview
- sequence [Database Engine]
- autonumbers, sequences
- sequence numbers [SQL Server]
- sequence number object
ms.assetid: c900e30d-2fd3-4d5f-98ee-7832f37e79d1
caps.latest.revision: 31
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f4c3b9bf5a98c2986e3acb1760154ba61eb97763
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708217"
---
# <a name="sequence-numbers"></a>Numéros séquentiels
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Une séquence est un objet lié par schéma défini par l'utilisateur qui génère une séquence de valeurs numériques d'après la spécification avec laquelle la séquence a été créée. La séquence de valeurs numériques est générée dans un ordre croissant ou décroissant à un intervalle défini et peut effectuer un cycle (répétition) selon la demande. Les séquences, contrairement aux colonnes d'identité, ne sont pas associées à des tables. Une application fait référence à un objet séquence pour recevoir sa valeur suivante. La relation entre les séquences et les tables est contrôlée par l'application. Les applications utilisateur peuvent référencer un objet séquence et coordonner les clés des valeurs entre plusieurs lignes et tables.  
  
 Une séquence est créée indépendamment des tables à l’aide de l’instruction **CREATE SEQUENCE** . Des options vous permettent de contrôler l'incrémentation, les valeurs maximales et minimales, le point de départ, la fonction de redémarrage automatique et la mise en cache afin d'améliorer les performances. Pour plus d’informations sur ces options, consultez [CREATE SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md).  
  
 Au lieu d’utiliser des valeurs de colonne d'identité, qui sont générées lors de l’insertion de lignes, une application peut obtenir le numéro séquentiel suivant avant d’insérer une ligne en appelant la fonction [NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md) . Le numéro séquentiel est alloué lors de l'appel de NEXT VALUE FOR, même si ce nombre n'est jamais inséré dans une table. La fonction NEXT VALUE FOR peut être utilisée comme valeur par défaut pour une colonne dans une définition de table. Utilisez [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) pour obtenir une plage de plusieurs numéros séquentiels à la fois.  
  
 Une séquence peut être définie comme n'importe quel type de données integer. Si le type de données n’est pas spécifié, une séquence prend la valeur par défaut **bigint**.  
  
## <a name="using-sequences"></a>Utilisation de séquences  
 Utilisez les séquences à la place des colonnes d'identité dans les scénarios suivants :  
  
-   L'application a besoin d'un nombre avant d'effectuer l'insertion dans la table.  
  
-   L'application requiert le partage d'une série de nombres unique entre plusieurs tables ou plusieurs colonnes dans une table.  
  
-   L'application doit redémarrer la série de nombres lorsqu'un nombre spécifié est atteint. Par exemple, après avoir affecté les valeurs 1 à 10, l'application recommence à affecter les valeurs 1 à 10.  
  
-   L'application requiert le tri des valeurs de séquence par un autre champ. La fonction NEXT VALUE FOR peut appliquer la clause OVER à l'appel de fonction. La clause OVER garantit que les valeurs retournées sont générées dans l'ordre de la clause ORDER BY de la clause OVER.  
  
-   Une application requiert que plusieurs nombres soient affectés en même temps. Par exemple, une application doit réserver cinq numéros séquentiels. La demande de valeurs d'identité peut provoquer des intervalles dans la série si des nombres ont été émis pour d'autres processus simultanément. L’appel de sp_sequence_get_range permet d’extraire plusieurs nombres à la fois dans la séquence.  
  
-   Vous devez modifier la spécification de la séquence, telle que la valeur de l'incrément.  
  
## <a name="limitations"></a>Limitations  
 Contrairement aux colonnes d'identité, dont les valeurs ne peuvent pas être modifiées, les valeurs de séquence ne sont pas protégées automatiquement après leur insertion dans la table. Pour empêcher toute modification des valeurs de séquence, utilisez un déclencheur de mise à jour sur la table pour annuler les modifications.  
  
 L'unicité ne s'applique pas automatiquement aux valeurs de séquence. La possibilité de réutiliser des valeurs de séquence est un processus normal. Si les valeurs de séquence dans une table doivent obligatoirement être uniques, créez un index unique sur la colonne. Si les valeurs de séquence d'une table doivent obligatoirement être uniques dans l'ensemble d'un groupe de tables, créez des déclencheurs pour empêcher les doublons provoqués par les instructions UPDATE ou les cycles de numéros séquentiels.  
  
 L'objet séquence génère des nombres en fonction de sa définition, mais l'objet séquence ne contrôle pas la façon dont les nombres sont utilisés. Les numéros séquentiels insérés dans une table peuvent présenter des intervalles en cas d'annulation d'une transaction, de partage d'un objet séquence par plusieurs tables ou lorsque des numéros séquentiels sont alloués sans les utiliser dans les tables. En cas de création avec l'option CACHE, un arrêt inattendu, tel qu'une panne de courant, peut conduire à la perte des numéros séquentiels dans le cache.  
  
 Si plusieurs instances de la fonction **NEXT VALUE FOR** spécifient le même générateur de séquence dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] unique, toutes ces instances retournent la même valeur pour une ligne donnée traitée par cette instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ce comportement est cohérent avec la norme ANSI.  
  
## <a name="typical-use"></a>Utilisation courante  
 Pour créer un numéro séquentiel entier qui incrémente par 1 de -2 147 483 648 à 2 147 483 647, utilisez l'instruction suivante.  
  
```  
CREATE SEQUENCE Schema.SequenceName  
    AS int  
    INCREMENT BY 1 ;  
```  
  
 Pour créer un numéro séquentiel entier similaire à une colonne d'identité qui incrémente par 1 de 1 à -2 147 483 648 à 2 147 483 647, utilisez l'instruction suivante.  
  
```  
CREATE SEQUENCE Schema.SequenceName  
    AS int  
    START WITH 1  
    INCREMENT BY 1 ;  
  
```  
  
## <a name="managing-sequences"></a>Gestion de séquences  
 Pour plus d’informations sur les séquences, interrogez [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 D’autres exemples sont fournis dans les rubriques [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md), [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md) et [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md).  
  
### <a name="a-using-a-sequence-number-in-a-single-table"></a>A. Utilisation d'un numéro séquentiel dans une table individuelle  
 L’exemple suivant crée un schéma nommé Test, une table nommée Orders et une séquence nommée CountBy1, puis insère des lignes dans la table à l’aide de la fonction NEXT VALUE FOR.  
  
```  
--Create the Test schema  
CREATE SCHEMA Test ;  
GO  
  
-- Create a table  
CREATE TABLE Test.Orders  
    (OrderID int PRIMARY KEY,  
    Name varchar(20) NOT NULL,  
    Qty int NOT NULL);  
GO  
  
-- Create a sequence  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
-- Insert three records  
INSERT Test.Orders (OrderID, Name, Qty)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Tire', 2) ;  
INSERT test.Orders (OrderID, Name, Qty)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Seat', 1) ;  
INSERT test.Orders (OrderID, Name, Qty)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Brake', 1) ;  
GO  
  
-- View the table  
SELECT * FROM Test.Orders ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `OrderID  Name    Qty`  
  
 `1        Tire    2`  
  
 `2        Seat    1`  
  
 `3        Brake   1`  
  
### <a name="b-calling-next-value-for-before-inserting-a-row"></a>B. Appel de la fonction NEXT VALUE FOR avant d'insérer une ligne  
 À l'aide de la table `Orders` créée dans l'exemple A, l'exemple suivant déclare une variable nommée `@nextID`, puis utilise la fonction NEXT VALUE FOR pour affecter à la variable le numéro séquentiel disponible suivant. L'application est censée effectuer un traitement de la commande, tel que fournir au client un numéro `OrderID` de commande potentielle, puis valider la commande. Quelle que soit la durée du traitement ou quel que soit le nombre de commandes supplémentaires ajoutées au cours du processus, le numéro d'origine est conservé pour être utilisé par cette connexion. Enfin, l'instruction `INSERT` ajoute la commande à la table `Orders` .  
  
```  
DECLARE @NextID int ;  
SET @NextID = NEXT VALUE FOR Test.CountBy1;  
-- Some work happens  
INSERT Test.Orders (OrderID, Name, Qty)  
    VALUES (@NextID, 'Rim', 2) ;  
GO  
  
```  
  
### <a name="c-using-a-sequence-number-in-multiple-tables"></a>C. Utilisation d'un numéro séquentiel dans plusieurs tables  
 Cet exemple suppose qu'un processus d'analyse de chaîne de fabrication reçoit une notification des événements qui se produisent dans tout l'atelier. Chaque événement reçoit un numéro unique `EventID` , à croissance monolithique. Tous les événements utilisent le même numéro séquentiel `EventID` afin que les rapports qui combinent tous les événements puissent identifier chaque événement de façon unique. Cependant les données d'événement sont stockées dans trois tables différentes, selon le type d'événement. L’exemple de code crée un schéma nommé `Audit`, une séquence nommée `EventCounter`et trois tables qui utilisent chacune la séquence `EventCounter` comme valeur par défaut. Puis l'exemple ajoute des lignes aux trois tables et interroge les résultats.  
  
```  
CREATE SCHEMA Audit ;  
GO  
CREATE SEQUENCE Audit.EventCounter  
    AS int  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
CREATE TABLE Audit.ProcessEvents  
(  
    EventID int PRIMARY KEY CLUSTERED   
        DEFAULT (NEXT VALUE FOR Audit.EventCounter),  
    EventTime datetime NOT NULL DEFAULT (getdate()),  
    EventCode nvarchar(5) NOT NULL,  
    Description nvarchar(300) NULL  
) ;  
GO  
  
CREATE TABLE Audit.ErrorEvents  
(  
    EventID int PRIMARY KEY CLUSTERED  
        DEFAULT (NEXT VALUE FOR Audit.EventCounter),  
    EventTime datetime NOT NULL DEFAULT (getdate()),  
    EquipmentID int NULL,  
    ErrorNumber int NOT NULL,  
    EventDesc nvarchar(256) NULL  
) ;  
GO  
  
CREATE TABLE Audit.StartStopEvents  
(  
    EventID int PRIMARY KEY CLUSTERED  
        DEFAULT (NEXT VALUE FOR Audit.EventCounter),  
    EventTime datetime NOT NULL DEFAULT (getdate()),  
    EquipmentID int NOT NULL,  
    StartOrStop bit NOT NULL  
) ;  
GO  
  
INSERT Audit.StartStopEvents (EquipmentID, StartOrStop)   
    VALUES (248, 0) ;  
INSERT Audit.StartStopEvents (EquipmentID, StartOrStop)   
    VALUES (72, 0) ;  
INSERT Audit.ProcessEvents (EventCode, Description)   
    VALUES (2735,   
    'Clean room temperature 18 degrees C.') ;  
INSERT Audit.ProcessEvents (EventCode, Description)   
    VALUES (18, 'Spin rate threashold exceeded.') ;  
INSERT Audit.ErrorEvents (EquipmentID, ErrorNumber, EventDesc)   
    VALUES (248, 82, 'Feeder jam') ;  
INSERT Audit.StartStopEvents (EquipmentID, StartOrStop)   
    VALUES (248, 1) ;  
INSERT Audit.ProcessEvents (EventCode, Description)   
    VALUES (1841, 'Central feed in bypass mode.') ;  
-- The following statement combines all events, though not all fields.  
SELECT EventID, EventTime, Description FROM Audit.ProcessEvents   
UNION SELECT EventID, EventTime, EventDesc FROM Audit.ErrorEvents   
UNION SELECT EventID, EventTime,   
CASE StartOrStop   
    WHEN 0 THEN 'Start'   
    ELSE 'Stop'  
END   
FROM Audit.StartStopEvents  
ORDER BY EventID ;  
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `EventID  EventTime                Description`  
  
 `1        2009-11-02 15:00:51.157  Start`  
  
 `2        2009-11-02 15:00:51.160  Start`  
  
 `3        2009-11-02 15:00:51.167  Clean room temperature 18 degrees C.`  
  
 `4        2009-11-02 15:00:51.167  Spin rate threshold exceeded.`  
  
 `5        2009-11-02 15:00:51.173  Feeder jam`  
  
 `6        2009-11-02 15:00:51.177  Stop`  
  
 `7        2009-11-02 15:00:51.180  Central feed in bypass mode.`  
  
### <a name="d-generating-repeating-sequence-numbers-in-a-result-set"></a>D. Génération de numéros séquentiels à répétition dans un jeu de résultats  
 L'exemple suivant montre deux fonctionnalités des numéros séquentiels : les cycles et l'utilisation de la fonction `NEXT VALUE FOR` dans une instruction select.  
  
```  
CREATE SEQUENCE CountBy5  
   AS tinyint  
    START WITH 1  
    INCREMENT BY 1  
    MINVALUE 1  
    MAXVALUE 5  
    CYCLE ;  
GO  
  
SELECT NEXT VALUE FOR CountBy5 AS SurveyGroup, Name FROM sys.objects ;  
GO  
```  
  
### <a name="e-generating-sequence-numbers-for-a-result-set-by-using-the-over-clause"></a>E. Génération de numéros séquentiels pour un jeu de résultats à l'aide de la clause OVER  
 L'exemple suivant utilise la clause `OVER` pour trier le jeu de résultats par `Name` avant que la colonne de numéros séquentiels soit ajoutée.  
  
```  
USE AdventureWorks2012 ;  
GO  
  
CREATE SCHEMA Samples ;  
GO  
  
CREATE SEQUENCE Samples.IDLabel  
    AS tinyint  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
SELECT NEXT VALUE FOR Samples.IDLabel OVER (ORDER BY Name) AS NutID, ProductID, Name, ProductNumber FROM Production.Product  
WHERE Name LIKE '%nut%' ;  
```  
  
### <a name="f-resetting-the-sequence-number"></a>F. Réinitialisation de numéros séquentiels  
 L'exemple E a utilisé les 79 premiers numéros séquentiels `Samples.IDLabel` (votre version d'`AdventureWorks2012` peut retourner un nombre différent de résultats). Exécutez le code suivant pour utiliser les 79 numéros séquentiels suivants (de 80 à 158).  
  
```  
SELECT NEXT VALUE FOR Samples.IDLabel OVER (ORDER BY Name) AS NutID, ProductID, Name, ProductNumber FROM Production.Product  
WHERE Name LIKE '%nut%' ;  
```  
  
 Exécutez l’instruction suivante pour redémarrer la séquence `Samples.IDLabel`.  
  
```  
ALTER SEQUENCE Samples.IDLabel  
RESTART WITH 1 ;  
```  
  
 Réexécutez l’instruction select pour vérifier que la séquence `Samples.IDLabel` a redémarré à partir du nombre 1.  
  
```  
SELECT NEXT VALUE FOR Samples.IDLabel OVER (ORDER BY Name) AS NutID, ProductID, Name, ProductNumber FROM Production.Product  
WHERE Name LIKE '%nut%' ;  
```  
  
### <a name="g-changing-a-table-from-identity-to-sequence"></a>G. Modification d'une table d'identité en séquence  
 L'exemple suivant crée un schéma et une table contenant trois lignes. L'exemple ajoute ensuite une nouvelle colonne et supprime l'ancienne.  
  
```  
-- Create a schema  
CREATE SCHEMA Test ;  
GO  
  
-- Create a table  
CREATE TABLE Test.Department  
    (  
        DepartmentID smallint IDENTITY(1,1) NOT NULL,  
        Name nvarchar(100) NOT NULL,  
        GroupName nvarchar(100) NOT NULL  
    CONSTRAINT PK_Department_DepartmentID PRIMARY KEY CLUSTERED   
         (DepartmentID ASC)   
    ) ;  
GO  
  
-- Insert three rows into the table  
INSERT Test.Department(Name, GroupName)  
    VALUES ('Engineering', 'Research and Development');  
GO  
  
INSERT Test.Department(Name, GroupName)  
    VALUES ('Tool Design', 'Research and Development');  
GO  
  
INSERT Test.Department(Name, GroupName)  
    VALUES ('Sales', 'Sales and Marketing');  
GO  
  
-- View the table that will be changed  
SELECT * FROM Test.Department ;  
GO  
  
-- End of portion creating a sample table  
--------------------------------------------------------  
-- Add the new column that does not have the IDENTITY property  
ALTER TABLE Test.Department   
    ADD DepartmentIDNew smallint NULL  
GO  
  
-- Copy values from the old column to the new column  
UPDATE Test.Department  
    SET DepartmentIDNew = DepartmentID ;  
GO  
  
-- Drop the primary key constraint on the old column  
ALTER TABLE Test.Department  
    DROP CONSTRAINT [PK_Department_DepartmentID];  
-- Drop the old column  
ALTER TABLE Test.Department  
    DROP COLUMN DepartmentID ;  
GO  
  
-- Rename the new column to the old columns name  
EXEC sp_rename 'Test.Department.DepartmentIDNew',   
    'DepartmentID', 'COLUMN';  
GO  
  
-- Change the new column to NOT NULL  
ALTER TABLE Test.Department  
    ALTER COLUMN DepartmentID smallint NOT NULL ;  
-- Add the unique primary key constraint  
ALTER TABLE Test.Department  
    ADD CONSTRAINT PK_Department_DepartmentID PRIMARY KEY CLUSTERED   
         (DepartmentID ASC) ;  
-- Get the highest current value from the DepartmentID column   
-- and create a sequence to use with the column. (Returns 3.)  
SELECT MAX(DepartmentID) FROM Test.Department ;  
-- Use the next desired value (4) as the START WITH VALUE;  
CREATE SEQUENCE Test.DeptSeq  
    AS smallint  
    START WITH 4  
    INCREMENT BY 1 ;  
GO  
  
-- Add a default value for the DepartmentID column  
ALTER TABLE Test.Department  
    ADD CONSTRAINT DefSequence DEFAULT (NEXT VALUE FOR Test.DeptSeq)   
        FOR DepartmentID;  
GO  
  
-- View the result  
SELECT DepartmentID, Name, GroupName  
FROM Test.Department ;   
-- Test insert  
INSERT Test.Department (Name, GroupName)  
    VALUES ('Audit', 'Quality Assurance') ;  
GO  
  
-- View the result  
SELECT DepartmentID, Name, GroupName  
FROM Test.Department ;  
GO  
  
```  
  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui utilisent `SELECT *` recevront la nouvelle colonne en tant que dernière colonne au lieu de première. Si ce n'est pas acceptable, vous devez créer une toute nouvelle table, y déplacez les données, puis y recréer les autorisations.  
  
## <a name="related-content"></a>Contenu associé  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)  
  
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)  
  
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)  
  
 [IDENTITY &#40;propriété&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
  
  
