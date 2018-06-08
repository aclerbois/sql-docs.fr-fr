---
title: Configurer la façon dont les valeurs java.sql.Time sont envoyées au serveur | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 07eb00dd-621a-46f9-a5a5-8cab4d6058b5
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8e2e91550767715616599c2720c99b864363a185
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>Configuration du mode d'envoi des valeurs java.sql.Time au serveur
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Si vous utilisez un objet java.sql.Time ou le type JDBC java.sql.Types.TIME pour définir un paramètre, vous pouvez configurer la façon dont la valeur java.sql.Time est envoyée au serveur ; comme un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **temps** type ou en tant qu’un **datetime** type.  
  
 Ce scénario s'applique lors de l'utilisation de l'une des méthodes suivantes :  
  
-   [SQLServerCallableStatement.registerOutParameter (int, int)](../../connect/jdbc/reference/registeroutparameter-method-int-int.md)  
  
-   [SQLServerCallableStatement.registerOutParameter (int, int, int)](../../connect/jdbc/reference/registeroutparameter-method-int-int-int.md)  
  
-   [SQLServerCallableStatement.setTime](../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)  
  
-   [SQLServerPreparedStatement.setTime](../../connect/jdbc/reference/settime-method-sqlserverpreparedstatement.md)  
  
-   [SQLServerCallableStatement.setObject](../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)  
  
-   [SQLServerPreparedStatement.setObject](../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)  
  
 Vous pouvez configurer la façon dont la valeur java.sql.Time est envoyée à l’aide de la **sendTimeAsDatetime** propriété de connexion. Pour plus d’informations, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
 Vous pouvez modifier par programmation la valeur de la **sendTimeAsDatetime** propriété de connexion avec [SQLServerDataSource.setSendTimeAsDatetime](../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] antérieure [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)] ne gèrent pas la **temps** des valeurs en tant que type de données, pour les applications utilisant java.sql.Time stockent généralement java.sql.Time **datetime** ou **smalldatetime** [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données.  
  
 Si vous souhaitez utiliser le **datetime** et **smalldatetime** [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] les types de données lorsque vous travaillez avec des valeurs java.sql.Time, vous devez définir le **sendTimeAsDatetime** propriété de connexion **true**. Si vous souhaitez utiliser le **temps** [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données fonctionne avec des valeurs java.sql.Time, vous devez définir le **sendTimeAsDatetime** propriété de connexion **false**.  
  
 Sachez que lorsque vous envoyez des valeurs java.sql.Time dans un paramètre dont le type de données peut stocker également la date, par défaut des dates sont différentes selon que la valeur java.sql.Time est envoyée en tant qu’un **datetime** (1/1/1970) ou **temps** les valeur (1/1/1900). Pour plus d’informations sur les conversions de données lors de l’envoi de données à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], consultez [à l’aide de données de Date et heure](http://go.microsoft.com/fwlink/?LinkID=145211).  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 3.0 du pilote JDBC, **sendTimeAsDatetime** a la valeur true par défaut. Dans une version ultérieure, le **sendTimeAsDatetime** propriété de connexion peut être définie sur false par défaut.  
  
 Pour garantir que votre application continue à fonctionner comme prévu quelle que soit la valeur par défaut de la **sendTimeAsDatetime** propriété de connexion que vous pouvez :  
  
-   Utilisez java.sql.Time avec le **temps** [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
-   Utilisez java.sql.Timestamp lorsque vous travaillez avec le **datetime**, **smalldatetime**, et **datetime2** [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données.  
  
Notez que sendTimeAsDatetime doit être false pour les colonnes chiffrées, comme des colonnes chiffrées ne prennent pas en charge la conversion à partir de l’heure en datetime. À partir de Microsoft JDBC Driver 6.0 pour SQL Server, la classe SQLServerConnection a deux méthodes suivantes pour la valeur de la propriété sendTimeAsDatetime de set/get.

```
  public boolean getSendTimeAsDatetime()
  public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)
```
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
