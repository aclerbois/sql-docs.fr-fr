---
title: Présentation des Transactions | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d3e0414c-6809-4bb1-93b1-4960507faecc
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6045c482a931329e3d62c49dedea7ea86a14c545
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-transactions"></a>Présentation des Transactions
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les transactions sont des groupes d'opérations combinés en unités logiques de travail. Elles sont utilisées pour contrôler et maintenir la cohérence et l'intégrité de chaque action dans une transaction, malgré les erreurs pouvant se produire dans le système.  
  
 Avec la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], les transactions peuvent être locales ou distribuées. Les transactions peuvent également utiliser des niveaux d'isolation. Pour plus d’informations sur les niveaux d’isolation pris en charge par le pilote JDBC, consultez [présentation des niveaux d’Isolation de](../../connect/jdbc/understanding-isolation-levels.md).  
  
 Les applications doivent contrôler les transactions en utilisant des instructions Transact-SQL ou les méthodes fournies par le pilote JDBC, mais pas les deux. L'utilisation des instructions Transact-SQL et des méthodes de l'API JDBC sur la même transaction peut entraîner des problèmes, par exemple une transaction qui ne peut pas être validée comme prévu, une transaction qui est validée ou restaurée, et une nouvelle qui démarre de façon inattendue, ou des exceptions de type « Le serveur n'a pas pu reprendre la transaction ».  
  
## <a name="using-local-transactions"></a>Utilisation de transactions locales  
 Une transaction est considérée comme locale lorsqu'il s'agit d'une transaction à phase unique traitée directement par la base de données. Le pilote JDBC prend en charge les transactions locales à l’aide de différentes méthodes de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe, y compris [setAutoCommit](../../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md), [validation](../../connect/jdbc/reference/commit-method-sqlserverconnection.md)et [restauration](../../connect/jdbc/reference/rollback-method.md). Les transactions locales sont généralement gérées explicitement par l'application ou automatiquement par le serveur d'applications Java EE (Java Platform, Enterprise Edition).  
  
 L’exemple suivant effectue une transaction locale comprenant deux instructions distinctes dans la `try` bloc. Les instructions sont exécutées sur la table Production.ScrapReason dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données et elles sont validées si aucune exception n’est levée. Le code dans le `catch` bloc annule la transaction si une exception est levée.  
  
 [!code[JDBC#UnderstandingTransactions1](../../connect/jdbc/codesnippet/Java/understanding-transactions_1.java)]  
  
## <a name="using-distributed-transactions"></a>Utilisation de transactions distribuées  
 Une transaction distribuée met à jour les données sur au moins deux bases de données en réseau, tout en conservant les propriétés ACID (Atomicité, Cohérence, Isolation et Durabilité) importantes du traitement de transactions. La prise en charge de transactions distribuées a été ajoutée à l'API de JDBC dans la spécification de l'API facultative de JDBC 2.0. Les transactions distribuées sont généralement gérées automatiquement par le gestionnaire de transactions JTS (Java Transaction Service) dans l'environnement d'un serveur d'applications Java EE. Toutefois, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge les transactions distribuées dans tout gestionnaire de transactions compatible avec les API de Transaction Java (JTA).  
  
 Le pilote JDBC s’intègre parfaitement avec [!INCLUDE[msCoName](../../includes/msconame_md.md)] Distributed Transaction Coordinator (MS DTC) pour offrir une véritable distributed prise en charge de la transaction avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. MS DTC est une fonctionnalité de transactions distribuées fournie par [!INCLUDE[msCoName](../../includes/msconame_md.md)] pour [!INCLUDE[msCoName](../../includes/msconame_md.md)] systèmes Windows. MS DTC utilise la technologie de traitement des transactions éprouvée de [!INCLUDE[msCoName](../../includes/msconame_md.md)] pour prendre en charge des fonctionnalités XA tels que le protocole de validation distribué complet à deux phases et la récupération des transactions distribuées.  
  
 Pour plus d’informations sur l’utilisation des transactions distribuées, consultez [compréhension des Transactions XA](../../connect/jdbc/understanding-xa-transactions.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Réalisation de transactions avec le pilote JDBC](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
  
  
