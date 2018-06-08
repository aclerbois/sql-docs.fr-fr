---
title: Utiliser les paramètres table (Moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- table-valued parameters
- table-valued parameters, about table-valued parameters
- parameters [SQL Server], table-valued
- TVP See table-valued parameters
ms.assetid: 5e95a382-1e01-4c74-81f5-055612c2ad99
caps.latest.revision: 31
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ff370e7b4814397b067fd82c566fe809bc27e18a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-table-valued-parameters-database-engine"></a>Utiliser les paramètres table (Moteur de base de données)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Les paramètres table sont déclarés en utilisant des types de table définis par l'utilisateur. Vous pouvez utiliser des paramètres table pour envoyer plusieurs lignes de données à une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ou à une routine, telle qu'une procédure stockée ou une fonction, sans créer de table temporaire ou de nombreux paramètres.  
  
 Les paramètres table sont comme les tableaux de paramètres dans OLE DB et ODBC, mais ils offrent plus de souplesse et une intégration plus étroite avec [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ils ont également l'avantage de pouvoir participer aux opérations basées sur des ensembles.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] passe des paramètres table aux routines par référence afin d’éviter d’effectuer une copie des données d’entrée. Vous pouvez créer et exécuter des routines [!INCLUDE[tsql](../../includes/tsql-md.md)] avec des paramètres table et les appeler depuis du code [!INCLUDE[tsql](../../includes/tsql-md.md)] , des clients gérés et natifs dans tout langage managé.  
  
 **Dans cette rubrique :**  
  
 [Avantages](#Benefits)  
  
 [Restrictions](#Restrictions)  
  
 [Paramètres table et opérations BULK INSERT](#BulkInsert)  
  
 [Exemple](#Example)  
  
##  <a name="Benefits"></a> Avantages  
 Un paramètre table a comme portée la procédure stockée, la fonction ou le texte [!INCLUDE[tsql](../../includes/tsql-md.md)] dynamique, exactement comme d'autres paramètres. De même, une variable de type de table a une portée semblable à toute autre variable locale créée à l'aide d'une instruction DECLARE. Vous pouvez déclarer des variables table dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dynamiques et passer ces variables comme paramètres table à des procédures stockées et des fonctions.  
  
 Les paramètres table offrent davantage de souplesse et dans certains cas de meilleures performances que les tables temporaires ou autres méthodes de passage d'une liste de paramètres. Les paramètres table offrent les avantages suivants :  
  
-   Pas d'acquisition de verrous pour le remplissage initial de données à partir d'un client.  
  
-   Fournissent un modèle de programmation simple.  
  
-   Vous permettent d'inclure une logique métier complexe dans une routine unique.  
  
-   Réduisent les boucles au serveur.  
  
-   Peuvent avoir une structure de table de cardinalité différente.  
  
-   Sont fortement typées.  
  
-   Permettent au client de spécifier un ordre de tri et des clés uniques.  
  
-   Sont mis en cache comme une table temporaire en cas de utilisation dans une procédure stockée. À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les paramètres table sont également mis en cache pour les requêtes paramétrables.  
  
##  <a name="Restrictions"></a> Restrictions  
 Les paramètres table ont les restrictions suivantes :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne maintient pas de statistiques sur les colonnes de paramètres table.  
  
-   Les paramètres table doivent être passés comme paramètres READONLY d'entrée aux routines [!INCLUDE[tsql](../../includes/tsql-md.md)] . Vous ne pouvez pas effectuer d'opérations DML telles que UPDATE, DELETE ou INSERT sur un paramètre table dans le corps d'une routine.  
  
-   Vous ne pouvez pas utiliser de paramètre table comme cible d'une instruction SELECT INTO ou INSERT EXEC. Un paramètre table peut être dans la clause FROM de SELECT INTO ou dans la chaîne ou procédure stockée INSERT EXEC.  
  
##  <a name="BulkInsert"></a> Paramètres table et opérations BULK INSERT  
 L'utilisation de paramètres table est comparable à d'autres méthodes d'utilisation de variables basées sur des ensembles ; toutefois, l'utilisation de paramètres table peut souvent s'avérer plus rapide pour les grands ensembles de données. Comparé aux opérations en bloc, qui ont un coût de démarrage supérieur, les paramètres table sont particulièrement adaptés à l'insertion de moins de 1000 lignes.  
  
 Les paramètres table réutilisés tirent parti de la mise en cache de table temporaire. Cette mise en cache de table autorise une meilleure extensibilité que les opérations BULK INSERT équivalentes. En utilisant de petites opérations d'insertion de ligne, vous pouvez retirer un petit avantage en matière de performances en utilisant des listes de paramètres ou des instructions groupées au lieu d'opérations BULK INSERT ou de paramètres table. Toutefois, ces méthodes sont moins commodes à programmer et les performances diminuent rapidement à mesure que le nombre de lignes augmente.  
  
 Les paramètres table procurent des performances supérieures ou égales à une implémentation de tableau de paramètres équivalente.  
  
##  <a name="Example"></a> Exemple  
 L'exemple suivant utilise [!INCLUDE[tsql](../../includes/tsql-md.md)] et montre comment créer un type de paramètre table, déclarer une variable pour y faire référence, remplir la liste de paramètres, puis passer les valeurs à une procédure stockée.  
  
```  
USE AdventureWorks2012;  
GO  
  
/* Create a table type. */  
CREATE TYPE LocationTableType AS TABLE   
( LocationName VARCHAR(50)  
, CostRate INT );  
GO  
  
/* Create a procedure to receive data for the table-valued parameter. */  
CREATE PROCEDURE dbo. usp_InsertProductionLocation  
    @TVP LocationTableType READONLY  
    AS   
    SET NOCOUNT ON  
    INSERT INTO AdventureWorks2012.Production.Location  
           (Name  
           ,CostRate  
           ,Availability  
           ,ModifiedDate)  
        SELECT *, 0, GETDATE()  
        FROM  @TVP;  
        GO  
  
/* Declare a variable that references the type. */  
DECLARE @LocationTVP AS LocationTableType;  
  
/* Add data to the table variable. */  
INSERT INTO @LocationTVP (LocationName, CostRate)  
    SELECT Name, 0.00  
    FROM AdventureWorks2012.Person.StateProvince;  
  
/* Pass the table variable data to a stored procedure. */  
EXEC usp_InsertProductionLocation @LocationTVP;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)   
 [sys.parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)   
 [sys.parameter_type_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
  
  
