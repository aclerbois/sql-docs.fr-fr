---
title: Membres de SQLServerXADataSource | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 04178645-915f-4569-8907-d45e299bbe7d
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7b41b37922855c65d9e02f7443aae8401c033e5d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverxadatasource-members"></a>Membres de SQLServerXADataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
  
|Nom| Description|  
|----------|-----------------|  
|[SQLServerXADataSource ()](../../../connect/jdbc/reference/sqlserverxadatasource-constructor.md)|Initialise une nouvelle instance de la [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) classe.|  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
 Aucun.  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne la valeur de la **applicationIntent** propriété de connexion.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) renvoie le nom de l’application.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) essaie d’établir une connexion avec la source de données représentée par cet objet de source de données.|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le nom de la base de données.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le nom du serveur de basculement qui est utilisé dans une configuration de mise en miroir de base de données.|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] nom de l’instance.|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne un **booléenne** valeur qui indique si la propriété lastUpdateCount est activée.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne un **int** valeur qui indique le nombre de millisecondes pendant lesquelles la base de données attend avant de signaler un délai de verrouillage.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le nombre de secondes pendant lesquelles cet objet de source de données doit patienter lors de la tentative établir une connexion.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne un flux de sortie de caractères à utiliser pour tous les messages de journalisation et le suivi.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) récupère la valeur de la **multiSubnetFailover** propriété de connexion.|  
|[getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)|(Héritée de [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)) essaie d’établir une connexion de base de données physique qui peut être utilisée en tant qu’une connexion regroupée.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) renvoie le numéro de port qui est utilisé pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverxadatasource.md)|Retourne une référence à ce [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) objet.|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le type de curseur par défaut qui est utilisé pour tous les jeux de résultats qui sont créés à l’aide de cet objet de source de données.|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne un **booléenne** valeur qui indique si l’envoi de **chaîne** paramètres au serveur au format UNICODE est activé.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le nom de l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) renvoie l’URL qui est utilisé pour se connecter à la source de données.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) renvoie le nom d’utilisateur qui est utilisé pour se connecter à la source de données.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne le nom du client de nom de l’ordinateur qui est utilisé pour se connecter à la source de données.|  
|[getXAConnection](../../../connect/jdbc/reference/getxaconnection-method-sqlserverxadatasource.md)|Essaie d'établir une connexion de base de données physique qui peut être utilisée dans une transaction distribuée.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) retourne un **booléenne** valeur qui indique si la conversion d’états SQL en États compatibles XOPEN est activée.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverxadatasource.md)|Indique si cet objet est un wrapper pour l'interface spécifiée.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit la valeur de la **applicationIntent** propriété de connexion.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le nom de l’application.|  
|[setAuthenticationSceme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) indique le type de sécurité intégrée que vous souhaitez que votre application à utiliser.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le nom de la base de données pour se connecter à.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit la description de la source de données.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le nom du serveur de basculement qui est utilisé dans une configuration de mise en miroir de base de données.|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] nom de l’instance.|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit un **booléenne** valeur qui indique si la propriété integratedSecurity est activée.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit un **booléenne** valeur qui indique si la propriété lastUpdateCount est activée.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit un **int** valeur qui indique le nombre de millisecondes à attendre avant que la base de données signale un délai de verrouillage.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le nombre de secondes pendant lesquelles cet objet de source de données doit patienter lors de la tentative établir une connexion.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit un flux de sortie de caractères à utiliser pour tous les messages de journalisation et le suivi.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit la valeur de la **multiSubnetFailover** propriété de connexion.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le mot de passe qui servira à se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le numéro de port à utiliser pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le type de curseur par défaut qui est utilisé pour tous les jeux de résultats qui sont créés à l’aide de cet objet de source de données.|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit un **booléenne** valeur qui indique lors de l’envoi **chaîne** paramètres au serveur au format UNICODE est activé.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le nom de l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit l’URL qui est utilisé pour se connecter à la source de données.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le nom d’utilisateur qui est utilisé pour se connecter à la source de données.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit le client de nom de l’ordinateur qui est utilisé pour se connecter à la source de données.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) définit un **booléenne** valeur qui indique si la conversion d’états SQL en États compatibles XOPEN est activée.|  
|[Unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md)|Retourne un objet qui implémente l’interface spécifiée pour autoriser l’accès à la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-des méthodes spécifiques.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource|getPooledConnection|  
|com.microsoft.sqlserver.jdbc.SQLServerDataSource|getApplicationName, getConnection, getDatabaseName, getDescription, getFailoverPartner, getInstanceName, getLastUpdateCount, getLockTimeout, getLoginTimeout, getLogWriter, getPortNumber, getSelectMethod, getSendStringParametersAsUnicode, getServerName, getURL, getUser, getWorkstationID, getXopenStates, setApplicationName, setDatabaseName, setDescription, setFailoverPartner, setInstanceName, setIntegratedSecurity, setLastUpdateCount, setLockTimeout, setLoginTimeout, setLogWriter, setPassword, setPortNumber, setSelectMethod, setSendStringParametersAsUnicode, setServerName, setURL, setUser, setWorkstationID, setXopenStates|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
|javax.sql.XADataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter|  
|javax.sql.ConnectionPoolDataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerXADataSource, classe](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)  
  
  
