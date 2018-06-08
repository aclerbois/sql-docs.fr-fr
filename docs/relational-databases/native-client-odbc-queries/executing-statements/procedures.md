---
title: Procédures | Documents Microsoft
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
- stored procedures [ODBC]
- stored procedures [ODBC], about ODBC stored procedures
- ODBC applications, statements
- statements [ODBC], stored procedures
- ODBC applications, stored procedures
ms.assetid: c64d5f3a-376b-48ef-84f3-b6148ac8600a
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3ae7aedfbbfc37a1ec4bfc1eb4b547b78e531df1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="procedures"></a>Procédures
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Une procédure stockée est un objet exécutable précompilé qui contient une ou plusieurs instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Les procédures stockées peuvent posséder des paramètres d'entrée et de sortie et peuvent également émettre un code de retour de type entier. Une application peut énumérer les procédures stockées disponibles en utilisant des fonctions de catalogue.  
  
 Les applications ODBC qui ciblent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent utiliser uniquement l'exécution directe pour appeler une procédure stockée. Lorsqu’il est connecté aux versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client implémente [fonction SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360) en créant une procédure stockée temporaire, qui est ensuite appelée **SQLExecute**. Il ajoute une charge d’avoir **SQLPrepare** créer une procédure stockée temporaire qui appelle la procédure stockée cible plutôt que directement de l’exécution de la cible de la procédure stockée. Même lors d'une connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la préparation d'un appel requiert une boucle réseau supplémentaire et l'établissement d'un plan d'exécution qui appelle simplement le plan d'exécution de procédure stockée.  
  
 Les applications ODBC doivent utiliser la syntaxe ODBC CALL lors de l'exécution d'une procédure stockée. Le pilote est optimisé pour utiliser un mécanisme d'appel de procédure distante pour appeler la procédure lorsque la syntaxe ODBC CALL est utilisée. Ce mécanisme est plus efficace que celui utilisé pour envoyer une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] EXECUTE au serveur.  
  
 Pour plus d’informations, consultez [en cours d’exécution des procédures stockées](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md).  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution d’instructions & #40 ; ODBC & #41 ;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
  
