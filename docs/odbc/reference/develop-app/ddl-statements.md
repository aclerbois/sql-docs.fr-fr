---
title: Instructions DDL | Documents Microsoft
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
- SQL statements [ODBC], interoperability
- interoperability of SQL statements [ODBC], DDL statements
- DDL statements [ODBC]
ms.assetid: 96ac9859-5976-4b06-ae1f-2fec3231e266
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d077c86fb7a87658bc62d9530e9019e9f25da987
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ddl-statements"></a>Instructions DDL
Instructions de langage de définition (DDL) de données varient considérablement SGBD. SQL ODBC définit des instructions pour les opérations de définition de données courantes : créer et supprimer des tables, des index et vues ; modification des tables ; l’octroi et révocation des privilèges. Toutes les autres instructions DDL sont spécifiques à la source de données. Par conséquent, les applications interopérables ne peut pas effectuer certaines opérations de définition de données. En règle générale, cela n’est pas un problème, car ces opérations ont tendance à être très propres au SGBD et sont meilleures gauche pour le logiciel d’administration propriétaire de la base de données fourni avec la plupart des SGBD ou le programme d’installation fourni avec le pilote.  
  
 Un autre problème dans la définition de données correspond à ce type de données noms varient considérablement entre les SGBD. Au lieu de définition des noms de type de données standard et forcer les pilotes pour les convertir en noms propres au SGBD **SQLGetTypeInfo** offre un moyen aux applications découvrir les noms de type de données propres au SGBD. Applications interopérables doivent utiliser ces noms dans les instructions SQL pour créer et modifier des tables ; les noms répertoriés dans [annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md), et [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md), sont des exemples uniquement.
