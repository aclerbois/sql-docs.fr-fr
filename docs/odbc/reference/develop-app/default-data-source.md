---
title: Valeur par défaut de la Source de données | Documents Microsoft
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
- connecting to data source [ODBC], default data source
- functions [ODBC], data source or driver connections
- data sources [ODBC], default
- default data source [ODBC]
- connecting to data source [ODBC], functions
- connecting to driver [ODBC], default data source
- connecting to driver [ODBC], functions
- connection functions [ODBC]
- ODBC drivers [ODBC], connection functions
ms.assetid: dd473cc6-f051-4aa0-ab14-3dd1b37fe99e
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8b756f9b553c622028266d1fc591596bf58ddf45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="default-data-source"></a>Source de données par défaut
Le pilote peut sélectionner une source de données, appelée la source de données par défaut, dans certains cas où l’application ne spécifie pas explicitement une :  
  
-   Dans un appel à **SQLConnect** où le *nom_serveur* argument est une chaîne de longueur nulle, un pointeur null ou par défaut.  
  
-   Dans un appel à **SQLDriverConnect** où *InConnectionString* soit spécifie **DSN**= valeur par défaut ou spécifie avec la **DSN** mot-clé une source de données qui ne figure pas dans les informations système.  
  
 Elle est définie par le pilote comment la source de données par défaut est spécifiée. Cela peut impliquer l’action d’administration et peut dépendre de l’utilisateur.
