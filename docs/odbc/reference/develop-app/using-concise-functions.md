---
title: L’utilisation de fonctions concises | Documents Microsoft
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
- concise functions [ODBC]
- functions [ODBC], concise functions
- descriptors [ODBC], concise functions
ms.assetid: 31ac070f-8c59-4fd5-bd5a-466bb27dbca0
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c53a628eaaa3ca8348ce8917ace36da35c33f653
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-concise-functions"></a>L’utilisation de fonctions Concise
Certaines fonctions ODBC accéder implicite aux descripteurs. Créateurs d’applications peuvent trouver plus pratique que l’appel **SQLSetDescField** ou **SQLGetDescField**. Ces fonctions sont appelées *concis* fonctionne car ils effectuent des fonctions, y compris les définir ou obtenir les champs de descripteur. Certaines fonctions concises permettent une application de définir ou de récupérer plusieurs champs de descripteur associés dans un seul appel de fonction.  
  
 Fonctions concises peuvent être appelées sans récupérer préalablement un handle de descripteur pour une utilisation en tant qu’argument. Ces fonctions de travail avec les champs de descripteur associés au handle d’instruction qu’elles sont appelées sur.  
  
 Les fonctions concises **SQLBindCol** et **SQLBindParameter** lier une colonne ou un paramètre en définissant les champs de descripteur qui correspondent à leurs arguments. Chacune de ces fonctions effectue des tâches plus que de simplement définir descripteurs. **SQLBindCol** et **SQLBindParameter** fournissent une spécification complète de la liaison d’une colonne de données ou d’un paramètre dynamique. Une application peut, toutefois, modifier les détails individuels d’une liaison en appelant **SQLSetDescField** ou **SQLSetDescRec** et peut lier complètement une colonne ou un paramètre, en effectuant une série d’appels appropriés à ces fonctions.  
  
 Les fonctions concises **SQLColAttribute**, **SQLDescribeCol**, **SQLDescribeParam**, **SQLNumParams**, et **SQLNumResultCols** récupérer des valeurs dans les champs de descripteur.  
  
 **SQLSetDescRec** et **SQLGetDescRec** sont des fonctions concises qui, lors d’un appel, définir ou obtenir plusieurs champs de descripteur qui affectent le type de données et le stockage de données de colonne ou du paramètre. **SQLSetDescRec** est un moyen efficace pour modifier la liaison de données de colonne ou du paramètre en une seule étape.  
  
 **SQLSetStmtAttr** et **SQLGetStmtAttr** servir en tant que fonctions concises dans certains cas. (Consultez [champs de descripteur](../../../odbc/reference/develop-app/descriptor-fields.md).)
