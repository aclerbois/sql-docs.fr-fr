---
title: À l’aide d’une procédure stockée avec des paramètres d’entrée | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 8f491b70-7d1b-42bd-964f-9a8b86af5eaa
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f736e2e901d17d4a6b8d114964a315afd389ab9e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-a-stored-procedure-with-input-parameters"></a>Utilisation d'une procédure stockée avec des paramètres d'entrée
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  A [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] procédure stockée que vous pouvez appeler est une qui contient un ou plus dans les paramètres qui sont des paramètres qui peuvent être utilisés pour passer des données dans la procédure stockée. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) (classe), que vous pouvez utiliser pour appeler ce type de procédure stockée et traiter les données qu’elle retourne.  
  
 Lorsque vous utilisez le pilote JDBC pour appeler une procédure stockée avec paramètres, vous devez utiliser le `call` séquence d’échappement SQL conjointement avec la [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. La syntaxe de la `call` séquence d’échappement avec des paramètres est comme suit :  
  
 `{call procedure-name[([parameter][,[parameter]]...)]}`  
  
> [!NOTE]  
>  Pour plus d’informations sur les séquences d’échappement SQL, consultez [à l’aide des séquences d’échappement SQL](../../connect/jdbc/using-sql-escape-sequences.md).  
  
 Lorsque vous construisez la `call` séquence d’échappement, spécifiez les paramètres IN à l’aide de le ? caractère ? (point d'interrogation). Ce caractère fait office d'espace réservé pour les valeurs de paramètre qui sont transmises à la procédure stockée. Pour spécifier une valeur pour un paramètre, vous pouvez utiliser une des méthodes d’accesseur Set de la classe SQLServerPreparedStatement. La méthode de définition que vous pouvez utiliser est déterminée par le type de données du paramètre IN.  
  
 Lorsque vous transmettez une valeur à la méthode de définition, vous devez spécifier non seulement la valeur réelle qui sera utilisée dans le paramètre, mais également la position ordinale dans la procédure stockée. Par exemple, si votre procédure stockée contient un seul paramètre IN, sa valeur ordinale sera 1. Si la procédure stockée contient deux paramètres, la première valeur ordinale sera 1 et la deuxième sera 2.  
  
 Par exemple comment appeler une procédure stockée qui contient un paramètre IN, utilisez la procédure stockée uspgetemployeemanagers figurant dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple. Cette procédure stockée n'accepte qu'un seul paramètre d'entrée nommé EmployeeID, qui est une valeur entière, et retourne une liste récursive d'employés et de leurs directeurs basée sur la valeur EmployeeID spécifiée. Le code Java pour l'appel de cette procédure stockée est le suivant :  
  
```  
public static void executeSprocInParams(Connection con) {  
   try {  
      PreparedStatement pstmt = con.prepareStatement("{call dbo.uspGetEmployeeManagers(?)}");  
      pstmt.setInt(1, 50);  
      ResultSet rs = pstmt.executeQuery();  
  
      while (rs.next()) {  
         System.out.println("EMPLOYEE:");  
         System.out.println(rs.getString("LastName") + ", " + rs.getString("FirstName"));  
         System.out.println("MANAGER:");  
         System.out.println(rs.getString("ManagerLastName") + ", " + rs.getString("ManagerFirstName"));  
         System.out.println();  
      }  
      rs.close();  
      pstmt.close();  
   }  
  
   catch (Exception e) {  
      e.printStackTrace();  
    }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md)  
  
  
