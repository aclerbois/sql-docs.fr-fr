---
title: Contraintes uniques et contraintes de validation | Microsoft Docs
ms.custom: ''
ms.date: 06/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- constraints [SQL Server], Visual Database Tools
- Visual Database Tools [SQL Server], constraints
ms.assetid: 637098af-2567-48f8-90f4-b41df059833e
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f342a9c7a90a3eb125bc87deacb272b109f82bdb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unique-constraints-and-check-constraints"></a>Contraintes uniques et contraintes de validation
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Les contraintes UNIQUE et CHECK sont deux types de contraintes qui peuvent être utilisées pour appliquer l'intégrité des données à des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce sont des objets de base de données importants.  
  
 Cette rubrique contient les sections suivantes.  
  
 [Contraintes UNIQUE](#Unique)  
  
 [Contraintes CHECK](#Check)  
  
 [Tâches associées](#Tasks)  
  
##  <a name="Unique"></a> Contraintes UNIQUE  
 Les contraintes sont des règles que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] applique automatiquement. par exemple, vous pouvez utiliser des contraintes UNIQUE pour garantir qu'aucune valeur en double n'est entrée dans des colonnes spécifiques ne faisant pas partie d'une clé primaire. Bien qu'une contrainte UNIQUE et une contrainte PRIMARY KEY assurent l'unicité, il est préférable d'avoir recours à une contrainte UNIQUE au lieu d'une contrainte PRIMARY KEY lorsque vous voulez assurer l'unicité d'une colonne (ou d'une combinaison de colonnes) qui n'est pas la clé primaire.  
  
 À la différence des contraintes PRIMARY KEY, les contraintes UNIQUE autorisent la valeur NULL. Cependant, comme pour toute valeur participant à une contrainte UNIQUE, une seule valeur NULL est autorisée par colonne. Une contrainte UNIQUE peut être référencée par une contrainte FOREIGN KEY.  
  
 Lorsque vous ajoutez une contrainte UNIQUE à une ou plusieurs colonnes dans la table, par défaut, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] examine les données existantes dans les colonnes pour s'assurer que toutes les valeurs sont uniques. Si une contrainte UNIQUE est ajoutée à une colonne qui comporte des valeurs dupliquées, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne une erreur et n’ajoute pas la contrainte.  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée automatiquement un index UNIQUE pour appliquer l’impératif d’unicité de la contrainte UNIQUE. Dès lors, en cas de tentative d’insertion d’une ligne dupliquée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne un message d’erreur indiquant que la contrainte UNIQUE a été violée et n’ajoute pas la ligne à la table. Sauf si un index cluster est explicitement spécifié, un index non-cluster unique est créé par défaut pour assurer l'application de la contrainte UNIQUE.  
  
##  <a name="Check"></a> Contraintes CHECK  
 Les contraintes CHECK assurent l'intégrité de domaine en limitant les valeurs acceptées par une ou plusieurs colonnes. Vous pouvez créer une contrainte CHECK avec n'importe quelle expression logique (booléenne) qui retourne TRUE ou FALSE sur la base des opérateurs logiques. Par exemple, il est possible de limiter la plage de valeurs d'une colonne **salary** en créant une contrainte CHECK qui autorise uniquement les données comprises entre 15 000 $ et 100 000 $. De cette façon, il est impossible d'entrer des salaires non compris dans cette fourchette de salaires normaux. L'expression logique est la suivante : `salary >= 15000 AND salary <= 100000`.  
  
 Vous pouvez appliquer plusieurs contraintes CHECK à une seule colonne. Vous pouvez aussi appliquer une seule contrainte CHECK à plusieurs colonnes en la créant au niveau de la table. Ainsi, vous pouvez utiliser une contrainte CHECK sur plusieurs colonnes pour confirmer que les lignes comportant la valeur **USA** dans leur colonne **country_region** possèdent également une valeur à deux caractères dans leur colonne **state** . Cela permet de vérifier plusieurs conditions au même emplacement.  
  
 Les contraites CHECK sont similaires aux contraintes FOREIGN KEY dans la mesure où elles contrôlent les valeurs qui sont placées dans une colonne. Leur différence réside dans la manière dont elles déterminent les valeurs considérées comme valides : les contraintes FOREIGN KEY obtiennent la liste des valeurs valides d'une autre table, tandis que les contraintes CHECK déterminent ces valeurs sur la base d'une expression logique.  
  
> [!CAUTION]  
>  Les contraintes qui incluent une conversion de type de données implicite ou explicite peuvent causer l'échec de certaines opérations. Par exemple, ces contraintes définies sur des tables qui sont les sources d'une commutation de partition peuvent causer l'échec d'une opération ALTER TABLE...SWITCH. Évitez les conversions de types de données dans les définitions des contraintes.  
  
### <a name="limitations-of-check-constraints"></a>Limitations des contraintes CHECK  
 Les contraintes CHECK rejettent les valeurs qui donnent FALSE. Comme les valeurs nulles donnent UNKNOWN, il se peut que leur présence dans les expressions supplante une contrainte. Supposons par exemple que vous placez une contrainte sur une colonne **int** **MyColumn** en spécifiant que **MyColumn** peut contenir uniquement la valeur 10 (**MyColumn=10**). Si vous insérez la valeur NULL dans **MyColumn**, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] insère NULL et ne retourne pas d'erreur.  
  
 Une contrainte CHECK retourne TRUE lorsque la condition qu'elle vérifie n'est pas FALSE pour une ligne quelconque de la table. Une contrainte CHECK s'exécute au niveau de la ligne. Si une table venant d'être créée ne comporte aucune ligne, les éventuelles contraintes CHECK sur cette table sont considérées comme valides. Cette situation peut produire des résultats inattendus, comme l'illustre l'exemple suivant.  
  
```  
CREATE TABLE CheckTbl (col1 int, col2 int);  
GO  
CREATE FUNCTION CheckFnctn()  
RETURNS int  
AS   
BEGIN  
   DECLARE @retval int  
   SELECT @retval = COUNT(*) FROM CheckTbl  
   RETURN @retval  
END;  
GO  
ALTER TABLE CheckTbl  
ADD CONSTRAINT chkRowCount CHECK (dbo.CheckFnctn() >= 1 );  
GO  
```  
  
 La contrainte `CHECK` ajoutée spécifie que la table `CheckTbl`doit contenir au moins une ligne. Toutefois, comme la table ne contient aucune ligne par rapport à laquelle vérifier la condition de cette contrainte, l'instruction ALTER TABLE réussit.  
  
 Les contraintes CHECK ne sont pas validées pendant les instructions DELETE. Par conséquent, l'exécution d'instructions DELETE sur des tables avec certains types de contraintes de vérification peuvent produire des résultats inattendus. Imaginons, par exemple, les instructions suivantes exécutées sur la table `CheckTbl`.  
  
```  
INSERT INTO CheckTbl VALUES (10, 10);  
GO  
DELETE CheckTbl WHERE col1 = 10;  
```  
  
 L'instruction `DELETE` aboutit, même si la contrainte `CHECK` spécifie que la table `CheckTbl` doit comporter au moins `1` ligne.  
  
##  <a name="Tasks"></a> Tâches associées  
  
> [!NOTE]  
>  Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au schéma à l'aide du Concepteur de tables ou du Concepteur de schémas de base de données, celui-ci tente d'abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Décrit comment créer une contrainte unique.|[Créer des contraintes uniques](../../relational-databases/tables/create-unique-constraints.md)|  
|Décrit comment modifier une contrainte unique.|[Modifier des contraintes uniques](../../relational-databases/tables/modify-unique-constraints.md)|  
|Décrit comment supprimer une contrainte unique.|[Supprimer des contraintes UNIQUE](../../relational-databases/tables/delete-unique-constraints.md)|  
|Décrit comment désactiver une contrainte de validation lorsque l'Agent de réplication insère ou met à jour les données dans votre table.|[Désactiver des contraintes de validation lors de la réplication](../../relational-databases/tables/disable-check-constraints-for-replication.md)|  
|Décrit comment désactiver une contrainte de validation lorsque vous ajoutez, mettez à jour ou supprimez des données dans une table.|[Désactiver des contraintes de validation avec des instructions INSERT et UPDATE](../../relational-databases/tables/disable-check-constraints-with-insert-and-update-statements.md)|  
|Décrit comment modifier l'expression de contrainte ou les options qui activent ou désactivent la contrainte pour des conditions spécifiques.|[Modifier des contraintes CHECK](../../relational-databases/tables/modify-check-constraints.md)|  
|Décrit comment supprimer une contrainte de validation.|[Supprimer des contraintes de validation](../../relational-databases/tables/delete-check-constraints.md)|  
|Décrit comment afficher les propriétés d'une contrainte de validation.|[Contraintes uniques et contraintes de validation](../../relational-databases/tables/unique-constraints-and-check-constraints.md)|  
  
  
