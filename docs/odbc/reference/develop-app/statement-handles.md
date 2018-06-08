---
title: Descripteurs d’instruction | Documents Microsoft
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
- statement handles [ODBC]
- handles [ODBC], statement
ms.assetid: 65d6d78b-a8c8-489a-9dad-f8d127a44882
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f844a4a783ccdbb2281857e440ce09d965a0c738
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="statement-handles"></a>Descripteurs d’instruction
A *instruction* est plus facilement considérée comme une instruction SQL, tel que **sélectionnez \* de l’employé**. Toutefois, une instruction est plus qu’une instruction SQL, il se compose de toutes les informations associées à cette instruction SQL, tels que des jeux de résultats créés par l’instruction et les paramètres utilisés dans l’exécution de l’instruction. Une instruction n’a même pas besoin d’avoir une instruction SQL définie par l’application. Par exemple, lorsqu’une fonction de catalogue comme **SQLTables** est exécutée sur une instruction, il s’exécute une instruction SQL prédéfinie qui retourne une liste des noms de table.  
  
 Chaque instruction est identifiée par un descripteur d’instruction. Une instruction est associée à une connexion unique, et il peut y avoir plusieurs instructions sur cette connexion. Certains pilotes de limitent le nombre d’instructions actives, qu'ils prennent en charge ; option le SQL_MAX_CONCURRENT_ACTIVITIES **SQLGetInfo** Spécifie le nombre d’instructions actif un pilote prend en charge sur une seule connexion. Une instruction est définie pour être *active* s’il possède des résultats en attente, où des résultats sont un jeu de résultats ou le nombre de lignes affectées par une **insérer**, **mise à jour**, ou **supprimer** instruction ou les données sont envoyées avec les appels multiples à **SQLPutData**.  
  
 Dans un fragment de code qui implémente ODBC (le Gestionnaire de pilotes ou un pilote), le handle d’instruction identifie une structure qui contient les informations de l’instruction, telle que :  
  
-   État de l’instruction  
  
-   Les diagnostics au niveau de l’instruction actuelles  
  
-   Les adresses des variables d’application liée aux paramètres de l’instruction et entraîner le jeu de colonnes  
  
-   Les paramètres actuels de chaque attribut d’instruction  
  
 Descripteurs d’instruction sont utilisés dans la plupart des fonctions ODBC. Notamment, ils sont utilisés dans les fonctions pour lier les paramètres et colonnes du jeu de résultats (**SQLBindParameter** et **SQLBindCol**), préparer et exécuter des instructions (**SQLPrepare**, **SQLExecute**, et **SQLExecDirect**), de récupérer les métadonnées (**SQLColAttribute** et **SQLDescribeCol**), extraire les résultats (**SQLFetch**) et récupérer les diagnostics (**SQLGetDiagField** et **SQLGetDiagRec**). Ils sont également utilisés dans les fonctions de catalogue (**SQLColumns**, **SQLTables**, et ainsi de suite) et un nombre d’autres fonctions.  
  
 Descripteurs d’instruction sont allouées avec **SQLAllocHandle** et libérée avec **SQLFreeHandle**.
