---
title: Curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante | Documents Microsoft
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
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: d9d271f6-d2d9-49b9-a365-4909ca06caae
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0a0bfa6c25afdd8e79a19051bcec997a0ecfe8bc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="block-cursors-scrollable-cursors-and-backward-compatibility"></a>Curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante
L’existence de deux **SQLFetchScroll** et **SQLExtendedFetch** représente le premier clear Fractionner dans ODBC entre l’Interface API (Application Programming), qui est l’ensemble de fonctions, les appels de l’application et le Service fournisseur Interface SPI (), qui est l’ensemble de fonctions le pilote implémente. Cette séparation est nécessaire afin que ODBC 3. *x*, qui utilise **SQLFetchScroll**, bealigned avec les normes et également être compatibles avec ODBC 2. *x*, qui utilise **SQLExtendedFetch**.  
  
 La version 3 ODBC *.x* API, qui est l’ensemble de l’application appelle les fonctions, inclut **SQLFetchScroll** et les attributs d’instruction. Les 3 ODBC *.x* SPI, qui est l’ensemble de fonctions, le pilote implémente, inclut **SQLFetchScroll**, **SQLExtendedFetch**et les attributs de l’instruction. ODBC n’assure pas formellement cette division entre l’API et le SPI, il est possible pour ODBC 3 *.x* applications d’appeler **SQLExtendedFetch** et les attributs d’instruction. Toutefois, il n’existe aucune raison de ODBC 3 *.x* application pour ce faire. Pour plus d’informations sur les API et SPI, consultez introduction à [Architecture ODBC](../../../odbc/reference/odbc-architecture.md).  
  
 Pour plus d’informations sur les fonctions et l’instruction d’attributs une ODBC 3. *x* application doit utiliser avec les blocs et les curseurs de défilement, consultez [curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante pour les Applications ODBC 3.x](../../../odbc/reference/develop-app/block-cursors-scrollable-backward-compatibility-odbc-3-x-applications.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Ce que fait le gestionnaire de pilotes](../../../odbc/reference/appendixes/what-the-driver-manager-does.md)  
  
-   [Ce que fait le pilote](../../../odbc/reference/appendixes/what-the-driver-does.md)
