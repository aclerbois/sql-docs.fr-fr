---
title: Plusieurs instructions actives et connexions | Documents Microsoft
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
- interoperability [ODBC], multiple active statements and connections
- multiple active statements and connections [ODBC]
ms.assetid: a6571356-b23e-4f10-a17b-bce09460b71e
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b548b6d1b1aa7e7b260abcc9f507e2dbd566e7c1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="multiple-active-statements-and-connections"></a>Plusieurs instructions actives et connexions
Des pilotes et un SGBD limite le nombre de connexions qui peuvent être actives en même temps et des instructions. Ces numéros peuvent être aussi petit qu’une. Pour plus d’informations, consultez les options SQL_MAX_CONCURRENT_ACTIVITIES et SQL_MAX_DRIVER_CONNECTIONS dans les [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description, de fonction et [instruction gère](../../../odbc/reference/develop-app/statement-handles.md) et [Handles de connexion](../../../odbc/reference/develop-app/connection-handles.md).
