---
title: Modification des données dans une table temporelle avec système par version | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 5f398470-c531-47b5-84d5-7c67c27df6e5
caps.latest.revision: 8
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: af9dd51130cd21ff188af473d66f78adfc54be8c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modifying-data-in-a-system-versioned-temporal-table"></a>Modification des données dans une table temporelle avec système par version
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Les données d'une table temporelle avec système par version sont modifiées à l'aide d’instructions DML régulières avec une différence importante : les données de la colonne de période ne peuvent pas être directement modifiées. Lorsque des données sont mises à jour, des versions sont générées, et la version précédente de chaque ligne mise à jour est insérée dans la table d'historique. Lorsque des données sont supprimées, la suppression est logique : la ligne est déplacée dans la table d'historique à partir de la table actuelle et n'est pas définitivement supprimée.  
  
## <a name="inserting-data"></a>Insertion de données  
 Lorsque vous insérez de nouvelles données, vous devez prendre en compte les colonnes **PERIOD** si elles ne sont pas **HIDDEN**. Vous pouvez également utiliser un basculement de partition avec des tables temporelles avec système par version.  
  
### <a name="insert-new-data-with-visible-period-columns"></a>Insérer de nouvelles données avec des colonnes de période visibles  
 Vous pouvez construire votre instruction **INSERT** si vous utilisez des colonnes **PERIOD** visibles afin de prendre en compte les nouvelles colonnes **PERIOD** :  
  
-   Si vous spécifiez la liste des colonnes dans votre instruction **INSERT** , vous pouvez omettre les colonnes **PERIOD** car le système générera automatiquement des valeurs pour ces colonnes.  
  
    ```  
    --Insert with column list and without period columns   
    INSERT INTO [dbo].[Department] ([DeptID] ,[DeptName] ,[ManagerID] ,[ParentDeptID])   
    VALUES(10, 'Marketing', 101, 1);  
  
    ```  
  
-   Si vous ne spécifiez pas les colonnes**PERIOD** pour la liste des colonnes dans votre instruction **INSERT** , vous devez leur attribuer la valeur **DEFAULT** .  
  
    ```  
    INSERT INTO [dbo].[Department] ([DeptID] ,[DeptName] ,[ManagerID] ,[ParentDeptID], SysStartTime, SysEndTime)   
    VALUES(11, 'Sales', 101, 1, default, default);  
  
    ```  
  
-   Si vous ne spécifiez pas la liste des colonnes dans votre instruction **INSERT** , spécifiez **DEFAULT** pour les colonnes **PERIOD** .  
  
    ```  
    --Insert without  column list and DEFAULT values for period columns   
    INSERT INTO [dbo].[Department]    
    VALUES(12, 'Production', 101, 1, default, default);  
  
    ```  
  
### <a name="insert-data-into-a-table-with-hidden-period-columns"></a>Insérer des données dans une table avec des colonnes de période HIDDEN (masquées)  
 Si des colonnes **PERIOD** sont spécifiées comme étant HIDDEN (masquées), il vous suffit de spécifier les valeurs des colonnes visibles lorsque vous utiliser l’instruction INSERT sans spécifier la liste des colonnes. Vous n'avez pas besoin de tenir compte des nouvelles colonnes **PERIOD** dans votre instruction **INSERT** . Ce comportement garantit que vos applications héritées continuent de fonctionner lorsque vous activez le contrôle de version système sur des tables qui bénéficieront de ce contrôle.  
  
```  
CREATE TABLE [dbo].[CompanyLocation]  
(   
     [LocID] [int] IDENTITY(1,1) NOT NULL PRIMARY KEY  
   , [LocName] [varchar](50) NOT NULL  
   , [City] [varchar](50) NOT NULL  
   , [SysStartTime] [datetime2](0) GENERATED ALWAYS AS ROW START HIDDEN NOT NULL   
   , [SysEndTime] [datetime2](0) GENERATED ALWAYS AS ROW END HIDDEN NOT NULL   
PERIOD FOR SYSTEM_TIME ([SysStartTime], [SysEndTime])   
)    
WITH ( SYSTEM_VERSIONING = ON );   
GO   
INSERT INTO [dbo].[CompanyLocation]   
VALUES  ('Headquarters', 'New York');  
  
```  
  
### <a name="inserting-data-using-partition-switch"></a>Insertion de données à l'aide du COMMUTATEUR DE PARTITION  
 Si la table actuelle est partitionnée, vous pouvez utiliser le basculement de partition comme un mécanisme efficace pour charger les données dans une partition vide ou dans plusieurs partitions en parallèle.   
La table intermédiaire utilisée dans l’instruction **PARTITION SWITCH IN** avec une table temporelle avec système par version doit avoir une valeur **SYSTEM_TIME PERIOD** définie, mais elle ne doit pas être une table temporelle avec système par version.    
Cela garantit que des vérifications de cohérence temporelle sont effectuées lorsque des données sont insérées dans une table intermédiaire ou qu’une période SYSTEM_TIME est ajoutée à une table intermédiaire préremplie.  
  
```  
/*Create staging table with period definition for SWITCH IN temporal table*/   
CREATE TABLE [dbo].[Staging_Department_Partition2]  
(   
     [DeptID] [int] NOT NULL  
   , [DeptName] [varchar](50)  NOT NULL  
   , [ManagerID] [int] NULL  
   , [ParentDeptID] [int] NULL  
   , [SysStartTime] [datetime2](7) GENERATED ALWAYS AS ROW START NOT NULL  
   , [SysEndTime] [datetime2](7) GENERATED ALWAYS AS ROW END NOT NULL  
   , PERIOD FOR SYSTEM_TIME ( [SysStartTime], [SysEndTime] )   
) ON [PRIMARY]   
  
/*Create aligned primary key*/   
ALTER TABLE [dbo].[Staging_Department_Partition2]    
ADD CONSTRAINT [Staging_Department_Partition2_PK]  
   PRIMARY KEY CLUSTERED  
   (  [DeptID] ASC )     
   ON [PRIMARY]   
  
/*   
Create and enforce constraints for partition boundaries.   
Partition 2 contains rows with DeptID > 100 and DeptID <=200   
*/   
ALTER TABLE [dbo].[Staging_Department_Partition2]      
   WITH CHECK ADD  CONSTRAINT [chk_staging_Department_partition_2]     
   CHECK  ([DeptID]>N'100' AND [DeptID]<=N'200')   
ALTER TABLE [dbo].[Staging_Department_Partition2]    
   CHECK CONSTRAINT [chk_staging_Department_partition_2]   
  
/*Load data into staging table*/   
INSERT INTO [dbo].[staging_Department] ([DeptID],[DeptName],[ManagerID],[ParentDeptID])   
VALUES (101,'D101',1,NULL)  
  
/*Use PARTITION SWITCH IN to efficiently add data to current table */    
ALTER TABLE [Staging_Department]    
SWITCH TO [dbo].[Department] PARTITION 2;  
  
```  
  
 Si vous essayez d'effectuer un BASCULEMENT DE PARTITION à partir d'une table sans définition de période, vous obtiendrez le message d'erreur : `Msg 13577, Level 16, State 1, Line 25    ALTER TABLE SWITCH statement failed on table 'MyDB.dbo.Staging_Department_2015_09_26' because target table has SYSTEM_TIME PERIOD while source table does not have it.`  
  
## <a name="updating-data"></a>mise à jour des données  
 Vous mettez à jour les données de la table actuelle avec une instruction **UPDATE** normale. Vous pouvez mettre à jour les données de la table actuelle à partir de la table d'historique pour le scénario « Désolé ». Toutefois, vous ne pouvez pas mettre à jour les colonnes **PERIOD** et que vous ne pouvez pas directement mettre à jour les données de la table d’historique si **SYSTEM_VERSIONING = ON**.   
Définissez **SYSTEM_VERSIONING = OFF** et mettez à jour les lignes de la table actuelle et de la table d’historique, mais n’oubliez pas qu’avec cette procédure, le système ne conserve pas l’historique des modifications.  
  
### <a name="updating-the-current-table"></a>Mise à jour de la table actuelle  
 Dans cet exemple, la colonne ManagerID est mise à jour pour chaque ligne où DeptID = 10. Les colonnes **PERIOD** ne sont référencées d’aucune façon.  
  
```  
UPDATE [dbo].[Department] SET [ManagerID] = 501 WHERE [DeptID] = 10  
```  
  
 Toutefois, vous ne pouvez pas mettre à jour une colonne **PERIOD** et vous ne pouvez pas mettre à jour la table d'historique.  Dans cet exemple, une tentative de mise à jour d’une colonne **PERIOD** génère une erreur.  
  
```  
UPDATE [dbo].[Department]    
SET SysStartTime = '2015-09-23 23:48:31.2990175'    
WHERE DeptID = 10 ;  
  
Msg 13537, Level 16, State 1, Line 3   
Cannot update GENERATED ALWAYS columns in table 'TmpDev.dbo.Department'.  
  
```  
  
### <a name="updating-the-current-table-from-the-history-table"></a>Mise à jour de la table actuelle à partir de la table d'historique  
 Vous pouvez utiliser **UPDATE** sur la table actuelle pour rétablir l’état réel de la ligne à un état valide à point précis dans le passé (retour à la « dernière bonne version de ligne connue »). L'exemple suivant montre un retour des valeurs dans la table d'historique en date du 2015-04-25 où DeptID = 10.  
  
```  
UPDATE Department   
SET DeptName = History.DeptName   
FROM Department    
FOR SYSTEM_TIME AS OF '2015-04-25' AS History   
WHERE History.DeptID  = 10   
AND Department.DeptID = 10 ;  
  
```  
  
## <a name="deleting-data"></a>Effacement de données  
 Vous supprimez les données de la table actuelle avec une instruction **DELETE** normale. La colonne de période de fin des lignes supprimées contiendra l'heure de début de la transaction sous-jacente.   
Vous ne pouvez pas directement supprimer des lignes d’une table d’historique si **SYSTEM_VERSIONING = ON**.   
Définissez **SYSTEM_VERSIONING = OFF** et supprimez les lignes de la table actuelle et de la table d’historique, mais n’oubliez pas qu’avec cette procédure, le système ne conserve pas l’historique des modifications.   
**TRUNCATE**, **SWITCH PARTITION OUT** de la table en cours et **SWITCH PARTITION IN** de la table d’historique ne sont pas pris en charge si **SYSTEM_VERSIONING = ON**.  
  
## <a name="using-merge-to-modify-data-in-temporal-table"></a>Utilisation de MERGE pour modifier les données d’une table temporelle  
 L’opération**MERGE** est prise en charge avec les mêmes limitations que celles que les instructions **INSERT** et **UPDATE** ont concernant les colonnes **PERIOD** .  
  
```  
CREATE TABLE DepartmentStaging (DeptId INT, DeptName varchar(50));   
GO   
INSERT INTO DepartmentStaging VALUES (1, 'Company Management');   
INSERT INTO DepartmentStaging VALUES (10, 'Science & Research');   
INSERT INTO DepartmentStaging VALUES (15, 'Process Management');   
  
MERGE dbo.Department AS target   
USING (SELECT DeptId, DeptName FROM DepartmentStaging) AS source (DeptId, DeptName)   
ON (target.DeptId = source.DeptId)   
WHEN MATCHED THEN    
    UPDATE   
   SET DeptName = source.DeptName   
WHEN NOT MATCHED THEN   
   INSERT (DeptName)   
   VALUES (source.DeptName);  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Création d’une table temporelle avec contrôle de version du système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)   
 [Interrogation des données dans une table temporelle avec système par version](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)   
 [Modification du schéma d’une table temporelle à version contrôlée par le système](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)   
 [Arrêt du contrôle de version par le système sur une table temporelle à version contrôlée par le système](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)  
  
  
