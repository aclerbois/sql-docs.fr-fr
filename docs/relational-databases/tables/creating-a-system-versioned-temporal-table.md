---
title: Création d’une table temporelle avec contrôle de version du système | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 21e6d74f-711f-40e6-a8b7-85f832c5d4b3
caps.latest.revision: 20
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 03077d7ede10d42b4d4812ce6ef93a35dd295a22
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="creating-a-system-versioned-temporal-table"></a>Création d’une table temporelle avec gestion de version du système
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Il existe trois façons de créer une table temporelle avec gestion de version du système en ce qui concerne la manière dont la table de l’historique est spécifiée :  
  
-   Table temporelle avec table de l’historique anonyme : vous spécifiez le schéma de la table actuelle et laissez le système créer une table de l’historique correspondante avec un nom généré automatiquement.  
  
-   Table temporelle avec table de l’historique par défaut : vous pouvez spécifier le nom de schéma de la table de l’historique et le nom de la table, puis laisser le système créer une table de l’historique dans ce schéma.  
  
-   Table temporelle avec table de l’historique définie par l’utilisateur créée au préalable : vous créez une table de l’historique adaptée à vos besoins, puis référencez cette table lors de la création de la table temporelle.  
  
## <a name="creating-a-temporal-table-with-an-anonymous-history-table"></a>Création d’une table temporelle avec une table de l’historique anonyme  
 La création d’une table temporelle avec une table de l’historique « anonyme » est une option pratique pour créer rapidement un objet, en particulier dans des environnements de test et de prototypage. Il s’agit également de la méthode la plus simple pour créer une table temporelle, car elle ne nécessite aucun paramètre dans la clause **SYSTEM_VERSIONING** . Dans l’exemple ci-dessous, une nouvelle table est créée, avec contrôle de version du système activé, sans qu’il faille définir le nom de la table de l’historique.  
  
```  
CREATE TABLE Department   
(    
     DeptID int NOT NULL PRIMARY KEY CLUSTERED  
   , DeptName varchar(50) NOT NULL  
   , ManagerID INT  NULL  
   , ParentDeptID int NULL  
   , SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL  
   , SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL  
   , PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)     
)    
WITH (SYSTEM_VERSIONING = ON)   
;  
```  
  
### <a name="important-remarks"></a>Remarques importantes  
  
-   Une table temporelle avec gestion de version du système doit avoir une clé primaire définie et précisément une instruction **PERIOD FOR SYSTEM_TIME** définie avec deux colonnes datetime2, déclarée comme **GENERATED ALWAYS AS ROW START / END**.  
  
-   Les colonnes **PERIOD** sont toujours considérées comme n’acceptant pas les valeurs Null, même si la possibilité de valeur Null n’est pas spécifiée. Si les colonnnes  **PERIOD** sont explicitement définies comme acceptant les valeurs Null, l’instruction **CREATE TABLE** échoue.  
  
-   La table de l’historique doit toujours être alignée par schéma sur la table actuelle ou temporelle, en termes de nombre de colonnes, de noms de colonnes, de classement et de types de données.  
  
-   Une table de l’historique anonyme est créée automatiquement sur le même schéma que la table en cours ou temporelle.  
  
-   Le nom de la table de l’historique anonyme a le format suivant : *MSSQL_TemporalHistoryFor_<ID_objet_table_temporelle_actuelle>_ [suffixe]*. Le suffixe est facultatif. Il est ajouté uniquement si la première partie du nom de la table n’est pas unique.  
  
-   La table de l’historique est créée en tant que table rowstore. Un compression de page est appliquée si possible. Autrement, la table de l’historique est décompressée. Par exemple, certaines configurations de table, telles des colonnes fragmentées, n’autorisent pas la compression.  
  
-   Un index cluster par défaut est créé pour la table de l’historique avec un nom généré automatiquement au format *IX_<nom_table_historique>*. L’index cluster contient les colonnes **PERIOD** (début, fin).  
  
-   Pour créer la table actuelle comme table optimisée en mémoire, consultez [Tables temporelles à système par version avec tables optimisées en mémoire](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md).  
  
## <a name="creating-a-temporal-table-with-a-default-history-table"></a>Création d’une table temporelle avec une table de l’historique par défaut  
 La création d’une table temporelle avec une table de l’historique par défaut est une option pratique quand vous voulez contrôler l’affectation des noms, tout en continuant de laisser le système créer la table de l’historique avec la configuration par défaut. Dans l’exemple ci-dessous, une nouvelle table est créée, avec le contrôle de version du système activé et le nom de la table de l’historique défini explicitement.  
  
```  
CREATE TABLE Department   
(    
     DeptID int NOT NULL PRIMARY KEY CLUSTERED  
   , DeptName varchar(50) NOT NULL  
   , ManagerID INT  NULL  
   , ParentDeptID int NULL  
   , SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL  
   , SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL  
   , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)     
)   
WITH    
   (   
      SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DepartmentHistory)   
   )   
;  
```  
  
### <a name="important-remarks"></a>Remarques importantes  
 La table de l’historique est créée à l’aide des règles appliquées à la création d’une table de l’historique « anonyme », les règles suivantes s’appliquant spécifiquement à la table de l’historique nommée.  
  
-   Le nom du schéma est obligatoire pour le paramètre **HISTORY_TABLE** .  
  
-   Si le schéma spécifié n’existe pas, l’instruction **CREATE TABLE** échoue.  
  
-   Si la table spécifiée par le paramètre **HISTORY_TABLE** existe déjà, elle est validée par rapport à la table temporelle nouvellement créée sur les plans [de la cohérence du schéma et de la cohérence des données temporelles](http://msdn.microsoft.com/library/dn935015.aspx). Si vous spécifiez une table de l’historique non valide, l’instruction **CREATE TABLE** échoue.  
  
## <a name="creating-a-temporal-table-with-a-user-defined-history-table"></a>Création d’une table temporelle avec une table de l’historique définie par l’utilisateur  
 La création d’une table temporelle avec une table de l’historique définie par l’utilisateur est une option pratique pour un utilisateur désireux de spécifier une table de l’historique avec des options de stockage et des index supplémentaires spécifiques. Dans l’exemple ci-dessous, une table de l’historique définie par l’utilisateur est créée avec un schéma qui est aligné avec la table temporelle qui sera créée. Sur cette table de l’historique définie par l’utilisateur, un index columnstore cluster et un index rowstore (Btree) non cluster supplémentaire sont créés pour les recherches de point. Une fois cette table de l’historique définie par l’utilisateur créée, la table temporelle avec contrôle de version du système est créée en spécifiant la table de l’historique définie par l’utilisateur en tant que la table de l’historique par défaut.  
  
```  
CREATE TABLE DepartmentHistory   
(    
     DeptID int NOT NULL  
   , DeptName varchar(50) NOT NULL  
   , ManagerID INT  NULL  
   , ParentDeptID int NULL  
   , SysStartTime datetime2 NOT NULL  
   , SysEndTime datetime2 NOT NULL   
);   
GO   
CREATE CLUSTERED COLUMNSTORE INDEX IX_DepartmentHistory   
   ON DepartmentHistory;   
CREATE NONCLUSTERED INDEX IX_DepartmentHistory_ID_PERIOD_COLUMNS   
   ON DepartmentHistory (SysEndTime, SysStartTime, DeptID);   
GO   
CREATE TABLE Department   
(    
    DeptID int NOT NULL PRIMARY KEY CLUSTERED  
   , DeptName varchar(50) NOT NULL  
   , ManagerID INT  NULL  
   , ParentDeptID int NULL  
   , SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL  
   , SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL     
   , PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)      
)    
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DepartmentHistory))   
;  
```  
  
### <a name="important-remarks"></a>Remarques importantes  
  
-   Si vous projetez d’exécuter des requêtes analytiques sur des données historiques qui emploient des agrégats ou des fonctions de fenêtrage, la création d’un index columnstore cluster en tant qu’index primaire est une option vivement recommandée sur les plans de la compression et des performances des requêtes.  
  
-   Si l’usage principal est l’audit de données (par exemple, la recherche des modifications historiques d’une seule ligne de la table actuelle), un bon choix consiste à créer une table de l’historique rowstore avec un index cluster.  
  
-   La table de l’historique ne peut pas avoir de clé primaire, de clés étrangères, d’index uniques, de contraintes de table ou de déclencheurs. Elle ne peut pas être configurée pour la capture des données modifiées, le suivi des modifications ou la réplication de fusion.  
  
## <a name="alter-non-temporal-table-to-be-system-versioned-temporal-table"></a>Modifier une table non temporelle pour la convertir en table temporelle avec contrôle de version du système  
 Cette solution est appropriée lorsque vous devez activer le contrôle de version du système à l’aide d’une table existante, par exemple, quand vous voulez migrer une solution temporelle personnalisée vers une prise en charge intégrée.   
Par exemple, vous avez peut-être un ensemble de tables où le contrôle de version est implémenté avec des déclencheurs. L’utilisation d’un contrôle de version du système temporel est moins complexe et offre des avantages supplémentaires, notamment :  
  
-   historique immuable  
  
-   nouvelle syntaxe pour les requêtes se déplaçant dans le temps  
  
-   meilleures performances DML  
  
-   coûts de maintenance minimal  
  
 Lors de la conversion d’une table existante, envisagez d’utiliser la clause **HIDDEN** pour masquer les nouvelles colonnes **PERIOD** afin d’éviter tout impact sur des applications existantes non conçues pour gérer de nouvelles colonnes.  
  
### <a name="adding-versioning-to-non-temporal-tables"></a>Ajout du contrôle de version à des tables non temporelles  
 Si vous voulez commencer à suivre les modifications apportées à une table non temporelle contenant des données, vous devez ajouter la définition **PERIOD** et éventuellement fournir un nom pour la table de l’historique vide que SQL Server créera pour vous :  
  
```  
CREATE SCHEMA History;   
GO   
ALTER TABLE InsurancePolicy   
   ADD   
      SysStartTime datetime2(0) GENERATED ALWAYS AS ROW START HIDDEN    
           CONSTRAINT DF_SysStart DEFAULT SYSUTCDATETIME()  
      , SysEndTime datetime2(0) GENERATED ALWAYS AS ROW END HIDDEN    
           CONSTRAINT DF_SysEnd DEFAULT CONVERT(datetime2 (0), '9999-12-31 23:59:59'),   
      PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);   
GO   
ALTER TABLE InsurancePolicy   
   SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.InsurancePolicy))   
;  
```  
  
#### <a name="important-remarks"></a>Remarques importantes  
  
-   L’ajout de colonnes n’acceptant pas les valeurs Null et comportant des valeurs par défaut à une table existante contenant des données est une opération sur la taille des données pour toutes les éditions autres que SQL Server Entreprise Edition (version sur laquelle il s’agit d’une opération de métadonnées). Sur SQL Server Édition Standard, l’ajout d’une colonne non Null à une table de l’historique volumineuse contenant des données peut être une opération coûteuse.  
  
-   Les contraintes applicables aux colonnes de fin et de début de la période doivent être choisies avec soin :  
  
    -   Par défaut, la colonne de début spécifie le point dans le temps à partir duquel vous considérez que les lignes existantes sont valides. Il ne peut pas s’agir d’un point DateHeure dans le futur.  
  
    -   L’heure de fin doit être spécifiée comme valeur maximale pour un point datetime2 donné.  
  
-   L’ajout d’une période entraîne une vérification de cohérence des données sur la table actuelle pour s’assurer que les valeurs par défaut pour les colonnes de période sont valides.  
  
-   Quand une table de l’historique existant est spécifiée lors de l’activation de **SYSTEM_VERSIONING**, une vérification de cohérence des données temporelles est effectuée sur les tables actuelles et de l’historique. Elle peut être ignorée si vous spécifiez **DATA_CONSISTENCY_CHECK = OFF** comme paramètre supplémentaire.  
  
### <a name="migrate-existing-tables-to-built-in-support"></a>Migrer de tables existantes vers la prise en charge intégrée  
 Cet exemple montre comment migrer une solution basée sur des déclencheurs vers la prise en charge temporelle intégrée. Pour cet exemple, nous partons du principe que la solution personnalisée active fractionne les données actuelles et historiques en deux tables utilisateur séparées (**ProjectTaskCurrent** et **ProjectTaskHistory**). Si votre solution utilise une table unique pour stocker les lignes réelles et historiques, vous devez fractionner les données en deux tables avant d’effectuer les étapes de migration présentées dans cet exemple :  
  
```  
/*Drop trigger on future temporal table*/   
DROP TRIGGER ProjectCurrent_OnUpdateDelete;   
/*Make sure that future period columns are non-nullable*/   
ALTER TABLE ProjectTaskCurrent ALTER COLUMN [ValidFrom] datetime2 NOT NULL;   
ALTER TABLE ProjectTaskCurrent ALTER COLUMN [ValidTo] datetime2 NOT NULL;   
ALTER TABLE ProjectTaskHistory ALTER COLUMN [ValidFrom] datetime2 NOT NULL;   
ALTER TABLE ProjectTaskHistory ALTER COLUMN [ValidTo] datetime2 NOT NULL;   
ALTER TABLE ProjectTaskCurrent   
   ADD PERIOD FOR SYSTEM_TIME ([ValidFrom], [ValidTo])   
ALTER TABLE ProjectTaskCurrent   
   SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))   
;  
```  
  
#### <a name="important-remarks"></a>Remarques importantes  
  
-   Référencer des colonnes existantes dans la définition **PERIOD** modifie implicitement generated_always_type en **AS_ROW_START** et **AS_ROW_END** pour ces colonnes.  
  
-   L’ajout de **PERIOD** entraîne une vérification de cohérence des données de la table actuelle pour s’assurer que les valeurs existantes dans les colonnes de période sont valides.  
  
-   Il est vivement recommandé de définir **SYSTEM_VERSIONING** avec **DATA_CONSISTENCY_CHECK = ON** pour appliquer les vérifications de cohérence des données sur les données existantes.  
  
 
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Modification des données dans une table temporelle avec système par version](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)   
 [Interrogation des données dans une table temporelle avec système par version](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)   
 [Modification du schéma d’une table temporelle à version contrôlée par le système](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)   
 [Arrêt du contrôle de version par le système sur une table temporelle à version contrôlée par le système](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)  
  
  
