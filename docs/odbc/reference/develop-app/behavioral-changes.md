---
title: Modifications comportementales | Documents Microsoft
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
- backward compatibility [ODBC], behavioral changes
- behavioral changes [ODBC]
- compatibility [ODBC], behavioral changes
ms.assetid: a17ae701-6ab6-4eaf-9e46-d3b9cd0a3a67
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ed9b06793abef5006e49b2f526d11ead798c7931
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="behavioral-changes"></a>Changements de comportement
Changements de comportement sont celles pour lesquelles le *syntaxe* de l’interface reste la même, mais la *sémantique* ont été modifiés. Pour que ces modifications, les fonctionnalités utilisées dans ODBC 2. *x* se comporte différemment de la même fonctionnalité dans ODBC 3. *x*.  
  
 Si une application présente ODBC 2. *x* comportement ou ODBC 3. *x* comportement est déterminé par l’attribut d’environnement SQL_ATTR_ODBC_VERSION. Cette valeur de 32 bits est définie à SQL_OV_ODBC2 expose ODBC 2. *x* comportement et SQL_OV_ODBC3 expose ODBC 3. *x* comportement.  
  
 L’attribut d’environnement SQL_ATTR_ODBC_VERSION est définie par un appel à **SQLSetEnvAttr**. Une fois une application appelle **SQLAllocHandle** pour allouer un handle d’environnement, il doit appeler**SQLSetEnvAttr** immédiatement pour définir le comportement qu’il expose. (Par conséquent, il est un nouvel état de l’environnement pour décrire le handle d’environnement dans allouée, mais pour un logiciel, état.) Pour plus d’informations, consultez [Tables de Transition d’état annexe b : ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
 Une application indique le comportement qu’il expose avec l’attribut d’environnement SQL_ATTR_ODBC_VERSION, mais l’attribut n’a aucun effet sur la connexion de l’application avec une API ODBC 2. *x* ou ODBC 3. *x* pilote. Une application ODBC 3. *x* application peut se connecter à l’un ODBC 2. *x* ou 3. *x* pilote, quel que soit le paramètre de l’attribut de l’environnement.  
  
 ODBC 3. *x* jamais les applications doivent appeler **SQLAllocEnv**. Par conséquent, si le Gestionnaire de pilote reçoit un appel à **SQLAllocEnv**, il reconnaît l’application en tant qu’une API ODBC 2. *x* application.  
  
 L’attribut SQL_ATTR_ODBC_VERSION affecte trois différents aspects d’un ODBC 3. *x* comportement du pilote :  
  
-   SQLSTATE  
  
-   Types de données de date, time et timestamp  
  
-   Le *CatalogName* argument dans **SQLTables** accepte les modèles de recherche dans ODBC 3. *x*, mais pas dans ODBC 2. *x*  
  
 Le paramètre de l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’affecte pas **SQLSetParam** ou **SQLBindParam**. **SQLColAttribute** n’est pas non plus affecté par ce bit. Bien que **SQLColAttribute** retourne les attributs qui sont affectés par la version d’ODBC (type de date, précision, échelle et longueur), le comportement attendu est déterminé par la valeur de la *FieldIdentifier* argument. Lorsque *FieldIdentifier* est égal à SQL_DESC_TYPE, **SQLColAttribute** retourne le ODBC 3. *x* codes pour la date, time et timestamp ; lorsque *FieldIdentifier* est égal à SQL_COLUMN_TYPE, **SQLColAttribute** retourne aux API ODBC 2. *x* codes pour la date, time et timestamp.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Mappages de SQLSTATE](../../../odbc/reference/develop-app/sqlstate-mappings.md)  
  
-   [Changements dans le type de données datetime](../../../odbc/reference/develop-app/datetime-data-type-changes.md)
