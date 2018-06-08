---
title: Handles de descripteur | Documents Microsoft
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
- application parameter descriptor [ODBC]
- automatically allocated descriptors [ODBC]
- implementation row descriptor [ODBC]
- descriptor handles [ODBC]
- handles [ODBC], descriptor
- implementation parameter descriptor [ODBC]
- apd [ODBC]
- ard [ODBC]
- explicitly allocated descriptors [ODBC]
- ipd [ODBC]
- ird [ODBC]
- application row descriptor [ODBC]
ms.assetid: 7741035c-f3e7-4c89-901e-fe528392f67d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 791a28ece56894c8bbe9167c8e2445da32c3e07b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="descriptor-handles"></a>Handles de descripteur
A *descripteur* est une collection de métadonnées qui décrivent les paramètres d’une instruction SQL ou les colonnes d’un jeu de résultats, comme indiqué par l’application ou le pilote (également connu sous le *implémentation*). Par conséquent, un descripteur peut remplir un des quatre rôles :  
  
-   **Descripteur de paramètre d’application (APD).** Contient des informations sur les tampons d’application liées aux paramètres dans une instruction SQL, telles que leurs adresses, les longueurs et les types de données C.  
  
-   **Descripteur de paramètre d’implémentation (IPD).** Contient des informations sur les paramètres dans une instruction SQL, telles que la possibilité de valeur null, des longueurs et leurs types de données SQL.  
  
-   **Descripteur de ligne d’application (ARD).** Contient des informations sur les tampons d’application liées aux colonnes dans un jeu de résultats, telles que leurs adresses, les longueurs et les types de données C.  
  
-   **Descripteur de ligne d’implémentation (IRD).** Contient des informations sur les colonnes dans un jeu de résultats, telles que leurs types de données SQL, longueurs et la possibilité de valeur null.  
  
 Les descripteurs de quatre (un remplissage chaque rôle) sont attribuées automatiquement lorsqu’une instruction est allouée. Ils sont appelés *allouée automatiquement descripteurs* et sont toujours associées à cette instruction. Les applications peuvent également allouer des descripteurs avec **SQLAllocHandle**. Ils sont appelés *explicitement alloué descripteurs*. Ils sont alloués sur une connexion et peuvent être associés à une ou plusieurs instructions sur cette connexion pour remplir le rôle d’un APD ou d’ARD sur ces instructions.  
  
 La plupart des opérations dans ODBC peut être effectuée sans utilisation explicite de descripteurs par l’application. Toutefois, les descripteurs de fournissent un raccourci commode pour certaines opérations. Par exemple, qu'une application souhaite insérer des données à partir de deux ensembles différents de mémoires tampons. Pour utiliser le premier jeu de mémoires tampons, qu’il appelle à plusieurs reprises **SQLBindParameter** pour les lier aux paramètres dans une **insérer** instruction, puis exécutez l’instruction. Pour utiliser le deuxième jeu de mémoires tampons, il serait Répétez ce processus. Il peut également définir des liaisons pour le premier jeu de mémoires tampons dans un descripteur et le deuxième jeu de mémoires tampons dans le descripteur d’un autre. Pour basculer entre les ensembles de liaisons, l’application appelle simplement **SQLSetStmtAttr** et associer le descripteur correct de l’instruction en tant que le descripteur APD.  
  
 Pour plus d’informations sur les descripteurs, consultez [Types de descripteurs](../../../odbc/reference/develop-app/types-of-descriptors.md).
