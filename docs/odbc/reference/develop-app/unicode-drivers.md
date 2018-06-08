---
title: Les pilotes Unicode | Documents Microsoft
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
- Unicode [ODBC], drivers
- Unicode [ODBC], functions
- functions [ODBC], Unicode functions
ms.assetid: 3b4742d5-74fb-4aff-aa21-d83a0064d73d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 310ae2855099544181cfeec75352bfa0742f397e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unicode-drivers"></a>Pilotes d’Unicode
Si un pilote doit être un pilote Unicode ou un pilote de ANSI dépend entièrement de la nature de la source de données. Si la source de données prend en charge les données Unicode, le pilote doit être un pilote Unicode. Si la source de données prend uniquement en charge les données ANSI, le pilote doit rester un pilote ANSI.  
  
 Un pilote Unicode doit exporter **SQLConnectW** pour être reconnus en tant que Unicode pilote par le Gestionnaire de pilotes.  
  
 Un pilote Unicode doit accepter des fonctions Unicode (avec un suffixe de nom *W*) et stocker des données Unicode. Il peut également accepter des fonctions ANSI, mais n’est pas nécessaire pour. (Le Gestionnaire de pilotes ne transmet pas d’un appel de fonction ANSI avec la *A* suffixe au pilote, mais convertit à ANSI sans le suffixe et la passe ensuite l’appel de fonction au pilote.)  
  
 Un pilote Unicode doit être en mesure de retourner des jeux de résultats dans Unicode ou ANSI, en fonction de liaison de l’application. Si une application lie à SQL_C_CHAR, le pilote Unicode doit convertir les données SQL_WCHAR SQL_CHAR. Le Gestionnaire de pilotes sera mappage SQL_C_WCHAR SQL_C_CHAR pour les pilotes ANSI Contrairement à aucun mappage pour les pilotes d’Unicode.  
  
> [!NOTE]  
>  Lorsque vous déterminez le type de pilote, le Gestionnaire de pilotes appellera **SQLSetConnectAttr** et définir l’attribut SQL_ATTR_ANSI_APP au moment de la connexion. Si l’application est à l’aide des API ANSI, SQL_ATTR_ANSI_APP est fixé à SQL_AA_TRUE, et si elle est à l’aide d’Unicode, elle sera définie sur la valeur SQL_AA_FALSE. Cet attribut est utilisé afin que le pilote peut présenter un comportement différent selon le type d’application. L’attribut ne peut pas être définie directement par l’application, et il n’est pas pris en charge par **SQLGetConnectAttr**. Si un pilote présente le même comportement pour les applications ANSI et Unicode, elle doit retourner SQL_ERROR pour cet attribut. Si le pilote retourne SQL_SUCCESS, le Gestionnaire de pilotes séparer les connexions ANSI et Unicode lorsque le regroupement de connexions est utilisé.
