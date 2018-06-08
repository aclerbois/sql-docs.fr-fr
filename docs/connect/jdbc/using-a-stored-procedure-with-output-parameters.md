---
title: À l’aide d’une procédure stockée avec des paramètres de sortie | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1c006f27-7e99-43d5-974c-7b782659290c
caps.latest.revision: 29
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5199b4d83b0c565015e98ab862366e9d1a53718a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-a-stored-procedure-with-output-parameters"></a>Utilisation d'une procédure stockée avec des paramètres de sortie
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  A [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] une procédure stockée que vous pouvez appeler est celle qui retourne un ou plusieurs paramètres OUT, qui sont des paramètres utilisés par la procédure stockée pour retourner des données en différé à l’application appelante. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) (classe), que vous pouvez utiliser pour appeler ce type de procédure stockée et traiter les données qu’elle retourne.  
  
 Lorsque vous appelez ce type de procédure stockée à l’aide du pilote JDBC, vous devez utiliser le `call` séquence d’échappement SQL conjointement avec la [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. La syntaxe de la `call` séquence d’échappement avec les paramètres de sortie est la suivante :  
  
 `{call procedure-name[([parameter][,[parameter]]...)]}`  
  
> [!NOTE]  
>  Pour plus d’informations sur les séquences d’échappement SQL, consultez [à l’aide des séquences d’échappement SQL](../../connect/jdbc/using-sql-escape-sequences.md).  
  
 Lorsque vous construisez la `call` séquence d’échappement, spécifiez les paramètres OUT à l’aide de le ? caractère ? (point d'interrogation). Ce caractère fait office d'espace réservé pour les valeurs de paramètre qui sont retournées par la procédure stockée. Pour spécifier une valeur pour un paramètre OUT, vous devez spécifier le type de données de chaque paramètre à l’aide de la [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) méthode de la classe SQLServerCallableStatement avant d’exécuter la procédure stockée.  
  
 La valeur que vous spécifiez pour le paramètre OUT dans la méthode registerOutParameter doit être un des types de données JDBC contenus dans java.sql.Types, qui à son tour est mappée à un des natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données. Pour plus d’informations sur JDBC et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données, consultez [présentation des Types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md).  
  
 Lorsque vous passez une valeur à la méthode registerOutParameter pour un paramètre OUT, vous devez spécifier non seulement le type de données à utiliser pour le paramètre, mais également la position du paramètre ordinal ou le nom du paramètre dans la procédure stockée. Par exemple, si votre procédure stockée contient un seul paramètre OUT, sa valeur ordinale est 1 ; si la procédure stockée contient deux paramètres, la première valeur ordinale est 1 et la seconde 2.  
  
> [!NOTE]  
>  Le pilote JDBC ne prend pas en charge l’utilisation de curseur, SQLVARIANT, TABLE et TIMESTAMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données comme des paramètres OUT.  
  
 Par exemple, créez la procédure stockée suivante dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple :  
  
```  
CREATE PROCEDURE GetImmediateManager  
   @employeeID INT,  
   @managerID INT OUTPUT  
AS  
BEGIN  
   SELECT @managerID = ManagerID   
   FROM HumanResources.Employee   
   WHERE EmployeeID = @employeeID  
END  
```  
  
 Cette procédure stockée retourne un seul paramètre OUT (ManagerID), un nombre entier, basé sur le paramètre IN spécifié (EmployeeID), qui est également un nombre entier. La valeur retournée dans le paramètre OUT est le ManagerID basé sur l'EmployeeID contenu dans la table HumanResources.Employee.  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction et la [exécuter](../../connect/jdbc/reference/execute-method-sqlserverstatement.md) méthode est utilisée pour appeler la procédure stockée GetImmediateManager :  
  
```  
public static void executeStoredProcedure(Connection con) {  
   try {  
      CallableStatement cstmt = con.prepareCall("{call dbo.GetImmediateManager(?, ?)}");  
      cstmt.setInt(1, 5);  
      cstmt.registerOutParameter(2, java.sql.Types.INTEGER);  
      cstmt.execute();  
      System.out.println("MANAGER ID: " + cstmt.getInt(2));  
   }  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
 Cet exemple utilise les positions ordinales pour identifier les paramètres. Vous pouvez également identifier un paramètre en utilisant son nom plutôt que sa position ordinale. L'exemple de code suivant modifie l'exemple précédent afin de démontrer comment utiliser des paramètres nommés dans une application Java. Notez que les noms des paramètres correspondent aux noms des paramètres dans la définition de la procédure stockée :  
  
```  
public static void executeStoredProcedure(Connection con) {  
   try {  
      CallableStatement cstmt = con.prepareCall("{call dbo.GetImmediateManager(?, ?)}");  
      cstmt.setInt("employeeID", 5);  
      cstmt.registerOutParameter("managerID", java.sql.Types.INTEGER);  
      cstmt.execute();  
      System.out.println("MANAGER ID: " + cstmt.getInt("managerID"));  
      cstmt.close();  
   }  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
```  
  
 }  
  
> [!NOTE]  
>  Ces exemples utilisent la méthode execute de la classe SQLServerCallableStatement pour exécuter la procédure stockée. Elle est utilisée parce que la procédure stockée n'a pas retourné de jeu de résultats. Si c’était le cas, le [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) méthode doit être utilisée.  
  
 Les procédures stockées peuvent retourner des nombres de mises à jour et des jeux de résultats multiples. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] suit la spécification JDBC 3.0, qui stipule que plusieurs jeux de résultats et les mises à jour multiples doivent être récupérés avant les paramètres OUT sont récupérés. Autrement dit, l’application doit récupérer tous les objets du jeu de résultats et nombres avant de récupérer les paramètres OUT à l’aide de méthodes CallableStatement.getter de mettre à jour. Dans le cas contraire, les objets de jeu de résultats et les nombres de mises à jour qui n’ont pas déjà été récupérées seront perdues lorsque les paramètres OUT sont récupérés. Pour plus d’informations sur le nombre de mises à jour et plusieurs jeux de résultats, consultez [à l’aide d’une procédure stockée avec un nombre de mettre à jour](../../connect/jdbc/using-a-stored-procedure-with-an-update-count.md) et [à l’aide de plusieurs jeux de résultats](../../connect/jdbc/using-multiple-result-sets.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md)  
  
  
