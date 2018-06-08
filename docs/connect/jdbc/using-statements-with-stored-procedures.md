---
title: À l’aide d’instructions avec des procédures stockées | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 0041f9e1-09b6-4487-b052-afd636c8e89a
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a2e5ff4dc6ffa18d553b0b0e3bbe0c9c3a629920
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-statements-with-stored-procedures"></a>Utilisation d'instructions avec des procédures stockées
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Une procédure stockée est une procédure de base de données, similaire à une procédure dans d'autres langages de programmation, qui est contenue dans la base de données elle-même. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], procédures stockées peuvent être créées à l’aide de [!INCLUDE[tsql](../../includes/tsql_md.md)], en utilisant le common language runtime (CLR) et l’autre de Visual Studio langages de programmation tels que Visual Basic ou c#. En règle générale, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des procédures stockées peuvent procédez comme suit :  
  
-   accepter des paramètres d'entrée et retourner plusieurs valeurs sous la forme de paramètres de sortie à la procédure ou au lot appelant ;  
  
-   contenir des instructions de programmation qui exécutent des opérations dans la base de données, y compris l'appel d'autres procédures ;  
  
-   retourner une valeur d'état à une procédure ou à un lot appelant pour indiquer une réussite ou un échec (et la raison de l'échec).  
  
> [!NOTE]  
>  Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des procédures stockées, consultez « Procédures stockées » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
 Pour utiliser des données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données à l’aide d’une procédure stockée, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), et [ SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classes. La classe que vous utilisez dépend des paramètres d'entrée (IN) ou de sortie (OUT) requis par la procédure stockée. Si la procédure stockée ne requiert aucun IN ou OUT, vous pouvez utiliser la classe SQLServerStatement ; Si la procédure stockée est appelée plusieurs fois, ou requiert uniquement des paramètres IN, vous pouvez utiliser la classe SQLServerPreparedStatement. Si la procédure stockée requiert à la fois dans et les paramètres de sortie, vous devez utiliser la classe SQLServerCallableStatement. Il s’agit uniquement lorsque la procédure stockée requiert des paramètres de sortie que vous devez la surcharge de la classe SQLServerCallableStatement.  
  
> [!NOTE]  
>  Les procédures stockées peuvent également retourner des nombres de mises à jour et des jeux de résultats multiples. Pour plus d’informations, consultez [à l’aide d’une procédure stockée avec un nombre de mises à jour](../../connect/jdbc/using-a-stored-procedure-with-an-update-count.md) et [à l’aide de plusieurs jeux de résultats](../../connect/jdbc/using-multiple-result-sets.md).  
  
 Lorsque vous utilisez le pilote JDBC pour appeler une procédure stockée avec des paramètres, vous devez utiliser le `call` séquence d’échappement SQL conjointement avec la [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. La syntaxe complète de la `call` séquence d’échappement est comme suit :  
  
 `{[?=]call procedure-name[([parameter][,[parameter]]...)]}`  
  
> [!NOTE]  
>  Pour plus d’informations sur la `call` et autres SQL séquences d’échappement, consultez [à l’aide des séquences d’échappement SQL](../../connect/jdbc/using-sql-escape-sequences.md).  
  
 Les rubriques de cette section décrivent les méthodes que vous pouvez appeler [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] procédures stockées à l’aide du pilote JDBC et `call` séquence d’échappement SQL.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Utilisation d’une procédure stockée sans paramètres](../../connect/jdbc/using-a-stored-procedure-with-no-parameters.md)|Décrit la méthode d'utilisation du pilote JDBC pour exécuter des procédures stockées ne contenant pas de paramètres d'entrée ou de sortie.|  
|[Utilisation d’une procédure stockée avec des paramètres d’entrée](../../connect/jdbc/using-a-stored-procedure-with-input-parameters.md)|Décrit la méthode d'utilisation du pilote JDBC pour exécuter des procédures stockées contenant des paramètres d'entrée.|  
|[Utilisation d’une procédure stockée avec des paramètres de sortie](../../connect/jdbc/using-a-stored-procedure-with-output-parameters.md)|Décrit la méthode d'utilisation du pilote JDBC pour exécuter des procédures stockées contenant des paramètres de sortie.|  
|[Utilisation d’une procédure stockée avec retour d’état](../../connect/jdbc/using-a-stored-procedure-with-a-return-status.md)|Décrit la méthode d'utilisation du pilote JDBC pour exécuter des procédures stockées contenant des valeurs de retour d'état.|  
|[Utilisation d’une procédure stockée avec un compteur des mises à jour](../../connect/jdbc/using-a-stored-procedure-with-an-update-count.md)|Décrit la méthode d'utilisation du pilote JDBC pour exécuter des procédures stockées qui retournent des nombres de mises à jour.|  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec le pilote JDBC](../../connect/jdbc/using-statements-with-the-jdbc-driver.md)  
  
  
