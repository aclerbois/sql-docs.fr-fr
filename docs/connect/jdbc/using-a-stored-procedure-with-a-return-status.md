---
title: À l’aide d’une procédure stockée avec un état de retour | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4b126e95-8458-41d6-af37-fc6662859f19
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 29bb95c06d86ad4d6e45002da1429f6c7d5a5c9e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-a-stored-procedure-with-a-return-status"></a>Utilisation d'une procédure stockée avec retour d'état
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  A [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] une procédure stockée que vous pouvez appeler est celle qui retourne un état ou un paramètre de résultat. Cette procédure permet généralement d'indiquer la réussite ou l'échec de la procédure stockée. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) (classe), que vous pouvez utiliser pour appeler ce type de procédure stockée et traiter les données qu’elle retourne.  
  
 Lorsque vous appelez ce type de procédure stockée à l’aide du pilote JDBC, vous devez utiliser le `call` séquence d’échappement SQL conjointement avec la [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe . La syntaxe de la `call` séquence d’échappement avec un paramètre d’état de retour est le suivant :  
  
 `{[?=]call procedure-name[([parameter][,[parameter]]...)]}`  
  
> [!NOTE]  
>  Pour plus d’informations sur les séquences d’échappement SQL, consultez [à l’aide des séquences d’échappement SQL](../../connect/jdbc/using-sql-escape-sequences.md).  
  
 Lorsque vous construisez la `call` séquence d’échappement, spécifiez le paramètre de retour d’état à l’aide de le ? caractère ? (point d'interrogation). Ce caractère fait office d'espace réservé pour la valeur de paramètre qui est retournée par la procédure stockée. Pour spécifier une valeur pour un paramètre de retour d’état, vous devez spécifier le type de données du paramètre à l’aide de la [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) méthode de la classe SQLServerCallableStatement, avant d’exécuter la procédure stockée.  
  
> [!NOTE]  
>  Lorsque vous utilisez le pilote JDBC avec une base de données SQL Server, la valeur que vous spécifiez pour le paramètre de retour d’état dans la méthode registerOutParameter sera toujours un entier, vous pouvez spécifier à l’aide du type de données java.sql.Types.INTEGER.  
  
 En outre, lorsque vous passez une valeur à la méthode registerOutParameter pour un paramètre de retour d’état, vous devez spécifier non seulement le type de données à utiliser pour le paramètre, mais également la position du paramètre ordinal dans l’appel de procédure stockée. La position ordinale du paramètre de retour d'état est toujours 1 car il s'agit toujours du premier paramètre de l'appel à la procédure stockée. Bien que la classe SQLServerCallableStatement fournit la prise en charge pour utiliser le nom du paramètre pour indiquer le paramètre spécifique, vous pouvez utiliser uniquement le numéro de position ordinale d’un paramètre pour les paramètres d’état de retour.  
  
 Par exemple, créez la procédure stockée suivante dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple :  
  
```  
CREATE PROCEDURE CheckContactCity  
   (@cityName CHAR(50))  
AS  
BEGIN  
   IF ((SELECT COUNT(*)  
   FROM Person.Address  
   WHERE City = @cityName) > 1)  
   RETURN 1  
ELSE  
   RETURN 0  
END  
```  
  
 Cette procédure stockée retourne une valeur de statut 1 ou 0, selon que la ville spécifiée dans le paramètre cityName figure ou non dans la table Person.Address.  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction et la [exécuter](../../connect/jdbc/reference/execute-method-sqlserverstatement.md) méthode est utilisée pour appeler la procédure stockée CheckContactCity :  
  
 [!code[JDBC#UsingSprocWithReturnStatus1](../../connect/jdbc/codesnippet/Java/using-a-stored-procedure_1_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md)  
  
  
