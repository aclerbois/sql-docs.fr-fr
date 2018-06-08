---
title: À l’aide d’une procédure stockée sans paramètres | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e9470a6d-a758-4c56-96ec-7b37139e36a7
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7db021b9d3fdf875c2c6074159b56d8e6cb0fd14
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-a-stored-procedure-with-no-parameters"></a>Utilisation d'une procédure stockée sans paramètres
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le type le plus simple de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] procédure stockée que vous pouvez appeler est une qui ne contient aucun paramètre et retourne un jeu de résultats unique. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) (classe), que vous pouvez utiliser pour appeler ce type de procédure stockée et traiter les données qu’elle retourne.  
  
 Lorsque vous utilisez le pilote JDBC pour appeler une procédure stockée sans paramètres, vous devez utiliser le `call` séquence d’échappement SQL. La syntaxe de la `call` séquence d’échappement sans paramètres est la suivante :  
  
 `{call procedure-name}`  
  
> [!NOTE]  
>  Pour plus d’informations sur les séquences d’échappement SQL, consultez [à l’aide des séquences d’échappement SQL](../../connect/jdbc/using-sql-escape-sequences.md).  
  
 Par exemple, créez la procédure stockée suivante dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple :  
  
```  
CREATE PROCEDURE GetContactFormalNames   
AS  
BEGIN  
   SELECT TOP 10 Title + ' ' + FirstName + ' ' + LastName AS FormalName   
   FROM Person.Contact  
END  
```  
  
 Cette procédure stockée retourne un seul jeu de résultats contenant une colonne de données affichant une combinaison des fonctions, des prénoms et des noms des dix premiers contacts figurant dans la table Person.Contact.  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction et la [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) méthode est utilisée pour appeler la procédure stockée GetContactFormalNames.  
  
```  
public static void executeSprocNoParams(Connection con) {  
   try {  
      Statement stmt = con.createStatement();  
      ResultSet rs = stmt.executeQuery("{call dbo.GetContactFormalNames}");  
  
      while (rs.next()) {  
         System.out.println(rs.getString("FormalName"));  
      }  
      rs.close();  
      stmt.close();  
   }  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md)  
  
  
