---
title: "Exécution de procédures stockées | Documents Microsoft"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: 
ms.component: native-client-odbc-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology: docset-sql-devref
ms.tgt_pltfrm: 
ms.topic: reference
helpviewer_keywords:
- ODBC, stored procedures
- stored procedures [ODBC], running
- SQL Server Native Client ODBC driver, stored procedures
- stored procedures [ODBC], executing
ms.assetid: 866b6dd3-2acd-4dfb-aeca-a0352b2d4c6a
caps.latest.revision: "36"
author: JennieHubbard
ms.author: jhubbard
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 6fe693f482620618e459483efa8eb85b55e0e544
ms.sourcegitcommit: 44cd5c651488b5296fb679f6d43f50d068339a27
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2017
---
# <a name="running-stored-procedures"></a>Exécution des procédures stockées
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Une procédure stockée est un objet exécutable stocké dans une base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge :  
  
-   Procédures stockées :  
  
     une ou plusieurs instructions SQL précompilées en une seule procédure exécutable.  
  
-   Les procédures stockées étendues :  
  
     Les DLL C ou C++ écrites dans l'API SQL Server Open Data Services pour les procédures stockées étendues. L'API Open Data Services étend les fonctions des procédures stockées pour inclure un code C ou C++.  
  
 Lorsque vous exécutez des instructions, l'appel d'une procédure stockée sur la source de données (au lieu d'exécuter ou de préparer directement une instruction dans l'application cliente) peut fournir les éléments suivants :  
  
-   Performances accrues  
  
     Les instructions SQL sont analysées et compilées lorsque les procédures sont créées. Cette charge est ensuite enregistrée lorsque les procédures sont exécutées.  
  
-   Charge réseau réduite  
  
     L'exécution d'une procédure au lieu d'envoyer des requêtes complexes à travers le réseau peut réduire le trafic réseau. Si une application ODBC utilise la syntaxe ODBC {CALL} pour exécuter une procédure stockée, le pilote ODBC procède à des optimisations supplémentaires qui éliminent le besoin de convertir les données des paramètres.  
  
-   Cohérence supérieure  
  
     Si les règles d'une organisation sont implémentées dans une ressource centrale, telle qu'une procédure stockée, elles peuvent être codées, testées et déboguées une fois. Les programmeurs individuels peuvent utiliser ensuite les procédures stockées testées au lieu de développer leurs propres implémentations.  
  
-   Exactitude supérieure  
  
     Comme les procédures stockées sont développées habituellement par des programmeurs expérimentés, elles tendent à être plus efficaces et à comporter moins d'erreurs qu'un code développé plusieurs fois par des programmeurs aux niveaux de compétence variables.  
  
-   Fonctionnalités supplémentaires  
  
     Les procédures stockées étendues peuvent utiliser les fonctionnalités C et C++ non disponibles dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
     Pour obtenir un exemple montrant comment appeler une procédure stockée, consultez [traiter des Codes de retour et les paramètres de sortie &#40; ODBC &#41;](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Appel d’une procédure stockée](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)  
  
-   [Traitement par lot des appels de procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)  
  
-   [Traitement des résultats des procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/processing-stored-procedure-results.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40; ODBC &#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [En cours d’exécution stockées rubriques de procédures &#40; ODBC &#41;](http://msdn.microsoft.com/library/c2220182-a23d-4475-b353-77a77ab613d6)  
  
  