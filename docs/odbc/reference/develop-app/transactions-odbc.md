---
title: Transactions ODBC | Documents Microsoft
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
- transactions [ODBC], about transactions
- transactions [ODBC]
ms.assetid: b4ca861a-c164-4e87-8672-d5de15e3823c
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: be280a9fab2b0f98ef83f0e2e4e36603e6c6fa68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transactions-odbc"></a>Transactions ODBC
A *transaction* est une unité de travail qui s’agit d’une opération atomique unique ; autrement dit, l’opération réussit ou échoue dans sa globalité. Par exemple, considérez le transfert d’argent d’un compte bancaire vers un autre. Cela implique deux étapes : retrait de l’argent à partir du premier compte et il dépôt dans la seconde. Il est important que les deux étapes réussissent ; Il n’est pas acceptable pour une seule étape réussisse, l’autre échec. Une base de données qui prend en charge des transactions est en mesure de garantir cela.  
  
 Transactions peuvent être effectuées par soit *validée* ou *restaurée*. Lorsqu’une transaction est validée, les modifications apportées dans la mesure où transaction deviennent permanentes. Lorsqu’une transaction est annulée, les lignes affectées sont retournés à l’état de qu'origine avant le début de la transaction. Pour étendre l’exemple de transfert de compte, une application exécute une instruction SQL pour le premier compte de débit et une instruction SQL différente pour le deuxième compte du crédit. Si les deux instructions réussissent, l’application puis valide la transaction. Mais si l’instruction échoue pour une raison quelconque, l’application annule la transaction. Dans les deux cas, l’application garantit un état cohérent à la fin de la transaction.  
  
 Une seule transaction peut englober plusieurs opérations de base de données qui se produisent à des moments différents. Si d’autres transactions avaient accès complet pour les résultats intermédiaires, les transactions peuvent interférer avec l’autre. Par exemple, supposons qu’une seule transaction insère une ligne, une deuxième transaction lit cette ligne de la première transaction est restaurée. Désormais, la seconde transaction a des données d’une ligne qui n’existe pas.  
  
 Pour résoudre ce problème, il existe différents schémas pour isoler les transactions à partir d’un autre. *Isolation des transactions* est généralement implémentée par le verrouillage de lignes, ce qui empêche d’autres transactions à partir de l’aide de la même ligne en même temps. Dans certaines bases de données, une ligne de verrouillage peut également verrouiller des autres lignes.  
  
 Accrue des transactions isolation s’accompagne de réduction *concurrence,* ou la possibilité de deux transactions utilisent les mêmes données en même temps. Pour plus d’informations, consultez [définissant le niveau d’Isolation de Transaction](../../../odbc/reference/develop-app/setting-the-transaction-isolation-level.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Transactions dans ODBC](../../../odbc/reference/develop-app/transactions-in-odbc-odbc.md)  
  
-   [Isolation des transactions](../../../odbc/reference/develop-app/transaction-isolation.md)  
  
-   [Contrôle de l’accès concurrentiel](../../../odbc/reference/develop-app/concurrency-control.md)
