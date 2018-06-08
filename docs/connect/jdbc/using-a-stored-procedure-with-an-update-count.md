---
title: À l’aide d’une procédure stockée avec un nombre de mises à jour | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 64cf4877-5995-4bfc-8865-b7618a5c8d01
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d858b255d5bdd6ce74509d36f4d0497220350694
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-a-stored-procedure-with-an-update-count"></a>Utilisation d'une procédure stockée avec un nombre de mises à jour
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour modifier les données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données à l’aide d’une procédure stockée, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe. À l’aide de la classe SQLServerCallableStatement, vous pouvez appeler des procédures stockées qui modifient les données contenues dans la base de données et retourner le nombre de lignes affectées, également appelé le nombre de mises à jour.  
  
 Une fois que vous avez configuré l’appel à la procédure stockée à l’aide de la classe SQLServerCallableStatement, vous pouvez ensuite appeler la procédure stockée à l’aide la [exécuter](../../connect/jdbc/reference/execute-method-sqlserverstatement.md) ou [executeUpdate](../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md) (méthode). La méthode executeUpdate retourne un **int** n’est pas le cas de valeur qui contient le nombre de lignes affectées par la procédure stockée, mais la méthode execute. Si vous utilisez la méthode d’exécution et que vous voulez obtenir le nombre de lignes affectées, vous pouvez appeler la [getUpdateCount](../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md) méthode après avoir exécuté la procédure stockée.  
  
> [!NOTE]  
>  Si vous souhaitez que le pilote JDBC retourne tous les nombres de mises à jour, y compris les nombres de mises à jour retournées par des déclencheurs qui ont pu se déclencher, définissez la propriété de chaîne de connexion lastUpdateCount sur « false ». Pour plus d’informations sur la propriété lastUpdateCount, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
 Par exemple, créez la table suivante et une procédure stockée et également insérer des données d’exemple dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple :  
  
```  
CREATE TABLE TestTable   
   (Col1 int IDENTITY,   
    Col2 varchar(50),   
    Col3 int);  
  
CREATE PROCEDURE UpdateTestTable  
   @Col2 varchar(50),  
   @Col3 int  
AS  
BEGIN  
   UPDATE TestTable  
   SET Col2 = @Col2, Col3 = @Col3  
END;  
INSERT INTO dbo.TestTable (Col2, Col3) VALUES ('b', 10);  
```  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction, la méthode execute est utilisée pour appeler la procédure stockée UpdateTestTable, puis la méthode getUpdateCount est utilisée pour retourner un nombre de lignes qui sont affecté par la procédure stockée.  
  
 [!code[JDBC#UsingSprocWithUpdateCount1](../../connect/jdbc/codesnippet/Java/using-a-stored-procedure_0_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md)  
  
  
