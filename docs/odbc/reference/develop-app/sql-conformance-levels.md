---
title: Les niveaux de conformité SQL | Documents Microsoft
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
- conformance levels [ODBC], SQL
- SQL conformance levels [ODBC]
- data sources [ODBC], conformance levels
- ODBC drivers [ODBC], conformance levels
ms.assetid: 3529df2c-a09b-4c16-9c60-eae7a06d903a
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dcdb0f98d7260985e418a6eaeb670c40409c518d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-conformance-levels"></a>Niveaux de conformité SQL
Le niveau de prise en charge par un pilote de la grammaire SQL-92 est indiqué par la valeur retournée par un appel à **SQLGetInfo** avec le type d’informations SQL_SQL_CONFORMANCE. Cela indique si le pilote est conforme aux niveaux de saisie, FIPS transitoires, intermédiaire ou intégral définis dans SQL-92.  
  
 Tous les pilotes ODBC doivent prendre en charge la grammaire SQL minimale décrite dans [grammaire minimale SQL](../../../odbc/reference/appendixes/sql-minimum-grammar.md) dans l’annexe c : SQL grammaire. Cette grammaire est un sous-ensemble du niveau d’entrée de SQL-92. Pilotes peuvent prendre en charge SQL supplémentaires et être conforme au niveau d’entrée SQL-92, intermédiaire ou intégral ou à la norme FIPS 127-2 au niveau de transition. Les pilotes qui sont conformes à un niveau donné de SQL-92 ou la norme FIPS 127-2 peuvent prendre en charge des fonctionnalités supplémentaires dans un des niveaux plus élevés mais non entièrement conforme à ce niveau. Pour déterminer si une fonctionnalité est prise en charge, une application doit appeler **SQLGetInfo** avec le type d’informations. Le niveau de conformité d’une fonction SQL est décrite dans le type d’information. (Consultez la [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description de fonction.)
