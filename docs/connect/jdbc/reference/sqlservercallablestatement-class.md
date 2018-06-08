---
title: Classe SQLServerCallableStatement | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 30710a63-c05d-47d9-9cf9-c087a1c76373
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b3813574070c53f16cd04ff262d7134c87d4ea85
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlservercallablestatement-class"></a>Classe SQLServerCallableStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Permet de spécifier le nom de la procédure stockée à appeler avec les paramètres d'entrée et de sortie. Cette classe offre également la possibilité de récupérer la valeur de l'état retourné avec la syntaxe ? = call( ?, ..) syntax.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Implémente :** [ISQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
 **Étend :** [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final class SQLServerCallableStatement  
```  
  
## <a name="remarks"></a>Notes  
 SQLServerCallableStatement vous permet de spécifier le nom de la procédure stockée à appeler avec les paramètres d’entrée et de sortie. SQLServerCallableStatement fournit également la possibilité de récupérer la valeur de retour d’état avec le `? = call( ?, ..)` syntaxe.  
  
 Cette classe prend en charge la Désencapsulation dans la classe SQLServerCallableStatement, interface ISQLServerCallableStatement, l’interface java.sql.CallableStatement et les classes et interfaces prises en charge par SQLServerPreparedStatement pour la Désencapsulation. Pour plus d’informations, consultez [Wrappers et Interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
 Lorsque un des SQLServerCallableStatement définie des méthodes est appelé pour un type, que ce type est en conflit avec le type spécifié avec [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md), le type spécifié par la dernière méthode set SQLServerCallableStatement est utilisé. Toutefois, cela peut entraîner des erreurs dues à une conversion de types de données qui sont incompatibles. Si un SQLServerCallableStatement définie la méthode n'est pas appelée, le type spécifié avec la première [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) appel est utilisé.  
  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] version 3.0 du pilote JDBC est conforme à la recommandation JDBC 4.0 qui a un jeu de résultats et de mises à jour multiples doivent être récupérés avant de récupérer les paramètres de sortie. Si les paramètres OUT sont récupérés avant que le jeu de résultats et les mises à jour n'aient été complètement traités, les résultats et les mises à jour qui n'auront pas été traités seront perdus.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Référence d’API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
