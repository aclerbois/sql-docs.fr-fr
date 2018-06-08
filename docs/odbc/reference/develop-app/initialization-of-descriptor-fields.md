---
title: L’initialisation de champs de descripteur | Documents Microsoft
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
- initializing descriptor fields [ODBC]
- allocating and freeing descriptors [ODBC]
ms.assetid: 1da157cb-8ea9-4a56-983b-1c45650217c5
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2c3a5d7a6abe5f7f21da802b1daeda8df9dd3e6c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="initialization-of-descriptor-fields"></a>Initialisation de champs de descripteur
Lors de l’allocation de descripteur de ligne d’une application, ses champs reçoivent des valeurs initiales comme indiqué dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). La valeur initiale du champ SQL_DESC_TYPE est SQL_DEFAULT. Ceci permet un traitement standard de la base de données pour la présentation à l’application. L’application peut spécifier un traitement différent des données en définissant les champs de l’enregistrement de descripteur.  
  
 La valeur initiale de SQL_DESC_ARRAY_SIZE dans l’en-tête de descripteur est 1. L’application peut modifier ce champ pour activer l’extraction de lignes multiples.  
  
 Le concept d’une valeur par défaut n’est pas valide pour les champs d’un IRD. Une application peut accéder aux champs d’un IRD uniquement s’il existe une instruction préparée ou exécutée associée.  
  
 Certains champs d’un IPD sont définies uniquement après que l’IPD a été rempli automatiquement par le pilote. Dans le cas contraire, ils ne sont pas définis. Ces champs sont SQL_DESC_CASE_SENSITIVE, SQL_DESC_FIXED_PREC_SCALE, SQL_DESC_TYPE_NAME, SQL_DESC_UNSIGNED et SQL_DESC_LOCAL_TYPE_NAME.
