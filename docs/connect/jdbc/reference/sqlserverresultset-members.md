---
title: Membres de SQLServerResultSet | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2a438d5d-2d6a-46a0-a2ae-f35fbae4a472
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cf318137b6d3f23a2161de97999df29b7f29ed8b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverresultset-members"></a>Membres de SQLServerResultSet
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
  
|Nom| Description|  
|----------|-----------------|  
|[CONCUR_SS_OPTIMISTIC_CC](../../../connect/jdbc/reference/concur-ss-optimistic-cc-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] en lecture/écriture de type d’accès concurrentiel optimiste avec aucun verrou de ligne.|  
|[CONCUR_SS_OPTIMISTIC_CCVAL](../../../connect/jdbc/reference/concur-ss-optimistic-ccval-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] en lecture/écriture de type d’accès concurrentiel optimiste avec aucun verrou de ligne.|  
|[CONCUR_SS_SCROLL_LOCKS](../../../connect/jdbc/reference/concur-ss-scroll-locks-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] en lecture/écriture de type d’accès concurrentiel optimiste avec les verrous de ligne.|  
|[TYPE_SS_DIRECT_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-direct-forward-only-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de curseur avant uniquement rapides, en lecture seule.|  
|[TYPE_SS_SCROLL_DYNAMIC](../../../connect/jdbc/reference/type-ss-scroll-dynamic-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de curseur dynamique.|  
|[TYPE_SS_SCROLL_KEYSET](../../../connect/jdbc/reference/type-ss-scroll-keyset-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de curseur de jeu de clés.|  
|[TYPE_SS_SCROLL_STATIC](../../../connect/jdbc/reference/type-ss-scroll-static-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de curseur statique.|  
|[TYPE_SS_SERVER_CURSOR_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-server-cursor-forward-only-field-sqlserverresultset.md)|Permet de spécifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de curseur avant uniquement rapides, en lecture seule.|  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Classe héritée de :| Description|  
|---------------------------|-----------------|  
|java.sql.ResultSet|CLOSE_CURSORS_AT_COMMIT, CONCUR_READ_ONLY, CONCUR_UPDATABLE, FETCH_FORWARD, FETCH_REVERSE, FETCH_UNKNOWN, HOLD_CURSORS_OVER_COMMIT, TYPE_FORWARD_ONLY, TYPE_SCROLL_INSENSITIVE, TYPE_SCROLL_SENSITIVE|  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[Absolu](../../../connect/jdbc/reference/absolute-method-sqlserverresultset.md)|Déplace le curseur à la ligne spécifiée dans cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[afterLast](../../../connect/jdbc/reference/afterlast-method-sqlserverresultset.md)|Déplace le curseur après la dernière ligne de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[beforeFirst](../../../connect/jdbc/reference/beforefirst-method-sqlserverresultset.md)|Déplace le curseur avant la première ligne de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[cancelRowUpdates](../../../connect/jdbc/reference/cancelrowupdates-method-sqlserverresultset.md)|Annule les mises à jour apportées à la ligne actuelle dans ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverresultset.md)|Efface tous les avertissements signalés sur cet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[Fermer](../../../connect/jdbc/reference/close-method-sqlserverresultset.md)|Cela libère [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) l’objet de base de données et ressources JDBC immédiatement au lieu d’attendre pour que cela se produit lorsqu’il est fermé automatiquement.|  
|[deleteRow](../../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md)|Supprime la ligne actuelle à partir de ce[SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet et à partir de la base de données sous-jacente.|  
|[Finaliser](../../../connect/jdbc/reference/finalize-method-sqlserverresultset.md)|Cela ferme explicitement [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[findColumn](../../../connect/jdbc/reference/findcolumn-method-sqlserverresultset.md)|Récupère l’index de la première colonne correspondante pour le nom de colonne spécifié dans ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[first](../../../connect/jdbc/reference/first-method-sqlserverresultset.md)|Déplace le curseur à la première ligne de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet de tableau.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant que flux de caractères ASCII.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)|Récupère la valeur de l’index de colonne désigné dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant que Java .Math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant que flux binaire d’octets non interprétés.|  
|[GetBlob](../../../connect/jdbc/reference/getblob-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet Blob dans le langage de programmation Java.|  
|[GetBoolean](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **booléenne** dans le langage de programmation Java.|  
|[GetByte](../../../connect/jdbc/reference/getbyte-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **octets** dans le langage de programmation Java.|  
|[GetBytes](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **octets** tableau dans le langage de programmation Java.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet Clob dans le langage de programmation Java.|  
|[getConcurrency](../../../connect/jdbc/reference/getconcurrency-method-sqlserverresultset.md)|Récupère le mode d’accès concurrentiel de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getCursorName](../../../connect/jdbc/reference/getcursorname-method-sqlserverresultset.md)|Récupère le nom de curseur SQL utilisé par ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet java.sql.Date dans le langage de programmation Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-sqlserverresultset.md)|Récupère la valeur de la colonne spécifiée en tant qu’un[classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) objet.|  
|[GetDouble](../../../connect/jdbc/reference/getdouble-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **double** dans le langage de programmation Java.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverresultset.md)|Récupère la direction d’extraction pour cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md)|Récupère la taille d’extraction pour cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[GetFloat](../../../connect/jdbc/reference/getfloat-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **float** dans le langage de programmation Java.|  
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverresultset.md)|Récupère la fonctionnalité de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **int** dans le langage de programmation Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **long** dans le langage de programmation Java.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverresultset.md)|Récupère le nombre, les types et les propriétés de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) colonnes de l’objet.|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet du lecteur.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)de l’objet en un **NClob** objet dans le langage de programmation Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet sous forme de chaîne dans le Java en langage de programmation.|  
|[Fonction GetObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)|Obtient la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet dans le langage de programmation Java.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet comme un objet de référence dans le langage de programmation Java.|  
|[getRow](../../../connect/jdbc/reference/getrow-method-sqlserverresultset.md)|Récupère le nombre de lignes actuel|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **court** dans le langage de programmation Java.|  
|[getStatement](../../../connect/jdbc/reference/getstatement-method-sqlserverresultset.md)|Récupère le [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet qui a généré ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[GetString](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **chaîne** dans le langage de programmation Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet en un **SQLXML** objet.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet java.sql.Time dans le langage de programmation Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet java.sql.Timestamp dans le langage de programmation Java.|  
|[GetType](../../../connect/jdbc/reference/gettype-method-sqlserverresultset.md)|Récupère le type de curseur de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getUnicodeStream](../../../connect/jdbc/reference/getunicodestream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant que flux de caractères Unicode.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet URL.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverresultset.md)|Récupère le premier avertissement signalé par des appels sur cet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md)|Insère le contenu de la ligne d’insertion dans cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet et dans la base de données.|  
|[isAfterLast](../../../connect/jdbc/reference/isafterlast-method-sqlserverresultset.md)|Récupère si le curseur se trouve après la dernière ligne dans cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[isBeforeFirst](../../../connect/jdbc/reference/isbeforefirst-method-sqlserverresultset.md)|Récupère si le curseur se trouve avant la première ligne dans cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[IsClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverresultset.md)|Indique si cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet a été fermé.|  
|[isFirst](../../../connect/jdbc/reference/isfirst-method-sqlserverresultset.md)|Récupère si le curseur se trouve sur la première ligne de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[isLast](../../../connect/jdbc/reference/islast-method-sqlserverresultset.md)|Récupère si le curseur se trouve sur la dernière ligne de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[last](../../../connect/jdbc/reference/last-method-sqlserverresultset.md)|Déplace le curseur vers la dernière ligne dans cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[moveToCurrentRow](../../../connect/jdbc/reference/movetocurrentrow-method-sqlserverresultset.md)|Déplace le curseur à la position de curseur mémorisée, généralement la ligne actuelle.|  
|[moveToInsertRow](../../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md)|Déplace le curseur vers la ligne d'insertion.|  
|[Suivant](../../../connect/jdbc/reference/next-method-sqlserverresultset.md)|Déplace le curseur d'une ligne vers le bas depuis sa position actuelle.|  
|[précédent](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md)|Déplace le curseur à la ligne précédente dans ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[refreshRow](../../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md)|Actualise la ligne actuelle avec sa valeur la plus récente dans la base de données.|  
|[relative](../../../connect/jdbc/reference/relative-method-sqlserverresultset.md)|Déplace le curseur de fonction du nombre de lignes, par rapport à la ligne actuelle, dans une direction négative ou positive.|  
|[RowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md)|Récupère les informations déterminant si une ligne a été supprimée.|  
|[rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md)|Récupère les informations déterminant si la ligne actuelle a eu une insertion.|  
|[RowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md)|Récupère les informations déterminant si la ligne actuelle a été mise à jour.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md)|Fournit un Conseil concernant la direction dans laquelle les lignes dans cette [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet est traité.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md)|Fournit un Conseil au concernant le nombre de lignes qui doivent être extraites à partir de la base de données lorsque plusieurs lignes sont nécessaires pour ce au pilote JDBC [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[updateArray](../../../connect/jdbc/reference/updatearray-method-sqlserverresultset.md)|Met à jour la colonne désignée avec un objet de tableau.|  
|[updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de flux ASCII.|  
|[updateBigDecimal](../../../connect/jdbc/reference/updatebigdecimal-method-sqlserverresultset.md)|Met à jour la colonne désignée avec un objet BigDecimal.|  
|[updateBinaryStream](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec la valeur de flux binaire.|  
|[updateBlob](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur java.sql.Blob.|  
|[updateBoolean](../../../connect/jdbc/reference/updateboolean-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **booléenne** valeur.|  
|[updateByte](../../../connect/jdbc/reference/updatebyte-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **octets** valeur.|  
|[updateBytes](../../../connect/jdbc/reference/updatebytes-method-sqlserverresultset.md)|Met à jour de la colonne désignée avec un tableau de **octets** valeurs.|  
|[updateCharacterStream](../../../connect/jdbc/reference/updatecharacterstream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de flux de caractères.|  
|[updateClob](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur java.sql.Clob.|  
|[updateDate](../../../connect/jdbc/reference/updatedate-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de date.|  
|[updateDateTimeOffset](../../../connect/jdbc/reference/updatedatetimeoffset-sqlserverresultset.md)|Les mises à jour un [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) colonne.|  
|[updateDouble](../../../connect/jdbc/reference/updatedouble-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **double** valeur.|  
|[updateFloat](../../../connect/jdbc/reference/updatefloat-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **float** valeur.|  
|[updateInt](../../../connect/jdbc/reference/updateint-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **int** valeur.|  
|[updateLong](../../../connect/jdbc/reference/updatelong-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **long** valeur.|  
|[updateNCharacterStream](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de flux de caractères.|  
|[updateNClob](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)|Met à jour la colonne désignée avec la valeur d'objet spécifiée.|  
|[updateNString](../../../connect/jdbc/reference/updatenstring-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **chaîne** valeur.|  
|[updateNull](../../../connect/jdbc/reference/updatenull-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur Null.|  
|[updateObject](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **objet** valeur.|  
|[updateRef](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur java.sql.Ref.|  
|[updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md)|Met à jour de la base de données sous-jacente avec le nouveau contenu de la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[updateShort](../../../connect/jdbc/reference/updateshort-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **court** valeur.|  
|[updateString](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **chaîne** valeur.|  
|[updateSQLXML](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une **SQLXML** valeur.|  
|[updateTime](../../../connect/jdbc/reference/updatetime-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur d'heure.|  
|[updateTimestamp](../../../connect/jdbc/reference/updatetimestamp-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur d'horodateur.|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlserverresultset.md)|Vérifie si la dernière valeur lue était une valeur NULL.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
