---
title: Les Modules SQL | Documents Microsoft
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
- SQL modules [ODBC]
- sending SQL statements to DBMS [ODBC]
- SQL [ODBC], modules
- modules [ODBC]
- SQL statements [ODBC], modules
ms.assetid: 07551472-87ee-4765-951f-1364ed32f0c0
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 10957c8e4a847f13d2dbf4b427382e65ea404c1f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-modules"></a>Modules SQL
La seconde technique pour l’envoi d’instructions SQL au SGBD est via les modules. En résumé, un module se compose d’un groupe de procédures qui sont appelées à partir de l’hôte de langage de programmation. Chaque procédure contient une instruction SQL unique, et les données sont transmises vers et à partir de la procédure de paramètres.  
  
 Un module peut être considéré comme une bibliothèque d’objets qui est liée au code d’application. Toutefois, exactement comment les procédures et le reste de l’application sont liés sont dépendant de l’implémentation. Par exemple, les procédures peuvent être compilés en code objet et directement liés au code de l’application, peut être compilés et stockés dans le SGBD et les appels pour accéder aux identificateurs de plan placés dans le code d’application, ou ils pourraient être interprétés au moment de l’exécution.  
  
 Le principal avantage des modules est séparer correctement les instructions SQL à partir du langage de programmation. En théorie, il doit être possible de modifier une sans modifier l’autre et simplement les réassocier.
