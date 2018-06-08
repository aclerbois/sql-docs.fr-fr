---
title: Niveau 2 Interface conformité | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- interface conformance levels [ODBC]
- level 2 interface conformance levels [ODBC]
- conformance levels [ODBC], interface
ms.assetid: 2dc87840-f2fe-43dd-9d7b-bd95523081d9
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ff0d3f86d4e83c842b6477d6d80b5b74c1fbfc58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="level-2-interface-conformance"></a>Conformité d’Interface de niveau 2
Niveau de conformité de l’interface de niveau 2 inclut les fonctionnalités de conformité au niveau de l’interface de niveau 1 ainsi que les fonctionnalités suivantes :  
  
|||  
|-|-|  
|201|Utilisez des noms en trois parties des tables de base de données et des vues. (Pour plus d’informations, voir l’en deux parties d’affectation de noms prise en charge de fonctionnalité de 101 dans [mise en conformité de niveau 1 Interface](../../../odbc/reference/develop-app/level-1-interface-conformance.md).)|  
|202|Décrire les paramètres dynamiques, en appelant **SQLDescribeParam**.|  
|203|Utiliser non seulement les paramètres d’entrée mais également des paramètres d’entrée/sortie et de sortie et les valeurs de résultat des procédures stockées.|  
|204|Utiliser des signets, y compris la récupération des signets, en appelant **SQLDescribeCol** et **SQLColAttribute** sur la colonne numéro 0 ; l’extraction basé sur un signet, en appelant **SQLFetchScroll** avec la *FetchOrientation* à l’argument SQL_FETCH_BOOKMARK ; et la mise à jour, supprimer et à extraire par les opérations de signet, en appelant **SQLBulkOperations** avec la *opération* argument défini à SQL_UPDATE_BY_BOOKMARK, SQL_DELETE_BY_BOOKMARK ou SQL_FETCH_BY_BOOKMARK.|  
|205|Récupérer des informations avancées sur le dictionnaire de données, en appelant **SQLColumnPrivileges**, **SQLForeignKeys**, et **SQLTablePrivileges**.|  
|206|Utiliser les fonctions ODBC au lieu d’instructions SQL pour effectuer des opérations de base de données supplémentaires, en appelant **SQLBulkOperations** avec SQL_ADD, ou **SQLSetPos** avec SQL_DELETE ou SQL_UPDATE. (Prise en charge pour les appels à **SQLSetPos** avec la *LockType* argument a la valeur SQL_LOCK_EXCLUSIVE ou SQL_LOCK_UNLOCK ne fait pas partie des niveaux de conformité, mais est une fonctionnalité facultative.)|  
|207|Activer l’exécution asynchrone de fonctions ODBC pour les instructions individuelles spécifiées.|  
|208|Obtenir la colonne d’identification de ligne SQL_ROWVER de tables, en appelant **SQLSpecialColumns**. (Pour plus d’informations, consultez la prise en charge pour **SQLSpecialColumns** avec la *IdentifierType* argument a la valeur SQL_BEST_ROWID en tant que fonction 20 dans [Core Interface conformité](../../../odbc/reference/develop-app/core-interface-conformance.md).)|  
|209|Définissez l’attribut d’instruction SQL_ATTR_CONCURRENCY au moins une valeur autre que SQL_CONCUR_READ_ONLY.|  
|210|Capacité de la demande de connexion de délai d’attente et des requêtes SQL (SQL_ATTR_LOGIN_TIMEOUT et SQL_ATTR_QUERY_TIMEOUT).|  
|211|La possibilité de modifier le niveau d’isolement par défaut ; la possibilité d’exécuter des transactions avec niveau d’isolement « sérialisable ».|
