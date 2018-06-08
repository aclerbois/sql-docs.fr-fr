---
title: Test d’Applications interopérables | Documents Microsoft
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
- interoperability [ODBC], testing interoperable applications
- testing interoperable applications [ODBC]
ms.assetid: 489083cb-8430-40be-9ef2-d75b9a2eea88
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c3025c0ae41e6c677917975efa2742ef4154ce45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="testing-interoperable-applications"></a>Test d’Applications interopérables
Test d’applications interopérables est au mieux une longue activité et au pire impossible, car les nouveaux pilotes s’affichent en permanence sur le marché. Toutefois, un niveau raisonnable de test est possible. Applications avec l’interopérabilité limitée ou faible doivent uniquement être testées par rapport à ces pilotes, qu'elles sont garanties pour prendre en charge. Toutefois, ils doivent être testés entièrement contre ces pilotes.  
  
 Les applications hautement interopérables ne peut pas être comparées pratiquement tous les pilotes. La meilleure que la plupart des développeurs d’applications peuvent effectuer est à tester entièrement sur un petit nombre de pilotes et cursorily plusieurs autres. Pilotes testés doivent inclure les pilotes les plus populaires pour les SGBD plus populaires dans le marché de l’application ; Si le marché couvre tous les SGBD, les pilotes pour le bureau et serveur SGBD doivent être testées.  
  
 Un des problèmes dans le test des applications ODBC est le nombre de composants impliqués : l’application elle-même, le Gestionnaire de pilotes, le pilote, le SGBD et éventuellement d’un logiciel réseau aux passerelles ou. Les applications peuvent faciliter effectuer le suivi des erreurs en validant les messages d’erreur retournées par les fonctions ODBC via **SQLGetDiagField** et **SQLGetDiagRec**. Ces messages indiquent le fabricant et le composant dans lequel les erreurs se produisent. Pour plus d’informations, consultez [Diagnostics](../../../odbc/reference/develop-app/diagnostics.md).
