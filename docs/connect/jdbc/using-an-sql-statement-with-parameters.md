---
title: À l’aide d’une instruction SQL avec des paramètres | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3202b88f-ce13-44dd-982c-c6a3b0260378
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 35f23003f62ab9ea0188d54d7c4ad08d094eb440
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-an-sql-statement-with-parameters"></a>Utilisation d'une instruction SQL avec paramètres
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour utiliser des données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données en utilisant une instruction SQL qui contient des paramètres, vous pouvez utiliser la [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md) méthode de la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe pour retourner un [ SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) qui contiendra les données demandées. Pour ce faire, vous devez d’abord créer un objet SQLServerPreparedStatement à l’aide de la [prepareStatement](../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe.  
  
 Lors de la construction d'une instruction SQL, les paramètres IN sont spécifiés à l'aide du caractère ? (point d'interrogation) qui fait office d'espace réservé pour les valeurs de paramètre qui sont transmises par la suite à l'instruction SQL. Pour spécifier une valeur pour un paramètre, vous pouvez utiliser une des méthodes d’accesseur Set de la classe SQLServerPreparedStatement. La méthode setter que vous utilisez est déterminée par le type de données de la valeur que vous souhaitez transmettre dans l'instruction SQL.  
  
 Lorsque vous transmettez une valeur à la méthode setter, vous devez spécifier non seulement la valeur réelle à utiliser dans l'instruction SQL, mais également la position ordinale du paramètre dans l'instruction. Par exemple, si votre instruction SQL contient un seul paramètre, sa valeur ordinale sera 1. Si l’instruction contient deux paramètres, la première valeur ordinale sera 1, tandis que la deuxième valeur ordinale sera 2.  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction, une instruction SQL préparée est générée et exécutée avec une valeur de paramètre de chaîne unique, puis les résultats sont lus à partir de l’ensemble de résultats.  
  
 [!code[JDBC#UsingSQLWithParams1](../../connect/jdbc/codesnippet/Java/using-an-sql-statement-w_1_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec SQL](../../connect/jdbc/using-statements-with-sql.md)  
  
  
