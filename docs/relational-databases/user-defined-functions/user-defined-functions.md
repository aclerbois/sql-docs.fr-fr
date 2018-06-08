---
title: Fonctions définies par l’utilisateur | Microsoft Docs
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: udf
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-udf
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], components
- user-defined functions [SQL Server], about user-defined functions
ms.assetid: d7ddafab-f5a6-44b0-81d5-ba96425aada4
caps.latest.revision: 23
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: cc7cf4ed368b63c7b070873dda3d23fcce909f35
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708997"
---
# <a name="user-defined-functions"></a>Fonctions définies par l'utilisateur
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  À l'instar des fonctions dans les langages de programmation, les fonctions définies par l'utilisateur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont des routines qui acceptent des paramètres, exécutent une action, par exemple un calcul complexe, et retournent le résultat de cette action sous forme de valeur. La valeur retournée peut être une valeur scalaire unique ou un jeu de résultats.  
   
##  <a name="Benefits"></a> Fonctions définies par l’utilisateur  
Pourquoi les utiliser ? 
  
-   Elles permettent d'utiliser la programmation modulaire.  
  
     Vous pouvez créer la fonction une fois, la stocker dans la base de données et l'appeler autant de fois que vous le voulez dans votre programme. Les fonctions définies par l'utilisateur sont modifiables indépendamment du code source du programme.  
  
-   Leur exécution est plus rapide.  
  
     De la même façon que les procédures stockées, les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] définies par l'utilisateur réduisent le coût de compilation du code [!INCLUDE[tsql](../../includes/tsql-md.md)] en mettant en cache les plans pour les réutiliser au cours d'exécutions répétées. La fonction définie par l'utilisateur n'a donc pas besoin d'être réanalysée et réoptimisée à chaque utilisation, ce qui réduit nettement les durées d'exécution.  
  
     Les fonctions CLR offrent des avantages considérables en termes de performance par rapport aux fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour ce qui concerne les tâches de calcul, la manipulation de chaînes et la logique métier. Les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont plus adaptées à une logique intensive d'accès aux données.  
  
-   Elles permettent une réduction du trafic sur le réseau.  
  
     Une opération qui filtre des données en fonction d'une contrainte complexe qui ne peut pas être exprimée dans une même expression scalaire peut être exprimée sous forme de fonction. La fonction peut ensuite être appelée dans la clause WHERE pour réduire le nombre de lignes envoyées aux clients.  
  
> [!NOTE]
> Les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] définies par l'utilisateur figurant dans les requêtes peuvent être exécutées uniquement sur un thread (plan d'exécution en série).  
  
##  <a name="FunctionTypes"></a> Types de fonctions  
**Fonction scalaire**  
 Les fonctions scalaires définies par l'utilisateur retournent une valeur de donnée unique dont le type est défini dans la clause RETURNS. Une fonction scalaire incluse ne contient pas de corps ; la valeur scalaire est le résultat d'une instruction unique. Le corps d'une fonction scalaire à instructions multiples, défini dans un bloc BEGIN...END, contient une série d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui retournent la valeur unique. Le type de retour peut être n’importe quel type de données, sauf **text**, **ntext**, **image**, **cursor**et **timestamp**. 
 **[Exemples.](https://msdn.microsoft.com/library/bb386973(v=vs.110).aspx)**
  
**Fonctions table**  
 Les fonctions table définies par l’utilisateur retournent un type de données **table** . Une fonction table incluse ne contient pas de corps ; la table est le jeu de résultats d'une instruction SELECT unique. **[Exemples.](https://msdn.microsoft.com/library/bb386954(v=vs.110).aspx)**
  
**Fonctions système**  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit de nombreuses fonctions système que vous pouvez utiliser pour effectuer diverses opérations. Elles ne peuvent pas être modifiées. Pour plus d’informations, consultez [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md), [Fonctions stockées système &#40;Transact-SQL&#41;](~/relational-databases/system-functions/system-functions-for-transact-sql.md) et [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
##  <a name="Guidelines"></a> Instructions  
 Les erreurs [!INCLUDE[tsql](../../includes/tsql-md.md)] qui provoquent l’annulation d’une instruction et la poursuite avec l’instruction suivante dans le module (comme les déclencheurs ou les procédures stockées) sont traitées différemment à l’intérieur d’une fonction. Dans les fonctions, ces erreurs provoquent l'arrêt de l'exécution de la fonction, lequel provoque à son tour l'annulation de l'instruction qui a invoqué la fonction.  
  
 Les instructions contenues dans un bloc BEGIN...END ne peuvent pas avoir d'effets secondaires. Les effets secondaires d'une fonction sont toutes les modifications définitives de l'état d'une ressource dont la portée s'étend hors de la fonction, comme la modification d'une table de base de données. Les instructions d'une fonction ne peuvent modifier que les objets locaux de cette fonction, tels les variables ou les curseurs locaux. Les modifications apportées aux tables de base de données, les opérations portant sur des curseurs non locaux par rapport à la fonction, l'envoi de courrier électronique, les tentatives de modification de catalogue et la génération d'un jeu de résultats retourné à l'utilisateur sont autant d'actions qui ne peuvent pas être exécutées dans une fonction.  
  
> [!NOTE]
> Si une instruction CREATE FUNCTION produit des effets secondaires contre des ressources qui n'existent pas lorsque l'instruction CREATE FUNCTION est publiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute l'instruction. Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'exécute pas la fonction lorsqu'elle est appelée.  
  
 Le nombre d'exécutions effectives d'une fonction spécifiée dans une requête peut varier d'un plan d'exécution de l'optimiseur à l'autre. C'est par exemple le cas d'une fonction invoquée par une sous-requête dans une clause WHERE. Le nombre d'exécutions de la sous-requête et de sa fonction peut varier en fonction du chemin d'accès choisi par l'optimiseur.  
  
##  <a name="ValidStatements"></a> Instructions valides dans une fonction  
Les types d'instructions valides dans une fonction sont les suivants :  
  
-   les instructions DECLARE permettant de définir des curseurs et des variables de données locaux de la fonction ;  
  
-   les affectations de valeurs à des objets locaux de la fonction, comme l'attribution de valeurs à des variables locales scalaires ou de table à l'aide de SET ;  
  
-   les opérations de curseur faisant référence à des curseurs locaux déclarés, ouverts, fermés et désalloués dans la fonction. Les instructions FETCH qui retournent des données au client ne sont pas autorisées (seules celles qui affectent des valeurs à des variables locales à l'aide de la clause INTO le sont) ;  
  
-   les instructions de contrôle de flux, à l'exception des instructions TRY...CATCH ;  
  
-   les instructions SELECT contenant des listes de sélection avec des expressions qui affectent des valeurs à des variables locales de la fonction ;  
  
-   les instructions UPDATE, INSERT et DELETE modifiant les variables table locales de la fonction ;  
  
-   les instructions EXECUTE appelant une procédure stockée étendue.  
  
### <a name="built-in-system-functions"></a>Fonctions système intégrées  
 Les fonctions intégrées non déterministes suivantes peuvent être utilisées dans les fonctions Transact-SQL définies par l'utilisateur.  
  
|||  
|-|-|  
|CURRENT_TIMESTAMP|@@MAX_CONNECTIONS|  
|GET_TRANSMISSION_STATUS|@@PACK_RECEIVED|  
|GETDATE|@@PACK_SENT|  
|GETUTCDATE|@@PACKET_ERRORS|  
|@@CONNECTIONS|@@TIMETICKS|  
|@@CPU_BUSY|@@TOTAL_ERRORS|  
|@@DBTS|@@TOTAL_READ|  
|@@IDLE|@@TOTAL_WRITE|  
|@@IO_BUSY||  
  
 Les fonctions intégrées non déterministes suivantes **ne peuvent pas** être utilisées dans les fonctions Transact-SQL définies par l’utilisateur.  
  
|||  
|-|-|  
|NEWID|RAND|  
|NEWSEQUENTIALID|TEXTPTR|  
  
 Pour obtenir la liste des fonctions système intégrées déterministes et non déterministes, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
##  <a name="SchemaBound"></a> Fonctions liées à un schéma  
 L'instruction CREATE FUNCTION prend en charge une clause SCHEMABINDING qui lie la fonction au schéma de tout objet auquel elle fait référence, tel qu'une table, une vue ou une fonction définie par l'utilisateur. Toute tentative de modification (ALTER) ou de suppression (DROP) d'un objet référencé par une fonction liée au schéma est vouée à l'échec.  
  
 Les conditions suivantes doivent être respectées pour que la clause SCHEMABINDING puisse être spécifiée dans [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md) :  
  
-   Toutes les vues et fonctions définies par l'utilisateur référencées par la fonction doivent être liées au schéma.  
  
-   Tous les objets référencés par la fonction doivent figurer dans la même base de données que la fonction. Les objets doivent être référencés à l'aide de noms à une ou deux composantes.  
  
-   Vous devez posséder l'autorisation REFERENCES sur tous les objets (tables, vues et fonctions définies par l'utilisateur) référencés dans la fonction.  
  
 Vous pouvez utiliser l'instruction ALTER FUNCTION pour supprimer la liaison au schéma. Cette instruction doit redéfinir la fonction sans spécifier WITH SCHEMABINDING.  
  
##  <a name="Parameters"></a> Spécification des paramètres  
 Une fonction définie par l'utilisateur accepte ou n'accepte pas de paramètres d'entrée et retourne une valeur scalaire ou une table. Une fonction peut comprendre jusqu'à 1 024 paramètres d'entrée. Lorsqu'un des paramètres de la fonction possède une valeur par défaut, le mot clé DEFAULT doit être spécifié lors de l'appel de la fonction afin d'obtenir la valeur par défaut. Ce comportement est différent de celui des paramètres avec valeurs par défaut des procédures stockées définies par l'utilisateur pour lesquelles l'omission du paramètre implique également la prise en compte de la valeur par défaut. Les fonctions définies par l'utilisateur ne prennent pas en charge les paramètres de sortie.  
  
##  <a name="Tasks"></a> Exemples supplémentaires  
  
|||  
|-|-|  
|**Description de la tâche**|**Rubrique**|  
|Décrit comment créer une fonction Transact-SQL définie par l'utilisateur.|[Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)|  
|Décrit comment créer une fonction CLR.|[Créer des fonctions CLR](../../relational-databases/user-defined-functions/create-clr-functions.md)|  
|Décrit comment créer une fonction d'agrégation définie par l'utilisateur.|[Créer des agrégats définis par l’utilisateur](../../relational-databases/user-defined-functions/create-user-defined-aggregates.md)|  
|Décrit comment modifier une fonction Transact-SQL définie par l'utilisateur.|[Modifier des fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/modify-user-defined-functions.md)|  
|Décrit comment supprimer une fonction définie par l'utilisateur.|[Supprimer des fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/delete-user-defined-functions.md)|  
|Décrit comment exécuter une fonction définie par l'utilisateur.|[Exécuter des fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/execute-user-defined-functions.md)|  
|Décrit comment renommer une fonction définie par l'utilisateur.|[Renommer des fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/rename-user-defined-functions.md)|  
|Décrit comment afficher la définition d'une fonction définie par l'utilisateur.|[Afficher des fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/view-user-defined-functions.md)|  
  
  
