---
title: Membres de SQLServerDataSource | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7e749bc5-d765-4864-be2b-7822d4c20c09
caps.latest.revision: 43
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 52f0cd9f341e7bc2ee4c45997542057d7e32983d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverdatasource-members"></a>Membres de SQLServerDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres exposés par le [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
  
|Nom| Description|  
|----------|-----------------|  
|[SQLServerDataSource ()](../../../connect/jdbc/reference/sqlserverdatasource-constructor.md)|Initialise une nouvelle instance de la [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) classe.|  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
 Aucun.  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|Retourne la valeur de la **applicationIntent** propriété de connexion.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|Renvoie le nom de l'application.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|Tente d’établir une connexion avec les données source que ce [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet représente.|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|Retourne le nom de la base de données.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverdatasource.md)|Retourne la valeur de **disableStatementPooling** propriété de connexion. Ce paramètre contrôle si le regroupement d’instructions est activé ou non pour cette connexion.|  
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|Retourne la valeur de **enablePrepareOnFirstPreparedStatementCall** propriété de connexion.|  
|[getEncrypt](../../../connect/jdbc/reference/getencrypt-method-sqlserverdatasource.md)|Retourne un **booléenne** valeur indiquant si la propriété encrypt est activée.|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|Retourne une description de la source de données.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|Retourne le nom du serveur de basculement utilisé dans la configuration de la mise en miroir de bases de données.|  
|[getHostNameInCertificate](../../../connect/jdbc/reference/gethostnameincertificate-method-sqlserverdatasource.md)|Retourne le nom d'hôte utilisé dans la validation du certificat SSL (Secure Sockets Layer) SQL Server.|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|Retourne le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] nom de l’instance.|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|Retourne un **booléenne** valeur indiquant si la propriété lastUpdateCount est activée.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|Retourne un **int** valeur indiquant le nombre de millisecondes d’attente de la base de données avant de signaler un délai de verrouillage.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|Retourne le nombre de secondes pendant lesquelles cet [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet attend lors de la tentative établir une connexion.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|Retourne un flux de sortie de caractères à utiliser pour tous les messages de journalisation et de suivi.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|Retourne la valeur de la **multiSubnetFailover** propriété de connexion.|  
|[getPacketSize](../../../connect/jdbc/reference/getpacketsize-method-sqlserverdatasource.md)|Retourne la taille du paquet réseau actuelle utilisée pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)], spécifié en octets.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|Retourne le numéro de port utilisé pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md)|Retourne une référence à ce [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverdatasource.md)|Retourne la réponse mise en mémoire tampon de mode pour cette [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet.|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|Retourne le type de curseur par défaut utilisé pour tous les jeux de résultats créés à l’aide de ce [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet.|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|Retourne un **booléenne** valeur indiquant si l’envoi de paramètres de chaîne au serveur au format UNICODE est activé.|  
|[getSendTimeAsDatetime](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|Retourne le paramètre de la **SendTimeAsDatetime** propriété de connexion.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|Retourne le nom de l’ordinateur en cours d’exécution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|Retourne la valeur de **serverPreparedStatementDiscardThreshold** propriété de connexion.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverdatasource.md)|Retourne la taille du cache d’instruction préparée pour cette connexion.|  
|[getTrustManagerClass](../../../connect/jdbc/reference/gettrustmanagerclass-method-sqlserverdatasource.md)|Retourne la valeur de chaîne de la propriété de connexion TrustManagerClass.|  
|[getTrustManagerConstructorArg](../../../connect/jdbc/reference/gettrustmanagerconstructorarg-method-sqlserverdatasource.md)|Retourne la valeur de chaîne de la propriété de connexion TrustManagerConstructorArg.|  
|[getTrustServerCertificate](../../../connect/jdbc/reference/gettrustservercertificate-method-sqlserverdatasource.md)|Retourne un **booléenne** valeur indiquant si la propriété trustServerCertificate est activée.|  
|[getTrustStore](../../../connect/jdbc/reference/gettruststore-method-sqlserverdatasource.md)|Retourne le chemin d'accès (y compris le nom de fichier) au fichier trustStore de certificat.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|Retourne l’URL utilisée pour se connecter à la source de données.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|Retourne le nom d'utilisateur utilisé pour la connexion à la source de données.|  
|[getUseSQLServerBaseDate](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|Retourne le paramètre de la propriété de connexion useSQLServerBaseDate.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|Retourne le nom du nom d’ordinateur client utilisé pour se connecter à la source de données.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|Retourne un **booléenne** valeur indiquant si la conversion d’états SQL en États compatibles XOPEN est activée.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverdatasource.md)|Indique si cet objet de source de données est un wrapper pour l'interface spécifiée.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|Définit la valeur de la **applicationIntent** propriété de connexion.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|Définit le nom de l'application.|  
|[setAuthenticationScheme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|Indique le genre de sécurité intégrée que votre application doit utiliser.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|Définit le nom de la base de données à laquelle se connecter.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|Définit la description de la source de données.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverdatasource.md)|Définit le regroupement d’instructions à true ou false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|Spécifie la nouvelle valeur de la **enablePrepareOnFirstPreparedStatementCall** propriété de connexion.|  
|[setEncrypt](../../../connect/jdbc/reference/setencrypt-method-sqlserverdatasource.md)|Définit un **booléenne** valeur indiquant si la propriété encrypt est activée.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|Définit le nom du serveur de basculement utilisé dans la configuration de la mise en miroir de bases de données.|  
|[setHostNameInCertificate](../../../connect/jdbc/reference/sethostnameincertificate-method-sqlserverdatasource.md)|Définit le nom d'hôte à utiliser pour valider le certificat SSL (Secure Sockets Layer) SQL Server.|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|Définit le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] nom de l’instance.|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|Définit un **booléenne** valeur indiquant si la propriété integratedSecurity est activée.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|Définit un **booléenne** valeur indiquant si la propriété lastUpdateCount est activée.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|Définit un **int** valeur indiquant le nombre de millisecondes à attendre avant que la base de données signale un délai de verrouillage.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|Définit le nombre de secondes que cela [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet attend lors de la tentative établir une connexion.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|Définit un flux de sortie de caractères à utiliser pour tous les messages de journalisation et de suivi.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|Définit la valeur de la **multiSubnetFailover** propriété de connexion.|  
|[setPacketSize](../../../connect/jdbc/reference/setpacketsize-method-sqlserverdatasource.md)|Définit la taille du paquet réseau actuelle utilisée pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)], spécifié en octets.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|Définit le mot de passe utilisé pour se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|Définit le numéro de port utilisé pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverdatasource.md)|Définit la réponse mise en mémoire tampon pour les connexions créées à l’aide de ce mode de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet.|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|Définit le type de curseur par défaut utilisé pour tous les jeux de résultats créés à l’aide de ce [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) objet.|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|Définit un **booléenne** valeur indiquant si l’envoi de paramètres de chaîne au serveur au format UNICODE est activé.|  
|[setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md)|Spécifie comment envoyer des valeurs java.sql.Time au serveur.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|Définit le nom de l’ordinateur en cours d’exécution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|Définit la nouvelle valeur de la **serverPreparedStatementDiscardThreshold** propriété de connexion.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverdatasource.md)|Définit la taille du cache d’instruction préparée pour cette connexion.|  
|[setTrustManagerClass](../../../connect/jdbc/reference/settrustmanagerclass-method-sqlserverdatasource.md)|Définit la valeur de chaîne de la propriété de connexion TrustManagerClass.|  
|[setTrustManagerConstructorArg](../../../connect/jdbc/reference/settrustmanagerconstructorarg-method-sqlserverdatasource.md)|Définit la valeur de chaîne de la propriété de connexion TrustManagerConstructorArg.|  
|[setTrustServerCertificate](../../../connect/jdbc/reference/settrustservercertificate-method-sqlserverdatasource.md)|Définit un **booléenne** valeur indiquant si la propriété trustServerCertificate est activée.|  
|[setTrustStore](../../../connect/jdbc/reference/settruststore-method-sqlserverdatasource.md)|Définit le chemin d'accès (y compris le nom de fichier) au fichier trustStore de certificat.|  
|[setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md)|Définit le mot de passe utilisé pour vérifier l'intégrité des données trustStore.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|Définit l'URL utilisée pour la connexion à la source de données.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|Définit le nom d’utilisateur utilisé pour se connecter à la source de données.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|Définit le nom de l'ordinateur client utilisé pour la connexion à la source de données.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|Définit un **booléenne** valeur indiquant si la conversion d’états SQL en États compatibles XOPEN est activée.|  
|[Unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverdatasource.md)|Retourne un objet qui implémente l’interface spécifiée pour autoriser l’accès à la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-des méthodes spécifiques.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
