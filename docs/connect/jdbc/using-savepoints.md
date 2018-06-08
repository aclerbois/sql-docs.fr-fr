---
title: À l’aide de points d’enregistrement | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3b48eb13-32ef-4fb3-8e95-dbc9468c9a44
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 72b354c805a3d46dd31f6f9df308e33493c2db2c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-savepoints"></a>Utilisation de points d'enregistrement
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les points d'enregistrement constituent un mécanisme permettant d'annuler certaines parties de transactions. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez créer un point d’enregistrement à l’aide de l’instruction SAVE TRANSACTION nom_point_enregistrement. Par la suite, vous pourrez exécuter une instruction ROLLBACK TRANSACTION nom_point_enregistrement pour revenir au point d'enregistrement au lieu de revenir au début de la transaction.  
  
 Les points d'enregistrement sont particulièrement utiles lorsque des erreurs sont peu susceptibles de se produire. L'utilisation d'un point d'enregistrement pour annuler une partie d'une transaction en cas d'erreur non fréquente peut s'avérer plus efficace que de tester chaque transaction afin de voir si une mise à jour est valide avant de l'implémenter. Les mises à jour et les restaurations étant des opérations coûteuses, les points d'enregistrement ne sont efficaces que si la probabilité de rencontrer l'erreur est faible et si le coût de vérification préalable de la validité d'une mise à jour est relativement élevé.  
  
 Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge l’utilisation de points de sauvegarde via le [setSavepoint](../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. À l’aide de la méthode setSavepoint, vous pouvez créer un point d’enregistrement nommé ou sans nom dans la transaction actuelle, et la méthode retourne un [SQLServerSavepoint](../../connect/jdbc/reference/sqlserversavepoint-class.md) objet. Il est possible de créer plusieurs points d'enregistrement à l'intérieur d'une transaction. Pour restaurer une transaction à un point d’enregistrement donné, vous pouvez passer l’objet SQLServerSavepoint pour le [rollback (java.sql.Savepoint)](../../connect/jdbc/reference/rollback-method-java-sql-savepoint.md) (méthode).  
  
 Dans l’exemple suivant, un point d’enregistrement est utilisé lors de l’exécution d’une transaction locale composée de deux instructions distinctes dans la `try` bloc. Les instructions sont exécutées sur la table Production.ScrapReason dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données et un point d’enregistrement est utilisé pour restaurer la deuxième instruction. Cela a pour effet que seule la première instruction est validée dans la base de données.  
  
 [!code[JDBC#UsingSavepoints1](../../connect/jdbc/codesnippet/Java/using-savepoints_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Réalisation de transactions avec le pilote JDBC](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
  
  
