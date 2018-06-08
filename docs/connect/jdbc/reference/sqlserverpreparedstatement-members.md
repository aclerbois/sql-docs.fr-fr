---
title: Membres de SQLServerPreparedStatement | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2363902f-d4c6-4cd4-a5fc-86079eb9e418
caps.latest.revision: 38
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 275115e8d23c48a564528de57fee14ae19513e00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverpreparedstatement-members"></a>Membres de SQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|Ajoute un ensemble de paramètres au lot de commandes pour cet objet d’instruction.|  
|[annuler](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Annule l’instruction SQL en cours d’exécution par cet objet d’instruction.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|Vide la liste actuelle des commandes SQL pour ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|Efface immédiatement les valeurs des paramètres actuels.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Efface tous les avertissements signalés sur cet objet d’instruction.|  
|[Fermer](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|Libère les ressources JDBC de cet objet d’instruction immédiatement au lieu de patienter jusqu'à leur libération automatique et de base de données.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|Exécute l’instruction SQL dans cet objet d’instruction, ce qui peut être n’importe quel type d’instruction SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|Envoie un lot de commandes à exécuter à la base de données. En cas d'exécution correcte de toutes les commandes, un tableau des nombres de mises à jour est retourné.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|Exécute la requête SQL dans cet objet d’instruction et retourne le [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet qui est généré par la requête.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|Exécute l’instruction SQL dans cet objet d’instruction, qui doit être une SQL INSERT, l’instruction UPDATE, MERGE ou DELETE ; ou une instruction SQL qui ne retourne rien, telle qu’une instruction DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet qui a produit cet objet d’instruction.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la direction d’extraction des lignes à partir des tables de base de données qui est la valeur par défaut pour les jeux de résultats générés à partir de cet objet d’instruction.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de lignes du jeu de résultats qui est la taille d’extraction par défaut pour les résultats du jeu d’objets générés à partir de cet objet d’instruction.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère toute clé générée automatiquement qui est créés à la suite de cet objet d’instruction en cours d’exécution.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal d’octets qui peuvent être retournées pour les caractères et les valeurs de colonne binaire dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet produit par cet objet d’instruction.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal de lignes qui une [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet produit par cet objet d’instruction peut contenir.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|Récupère un [classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) objet qui contient des informations sur les colonnes de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet qui sera renvoyé lors de l’exécution de cet objet d’instruction.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Se déplace vers le résultat suivant de cet objet d’instruction.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|Récupère le nombre, les types et les propriétés des paramètres de cet objet d’instruction.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de secondes pendant lesquelles le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] attendra pour cet objet d’instruction à exécuter.|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel en tant qu’un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le jeu de résultats d’accès concurrentiel pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par cet objet d’instruction.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le jeu de résultats mise en attente pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par cet objet d’instruction.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le jeu de résultats type pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par cet objet d’instruction.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md),) récupère le résultat actuel en tant qu’un nombre de mises à jour.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le premier avertissement signalé par des appels sur cet objet d’instruction.|  
|[IsClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Indique si cet objet d’instruction a été fermé.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Retourne une valeur qui indique s'il est possible d'ajouter une instruction au regroupement d'instructions fourni par l'utilisateur.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)|Indique si cet objet d'instruction est un wrapper pour l'interface spécifiée.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|Définit le nombre de paramètre désigné à l’objet de tableau donné.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-method-sqlserverpreparedstatement.md)|Définit le nombre de paramètre désigné à l’objet InputStream donné.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlserverpreparedstatement.md)|Définit le nombre de paramètre désigné à l’objet BigDecimal donné.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon le flux d'entrée spécifié.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet Blob spécifié.|  
|[SetBoolean](../../../connect/jdbc/reference/setboolean-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **booléenne** valeur.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **octets** valeur.|  
|[SetBytes](../../../connect/jdbc/reference/setbytes-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon le tableau d'octets spécifié.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet Reader spécifié.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet Clob donné.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Affecte la chaîne spécifiée en tant que nom de curseur SQL en vue de son utilisation par les méthodes d'exécution suivantes.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur de date spécifiée.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md)|Définit la valeur de la colonne spécifiée pour le [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) valeur.|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **double** valeur.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le mode de traitement d'échappement.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant la direction de traitement des lignes du jeu de résultats.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites depuis la base de données, lorsque davantage de lignes sont nécessaires. |  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **float** valeur.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **int** valeur.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **long** valeur.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la limite pour le nombre maximal d’octets dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) colonne qui stocke des valeurs de caractères ou binaires pour le nombre d’octets spécifié.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la limite pour le nombre maximal de lignes que tout [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet peut contenir le nombre donné.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet Reader spécifié.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l'objet spécifié.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné avec une valeur Null, en fonction du type de paramètre à définir.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-int-java-lang-string.md)|Définit le paramètre désigné à l’objet **chaîne** objet.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)|Définit la valeur du paramètre désigné à l’aide de l’objet donné.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Demande qu'une instruction soit regroupée ou non. Par défaut, un objet SQLServerPreparedStatement est regroupé.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre de secondes pendant lesquelles que le pilote doit attendre un objet d’instruction à exécuter pour le nombre de secondes spécifié.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet de référence spécifié.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet à la casse **chaîne complète** ou **adaptive**.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **court** valeur.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **chaîne** valeur.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné à l’objet **SQLXML** objet.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur d'heure spécifiée.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur d'horodatage spécifiée.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|Définit le numéro de paramètre désigné selon le flux d’entrée spécifiée, qui disposera du nombre spécifié d’octets.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur de l'URL spécifiée.|  
|[Unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)|Retourne un objet qui implémente l’interface spécifiée pour autoriser l’accès à la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-des méthodes spécifiques.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Statement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerPreparedStatement, classe](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
