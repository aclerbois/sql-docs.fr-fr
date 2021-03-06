---
title: "Instructions de génération de résultat et sans résultat | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: odbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- result-generating statements [ODBC]
- batches [ODBC], result-generating statements
- batches [ODBC], result-free statements
- SQL statements [ODBC], batches
- result-free statements [ODBC]
ms.assetid: 2f3475d1-3999-4dd8-aba2-a6e1299c95f8
caps.latest.revision: "6"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: ef4e297719d875b70d76c94c58b5c450740d457f
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="result-generating-and-result-free-statements"></a>Instructions de génération de résultat et sans résultat
Instructions SQL peuvent être faiblement divisées en cinq catégories :  
  
-   **Instructions de génération de jeu de résultat** il s’agit d’instructions SQL qui génèrent un jeu de résultats. Par exemple, un **sélectionnez** instruction.  
  
-   **Instructions de génération de nombre de lignes** il s’agit d’instructions SQL qui génèrent un nombre de lignes affectées. Par exemple, un **mise à jour** ou **supprimer** instruction.  
  
-   **Instructions de langage de définition (DDL) de données** il s’agit d’instructions SQL qui modifient la structure de la base de données. Par exemple, **CREATE TABLE** ou **DROP INDEX**.  
  
-   **Instructions de modification de contexte** il s’agit d’instructions SQL qui modifient le contexte d’une base de données. Par exemple, le **utilisez** et **définir** instructions dans SQL Server.  
  
-   **Instructions d’administration** il s’agit d’instructions SQL utilisées pour effectuer des tâches administratives dans une base de données. Par exemple, **GRANT** et **RÉVOQUER**.  
  
 Les instructions SQL dans les deux premières catégories sont désignés collectivement par *instructions de génération de résultat*. Instructions SQL dans les trois dernières catégories sont désignés collectivement par *instructions exempt résultat*. ODBC définit les sémantiques de lots contenant des instructions de génération uniquement de résultat. Ces sémantiques peuvent varier et sont donc spécifiques à la source de données. Par exemple, le pilote SQL Server ne prend pas en charge la suppression d’un objet, puis en faisant référence à ou recréer le même objet dans le même lot. Par conséquent, le terme *lot* dans ce manuel se réfère à des lots de générer des résultats instructions.
