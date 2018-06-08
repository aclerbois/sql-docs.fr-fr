---
title: Créer de déclencheurs DML pour gérer plusieurs lignes de données | Microsoft Docs
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
- multiple row DML triggers
- UPDATE statement [SQL Server], DML triggers
- DELETE statement [SQL Server], DML triggers
- multirow DML triggers [SQL Server]
- INSERT statement [SQL Server], DML triggers
- DML triggers, multirow
ms.assetid: d476c124-596b-4b27-a883-812b6b50a735
caps.latest.revision: 25
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a7309d66c4fdc154ca30707133145e94dbf62f86
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-dml-triggers-to-handle-multiple-rows-of-data"></a>Créer de déclencheurs DML pour gérer plusieurs lignes de données
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Lors de l'écriture du code d'un déclencheur DML, tenez compte du fait que l'instruction qui active le déclencheur peut être une instruction unique concernant plusieurs lignes de données au lieu d'une seule. Ce comportement est courant pour les déclencheurs UPDATE et DELETE qui concernent souvent plusieurs lignes. Il est moins fréquent dans le cas des déclencheurs INSERT car l'instruction INSERT de base n'ajoute qu'une seule ligne. Toutefois, comme un déclencheur INSERT peut être activé par une instruction SELECT INSERT INTO (*nom_table*), l’insertion de nombreuses lignes peut aboutir à un appel de déclencheur unique.  
  
 Les facteurs à prendre en compte au sujet des lignes multiples sont particulièrement importants lorsque la fonction d'un déclencheur DML consiste à recalculer automatiquement les totaux d'une table et à enregistrer les résultats dans une autre table en vue de subir d'autres calculs.  
  
> [!NOTE]  
>  Il n'est pas conseillé d'utiliser les curseurs dans le déclencheur, car ils pourraient réduire les performances. Pour concevoir un déclencheur portant sur plusieurs lignes, utilisez au lieu des curseurs une logique basée sur un ensemble de lignes.  
  
## <a name="examples"></a>Exemples  
 Les déclencheurs DML des exemples suivants sont conçus pour stocker le total cumulé d'une colonne dans une autre table de l'exemple de bases de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
### <a name="a-storing-a-running-total-for-a-single-row-insert"></a>A. Stockage d'un total cumulé pour l'insertion d'une seule ligne  
 La première version du déclencheur DML fonctionne correctement pour l'insertion d'une seule ligne, lorsqu'une ligne de données est chargée dans la table `PurchaseOrderDetail` . Le déclencheur DML est activé par une instruction INSERT et la nouvelle ligne est chargée dans la table **inserted** pendant la durée d’exécution du déclencheur. L'instruction `UPDATE` lit la valeur de la colonne `LineTotal` correspondant à la ligne et l'ajoute à la valeur existante dans la colonne `SubTotal` de la table `PurchaseOrderHeader` . La clause `WHERE` garantit que la ligne mise à jour dans la table `PurchaseOrderDetail` correspond à la valeur `PurchaseOrderID` de la ligne dans la table **inserted** .  
  
```  
-- Trigger is valid for single-row inserts.  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER NewPODetail  
ON Purchasing.PurchaseOrderDetail  
AFTER INSERT AS  
   UPDATE PurchaseOrderHeader  
   SET SubTotal = SubTotal + LineTotal  
   FROM inserted  
   WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID ;  
```  
  
### <a name="b-storing-a-running-total-for-a-multirow-or-single-row-insert"></a>B. Stockage d'un total cumulé pour l'insertion d'une ou de plusieurs lignes  
 Dans le cas d’une insertion de plusieurs lignes, le déclencheur DML de l’exemple A peut ne pas fonctionner correctement ; l’expression à droite d’une expression d’affectation dans une instruction UPDATE (`SubTotal` + `LineTotal`) ne peut être qu’une unique valeur, et non une liste de valeurs. Ainsi, le déclencheur extrait une valeur à partir d’une seule ligne de la table **inserted** et l’ajoute à la valeur `SubTotal` existante de la table `PurchaseOrderHeader` correspondant à une valeur `PurchaseOrderID` spécifique. Cette opération risque de ne pas produire l’effet escompté si une valeur unique `PurchaseOrderID` se trouve plus d’une fois dans la table **inserted** .  
  
 Pour mettre à jour correctement la table `PurchaseOrderHeader` , le déclencheur doit tenir compte de l’existence possible de plusieurs lignes dans la table **inserted** . Pour ce faire, vous pouvez utiliser la fonction `SUM` , qui calcule le total `LineTotal` d’un groupe de lignes de la table **inserted** pour chaque `PurchaseOrderID`. La fonction `SUM` est incluse dans une sous-requête corrélée (l'instruction `SELECT` entre parenthèses). Cette sous-requête retourne une valeur unique pour chaque `PurchaseOrderID` de la table **inserted** qui correspond ou est corrélée à `PurchaseOrderID` dans la table `PurchaseOrderHeader` .  
  
```  
-- Trigger is valid for multirow and single-row inserts.  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER NewPODetail2  
ON Purchasing.PurchaseOrderDetail  
AFTER INSERT AS  
   UPDATE Purchasing.PurchaseOrderHeader  
   SET SubTotal = SubTotal +   
      (SELECT SUM(LineTotal)  
      FROM inserted  
      WHERE PurchaseOrderHeader.PurchaseOrderID  
       = inserted.PurchaseOrderID)  
   WHERE PurchaseOrderHeader.PurchaseOrderID IN  
      (SELECT PurchaseOrderID FROM inserted);  
```  
  
 Le fonctionnement de ce déclencheur est également correct dans le cas de l'insertion d'une seule ligne ; le total de la colonne `LineTotal` est alors la somme d'une seule ligne. La sous-requête corrélée et l'opérateur `IN` utilisé dans la clause `WHERE` demandent toutefois un traitement complémentaire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce qui n'est pas nécessaire pour l'insertion d'une seule ligne.  
  
### <a name="c-storing-a-running-total-based-on-the-type-of-insert"></a>C. Stockage d'un total cumulé basé sur le type d'insertion  
 Pour rectifier ce problème, vous pouvez modifier le déclencheur afin d'utiliser la méthode optimale en fonction du nombre de lignes. Par exemple, la fonction `@@ROWCOUNT` peut être utilisée dans la logique du déclencheur pour différencier une insertion d'une seule ligne d'une insertion de plusieurs lignes.  
  
```  
-- Trigger valid for multirow and single row inserts  
-- and optimal for single row inserts.  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER NewPODetail3  
ON Purchasing.PurchaseOrderDetail  
FOR INSERT AS  
IF @@ROWCOUNT = 1  
BEGIN  
   UPDATE Purchasing.PurchaseOrderHeader  
   SET SubTotal = SubTotal + LineTotal  
   FROM inserted  
   WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID  
END  
ELSE  
BEGIN  
      UPDATE Purchasing.PurchaseOrderHeader  
   SET SubTotal = SubTotal +   
      (SELECT SUM(LineTotal)  
      FROM inserted  
      WHERE PurchaseOrderHeader.PurchaseOrderID  
       = inserted.PurchaseOrderID)  
   WHERE PurchaseOrderHeader.PurchaseOrderID IN  
      (SELECT PurchaseOrderID FROM inserted)  
END;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Déclencheurs DML](../../relational-databases/triggers/dml-triggers.md)  
  
  
