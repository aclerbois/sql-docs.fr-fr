---
title: Sécurité au niveau des lignes | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- access control predicates
- row level security
- security [SQL Server], predicate based access control
- row level security described
- predicate based security
ms.assetid: 7221fa4e-ca4a-4d5c-9f93-1b8a4af7b9e8
caps.latest.revision: 47
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 78a3e1731204dc8d75a4783135dc10caa8e4d164
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="row-level-security"></a>Sécurité au niveau des lignes
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  ![Graphique de sécurité au niveau des lignes](../../relational-databases/security/media/row-level-security-graphic.png "Graphique de sécurité au niveau des lignes")  
  
 La sécurité au niveau des lignes permet aux clients de contrôler l'accès aux lignes d'une table de base de données en fonction des caractéristiques de l'utilisateur exécutant une requête (appartenance à un groupe ou contexte d'exécution, par exemple).  
  
 La sécurité au niveau des lignes simplifie la conception et codage de la sécurité dans votre application. La sécurité au niveau des lignes vous permet d'implémenter des restrictions sur l'accès aux lignes de données. Par exemple, s'assurer que les employés peuvent accéder uniquement aux lignes de données pertinentes par rapport à leur service ou restreindre l'accès aux données d'un client aux seules données pertinentes pour son entreprise.  
  
 La logique de la restriction d'accès est située dans la couche de base de données plutôt que loin des données d'une autre couche Application. Le système de base de données applique les restrictions d'accès chaque fois que cet accès aux données est tenté à partir d'une couche quelconque. Votre système de sécurité est ainsi rendu plus fiable et plus robuste, grâce à une réduction de sa surface d'exposition.  
  
 Implémentez une sécurité au niveau des lignes à l’aide de l’instruction [CREATE SECURITY POLICY](../../t-sql/statements/create-security-policy-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] , et de prédicats créés en tant que [fonctions table incluses](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([Obtenez-le](http://azure.microsoft.com/documentation/articles/sql-database-preview-whats-new/?WT.mc_id=TSQL_GetItTag)).  
  

##  <a name="Description"></a> Description  
 La sécurité au niveau des lignes prend en charge deux types de prédicats de sécurité.  
  
-   Les prédicats de filtre filtrent en mode silencieux les lignes disponibles pour les opérations de lecture (SELECT, UPDATE et DELETE).  
  
-   Les prédicats BLOCK bloquent explicitement les opérations d’écriture (AFTER INSERT, AFTER UPDATE, BEFORE UPDATE et BEFORE DELETE) qui violent le prédicat.  
  
 L’accès aux données au niveau des lignes dans une table est restreint par un prédicat de sécurité défini en tant que fonction table incluse. La fonction est ensuite appelée et appliquée par une stratégie de sécurité. Pour les prédicats de filtre, rien n’indique à l’application que des lignes ont été filtrées du jeu de résultats. Si toutes les lignes sont filtrées, un jeu de valeurs null est retourné. Pour les prédicats BLOCK, toutes les opérations qui violent le prédicat échouent avec une erreur.  
  
 Les prédicats de filtre sont appliqués lors de la lecture des données de la table de base, et toutes les opérations get sont affectées : **SELECT**, **DELETE** (l’utilisateur ne peut pas supprimer des lignes filtrées) et **UPDATE** (l’utilisateur ne peut pas mettre à jour les lignes filtrées, bien qu’il soit possible de mettre à jour les lignes de sorte qu’elles soient filtrées par la suite). Les prédicats BLOCK affectent toutes les opérations d’écriture.  
  
-   Les prédicats AFTER INSERT et AFTER UPDATE peuvent empêcher les utilisateurs de mettre à jour des lignes avec des valeurs qui violent le prédicat.  
  
-   Les prédicats BEFORE UPDATE peuvent empêcher les utilisateurs de mettre à jour les lignes qui violent réellement le prédicat.  
  
-   Les prédicats BEFORE DELETE peuvent bloquer les opérations de suppression.  
  
 Les prédicats de filtre et BLOCK, ainsi que les stratégies de sécurité, se comportent comme suit :  
  
-   Vous pouvez définir une fonction de prédicat qui crée une jointure avec une autre table et/ou appelle une fonction. Si la stratégie de sécurité est créée avec `SCHEMABINDING = ON`, alors la jointure ou la fonction est accessible à partir de la requête et fonctionne comme prévu, sans aucun contrôle d’autorisation supplémentaire. Si la stratégie de sécurité est créée avec `SCHEMABINDING = OFF`, alors les utilisateurs ont besoin d’autorisations **SELECT** ou **EXECUTE** sur ces tables et fonctions supplémentaires pour interroger la table cible.
  
-   Vous pouvez émettre une requête sur une table pour laquelle un prédicat de sécurité est défini, mais désactivé. Les lignes qui auraient été filtrées ou bloquées ne sont pas affectées.  
  
-   Si l’utilisateur dbo, membre du rôle **db_owner** , ou le propriétaire de la table interrogent une table pour laquelle une stratégie de sécurité est définie et activée, les lignes sont filtrées ou bloquées conformément à celle-ci.  
  
-   Toute tentative de modification du schéma d’une table liée par une stratégie de sécurité liée au schéma génère une erreur. En revanche, les colonnes non référencées par le prédicat peuvent être modifiées.  
  
-   Toute tentative d’ajouter un prédicat sur une table pour laquelle un prédicat est déjà un défini pour l’opération spécifiée (qu’il soit activé ou pas) génère une erreur.  
  
-   Pour les stratégies de sécurité liée au schéma, toute tentative de modifier une fonction utilisée comme prédicat sur une table à l’intérieur d’une stratégie de sécurité génère une erreur.  
  
-   La définition de plusieurs stratégies de sécurité actives contenant des prédicats sans chevauchement réussit.  
  
 Les prédicats de filtre se comportent comme suit :  
  
-   Définir une stratégie de sécurité qui filtre les lignes d'une table. L'application ignore que des lignes ont été filtrées pour les opérations **SELECT**, **UPDATE**et **DELETE** , y compris dans les cas où la totalité des lignes a été filtrée. L'application peut **INSERT** toutes les lignes, qu'elles soient ou non filtrées lors de toute autre opération.  
  
 Les prédicats BLOCK se comportent comme sui  :  
  
-   Les prédicats BLOCK pour l’opération UPDATE sont divisés en opérations distinctes pour BEFORE et AFTER. Vous ne pouvez donc pas, par exemple, bloquer des utilisateurs en les empêchant de mettre à jour une ligne pour obtenir une valeur supérieure à la valeur actuelle. Si ce type de logique est requis, vous devez utiliser des déclencheurs avec les tables intermédiaires DELETED et INSERTED pour référencer ensemble les valeurs anciennes et nouvelles.  
  
-   L’optimiseur ne vérifie pas un prédicat BLOCK AFTER UPDATE si aucune des colonnes utilisées par la fonction de prédicat n’a été modifiée. Par exemple : Alice ne doit pas pouvoir modifier un salaire en lui attribuant une valeur supérieure à 100 000, mais elle pouvoir modifier l’adresse d’un employé dont le salaire est déjà supérieur à 100 000 (et viole donc déjà le prédicat).  
  
-   Aucune modification n’a été apportée aux API bulk, y compris à BULK INSERT. Cela signifie que les prédicats BLOCK AFTER INSERT s’appliquent aux opérations d’insertion en bloc comme s’il s’agissait d’opérations d’insertion standard.  
  
  
##  <a name="UseCases"></a> Cas d'usage  
 Voici des exemples d'utilisation de la sécurité au niveau des lignes :  
  
-   Un hôpital peut créer une stratégie de sécurité qui autorise les infirmières à n'afficher les lignes de données que pour leurs propres patients.  
  
-   Une banque peut créer une stratégie pour limiter l'accès aux lignes de données financières en fonction de la division de l'entreprise de l'employé ou du rôle de l'employé au sein de la société.  
  
-   Une application mutualisée peut créer une stratégie afin d'appliquer une séparation logique entre les lignes de données de chaque locataire et celles des autres locataires. L'efficacité est obtenue grâce au stockage des données de nombreux locataires dans une seule table. Bien sûr, chaque locataire peut afficher uniquement ses lignes de données.  
  
 Les prédicats de filtre de la sécurité au niveau des lignes sont fonctionnellement équivalents à l'ajout d'une clause **WHERE** . Le prédicat peut être aussi sophistiqué que l'exigent les pratiques professionnelles, ou la clause aussi simple que `WHERE TenantId = 42`.  
  
 En termes plus formels, la sécurité au niveau des lignes introduit le contrôle d'accès basé sur le prédicat. Elle propose une évaluation flexible, centralisée et basée sur le prédicat, qui peut prendre en compte les métadonnées ou tout autre critère que l'administrateur détermine comme approprié. Le prédicat est utilisé comme critère pour déterminer si l'utilisateur a l'accès approprié aux données en fonction de ses propres attributs. Le contrôle d'accès basé sur l'étiquette peut être implémenté à l'aide du contrôle d'accès basé sur le prédicat.  
  
  
##  <a name="Permissions"></a> Permissions  
 La création, modification ou suppression des stratégies de sécurité nécessite l'autorisation **ALTER ANY SECURITY POLICY** . La création ou suppression d'une stratégie de sécurité nécessite l'autorisation **ALTER** sur le schéma.  
  
 En outre, les autorisations suivantes sont requises pour chaque prédicat ajouté :  
  
-   Les autorisations**SELECT** et **REFERENCES** sur la fonction utilisée comme prédicat.  
  
-   L'autorisation**REFERENCES** sur la table cible liée à la stratégie.  
  
-   L'autorisation**REFERENCES** sur chaque colonne de la table cible utilisée comme argument.  
  
 Les stratégies de sécurité s'appliquent à tous les utilisateurs, y compris les utilisateurs dbo de la base de données. Les utilisateurs dbo peuvent modifier ou supprimer les stratégies de sécurité, mais leurs modifications des stratégies de sécurité peuvent être auditées. Si des utilisateurs à privilèges élevés (par exemple, sysadmin ou db_owner) doivent voir toutes les lignes afin de dépanner ou de valider des données, la stratégie de sécurité doit être écrite pour le permettre.  
  
 Si une stratégie de sécurité est créée avec `SCHEMABINDING = OFF`, alors pour interroger la table cible, les utilisateurs doivent avoir des autorisations  **SELECT** ou **EXECUTE** sur la fonction de prédicat et toutes les autres tables, vues ou fonctions utilisées au sein de la fonction de prédicat. Si une stratégie de sécurité est créée avec `SCHEMABINDING = ON` (valeur par défaut), alors ces contrôles d’autorisations sont ignorés quand les utilisateurs interrogent la table cible.  
  
  
##  <a name="Best"></a> Bonnes pratiques  
  
-   Il est fortement recommandé de créer un schéma distinct pour les objets de la sécurité au niveau des lignes (fonction de prédicat et stratégie de sécurité).  
  
-   L’autorisation **ALTER ANY SECURITY POLICY** est destinée aux utilisateurs disposant de privilèges élevés (par exemple, un gestionnaire de stratégie de sécurité). Le gestionnaire de stratégie de sécurité ne nécessite pas l'autorisation **SELECT** sur les tables qu'il protège.  
  
-   Évitez les conversions de type dans les fonctions de prédicat pour éviter les erreurs d'exécution potentielles.  
  
-   Évitez la récursivité dans les fonctions de prédicat chaque fois que possible pour éviter une dégradation des performances. L'optimiseur de requête tente de détecter les récurrences directes, mais il n'est pas assuré de trouver les récurrences indirectes (à savoir, quand une deuxième fonction appelle la fonction de prédicat).  
  
-   Évitez d'utiliser les jointures de table excessives dans les fonctions de prédicat pour optimiser les performances.  
  
 Évitez toute logique de prédicat dépendant d’ [options SET](../../t-sql/statements/set-statements-transact-sql.md)spécifiques de la session : s’il est peu probable qu’elles soient utilisées dans des applications pratiques, les fonctions de prédicat dont la logique dépend de certaines options **SET** spécifiques de la session peuvent entraîner des fuites d’informations si des utilisateurs sont en mesure d’exécuter des requêtes arbitraires. Par exemple, une fonction de prédicat qui convertit implicitement une chaîne en **datetime** pourrait filtrer des lignes différentes, selon l’option **SET DATEFORMAT** définie pour la session active. En règle générale, les fonctions de prédicat doivent respecter les règles suivantes :  
  
-   Les fonctions de prédicat ne doivent pas convertir implicitement des chaînes de caractères en **date**, **smalldatetime**, **datetime**, **datetime2** ou **datetimeoffset**, ou inversement, car ces conversions sont affectées par les options [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md) et [SET LANGUAGE &#40;Transact-SQL&#41;](../../t-sql/statements/set-language-transact-sql.md). Utilisez plutôt la fonction **CONVERT** , et spécifiez explicitement le paramètre de style.  
  
-   Les fonctions de prédicat ne doivent pas dépendre de la valeur du premier jour de la semaine, car celle-ci est affectée par l’option [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).  
  
-   Les fonctions de prédicat ne doivent pas dépendre d’expressions arithmétiques ou d’agrégation retournant **NULL** en cas d’erreur (telle qu’un dépassement ou une division par zéro), car ce comportement est affecté par les options [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md), [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md) et [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md).  
  
-   Les fonctions de prédicat ne doivent pas comparer des chaînes concaténées à **NULL**, car ce comportement est affecté par l’option [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  
   
  
##  <a name="SecNote"></a> Note de sécurité : Attaques côté canal  
 **Gestionnaire de stratégie de sécurité malveillant :** il est important d’observer qu’un gestionnaire de stratégie de sécurité malveillant, disposant d’autorisations suffisantes pour créer une stratégie de sécurité sur une colonne sensible, et qui est autorisé à créer ou à modifier des fonctions table incluses, pourrait agir en collusion avec un autre utilisateur ayant des autorisations select sur une table pour effectuer une exfiltration de données en créant à des fins malveillantes des fonctions table incluses destinées à utiliser des attaques côté canal pour inférer des données. Ces attaques nécessitent une collusion (ou des autorisations excessives accordées à un utilisateur malveillant) et probablement plusieurs itérations de modification de la stratégie (nécessitant l'autorisation de supprimer le prédicat pour rompre la liaison de schéma), de modification des fonctions table inline et d'exécution répétée d'instructions select sur la table cible. Il est fortement recommandé de limiter les autorisations au strict nécessaire et de surveiller toute activité suspecte, telle que la modification permanente des stratégies et des fonctions table inline associées à la sécurité au niveau des lignes.  
  
 **Requêtes élaborées avec soin :** il est possible de provoquer des fuites d’informations via l’utilisation de requêtes élaborées avec soin. Par exemple, `SELECT 1/(SALARY-100000) FROM PAYROLL WHERE NAME='John Doe'` permettrait à un utilisateur malveillant de savoir que le salaire de John Doe est 100 000 $. Même s'il existe un prédicat de sécurité en vigueur pour empêcher un utilisateur malveillant d'interroger directement le salaire d'autres personnes, l'utilisateur peut déterminer si la requête retourne une exception de division par zéro.  
   
  
##  <a name="Limitations"></a> Compatibilité entre fonctionnalités  
 En général, la sécurité au niveau des lignes fonctionne comme prévu pour les fonctionnalités. Il existe cependant quelques exceptions. Cette section contient plusieurs remarques et avertissements concernant l’utilisation de la sécurité au niveau des lignes avec certaines autres fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **DBCC SHOW_STATISTICS** fournit des statistiques sur des données non filtrées et peut donc entraîner des fuites d’informations qui sont autrement protégées par une stratégie de sécurité. C’est pourquoi, pour afficher un objet de statistiques pour une table faisant l’objet d’une politique de sécurité au niveau des lignes, l’utilisateur doit être propriétaire de la table ou membre du rôle serveur fixe sysadmin, du rôle de base de données fixe db_owner ou du rôle de base de données fixe db_ddladmin.  
  
-   **Filestream** La sécurité au niveau des lignes est incompatible avec Filestream.  
  
-   **Polybase** La sécurité au niveau des lignes est incompatible avec Polybase.  
  
-   **Tables optimisées en mémoire**La fonction table incluse utilisée comme prédicat de sécurité sur une table optimisée en mémoire doit être définie avec l’option `WITH NATIVE_COMPILATION` . Avec cette option, les fonctionnalités de langue non prises en charge par les tables optimisées en mémoire sont interdites, et l’erreur appropriée est au moment de la création. Pour plus d’informations, consultez la section **Sécurité au niveau des lignes dans les tables optimisées en mémoire** dans [Introduction aux tables optimisées en mémoire](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
-   **Vues indexées** En général, des stratégies de sécurité peuvent être créées sur des vues, et des vues créées sur des tables liées par des stratégies de sécurité. En revanche, des vues indexées ne peuvent pas être créées sur des tables ayant une stratégie de sécurité, car les recherches de ligne via l’index contourneraient la stratégie.  
  
-   **Capture des modifications de données** La capture des modifications de données peut entraîner une fuite de lignes entières qui devraient être filtrées pour les membres de **db_owner** ou les utilisateurs membres du rôle de régulation spécifié, lorsque la capture de données modifiées est activée pour une table (remarque : vous pouvez définir explicitement cela sur **NULL** pour permettre à tous les utilisateurs d’accéder aux modifications de données). En effet, **db_owner** et les membres de ce rôle de régulation peuvent voir toutes les modifications de données sur une table, même si une stratégie de sécurité s’applique à celle-ci.  
  
-   **Suivi des modifications** Le suivi des modifications peut entraîner une fuite de la clé primaire de lignes qui devraient être filtrées pour les utilisateurs disposant d’autorisations **SELECT** et **VIEW CHANGE TRACKING** . La fuite n’a pas trait aux valeurs de données réelles, mais uniquement à la survenance d’une mise à jour, d’une insertion ou d’une suppression dans une colonne A pour la ligne contenant une clé primaire B. Cela peut être problématique si la clé primaire contient un élément confidentiel tel un numéro de sécurité sociale. Toutefois, dans la pratique, cette fonction **CHANGETABLE** est presque toujours jointe à la table d’origine afin d’obtenir les données les plus récentes.  
  
-   **Recherche en texte intégral** Un gain de performances est attendu pour des requêtes utilisant les fonctions de recherche en texte intégral et de recherche sémantique ci-après, en raison d’une jointure supplémentaire introduite pour appliquer la sécurité au niveau des lignes et éviter la fuite les clés primaires de lignes qui devraient être filtrées : **CONTAINSTABLE**, **FREETEXTTABLE**, semantickeyphrasetable, semanticsimilaritydetailstable, semanticsimilaritytable.  
  
-   **Index columnstore** La sécurité au niveau des lignes est compatible avec les index columnstore tant cluster que non cluster. Toutefois, étant donné que la sécurité au niveau des lignes s’applique à une fonction, il est possible que l’optimiseur puisse modifier le plan de requête de façon à ce qu’il n’utilise pas le mode batch.  
  
-   **Vues partitionnées** Les prédicats BLOCK ne peuvent pas être définis sur des vues partitionnées, et celles-ci ne peuvent pas être créées sur des tables qui utilisent des prédicats BLOCK. Les prédicats de filtre sont compatibles avec les vues partitionnées.  
  
-   Les**tables temporelles** sont compatibles avec la fonction de sécurité au niveau des lignes. Toutefois, les prédicats de sécurité sur la table actuelle ne sont pas automatiquement répliqués dans la table de l’historique. Pour appliquer une stratégie de sécurité aux tables actuelle et de l’historique, vous devez ajouter un prédicat de sécurité à chaque table.  
  
  
##  <a name="CodeExamples"></a> Exemples  
  
###  <a name="Typical"></a> A. Scénario pour les utilisateurs qui s’authentifient auprès de la base de données  
 Ce bref exemple crée trois utilisateurs, crée et remplit une table de 6 lignes, puis crée une fonction table inline et une stratégie de sécurité pour la table. L'exemple montre comment les instructions select sont filtrées pour les différents utilisateurs.  
  
 Créez trois comptes d'utilisateur qui illustrent différentes fonctionnalités d'accès.  
  
```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER Sales1 WITHOUT LOGIN;  
CREATE USER Sales2 WITHOUT LOGIN;  
```  
  
 Créez une table simple pour stocker des données.  
  
```  
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```  
  
 Remplissez la table avec 6 lignes de données, en affichant 3 commandes pour chaque représentant commercial.  
  
```  
INSERT Sales VALUES   
(1, 'Sales1', 'Valve', 5),   
(2, 'Sales1', 'Wheel', 2),   
(3, 'Sales1', 'Valve', 4),  
(4, 'Sales2', 'Bracket', 2),   
(5, 'Sales2', 'Wheel', 5),   
(6, 'Sales2', 'Seat', 5);  
-- View the 6 rows in the table  
SELECT * FROM Sales;  
```  
  
 Accordez l'accès en lecture sur la table à chaque utilisateur.  
  
```  
GRANT SELECT ON Sales TO Manager;  
GRANT SELECT ON Sales TO Sales1;  
GRANT SELECT ON Sales TO Sales2;  
```  
  
 Créez un schéma et une fonction table inline. La fonction renvoie 1 lorsqu'une ligne de la colonne SalesRep est identique à l'utilisateur exécutant la requête (`@SalesRep = USER_NAME()`) ou si l'utilisateur exécutant la requête est l'utilisateur Manager (`USER_NAME() = 'Manager'`).  
  
```  
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result   
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```  
  
 Créez une stratégie de sécurité en ajoutant la fonction comme prédicat de filtre. L'état doit être défini sur ON pour activer la stratégie.  
  
```  
CREATE SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)   
ON dbo.Sales  
WITH (STATE = ON);  
```  
  
 Maintenant testez le prédicat de filtrage, tel que sélectionné à partir de la table Sales pour chaque utilisateur.  
  
```  
EXECUTE AS USER = 'Sales1';  
SELECT * FROM Sales;   
REVERT;  
  
EXECUTE AS USER = 'Sales2';  
SELECT * FROM Sales;   
REVERT;  
  
EXECUTE AS USER = 'Manager';  
SELECT * FROM Sales;   
REVERT;  
```  
  
 L'utilisateur Manager doit afficher l'ensemble des 6 lignes. Les utilisateurs Sales1 et Sales2 doivent voir uniquement leurs propres ventes.  
  
 Modifiez la stratégie de sécurité pour désactiver la stratégie.  
  
```  
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```  
  
 Les utilisateurs Sales1 et Sales2 peuvent maintenant voir l'ensemble des 6 lignes.  
  
  
###  <a name="MidTier"></a> B. Scénario pour les utilisateurs qui se connectent à la base de données via une application intermédiaire  
 Cet exemple montre comment une application de couche intermédiaire peut implémenter le filtrage des connexions, lorsque les utilisateurs (ou locataires) d'application partagent le même utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (l'application). L’application définit l’ID d’utilisateur d’application actuel dans [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md) après la connexion à la base de données, puis les stratégies de sécurité filtrent en toute transparence les lignes qui ne devraient pas être visibles par cet ID, et empêchent l’utilisateur d’insérer des lignes pour l’ID d’utilisateur incorrect. Aucune autre modification de l’application n’est nécessaire.  
  
 Créez une table simple pour stocker des données.  
  
```  
CREATE TABLE Sales (  
    OrderId int,  
    AppUserId int,  
    Product varchar(10),  
    Qty int  
);  
```  
  
 Remplissez la table avec 6 lignes de données, en affichant 3 commandes pour chaque utilisateur de l'application.  
  
```  
INSERT Sales VALUES   
    (1, 1, 'Valve', 5),   
    (2, 1, 'Wheel', 2),   
    (3, 1, 'Valve', 4),  
    (4, 2, 'Bracket', 2),   
    (5, 2, 'Wheel', 5),   
    (6, 2, 'Seat', 5);  
```  
  
 Créez un utilisateur à faibles privilèges que l’application utilisera pour se connecter.  
  
```  
-- Without login only for demo  
CREATE USER AppUser WITHOUT LOGIN;   
GRANT SELECT, INSERT, UPDATE, DELETE ON Sales TO AppUser;  
  
-- Never allow updates on this column  
DENY UPDATE ON Sales(AppUserId) TO AppUser;  
```  
  
 Créez un schéma et une fonction de prédicat qui utilisent l’ID d’utilisateur de l’application stocké dans **SESSION_CONTEXT** pour filtrer les lignes.  
  
```  
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@AppUserId int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result  
    WHERE  
        DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('AppUser')    
        AND CAST(SESSION_CONTEXT(N'UserId') AS int) = @AppUserId;   
GO  
```  
  
 Créez une stratégie de sécurité qui ajoute cette fonction en tant que prédicat de filtre et prédicat BLOCK sur la table `Sales`. Le prédicat BLOCK a uniquement besoin de **AFTER INSERT**, car **BEFORE UPDATE** et **BEFORE DELETE** sont déjà filtrés, et **AFTER UPDATE** n’est pas nécessaire puisque la colonne `AppUserId` ne peut pas être mise à jour avec d’autres valeurs, en raison de l’autorisation définie précédemment.  
  
```  
CREATE SECURITY POLICY Security.SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(AppUserId)   
        ON dbo.Sales,  
    ADD BLOCK PREDICATE Security.fn_securitypredicate(AppUserId)   
        ON dbo.Sales AFTER INSERT   
    WITH (STATE = ON);  
```  
  
 À présent, nous pouvons simuler le filtrage des connexions en opérant une sélection dans la table `Sales` , après avoir défini différents ID d’utilisateur dans **SESSION_CONTEXT**. Dans la pratique, l’application est chargée de définir l’ID d’utilisateur actuel dans **SESSION_CONTEXT** après l’ouverture d’une connexion.  
  
```  
EXECUTE AS USER = 'AppUser';  
EXEC sp_set_session_context @key=N'UserId', @value=1;  
SELECT * FROM Sales;  
GO  
  
--  Note: @read_only prevents the value from changing again   
--  until the connection is closed (returned to the connection pool)  
EXEC sp_set_session_context @key=N'UserId', @value=2, @read_only=1;   
  
SELECT * FROM Sales;  
GO  
  
INSERT INTO Sales VALUES (7, 1, 'Seat', 12); -- error: blocked from inserting row for the wrong user ID  
GO  
  
REVERT;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)   
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)   
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)   
 [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)  
  
  
