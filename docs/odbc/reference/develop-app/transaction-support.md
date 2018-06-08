---
title: Prise en charge des transactions | Documents Microsoft
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
- transactions [ODBC], degree of support
ms.assetid: d56e1458-8da2-4d73-a777-09e045c30a33
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2f3e62cd93f3823a2205ed2c2721ed95749f7a1a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transaction-support"></a>Prise en charge des transactions
Le degré de prise en charge des transactions est définie par le pilote. ODBC est conçu pour être implémentée sur une base de données mono-utilisateur ou bureau n’a pas besoin de gérer plusieurs mises à jour ses données. En outre, certaines bases de données qui prennent en charge les transactions se faire que pour les instructions de langage de Manipulation de données (DML) de SQL ; des restrictions ou la sémantique de transaction spéciales concernant l’utilisation du langage DDL (Data Definition) lorsqu’une transaction est active. Autrement dit, il peut être prise en charge des transactions pour plusieurs mises à jour simultanées de tables, mais ne pas pour modifier le nombre et la définition des tables lors d’une transaction.  
  
 Une application détermine si les transactions sont prises en charge, si le DDL peut être incluse dans une transaction et tous les effets spéciaux de l’intégration des DDL dans une transaction, en appelant **SQLGetInfo** avec l’option SQL_TXN_CAPABLE. Pour plus d’informations, consultez la [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description de fonction.  
  
 Si le pilote ne prend pas en charge les transactions, mais que l’application a la possibilité (à l’aide d’une API que ODBC) pour verrouiller et déverrouiller des données, les applications peuvent obtenir prise en charge de la transaction par verrouillage et déverrouillage des enregistrements et les tables en fonction des besoins. Pour implémenter l’exemple de transfert de compte, l’application serait verrouiller les enregistrements pour les deux comptes, copier les valeurs actuelles, le premier compte de débit, le deuxième compte du crédit et déverrouiller les enregistrements. En cas d’échec de toutes les étapes, l’application serait réinitialiser les comptes à l’aide de la copie.  
  
 Sources même les données qui prennent en charge des transactions n’est peut-être pas en mesure de prendre en charge de plusieurs transactions à la fois dans un environnement particulier. Appel d’applications **SQLGetInfo** avec l’option SQL_MULTIPLE_ACTIVE_TXN pour déterminer si une source de données peut prendre en charge les transactions actives simultanées sur plusieurs connexions dans le même environnement. Étant donné qu’une seule transaction par connexion, cela n’est plus intéressant pour les applications qui ont plusieurs connexions à la même source de données.
