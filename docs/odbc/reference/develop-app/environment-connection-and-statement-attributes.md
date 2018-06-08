---
title: Environnement, la connexion et les attributs d’instruction | Documents Microsoft
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
- environment attributes [ODBC]
- connection attributes [ODBC]
- statement attributes [ODBC]
ms.assetid: 9e15b276-3b7a-428a-b72f-a3ddfe1ba1ce
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3b8e3ee068d160269336de15ce1ddef3e7c78d58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="environment-connection-and-statement-attributes"></a>Environnement, la connexion et les attributs d’instruction
ODBC définit un nombre d’attributs qui sont associés à des environnements, des connexions ou des instructions.  
  
 Attributs d’environnement affectent tout l’environnement, par exemple si le regroupement de connexions est activé. Attributs d’environnement sont définies avec **SQLSetEnvAttr** et récupéré avec **cas**.  
  
 Attributs de connexion affectent chaque connexion individuellement, comme la durée pendant laquelle un pilote doit attendre lors de la tentative pour vous connecter à une source de données avant d’arriver à expiration. Attributs de connexion sont définis avec **SQLSetConnectAttr** et récupéré avec **SQLGetConnectAttr**. Pour plus d’informations sur les attributs de connexion, consultez [attributs de connexion](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Attributs de l’instruction affectent chaque instruction individuellement, par exemple si une instruction doit être exécutée de façon asynchrone. Attributs d’instruction sont définis avec **SQLSetStmtAttr** et récupéré avec **SQLGetStmtAttr**. Certains attributs de l’instruction sont des attributs en lecture seule et ne peut pas être définies. Par exemple, l’attribut d’instruction SQL_ATTR_ROW_NUMBER, qui est utilisé pour récupérer le numéro de la ligne actuelle dans le curseur, est en lecture seule. Pour plus d’informations sur les attributs d’instruction, consultez [les attributs d’instruction](../../../odbc/reference/develop-app/statement-attributes.md).  
  
 Outre les attributs définis par ODBC, un pilote peut définir sa propre connexion et les attributs d’instruction. Attributs définis par le pilote doivent être enregistrés avec Open Group pour vous assurer que deux fournisseurs de pilote n’affectez pas la même valeur d’entier à des attributs différents propriétaires. Pour plus d’informations, consultez [les Types de données spécifiques au pilote, Types de descripteur, Types d’informations, Types de diagnostics et attributs](../../../odbc/reference/develop-app/driver-specific-data-types-descriptor-information-diagnostic.md).  
  
 Pour obtenir une liste complète des attributs, consultez [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md), [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), et [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md). La plupart des attributs sont également décrits dans la description de la fonction ODBC qu’elles affectent.
