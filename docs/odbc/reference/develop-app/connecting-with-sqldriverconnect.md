---
title: Connexion avec SQLDriverConnect | Documents Microsoft
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
- data sources [ODBC], connection functions
- functions [ODBC], data source or driver connections
- connecting to data source [ODBC], SQLDriverConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], functions
- connecting to driver [ODBC], functions
- SQLDriverConnect function [ODBC], connecting
- connection functions [ODBC]
- ODBC drivers [ODBC], connection functions
ms.assetid: e46e959f-d3c5-4ddb-810a-107bfcb83fd2
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: da68bea5d1cf62effc85911b8d9a4d66568dd823
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-with-sqldriverconnect"></a>Connexion avec SQLDriverConnect
**SQLDriverConnect** est utilisé pour se connecter à une source de données à l’aide d’une chaîne de connexion. **SQLDriverConnect** est utilisé à la place de **SQLConnect** pour les raisons suivantes :  
  
-   Pour laisser l’application à utiliser les informations de connexion spécifiques au pilote.  
  
-   Pour demander que le pilote invite l'utilisateur à fournir des informations sur la connexion.  
  
-   Pour vous connecter sans spécifier une source de données.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Informations de connexion spécifiques du pilote](../../../odbc/reference/develop-app/driver-specific-connection-information.md)  
  
-   [Demande des informations de connexion à l’utilisateur](../../../odbc/reference/develop-app/prompting-the-user-for-connection-information.md)  
  
-   [Connexion à l’aide de sources de données de fichier](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)  
  
-   [Connexion directe à des pilotes](../../../odbc/reference/develop-app/connecting-directly-to-drivers.md)
