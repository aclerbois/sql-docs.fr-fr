---
title: Accès aux tables optimisées en mémoire à l’aide du Transact-SQL interprété | Microsoft Docs
ms.custom: ''
ms.date: 05/31/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: ''
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 92a44d4d-0e53-4fb0-b890-de264c65c95a
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 554b1c2240d06fc0c9180594bc00ecc6473c9bd5
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="accessing-memory-optimized-tables-using-interpreted-transact-sql"></a>Accès aux tables optimisées en mémoire à l’aide du Transact-SQL interprété
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

 À quelques exceptions près, vous accédez aux tables mémoire optimisées à l'aide d'une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] ou d'une opération DML (sélectionner, insérer, mettre à jour ou supprimer), de lots ad hoc et de modules SQL, tels que des procédures stockées, des fonctions table, des déclencheurs et des vues.  
  
Le [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété fait référence à des lots ou des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] autres qu'une procédure stockée compilée en mode natif. L'accès par [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété aux tables mémoire optimisées est appelé un accès d'interopérabilité.  

À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], les requêtes interprétées dans [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent analyser en parallèle des tables optimisées en mémoire, au lieu de les analyser en série.

Les tables mémoire optimisées sont également accessibles à l'aide d'une procédure stockée compilée en mode natif. Les procédures stockées compilées en mode natif sont recommandées pour les opérations OLTP critiques pour les performances.  
  
L'accès en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété est recommandé dans les scénarios suivants :  
  
- Requêtes ad hoc et tâches d'administration.  
  
- Requêtes de rapports qui utilisent généralement des constructions non disponibles dans les procédures stockées compilées en mode natif (telles que les fonctions de *fenêtre* , parfois désignées sous le terme de fonctions [OVER](../../t-sql/queries/select-over-clause-transact-sql.md) ).  
  
- Pour migrer des parties de votre application qui ont un impact sur les performances vers des tables mémoire optimisées, avec des modifications de code minimes (ou sans aucune modification du code). Potentiellement, vous constaterez de meilleures performances en migrant des tables. Si vous migrez ensuite des procédures stockées vers des procédures stockées compilées en mode natif, vous constaterez des améliorations de performances accrues.  
  
- Lorsqu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] n'est pas disponible pour les procédures stockées compilées en mode natif.  
  
Cependant, les constructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes ne sont pas prises en charge dans les procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] interprétées qui accèdent aux données dans une table mémoire optimisée.  
  
|Domaine|Non pris en charge|  
|----------|-----------------|  
|Accès aux tables|TRUNCATE TABLE<br /><br /> MERGE (table mémoire optimisée comme cible).<br /><br /> Curseurs DYNAMIC et KEYSET (dégradés automatiquement en STATIC).<br /><br /> Accédez aux modules CLR à l'aide de la connexion contextuelle.<br /><br /> Référencement d'une table mémoire optimisée à partir d'une vue indexée.|  
|Bases de données croisées|Requêtes de bases de données croisées<br /><br /> Transactions de bases de données croisées<br /><br /> Serveurs liés|  
  
## <a name="table-hints"></a>Indicateurs de table

Pour plus d'informations sur les indicateurs de table, consultez. [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md). L'isolement SNAPSHOT a été ajouté pour prendre en charge [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
Les indicateurs de table suivants ne sont pas pris en charge lors de l'accès à une table de mémoire optimisée à l'aide du [!INCLUDE[tsql](../../includes/tsql-md.md)]interprété.  

  
|||||  
|-|-|-|-|  
|HOLDLOCK|IGNORE_CONSTRAINTS|IGNORE_TRIGGERS|NOWAIT|  
|PAGLOCK|READCOMMITTED|READCOMMITTEDLOCK|READPAST|  
|READUNCOMMITTED|ROWLOCK|SPATIAL_WINDOW_MAX_CELLS = *integer*|TABLOCK|  
|TABLOCKXX|UPDLOCK|XLOCK||  
  

Lorsque vous accédez à une table optimisée en mémoire à partir d’une transaction explicite ou implicite à l’aide du [!INCLUDE[tsql](../../includes/tsql-md.md)]interprété, vous devez au moins effectuer l’une des opérations suivantes :  
  
- Spécifiez un [indicateur de table de niveau d’isolation](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md) tel que SNAPSHOT, REPEATABLEREAD ou SERIALIZABLE.  
  
- Définissez l’option de base de données [MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT](../../t-sql/statements/alter-database-transact-sql-set-options.md) sur ON.  
  
Un indicateur de table de niveau d’isolation n’est pas obligatoire pour les tables optimisées en mémoire accessibles par des requêtes qui s’exécutent en [mode de validation automatique](http://msdn.microsoft.com/en-us/c8de5b60-d147-492d-b601-2eeae8511d00).  
  
## <a name="see-also"></a> Voir aussi

[Prise en charge d'OLTP en mémoire par Transact-SQL](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   

[Migration vers OLTP en mémoire](../../relational-databases/in-memory-oltp/migrating-to-in-memory-oltp.md)  

