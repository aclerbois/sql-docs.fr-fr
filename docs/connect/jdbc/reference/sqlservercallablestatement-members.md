---
title: Membres de SQLServerCallableStatement | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: Assembly
ms.assetid: 5ebdc186-e50f-4d14-bbf4-95af5051e4a4
caps.latest.revision: 50
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4efa8910cb11b6cada26afa4ebd034db8aac242c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlservercallablestatement-members"></a>Membres de SQLServerCallableStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="inherited-fields"></a>Champs hérités  
 Aucun.  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Ajoute un ensemble de paramètres au lot de commandes pour cet objet CallableStatement.|  
|[annuler](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Annule l’instruction SQL en cours d’exécution par cet objet CallableStatement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Vide la liste actuelle des commandes SQL pour cet objet CallableStatement.|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Efface immédiatement les valeurs des paramètres actuels.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Efface tous les avertissements signalés sur cet objet CallableStatement.|  
|[Fermer](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Libère les ressources JDBC de cet objet CallableStatement immédiatement au lieu de patienter jusqu'à leur libération automatique et de base de données.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Exécute l’instruction SQL dans cet objet CallableStatement, qui peut être n’importe quel type d’instruction SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Envoie un lot de commandes à exécuter à la base de données. En cas d'exécution correcte de toutes les commandes, un tableau des nombres de mises à jour est retourné.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Exécute la requête SQL dans cet objet de CallableStatement et retourne le [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet qui est généré par la requête.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Exécute l’instruction SQL dans cet objet CallableStatement, qui doit être une SQL INSERT, l’instruction UPDATE, MERGE ou DELETE ; ou une instruction SQL qui ne retourne rien, telle qu’une instruction DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet qui a produit cet objet CallableStatement.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Récupère la valeur de la colonne spécifiée en tant qu’un [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) objet.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la direction d’extraction des lignes à partir des tables de base de données qui est la valeur par défaut pour les jeux de résultats générés à partir de cet objet CallableStatement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de lignes du jeu de résultats qui est la taille d’extraction par défaut pour les résultats du jeu d’objets générés à partir de cet objet CallableStatement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère toute clé générée automatiquement qui est créés suite à l’exécution de cet objet CallableStatement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal d’octets qui peuvent être retournées pour les caractères et les valeurs de colonne binaire dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet produit par cet objet CallableStatement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal de lignes qui une [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet produit par cet objet CallableStatement peut contenir.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Récupère un [classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) objet qui contient des informations sur les colonnes de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet qui sera renvoyé lors de l’exécution de cet objet CallableStatement.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Passe au résultat suivant de cet objet CallableStatement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Récupère le nombre, les types et les propriétés des paramètres pour cet objet CallableStatement.|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’objet de tableau.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que flux de **ASCII** caractères.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que flux binaire d'octets ininterrompus.|  
|[GetBlob](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre Blob JDBC désigné en tant qu’objet Blob dans le langage de programmation Java.|  
|[GetBoolean](../../../connect/jdbc/reference/getboolean-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **booléenne** valeur.|  
|[GetByte](../../../connect/jdbc/reference/getbyte-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **octets** valeur.|  
|[GetBytes](../../../connect/jdbc/reference/getbytes-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que tableau d'octets.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre Blob JDBC désigné en tant qu’objet Clob dans le langage de programmation Java.|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.Date dans le langage de programmation Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Récupère la valeur de la colonne spécifiée en tant qu’un[classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) objet.|  
|[GetDouble](../../../connect/jdbc/reference/getdouble-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **double** dans le langage de programmation Java.|  
|[GetFloat](../../../connect/jdbc/reference/getfloat-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **float** dans le langage de programmation Java.|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **int** dans le langage de programmation Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **long** dans le langage de programmation Java.|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’objet du lecteur.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)|Récupère la valeur de JDBC désigné **NCLOB** paramètre comme un **NClob** objet dans le langage de programmation Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlservercallablestatement.md)|Récupère la valeur de l’élément désigné **NCHAR**, **NVARCHAR** ou **LONGNVARCHAR** paramètre sous forme de chaîne dans le Java en langage de programmation.|  
|[Fonction GetObject](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet dans le langage de programmation Java.|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de secondes pendant lesquelles le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] attendra pour cet objet CallableStatement s’exécute.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’objet Ref dans le langage de programmation Java.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel en tant qu’un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le jeu de résultats d’accès concurrentiel pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par cet objet CallableStatement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le jeu de résultats mise en attente pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par cet objet CallableStatement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le jeu de résultats type pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par cet objet CallableStatement.|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **court** dans le langage de programmation Java.|  
|[GetString](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’un **chaîne** dans le langage de programmation Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.SQLXML.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.Time dans le langage de programmation Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.Timestamp dans le langage de programmation Java.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel en tant que nombre de mises à jour.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu’objet URL dans le langage de programmation Java.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le premier avertissement signalé par des appels sur cet objet CallableStatement.|  
|[IsClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Indique si cet objet d’instruction a été fermé.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Retourne une valeur qui indique s'il est possible d'ajouter une instruction au regroupement d'instructions fourni par l'utilisateur.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)|Indique si cet objet d'instruction est un wrapper pour l'interface spécifiée.|  
|[registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)|Inscrit le paramètre OUT.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le nombre de paramètre désigné à l’objet de tableau donné.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-sqlservercallablestatement.md)|Définit le nombre de paramètres désignés selon le flux d'entrée donné.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlservercallablestatement.md)|Définit le nombre de paramètre désigné à l’objet BigDecimal donné.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)|Définit le paramètre désigné selon le flux d'entrée spécifié.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le paramètre désigné à l’objet Blob donné.|  
|[SetBoolean](../../../connect/jdbc/reference/setboolean-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la donnée **booléenne** valeur.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la donnée **octets** valeur.|  
|[SetBytes](../../../connect/jdbc/reference/setbytes-method-sqlservercallablestatement.md)|Définit le paramètre désigné dans le tableau donné de **octets** valeurs.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet lecteur donné.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le paramètre désigné selon l'objet spécifié.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nom de curseur SQL selon la chaîne donnée, qui est exécutée par les méthodes d'exécution suivantes.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlservercallablestatement.md)|Définit le paramètre désigné à la valeur de date donnée.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md)|Définit la valeur de la colonne spécifiée pour le [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) valeur.|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la donnée **double** valeur.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le mode de traitement d'échappement.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant la direction de traitement des lignes du jeu de résultats.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites depuis la base de données, lorsque davantage de lignes sont nécessaires. |  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet **float** valeur.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet **int** valeur.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet **long** valeur.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la limite pour le nombre maximal d’octets dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) colonne qui stocke des valeurs de caractères ou binaires pour le nombre spécifié d’octets.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la limite pour le nombre maximal de lignes que tout [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet peut contenir le nombre spécifié.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet Reader spécifié.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon l'objet spécifié.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet String spécifié.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlservercallablestatement.md)|Définit le paramètre désigné avec une valeur Null, en fonction du type de paramètre à définir.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)|Définit la valeur du paramètre désigné à l’aide de l’objet donné.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Demande qu'une instruction soit regroupée ou non. Par défaut, un objet SQLServerCallableStatement est regroupé.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre de secondes pendant lesquelles que le pilote doit attendre un objet CallableStatement s’exécute pour le nombre de secondes spécifié.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le paramètre désigné à l’objet de référence spécifié.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|(Héritée de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet à la casse **chaîne complète** ou **adaptive**.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet **court** valeur.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon le Java spécifié **chaîne** valeur.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlservercallablestatement.md)|Définit le paramètre désigné à l’objet **SQLXML** objet.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur d'heure spécifiée.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur d'horodatage spécifiée.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le numéro de paramètre désigné selon le flux d'entrée donné, qui disposera du nombre spécifique d'octets.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur de l'URL spécifiée.|  
|[Unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)|Retourne un objet qui implémente l’interface spécifiée pour autoriser l’accès à la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-des méthodes spécifiques.|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlservercallablestatement.md)|Récupère les informations déterminant si le dernier paramètre OUT lu possédait la valeur SQL NULL.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement|addBatch, clearBatch, clearParameters, close, execute, executeBatch, executeQuery, executeUpdate, getMetaData, getParameterMetaData, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setTime, setTimestamp, setUnicodeStream, setURL|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|class java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait,|  
|java.sql.PreparedStatement|addBatch, clearParameters, execute, executeQuery, executeUpdate, getMetaData, getParameterMetaData, getSQLXML, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setSQLXML, setTime, setTimestamp, setUnicodeStream, setURL|  
|java.sql.Statement|addBatch, cancel, clearBatch, clearWarnings, close, execute, executeBatch, executeQuery, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerCallableStatement, classe](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
