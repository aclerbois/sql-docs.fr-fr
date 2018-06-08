---
title: Types de modifications | Documents Microsoft
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
- compatibility [ODBC], types of changes
- backward compatibility [ODBC], types of changes
ms.assetid: 6a7db81a-20aa-4915-aed8-429711a36f49
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 08e478da2080cf3d457d2c0a1ec5673e95ba2ea4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="types-of-changes"></a>Types de modifications
Trois types de modifications sont apportées dans ODBC 3. *x* (et n’importe quelle version d’ODBC). Chacun de ces affecte différemment de la compatibilité descendante et est géré d’une manière différente. Ces modifications sont décrites dans le tableau suivant.  
  
|Type de modification| Description|  
|--------------------|-----------------|  
|Nouvelles fonctionnalités|Il s’agit des fonctionnalités qui sont nouvelles dans ODBC 3. *x*, tels que la liaison hors ligne ou de descripteurs. Celles-ci sont implémentées uniquement lorsque l’application et de pilote, ainsi que le Gestionnaire de pilotes, sont de la version 3 *.x*, donc il n’existe aucune tentative pour rendre ces une compatibilité descendante.|  
|Fonctionnalités en double|Il s’agit des fonctionnalités qui existent dans ODBC 2 *.x* alors que ODBC 3. *x* mais sont implémentées de différentes façons dans chacun. Les fonctions **SQLAllocHandle** et **SQLAllocStmt** sont un exemple. Problèmes de compatibilité descendante pour ces et autres fonctionnalités en double sont principalement traitées par les mappages dans le Gestionnaire de pilotes.|  
|Changements de comportement|Il s’agit des fonctionnalités qui sont gérées différemment dans ODBC 2 *.x* alors que ODBC 3. *x*. Une valeur datetime **#define** est un exemple. Ces fonctionnalités sont gérées par la version ODBC 3. *x* pilote basé sur un paramètre d’attribut environnement. (Consultez [modifications comportementales](../../../odbc/reference/develop-app/behavioral-changes.md) pour plus d’informations.)|
