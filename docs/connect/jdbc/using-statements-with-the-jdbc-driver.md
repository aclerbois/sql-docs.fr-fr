---
title: Utilisation d’instructions avec le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7f8f3e8f-841e-4449-9154-b5366870121f
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c7da2ce5f69df8f28b281a935ea938b2648fa555
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-statements-with-the-jdbc-driver"></a>Utilisation d'instructions avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] peut être utilisé pour manipuler des données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données de différentes façons. Vous pouvez utiliser le pilote JDBC pour appliquer des instructions SQL à la base de données ou pour appeler des procédures stockées dans la base de données en utilisant des paramètres d'entrée et de sortie. Le pilote JDBC prend également en charge l'utilisation de séquences d'échappement SQL, de nombres de mises à jour, de clés générées automatiquement, ainsi que l'exécution de mises à jour dans le cadre d'une opération par lot.  
  
 Le pilote JDBC fournit trois classes pour récupérer des données d’une [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données :  
  
1.  [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) : permet d’exécuter des instructions SQL sans paramètres.  
  
2.  [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) - (hérité de SQLServerStatement), utilisé pour les instructions SQL compilées en cours d’exécution qui peuvent contenir des paramètres IN.  
  
3.  [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) - (héritée de SQLServerPreparedStatement), utilisé pour l’exécution des procédures stockées qui peuvent contenir de paramètres, les paramètres OUT, ou les deux.  
  
 Les rubriques de cette section expliquent comment vous pouvez utiliser chacune des trois classes d’instructions pour travailler avec des données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Utilisation d’instructions avec SQL](../../connect/jdbc/using-statements-with-sql.md)|Décrit comment utiliser des instructions SQL avec le pilote JDBC pour travailler avec des données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.|  
|[Utilisation d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md)|Décrit comment utiliser des procédures stockées avec le pilote JDBC pour travailler avec des données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.|  
|[Utilisation de plusieurs jeux de résultats](../../connect/jdbc/using-multiple-result-sets.md)|Décrit l'utilisation du pilote JDBC pour extraire des données de plusieurs jeux de résultats.|  
|[Utilisation de séquences d’échappement SQL](../../connect/jdbc/using-sql-escape-sequences.md)|Décrit l'utilisation de séquences d'échappement SQL, telles que les littéraux et fonctions de date et d'heure.|  
|[Utilisation de clés générées automatiquement](../../connect/jdbc/using-auto-generated-keys.md)|Décrit l'utilisation des clés générées automatiquement.|  
|[Exécution d’opérations par lot](../../connect/jdbc/performing-batch-operations.md)|Décrit l'utilisation du pilote JDBC pour effectuer des opérations par lot.|  
|[Gestion des instructions complexes](../../connect/jdbc/handling-complex-statements.md)|Décrit l'utilisation du pilote JDBC pour exécuter des instructions complexes effectuant plusieurs tâches et pouvant retourner différents types de données.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
