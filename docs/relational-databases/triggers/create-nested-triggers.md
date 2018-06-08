---
title: Créer des déclencheurs imbriqués | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: triggers
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-dml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- recursive DML triggers [SQL Server]
- DML triggers, nested
- triggers [SQL Server], nested
- direct recursion [SQL Server]
- triggers [SQL Server], recursive
- DML triggers, recursive
- RECURSIVE_TRIGGERS option
- indirect recursion [SQL Server]
- nested DML triggers
ms.assetid: cd522dda-b4ab-41b8-82b0-02445bdba7af
caps.latest.revision: 32
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: da06d8890774564e2919509a44e6130ac10f3a45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-nested-triggers"></a>Créer des déclencheurs imbriqués
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Les déclencheurs DML et DDL sont imbriqués lorsqu'un déclencheur exécute une action qui lance un autre déclencheur. Ces actions peuvent lancer d'autres déclencheurs, et ainsi de suite. Les déclencheurs DML et DDL peuvent avoir jusqu'à 32 niveaux d'imbrication. Vous pouvez définir si les déclencheurs AFTER peuvent être imbriqués à l’aide de l’option de configuration serveur **nested triggers** . Les déclencheurs INSTEAD OF (seuls les déclencheurs DML peuvent être des déclencheurs INSTEAD OF) peuvent être imbriqués quelle que soit la valeur de ce paramètre.  
  
> [!NOTE]  
>  Une référence au code managé à partir d'un déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)] compte pour un niveau quant à la limite d'imbrication de 32 niveaux. Les méthodes appelées à partir du code managé ne comptent pas par rapport à cette limite.  
  
 Si l'imbrication est permise et qu'un déclencheur de la chaîne démarre une boucle infinie, le niveau d'imbrication maximal est dépassé et le déclencheur s'arrête.  
  
 Les déclencheurs imbriqués peuvent s'avérer utiles pour effectuer certaines fonctions de maintenance, comme stocker une copie de sauvegarde des lignes concernées par un déclencheur précédent. Par exemple, vous pouvez créer un déclencheur sur `PurchaseOrderDetail` , qui enregistre une copie de sauvegarde des lignes `PurchaseOrderDetail` qui ont été supprimées par le déclencheur `delcascadetrig` . Lorsque le déclencheur `delcascadetrig` est actif, la suppression de `PurchaseOrderID` 1965 de `PurchaseOrderHeader` supprime la ou les lignes correspondantes de `PurchaseOrderDetail`. Pour enregistrer les données, vous pouvez créer un déclencheur DELETE sur `PurchaseOrderDetail` , qui enregistre les données supprimées dans une autre table `del_save`créée séparément. Exemple :  
  
```  
CREATE TRIGGER Purchasing.savedel  
   ON Purchasing.PurchaseOrderDetail  
FOR DELETE  
AS  
   INSERT del_save;  
   SELECT * FROM deleted;  
```  
  
 Il n'est pas recommandé d'utiliser des déclencheurs imbriqués dans une séquence où l'ordre a de l'importance. Employez des déclencheurs séparés pour effectuer des modifications de données en cascade.  
  
> [!NOTE]  
>  Comme les déclencheurs s'exécutent au sein d'une transaction, un échec à un quelconque niveau d'un ensemble de déclencheurs imbriqués annule la transaction tout entière, entraînant la restauration de toutes les modifications de données. Pour déterminer l'emplacement où l'erreur a eu lieu, incluez dans vos déclencheurs des instructions PRINT.  
  
## <a name="recursive-triggers"></a>Déclencheurs récursifs  
 Un déclencheur AFTER ne s'appelle pas lui-même de manière récursive, à moins que l'option de base de données RECURSIVE_TRIGGERS ne soit définie.  
  
 Il existe deux types d'autorisations :  
  
-   Récurrence directe.  
  
     Elle se produit lorsqu'un déclencheur est activé et exécute une action qui l'active de nouveau. Par exemple, une application met à jour la table **T3**, ce qui active le déclencheur **Trig3** . **Trig3** met à jour de nouveau la table **T3** , ce qui active encore le déclencheur **Trig3** .  
  
     La récurrence directe peut également se produire si le même déclencheur est rappelé, mais après l'appel d'un type différent (AFTER ou INSTEAD OF) de déclencheur. Autrement dit, la récurrence directe d'un déclencheur INSTEAD OF peut se produire lorsque le même déclencheur est rappelé, même si un ou plusieurs déclencheurs AFTER ont été appelés entre temps. De même, la récurrence directe d'un déclencheur AFTER peut se produire lorsque le même déclencheur est rappelé, même si un ou plusieurs déclencheurs INSTEAD OF ont été appelés entre temps. Par exemple, une application met à jour la table **T4**. Cette mise à jour entraîne l’activation du déclencheur INSTEAD OF **Trig4** . **Trig4** met à jour la table **T5**. Cette mise à jour entraîne l’activation du déclencheur AFTER **Trig5** . **Trig5** met à jour la table **T4**, et cette mise à jour entraîne de nouveau l’activation du déclencheur INSTEAD OF **Trig4** . Cette chaîne d’événements est une récursion directe pour **Trig4**.  
  
-   Récurrence indirecte.  
  
     Elle se produit lorsqu'un déclencheur est activé et exécute une action qui active un autre déclencheur du même type (AFTER ou INSTEAD OF). Le deuxième déclencheur exécute une action qui active de nouveau le déclencheur de départ. En d'autres termes, la récursion indirecte peut se produire lorsqu'un déclencheur INSTEAD OF est rappelé, mais pas avant l'appel d'un autre déclencheur INSTEAD OF entre temps. De même, la récursion indirecte peut se produire lorsqu'un déclencheur AFTER est rappelé, mais pas avant l'appel d'un autre déclencheur AFTER entre temps. Par exemple, une application met à jour la table **T1**. Cette mise à jour entraîne l’activation du déclencheur AFTER **Trig1** . **Trig1** met à jour la table **T2**, ce qui active le déclencheur AFTER **Trig2** . **Trig2** à son tour met à jour la table **T1** , ce qui active de nouveau le déclencheur AFTER **Trig1** .  
  
 Seule la récurrence directe des déclencheurs AFTER est neutralisée lorsque l'option de base de données RECURSIVE_TRIGGERS est désactivée (OFF). Pour désactiver la récurrence indirecte des déclencheurs AFTER, affectez en plus la valeur **0** à l’option de serveur **nested triggers**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre l'utilisation des déclencheurs récursifs pour résoudre une relation faisant référence à elle-même, également appelé fermeture transitive. Par exemple, la table `emp_mgr` définit les éléments suivants :  
  
-   un salarié (`emp`) dans une entreprise ;  
  
-   le responsable de chaque salarié (`mgr`) ;  
  
-   le nombre total de salariés dans l'arborescence de l'organisation rattachés à chaque salarié (`NoOfReports`).  
  
 Un déclencheur de mise à jour récursif peut être utilisé pour maintenir à jour la colonne `NoOfReports` au fur et à mesure de l'insertion de l'enregistrement de nouveaux salariés. Le déclencheur INSERT met à jour la colonne `NoOfReports` de l'enregistrement du responsable, provoquant la mise à jour récursive de la colonne `NoOfReports` des autres enregistrements en remontant la hiérarchie.  
  
```  
USE AdventureWorks2012;  
GO  
-- Turn recursive triggers ON in the database.  
ALTER DATABASE AdventureWorks2012  
   SET RECURSIVE_TRIGGERS ON;  
GO  
CREATE TABLE dbo.emp_mgr (  
   emp char(30) PRIMARY KEY,  
    mgr char(30) NULL FOREIGN KEY REFERENCES emp_mgr(emp),  
    NoOfReports int DEFAULT 0  
);  
GO  
CREATE TRIGGER dbo.emp_mgrins ON dbo.emp_mgr  
FOR INSERT  
AS  
DECLARE @e char(30), @m char(30);  
DECLARE c1 CURSOR FOR  
   SELECT emp_mgr.emp  
   FROM   emp_mgr, inserted  
   WHERE emp_mgr.emp = inserted.mgr;  
  
OPEN c1;  
FETCH NEXT FROM c1 INTO @e;  
WHILE @@fetch_status = 0  
BEGIN  
   UPDATE dbo.emp_mgr  
   SET emp_mgr.NoOfReports = emp_mgr.NoOfReports + 1 -- Add 1 for newly  
   WHERE emp_mgr.emp = @e ;                           -- added employee.  
  
   FETCH NEXT FROM c1 INTO @e;  
END  
CLOSE c1;  
DEALLOCATE c1;  
GO  
-- This recursive UPDATE trigger works assuming:  
--   1. Only singleton updates on emp_mgr.  
--   2. No inserts in the middle of the org tree.  
CREATE TRIGGER dbo.emp_mgrupd ON dbo.emp_mgr FOR UPDATE  
AS  
IF UPDATE (mgr)  
BEGIN  
   UPDATE dbo.emp_mgr  
   SET emp_mgr.NoOfReports = emp_mgr.NoOfReports + 1 -- Increment mgr's  
   FROM inserted                            -- (no. of reports) by  
   WHERE emp_mgr.emp = inserted.mgr;         -- 1 for the new report.  
  
   UPDATE dbo.emp_mgr  
   SET emp_mgr.NoOfReports = emp_mgr.NoOfReports - 1 -- Decrement mgr's  
   FROM deleted                             -- (no. of reports) by 1  
   WHERE emp_mgr.emp = deleted.mgr;          -- for the new report.  
END  
GO  
-- Insert some test data rows.  
INSERT dbo.emp_mgr(emp, mgr) VALUES  
    ('Harry', NULL)  
    ,('Alice', 'Harry')  
    ,('Paul', 'Alice')  
    ,('Joe', 'Alice')  
    ,('Dave', 'Joe');  
GO  
SELECT emp,mgr,NoOfReports  
FROM dbo.emp_mgr;  
GO  
-- Change Dave's manager from Joe to Harry  
UPDATE dbo.emp_mgr SET mgr = 'Harry'  
WHERE emp = 'Dave';  
GO  
SELECT emp,mgr,NoOfReports FROM emp_mgr;  
  
GO  
```  
  
 Résultats avant la mise à jour :  
  
```  
emp                            mgr                           NoOfReports  
------------------------------ ----------------------------- -----------  
Alice                          Harry                          2  
Dave                           Joe                            0  
Harry                          NULL                           1  
Joe                            Alice                          1  
Paul                           Alice                          0  
```  
  
 Résultats après la mise à jour :  
  
```  
emp                            mgr                           NoOfReports  
------------------------------ ----------------------------- -----------  
Alice                          Harry                          2  
Dave                           Harry                          0  
Harry                          NULL                           2  
Joe                            Alice                          0  
Paul                           Alice                          0  
```  
  
 **Pour définir l'option nested triggers**  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
 **Pour définir l'option de base de données RECURSIVE_TRIGGERS**  
  
-   [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Configurer l'option de configuration du serveur nested triggers](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md)  
  
  
