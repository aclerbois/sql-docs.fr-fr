---
title: Connexion et l’extraction de données | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ce43cc20-46a3-42ff-a3fb-75ad1ed10e08
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 86b7dce984408c7d49e302b9b450e07a32fdacc4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-and-retrieving-data"></a>Connexion et extraction de données
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Lorsque vous travaillez avec le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)], il existe deux méthodes principales pour établir une connexion à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données. Consiste à définir les propriétés de connexion dans l’URL de connexion, puis appelez la méthode getConnection de la classe DriverManager pour retourner un [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.  
  
> [!NOTE]  
>  Pour obtenir la liste des propriétés de connexion pris en charge par le pilote JDBC, consultez [définissant les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md).  
  
 La seconde méthode implique la définition des propriétés de connexion à l’aide de méthodes setter de la [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) classe, puis en appelant le [getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md) méthode pour retourner une SQLServerConnection objet.  
  
 Les rubriques de cette section décrivent les différentes façons dans laquelle vous pouvez vous connecter à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données et elles présentent également différentes techniques pour récupérer des données.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Exemple d’URL de connexion](../../../connect/jdbc/connection-url-sample.md)|Décrit comment utiliser une URL de connexion pour se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] , puis utilisez une instruction SQL pour récupérer des données.|  
|[Exemple de source de données](../../../connect/jdbc/data-source-sample.md)|Décrit l'utilisation d'une source de données pour se connecter à SQL Server et l'utilisation d'une procédure stockée pour extraire des données.|  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples d’applications JDBC Driver](../../../connect/jdbc/sample-jdbc-driver-applications.md)  
  
  
