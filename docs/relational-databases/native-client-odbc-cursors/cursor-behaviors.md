---
title: Comportements de curseur | Documents Microsoft
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-cursors
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [SQL Server]
- SQL Server Native Client ODBC driver, cursors
- version-based optimistic concurrency
- cursors [ODBC], cursor behaviors
- ODBC applications, cursors
- SQL_ATTR_CURSOR_SENSITIVITY option
- SQL_ATTR_CURSOR_SCROLLABLE option
- sensitivity behavior of cursor
- ODBC cursors, cursor behaviors
ms.assetid: 742ddcd2-232b-4aa1-9212-027df120ad35
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3bb10bf9bf855b7c343b07c597dc0436015344be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cursor-behaviors"></a>Comportements de curseur
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  ODBC prend en charge les options ISO pour spécifier le comportement des curseurs en termes de capacité de défilement et de sensibilité. Ces comportements sont spécifiés en définissant les options SQL_ATTR_CURSOR_SCROLLABLE et SQL_ATTR_CURSOR_SENSITIVITY lors d’un appel à [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client implémente ces options en demandant des curseurs côté serveur avec les caractéristiques suivantes.  
  
|Paramètres de comportement du curseur|Caractéristiques du curseur côté serveur demandées|  
|------------------------------|---------------------------------------------|  
|SQL_SCROLLABLE et SQL_SENSITIVE|Curseur de jeu de clés et accès concurrentiel optimiste basé sur la version|  
|SQL_SCROLLABLE et SQL_INSENSITIVE|Curseur statique et concurrence en lecture seule|  
|SQL_SCROLLABLE et SQL_UNSPECIFIED|Curseur statique et concurrence en lecture seule|  
|SQL_NONSCROLLABLE et SQL_SENSITIVE|Curseur avant uniquement et accès concurrentiel optimiste basé sur la version|  
|SQL_NONSCROLLABLE et SQL_INSENSITIVE|Jeu de résultats par défaut (avant uniquement, en lecture seule)|  
|SQL_NONSCROLLABLE et SQL_UNSPECIFIED|Jeu de résultats par défaut (avant uniquement, en lecture seule)|  
  
 Accès concurrentiel optimiste basé sur une version nécessite un **timestamp** colonne dans la table sous-jacente. Si le contrôle d’accès concurrentiel optimiste basé sur la version est demandé sur une table qui n’a pas un **timestamp** colonne, le serveur utilise en fonction des valeurs d’accès concurrentiel optimiste.  
  
## <a name="scrollability"></a>Capacité de défilement  
 Quand SQL_ATTR_CURSOR_SCROLLABLE a la valeur SQL_SCROLLABLE, le curseur prend en charge les différentes valeurs pour le *FetchOrientation* paramètre de [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). Quand SQL_ATTR_CURSOR_SCROLLABLE a la valeur SQL_NONSCROLLABLE, le curseur prend uniquement en charge un *FetchOrientation* valeur de SQL_FETCH_NEXT.  
  
## <a name="sensitivity"></a>Sensibilité  
 Quand SQL_ATTR_CURSOR_SENSITIVITY est défini avec la valeur SQL_SENSITIVE, le curseur reflète les modifications des données effectuées par l'utilisateur en cours ou validées par d'autres utilisateurs. Quand SQL_ATTR_CURSOR_SENSITIVITY est défini avec la valeur SQL_INSENSITIVE, le curseur ne reflète pas les modifications des données.  
  
## <a name="see-also"></a>Voir aussi  
 [L’utilisation de curseurs (ODBC)](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md) [propriétés du curseur](properties/cursor-properties.md) 
  
  
