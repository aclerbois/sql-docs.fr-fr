---
title: À l’aide du regroupement de connexions | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 699d4e8a-34bf-4c60-b0d5-4a10dad6084a
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c3b69e6cd19b493120a976c722db9d3efb01f57d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-connection-pooling"></a>Utilisation d'un regroupement de connexions
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit la prise en charge de Java Platform, Enterprise Edition (Java EE) le regroupement de connexions. Le pilote JDBC implémente les interfaces JDBC 3.0 requises pour permettre au pilote de participer aux implémentations de regroupements de connexions fournies par des fournisseurs d'intergiciels (middleware) et compatibles JDBC 3.0. Des intergiciels (middleware) tels que des serveurs d'applications Java EE offrent souvent des fonctionnalités de regroupement de connexions compatibles. Le pilote JDBC prendra part à des connexions regroupées dans ces environnements.  
  
> [!NOTE]  
>  Même si le pilote JDBC prend en charge le regroupement de connexions Java EE, il ne fournit pas sa propre implémentation de regroupement. Le pilote recourt à des serveurs d'applications Java tiers pour gérer les connexions.  
  
## <a name="remarks"></a>Notes  
 Les classes pour l'implémentation du regroupement de connexions sont les suivantes.  
  
|Classe|Implémentations| Description|  
|-----------|----------------|-----------------|  
|com.Microsoft.SqlServer.JDBC. SQLServerXADataSource|javax.sql.ConnectionPoolDataSource et javax.sql.XADataSource|Nous vous recommandons d’utiliser le [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) classe pour tous vos serveurs Java EE a besoin, car elle implémente l’ensemble du regroupement de JDBC 3.0 et des interfaces XA.|  
|com.Microsoft.SqlServer.JDBC. SQLServerConnectionPoolDataSource|javax.sql.ConnectionPoolDataSource|Cette classe est une fabrique de connexions qui permet au serveur d'applications Java EE de peupler son regroupement de connexions à l'aide de connexions physiques. Si la configuration de votre fournisseur de Java EE requiert une classe implémentant javax.sql.ConnectionPoolDataSource, spécifiez le nom de classe [SQLServerConnectionPoolDataSource](../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md). Nous déconseillons généralement que vous utilisez le [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) classe à la place, car il implémente le regroupement et les interfaces XA et a été vérifiée dans davantage de configurations de serveur Java EE.|  
  
 Le code d'application JDBC doit toujours fermer les connexions de façon explicite afin de profiter au maximum du regroupement. Si l'application ferme explicitement une connexion, l'implémentation du regroupement peut réutiliser la connexion immédiatement. Si la connexion n'est pas fermée, d'autres applications ne peuvent pas la réutiliser. Les applications peuvent utiliser le `finally` construction pour s’assurer que les connexions regroupées sont fermées même si une exception se produit.  
  
> [!NOTE]  
>  Le pilote JDBC n'appelle pas actuellement la procédure stockée sp_reset_connection lorsqu'il retourne la connexion au regroupement. Au lieu de cela, le pilote recourt à des serveurs d'applications Java tiers pour rétablir les connexions à leur état original.  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
  
  
