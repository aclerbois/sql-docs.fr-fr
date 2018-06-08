---
title: Les identificateurs de Type SQL | Documents Microsoft
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
- data types [ODBC], identifiers
- SQL data types [ODBC], identifiers
- type identifiers [ODBC], SQL
- identifiers [ODBC], SQL type
- SQL type identifiers [ODBC]
ms.assetid: 22f6793b-2f43-4281-b35a-28f48e504dd8
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7f3a9270e698f7f0ff12bd12c02de865a3c6f2b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-type-identifiers"></a>Identificateurs de Type SQL
Chaque source de données définit ses propres types de données SQL. ODBC définit les identificateurs de type et décrit les caractéristiques générales des types de données SQL qui peuvent être mappés à l’identificateur de chaque type. Il est spécifique au pilote comment chaque type de données dans la source de données sous-jacente est mappé à un identificateur de type SQL d’ODBC.  
  
 Par exemple, SQL_CHAR est l’identificateur de type pour une colonne de caractères de longueur fixe, en général entre 1 et 254 caractères. Ces caractéristiques correspondent au type de données CHAR trouvé dans plusieurs sources de données SQL. Par conséquent, lorsqu’une application découvre que l’identificateur de type pour une colonne est SQL_CHAR, peut supposer que probablement traite avec une colonne de type CHAR. Toutefois, il doit toujours vérifier de la longueur d’octet de la colonne avant de considérer qu’elle est comprise entre 1 et 254 caractères ; le pilote pour une source de données autre que SQL, par exemple, peut mapper une colonne de caractères de longueur fixe de 500 caractères SQL_CHAR ou SQL_LONGVARCHAR, car aucun n’est une correspondance exacte.  
  
 ODBC définit un large éventail d’identificateurs de type SQL. Toutefois, le pilote n’est pas obligatoirement à utiliser la totalité de ces identificateurs. Au lieu de cela, il utilise uniquement ces identificateurs qu'il doit exposer les types de données SQL pris en charge par la source de données sous-jacente. Si la source de données sous-jacente prend en charge les types de données SQL qui aucun identificateur de type ne correspondant, le pilote peut définir des identificateurs de type supplémentaire. Pour plus d’informations, consultez [les Types de données spécifiques au pilote, Types de descripteur, Types d’informations, Types de diagnostics et attributs](../../../odbc/reference/develop-app/driver-specific-data-types-descriptor-information-diagnostic.md).  
  
 Pour obtenir une description complète des identificateurs de type SQL, consultez [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) annexe d : Types de données.
