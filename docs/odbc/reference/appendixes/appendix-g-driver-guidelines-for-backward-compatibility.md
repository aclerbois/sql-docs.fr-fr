---
title: 'Annexe g : les instructions de pilote pour la compatibilité descendante | Documents Microsoft'
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
- ODBC drivers [ODBC], backward compatibility
- backward compatibility [ODBC], drivers
- compatibility [ODBC], drivers
ms.assetid: 911cd335-f2c0-4d03-9739-1078308a678a
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 810aa157ef88ba264ff24e3242fea794b3e2e475
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="appendix-g-driver-guidelines-for-backward-compatibility"></a>Annexe g : les instructions de pilote pour la compatibilité descendante
Cette annexe contient des informations pour les rédacteurs de pilotes vous travaillez sur ODBC 3. *x* pilotes doivent prendre en charge d’ODBC 2. *x* applications. Pour plus d’informations sur la compatibilité descendante, consultez [la compatibilité descendante et la conformité aux normes](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante pour les pilotes ODBC 3.x](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) : nouvelles fonctionnalités sont des fonctionnalités qui existent dans ODBC 3. *x* et non dans ODBC 2. *x*. ODBC 3. *x* pilotes ne disposent généralement pas à vous soucier de la compatibilité descendante avec les nouvelles fonctionnalités, car ODBC 2. *x* applications jamais les utilisent. Les seules exceptions sont les fonctionnalités liées à **SQLFetch**, **SQLFetchScroll**, **SQLSetPos**, et **SQLExtendedFetch**; pour plus d’informations, consultez, plus loin dans cette annexe.  
  
-   [Mappage des fonctions de déconseillée](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) — dupliqué fonctionnalités sont des fonctionnalités qui sont implémentées différemment dans ODBC 3. *x* et ODBC 2. *x*. ODBC 3. *x* pilotes n’ont pas à vous soucier de la compatibilité descendante avec les fonctionnalités en double, car le Gestionnaire de pilotes est toujours mappée ODBC 2. *x* fonctions ODBC 3. *x* fonctionnalités lors de l’appel d’un ODBC 3. *x* pilote. Par conséquent, une application ODBC 3. *x* pilote ne voit que ODBC 3. *x* fonctionnalités. Pour plus d’informations sur ces mappages, consultez, plus loin dans cette annexe.  
  
-   [Changements de comportement et les pilotes ODBC 3.x](../../../odbc/reference/appendixes/behavioral-changes-and-odbc-3-x-drivers.md) , changements de comportement sont des fonctionnalités qui sont gérées différemment dans ODBC 3. *x* et ODBC 2. *x*. ODBC 3. *x* pilotes ont soucier des changements de comportement et d’agir en réponse à l’attribut d’environnement SQL_ATTR_ODBC_VERSION définie par l’application.
