---
title: Exécution d’opérations de traitement par lots | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1a576d95-7da6-4b7b-8b32-59e5b4d354c4
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 55470e4246256f2dfce11464ab8aafb9c9e7873c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="performing-batch-operations"></a>Exécution d'opérations de traitement par lot
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour améliorer les performances lorsque plusieurs mises à jour à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données se produisent, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] offre la possibilité d’envoyer plusieurs mises à jour comme une seule unité de travail, également appelé un lot.  
  
 Le [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), et [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classes peuvent tous être utilisées pour envoyer des mises à jour par lots. Le [addBatch](../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md) méthode est utilisée pour ajouter une commande. Le [clearBatch](../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md) méthode est utilisée pour effacer la liste des commandes. Le [executeBatch](../../connect/jdbc/reference/executebatch-method-sqlserverstatement.md) méthode est utilisée pour envoyer toutes les commandes de traitement. Seules des instructions DDL (Data Definition Language, langage de définition de données) et DML (Data Manipulation Language, langage de manipulation de données) qui retournent un seul nombre de mises à jour peuvent être exécutées dans un lot.  
  
 La méthode executeBatch retourne un tableau de **int** valeurs qui correspondent au nombre de mises à jour de chaque commande. Si une des commandes échoue, une BatchUpdateException est levée, et vous devez utiliser la méthode getUpdateCounts de la classe BatchUpdateException pour récupérer le tableau de nombres de mise à jour. En cas d'échec d'une commande, le pilote continue à traiter les commandes restantes. Toutefois, si une commande contient une erreur de syntaxe, les instructions contenues dans le lot échouent.  
  
> [!NOTE]  
>  Si vous n’avez pas à utiliser les mises à jour multiples, vous pouvez d’abord émettre une instruction SET NOCOUNT ON vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Ceci réduira le trafic réseau et améliorera les performances de votre application.  
  
 Par exemple, créez la table suivante dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple :  
  
```  
CREATE TABLE TestTable   
   (Col1 int IDENTITY,   
    Col2 varchar(50),   
    Col3 int);  
```  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple est transmise à la fonction, la méthode addBatch permet de créer les instructions à exécuter, et la méthode executeBatch est appelée pour soumettre le lot à la base de données.  
  
```  
public static void executeBatchUpdate(Connection con) {  
   try {  
      Statement stmt = con.createStatement();  
      stmt.addBatch("INSERT INTO TestTable (Col2, Col3) VALUES ('X', 100)");  
      stmt.addBatch("INSERT INTO TestTable (Col2, Col3) VALUES ('Y', 200)");  
      stmt.addBatch("INSERT INTO TestTable (Col2, Col3) VALUES ('Z', 300)");  
      int[] updateCounts = stmt.executeBatch();  
      stmt.close();  
   }  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec le pilote JDBC](../../connect/jdbc/using-statements-with-the-jdbc-driver.md)  
  
  
