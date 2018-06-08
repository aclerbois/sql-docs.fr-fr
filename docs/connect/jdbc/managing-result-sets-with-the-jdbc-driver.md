---
title: Résultat de la gestion des jeux avec le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9ed5ad41-22e0-4e4a-8a79-10512db60d50
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6e428a66a506005cdbde4fbc58b724e8b22972ab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="managing-result-sets-with-the-jdbc-driver"></a>Gestion de jeux de résultats avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le jeu de résultats est un objet représentant un ensemble de données retourné par une source de données, généralement à la suite d'une requête. Le jeu de résultats contient des lignes et des colonnes destinées à contenir des éléments de données requis et dans lesquelles il est possible de naviguer à l'aide d'un curseur. Un jeu de résultats peut être mis à jour, ce qui signifie qu'il est possible de le modifier et de faire en sorte que ces modifications soient répercutées dans la source de données originale. Un jeu de résultats peut également présenter plusieurs niveaux de sensibilité aux modifications apportées à la source de données sous-jacente.  
  
 Le type de jeu de résultats est déterminé lors de la création d’une instruction, c'est-à-dire lorsqu’un appel à la [createStatement](../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe est rendue. Le rôle fondamental d'un jeu de résultats est de fournir des applications Java avec une représentation utilisable du contenu de la base de données. Cela est généralement effectué à l'aide des méthodes getter et setter entrées dans les éléments de données du jeu de résultats.  
  
 Dans l’exemple suivant, qui est basé sur le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple, un jeu de résultats est créé en appelant le [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe. Les données du jeu de résultats sont ensuite affichées à l’aide du [getString](../../connect/jdbc/reference/getstring-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe.  
  
 [!code[JDBC#ManagingResultSets1](../../connect/jdbc/codesnippet/Java/managing-result-sets-with-t_1.java)]  
  
 Les rubriques de cette section décrivent divers aspects liés à l'utilisation du jeu de résultats, notamment les types de curseur, les accès simultanés et le verrouillage de ligne.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Présentation des types de curseurs](../../connect/jdbc/understanding-cursor-types.md)|Décrit les différents types de curseurs le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge.|  
|[Présentation du contrôle d’accès concurrentiel](../../connect/jdbc/understanding-concurrency-control.md)|Décrit la manière dont le pilote JDBC prend en charge le contrôle de concurrence.|  
|[Présentation du verrouillage des lignes](../../connect/jdbc/understanding-row-locking.md)|Décrit la manière dont le pilote JDBC prend en charge le verrouillage de ligne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
