---
title: Caractéristiques du curseur et le Type de curseur | Documents Microsoft
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
- scrollable cursors [ODBC]
- cursors [ODBC], scrollable
- cursors [ODBC], creating
ms.assetid: 6f67edd2-ae71-4ca0-9b2d-abf4c20dc17b
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b73c8d966f0974b09b672e497238f122bf1b13da
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cursor-characteristics-and-cursor-type"></a>Caractéristiques du curseur et le Type de curseur
Une application peut spécifier les caractéristiques d’un curseur au lieu de spécifier le type de curseur (avant uniquement, statique, commandé par keyset ou dynamique). Pour ce faire, l’application sélectionne capacité de défilement du curseur (en définissant l’attribut d’instruction SQL_ATTR_CURSOR_SCROLLABLE) et la sensibilité (en définissant l’attribut d’instruction SQL_ATTR_CURSOR_SENSITIVITY) avant d’ouvrir le curseur sur le handle d’instruction. Le pilote choisit ensuite le type de curseur qui fournit plus efficacement les caractéristiques de l’application demandée.  
  
 Chaque fois qu’une application définit tous les attributs d’instruction SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_SCROLLABLE, SQL_ATTR_CURSOR_SENSITIVITY ou SQL_ATTR_CURSOR_TYPE, le pilote rend toute modification nécessaire pour les autres attributs d’instruction dans cet ensemble de quatre attributs afin que leurs valeurs restent cohérentes. Par conséquent, lors de l’application spécifie une caractéristique de curseur, le pilote peut modifier l’attribut qui indique le type de curseur en fonction de cette sélection implicite ; Lorsque l’application spécifie un type, le pilote peut modifier tous les autres attributs pour être cohérent avec les caractéristiques du type sélectionné. Pour plus d’informations sur ces attributs d’instruction, consultez la [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md) description de fonction.  
  
 Une application qui définit des attributs pour spécifier un type de curseur et caractéristiques du curseur de l’instruction s’exécute le risque d’obtention d’un curseur qui n’est pas disponible sur le pilote de répondre aux besoins de l’application la méthode la plus efficace.  
  
 Le paramètre implicit d’attributs d’instruction est définies par le pilote, sauf qu’elle doit suivre ces règles :  
  
-   Les curseurs avant uniquement ne sont jamais déroulant ; consultez la définition de SQL_ATTR_CURSOR_SCROLLABLE dans [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
-   Les curseurs insensibles ne sont jamais être mise à jour (et par conséquent, leur accès concurrentiel est en lecture seule) ; Il est basé sur leur définition des curseurs, non-respect de la norme ISO SQL.  
  
 Par conséquent, le paramètre implicit d’attributs d’instruction se produit dans les cas décrits dans le tableau suivant.  
  
|Application affecte à un attribut|Les autres attributs définis implicitement|  
|-----------------------------------|-------------------------------------|  
|SQL_ATTR_CONCURRENCY à SQL_CONCUR_READ_ONLY|SQL_ATTR_CURSOR_SENSITIVITY avec la valeur SQL_INSENSITIVE.|  
|SQL_ATTR_CONCURRENCY aux SQL_CONCUR_LOCK, SQL_CONCUR_ROWVER, SQL_CONCUR_VALUES|SQL_ATTR_CURSOR_SENSITIVITY SQL_UNSPECIFIED ou SQL_SENSITIVE, tel que défini par le pilote. Il ne peut jamais être défini avec la valeur SQL_INSENSITIVE, étant donné que le non-respect de curseurs est toujours en lecture seule.|  
|SQL_ATTR_CURSOR_SCROLLABLE SQL_NONSCROLLABLE|SQL_ATTR_CURSOR_TYPE à SQL_CURSOR_FORWARD_ONLY|  
|SQL_ATTR_CURSOR_SCROLLABLE SQL_SCROLLABLE|SQL_ATTR_CURSOR_TYPE SQL_CURSOR_STATIC, SQL_CURSOR_KEYSET_DRIVEN ou de type SQL_CURSOR_DYNAMIC, tel que spécifié par le pilote. Il n’est jamais définie sur SQL_CURSOR_FORWARD_ONLY.|  
|SQL_ATTR_CURSOR_SENSITIVITY avec la valeur SQL_INSENSITIVE|SQL_ATTR_CONCURRENCY à SQL_CONCUR_READ_ONLY.<br /><br /> SQL_ATTR_CURSOR_TYPE à SQL_CURSOR_STATIC.|  
|SQL_ATTR_CURSOR_SENSITIVITY SQL_SENSITIVE|SQL_ATTR_CONCURRENCY aux SQL_CONCUR_LOCK, SQL_CONCUR_ROWVER, SQL_CONCUR_VALUES, tel que spécifié par le pilote. Il n’est jamais définie sur la valeur SQL_CONCUR_READ_ONLY.<br /><br /> SQL_ATTR_CURSOR_TYPE SQL_CURSOR_FORWARD_ONLY, SQL_CURSOR_STATIC, SQL_CURSOR_KEYSET_DRIVEN ou de type SQL_CURSOR_DYNAMIC, tel que spécifié par le pilote.|  
|SQL_ATTR_CURSOR_SENSITIVITY à SQL_UNSPECIFIED|SQL_ATTR_CONCURRENCY aux SQL_CONCUR_READ_ONLY, SQL_CONCUR_LOCK, SQL_CONCUR_ROWVER, SQL_CONCUR_VALUES, tel que spécifié par le pilote.<br /><br /> SQL_ATTR_CURSOR_TYPE SQL_CURSOR_FORWARD_ONLY, SQL_CURSOR_STATIC, SQL_CURSOR_KEYSET_DRIVEN ou de type SQL_CURSOR_DYNAMIC, tel que spécifié par le pilote.|  
|SQL_ATTR_CURSOR_TYPE de type SQL_CURSOR_DYNAMIC|SQL_ATTR_SCROLLABLE à la valeur SQL_SCROLLABLE.<br /><br /> SQL_ATTR_CURSOR_SENSITIVITY SQL_SENSITIVE. (Mais uniquement si SQL_ATTR_CONCURRENCY n’est pas égale à la valeur SQL_CONCUR_READ_ONLY. Mettre à jour les curseurs dynamiques sont toujours sensibles aux modifications qui ont été apportées dans leur propre transaction.)|  
|SQL_ATTR_CURSOR_TYPE à SQL_CURSOR_FORWARD_ONLY|SQL_ATTR_CURSOR_SCROLLABLE SQL_NONSCROLLABLE.|  
|SQL_ATTR_CURSOR_TYPE SQL_CURSOR_KEYSET_DRIVEN|SQL_ATTR_SCROLLABLE à la valeur SQL_SCROLLABLE.<br /><br /> SQL_ATTR_SENSITIVITY SQL_UNSPECIFIED ou SQL_SENSITIVE (critères définis par le pilote, si SQL_ATTR_CONCURRENCY n’est pas SQL_CONCUR_READ_ONLY).|  
|SQL_ATTR_CURSOR_TYPE à SQL_CURSOR_STATIC|SQL_ATTR_SCROLLABLE à la valeur SQL_SCROLLABLE.<br /><br /> SQL_ATTR_SENSITIVITY avec la valeur SQL_INSENSITIVE (si SQL_ATTR_CONCURRENCY est SQL_CONCUR_READ_ONLY).<br /><br /> SQL_ATTR_SENSITIVITY SQL_UNSPECIFIED ou SQL_SENSITIVE (si SQL_ATTR_CONCURRENCY n’est pas SQL_CONCUR_READ_ONLY).|
