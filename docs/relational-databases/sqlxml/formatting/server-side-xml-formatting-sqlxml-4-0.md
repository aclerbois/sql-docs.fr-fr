---
title: (SQLXML 4.0) de la mise en forme XML côté serveur | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- FOR XML clause, formatting
- server-side XML formatting
ms.assetid: ae9ea068-0857-4505-a3b2-f53d256b644c
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: dd37f50bb838717d9a245b038f624dee57bbf7d4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-side-xml-formatting-sqlxml-40"></a>Mise en forme XML côté serveur (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique fournit des informations sur la mise en forme des documents XML sur le côté serveur des ensembles de lignes générés par les requêtes exécutées dans une base de données dans Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez stocker et extraire des documents XML vers et depuis des tables de base de données. Pour extraire un document XML, utilisez l'extension de requête FOR XML dans une requête SELECT.  
  
 Par exemple, supposons une application cliente exécute une commande sur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui se compose des éléments suivants [!INCLUDE[tsql](../../../includes/tsql-md.md)] requête :  
  
```  
SELECT FirstName, LastName  
FROM   Person.Contact  
FOR XML AUTO  
```  
  
 Le serveur exécute la requête en deux étapes. D'abord, le serveur exécute cette instruction SELECT :  
  
```  
SELECT FirstName, LastName  
FROM   Person.Contact  
```  
  
 Le serveur applique ensuite la transformation FOR XML à l'ensemble de lignes généré. Les données XML obtenues sont alors transmises au client dans un ensemble de lignes d'une colonne. Dans cette documentation, ce processus est connu sous le nom de mise en forme XML côté serveur.  
  
 Côté serveur, vous pouvez spécifier les modes suivants avec une clause FOR XML :  
  
-   RAW  
  
-   AUTO  
  
-   EXPLICIT  
  
 Pour plus d’informations sur la clause FOR XML, consultez [construction de code XML à l’aide de XML](../../../relational-databases/xml/for-xml-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Architecture de mise en forme côté Client et côté serveur XML &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/formatting/architecture-of-client-side-and-server-side-xml-formatting-sqlxml-4-0.md)   
 [La mise en forme XML côté client &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/formatting/client-side-xml-formatting-sqlxml-4-0.md)   
 [FOR XML &#40;SQL Server&#41;](../../../relational-databases/xml/for-xml-sql-server.md)  
  
  
