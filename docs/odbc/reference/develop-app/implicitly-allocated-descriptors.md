---
title: Implicitement alloué descripteurs | Documents Microsoft
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
- descriptors [ODBC], allocating and freeing
- implicitly allocated descriptors [ODBC]
- allocating and freeing descriptors [ODBC]
ms.assetid: 9f88c863-affc-4ab4-a558-63a3ef766f37
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8d805b110453c5fe0bb5be8c8df067c09ff8146e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="implicitly-allocated-descriptors"></a>Descripteurs implicitement alloués
Quand un descripteur d’instruction est alloué, l’application alloue implicitement un ensemble de descripteurs. L’application peut obtenir les descripteurs de ces implicitement alloué descripteurs en tant qu’attributs du handle d’instruction. Lorsque l’application libère le handle d’instruction, il libère tous les descripteurs implicitement alloués sur ce handle.
