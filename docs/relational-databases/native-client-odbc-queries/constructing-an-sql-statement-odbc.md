---
title: Construction d’une instruction SQL (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-queries
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, statements
- statements [ODBC], constructing
- ODBC applications, statements
ms.assetid: 0acc71e2-8004-4dd8-8592-05c022bdd692
caps.latest.revision: 33
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4cd6e998bae2dbcc3f09ee7ceac47f756c7685fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="constructing-an-sql-statement-odbc"></a>Construction d'une instruction SQL (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les applications ODBC effectuent la plus grande partie de leur accès aux bases de données en exécutant des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. La forme de ces instructions dépend des spécifications d'application. Les instructions SQL peuvent être construites des manières suivantes :  
  
-   Codées de manière irréversible  
  
     Instructions statiques exécutées par une application en tant que tâche fixe.  
  
-   Construction au moment de l'exécution  
  
     Les instructions SQL construites au moment de l'exécution permettent à l'utilisateur de personnaliser l'instruction en utilisant des clauses courantes telles que SELECT, WHERE et ORDER BY. Cela inclut les requêtes ad hoc entrées par les utilisateurs.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC du Client analyse les instructions SQL uniquement pour la syntaxe ODBC et ISO pas directement pris en charge par le [!INCLUDE[ssDE](../../includes/ssde-md.md)], dont le pilote transforme en [!INCLUDE[tsql](../../includes/tsql-md.md)]. Toute autre syntaxe SQL est passée au [!INCLUDE[ssDE](../../includes/ssde-md.md)] inchangée, où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déterminera s'il s'agit de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide. Cette approche fournit deux avantages :  
  
-   Réduction des coûts  
  
     Les coûts de traitement pour le pilote sont réduits car il doit analyser seulement un petit ensemble de clauses ODBC et ISO.  
  
-   Souplesse  
  
     Les programmeurs peuvent personnaliser la portabilité de leurs applications. Pour améliorer la portabilité contre plusieurs bases de données, utilisez principalement la syntaxe ODBC et ISO. Pour utiliser des améliorations spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] appropriée. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client prend en charge le texte complet [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe pour les applications ODBC peuvent tirer parti de toutes les fonctionnalités dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La liste de colonnes dans une instruction SELECT doit contenir uniquement les colonnes requises pour effectuer la tâche actuelle. Cela réduit non seulement la quantité de données envoyées sur le réseau, mais réduit également l'impact des modifications de base de données sur l'application. Si une application ne fait pas référence à une colonne d'une table, elle n'est pas affectée par les modifications apportées à cette colonne.  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution de requêtes &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
