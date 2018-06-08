---
title: Membres de SQLServerConnection | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3115a533-756b-4c78-aee9-4ba7253c85e0
caps.latest.revision: 25
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5a0d90e2b6b6c0faaa1dd8d6376b11b95a2ad523
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverconnection-members"></a>Membres de SQLServerConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
  
|Nom| Description|  
|----------|-----------------|  
|[TRANSACTION_SNAPSHOT](../../../connect/jdbc/reference/transaction-snapshot-field-sqlserverconnection.md)|Utilisé pour spécifier le niveau d'isolation de la transaction d'instantané.|  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Classe héritée de :| Description|  
|---------------------------|-----------------|  
|java.sql.Connection|TRANSACTION_NONE, TRANSACTION_READ_COMMITTED, TRANSACTION_READ_UNCOMMITTED, TRANSACTION_REPEATABLE_READ, TRANSACTION_SERIALIZABLE|  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverconnection.md)|Efface tous les avertissements signalés pour cette [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[Fermer](../../../connect/jdbc/reference/close-method-sqlserverconnection.md)|Mises à jour de la base de données pour ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) ressources et JDBC objet immédiatement au lieu de patienter jusqu'à leur libération automatique.|  
|[closeUnreferencedPreparedStatementHandles](../../../connect/jdbc/reference/closeunreferencedpreparedstatementhandles-method-sqlserverconnection.md)|Force l’annulation-préparer des demandes pour toute instruction préparée rejetées en suspens doit être exécutée.| 
|[Validation](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md)|Rend toutes les modifications apportées depuis le précédent commit ou rollback permanentes et libère tous les verrous de base de données qui sont actuellement détenus par ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md)|Crée un **java.sql.Blob** objet sans aucune donnée.|  
|[createClob](../../../connect/jdbc/reference/createclob-method-sqlserverconnection.md)|Crée un **java.sql.Clob** objet sans aucune donnée.|  
|[createNClob](../../../connect/jdbc/reference/createnclob-method-sqlserverconnection.md)|Crée un **java.sql.NClob** objet sans aucune donnée.|  
|[createStatement](../../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md)|Crée un [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet pour l’envoi d’instructions SQL à la base de données.|  
|[createSQLXML](../../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md)|Crée un **java.sql.SQLXML** objet sans aucune donnée.|  
|[getAutoCommit](../../../connect/jdbc/reference/getautocommit-method-sqlserverconnection.md)|Récupère le mode de validation automatique actuel pour cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[getCatalog](../../../connect/jdbc/reference/getcatalog-method-sqlserverconnection.md)|Récupère le nom du catalogue en cours pour ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[getclientconnectionid, méthode &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/getclientconnectionid-method-sqlserverconnection.md)|Obtient l'ID de la tentative de connexion la plus récente, que cette tentative ait réussi ou non.|  
|[getClientInfo](../../../connect/jdbc/reference/getclientinfo-method-sqlserverconnection.md)|Récupère les informations concernant les propriétés d'informations client prises en charge par le pilote JDBC.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverconnection.md)|Retourne la valeur de **disableStatementPooling** propriété de connexion. Ce paramètre contrôle si le regroupement d’instructions est activé ou non pour cette connexion.|
|[getDiscardedServerPreparedStatementCount](../../../connect/jdbc/reference/getdiscardedserverpreparedstatementcount-method-sqlserverconnection.md)|Retourne le nombre d’en suspens préparée instruction ces actions.|
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Retourne la valeur de **enablePrepareOnFirstPreparedStatementCall** propriété de connexion.|
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverconnection.md)|Récupère la fonctionnalité actuelle des [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets qui sont créés à l’aide de ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverconnection.md)|Récupère un [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) objet qui contient des métadonnées sur la base de données à laquelle cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet représente une connexion.|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Retourne la valeur de **serverPreparedStatementDiscardThreshold** propriété de connexion.|  
|[getStatementHandleCacheEntryCount](../../../connect/jdbc/reference/getstatementhandlecacheentrycount-method-sqlserverconnection.md)|Retourne le nombre actuel de handles d’instructions préparées mis en pool.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverconnection.md)|Retourne la taille du cache d’instruction préparée pour cette connexion.|  
|[getTransactionIsolation](../../../connect/jdbc/reference/gettransactionisolation-method-sqlserverconnection.md)|Récupère le niveau d’isolation de transaction en cours pour ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[getTypeMap](../../../connect/jdbc/reference/gettypemap-method-sqlserverconnection.md)|Récupère l’objet Map est associé à ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverconnection.md)|Récupère le premier avertissement signalé par des appels sur cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[IsClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverconnection.md)|Indique si cette [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet a été fermé.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverconnection.md)|Indique si cette [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet est en mode lecture seule.|  
|[isStatementPoolingEnabled](../../../connect/jdbc/reference/isstatementpoolingenabled-method-sqlserverconnection.md)|Retourne si le regroupement d’instructions est activé ou non pour cette connexion.|  
|[isValid](../../../connect/jdbc/reference/isvalid-method-sqlserverconnection.md)|Indique si cette [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet n’a pas été fermé et qu’il est toujours valide.|  
|[nativeSQL](../../../connect/jdbc/reference/nativesql-method-sqlserverconnection.md)|Convertit l'instruction SQL donnée en grammaire SQL native du serveur de base de données.|  
|[prepareCall](../../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md)|Crée un [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) objet pour appeler les procédures stockée de la base de données.|  
|[prepareStatement](../../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md)|Crée un [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) objet pour envoyer des instructions SQL paramétrées à la base de données.|  
|[releaseSavepoint](../../../connect/jdbc/reference/releasesavepoint-method-sqlserverconnection.md)|Supprime l’élément spécifié [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) objet à partir de la transaction en cours.|  
|[Restauration](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md)|Annule toutes les modifications effectuées dans la transaction actuelle et libère tous les verrous de base de données actuellement maintenus par cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[setAutoCommit](../../../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md)|Définit le mode de validation automatique pour cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet à l’état donné.|  
|[setCatalog](../../../connect/jdbc/reference/setcatalog-method-sqlserverconnection.md)|Définit le nom de catalogue spécifié pour sélectionner un sous-espace de ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) base de données de l’objet dans lequel travailler.|  
|[setClientInfo](../../../connect/jdbc/reference/setclientinfo-method-sqlserverconnection.md)|Définit la valeur des propriétés d'informations client.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverconnection.md)|Définit le regroupement d’instructions à true ou false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Spécifie la nouvelle valeur de la **enablePrepareOnFirstPreparedStatementCall** propriété de connexion.|  
|[setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md)|Modifie la fonctionnalité des [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets qui sont créés à l’aide de ce [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) objet à la fonctionnalité donnée.|  
|[setReadOnly](../../../connect/jdbc/reference/setreadonly-method-sqlserverconnection.md)|Cela place [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet en mode lecture seule en tant qu’indicateur du pilote JDBC pour activer les optimisations de la base de données.|  
|[setSavepoint](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)|Crée un point de sauvegarde sans nom dans la transaction actuelle et retourne le nouvel [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) objet qui le représente.|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Définit la nouvelle valeur de la **serverPreparedStatementDiscardThreshold** propriété de connexion.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverconnection.md)|Définit la taille du cache d’instruction préparée pour cette connexion.|  
|[setTransactionIsolation](../../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md)|Tente de modifier le niveau d’isolation de transaction pour ce [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet à celui indiqué.|  
|[setTypeMap](../../../connect/jdbc/reference/settypemap-method-sqlserverconnection.md)|Installe l’objet TypeMap donné comme mappage de type pour cette [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.lang.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, classe](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
