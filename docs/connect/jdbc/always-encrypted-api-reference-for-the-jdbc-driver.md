---
title: Always Encrypted référence des API pour le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 1/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6962a2aa-9508-4d4f-a78c-905e2bc68615
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d44903f7d57dd24f8cad3463a87a54e5ab5c290f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="always-encrypted-api-reference-for-the-jdbc-driver"></a>Always Encrypted référence des API pour le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le Chiffrement intégral permet aux clients de chiffrer des données sensibles dans des applications clientes et de ne jamais révéler les clés de chiffrement à SQL Server. À cette fin, un pilote de Chiffrement intégral installé sur l’ordinateur client chiffre et déchiffre automatiquement les données sensibles dans l’application cliente SQL Server. Le pilote chiffre les données dans les colonnes sensibles avant de les transmettre à SQL Server et il réécrit automatiquement les requêtes pour que la sémantique de l’application soit conservée. De même, il déchiffre de manière transparente les données stockées dans les colonnes de base de données chiffrées qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et [à l’aide d’Always Encrypted avec le pilote JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Always Encrypted est pris en charge uniquement par le pilote Microsoft JDBC 6.0 ou version ultérieure de SQL Server avec SQL Server 2016.  
  
 ## <a name="always-encrypted-api-references"></a>Always Encrypted références de l’API
 
 Plusieurs ajouts et modifications ont été apportés à l’API du pilote JDBC pour une utilisation dans les applications clientes qui utilisent Always Encrypted.  
  
 **SQLServerConnection, classe**  
  
|Nom| Description|  
|----------|-----------------|  
|Nouveau mot clé de chaîne de connexion :<br /><br /> columnEncryptionSetting|columnEncryptionSetting = Enabled Active la fonctionnalité de chiffrement intégral pour la connexion et columnEncryptionSetting = Disabled la désactive. Les valeurs acceptées sont Enabled/Disabled. La valeur par défaut est Disabled.|  
|Nouvelles méthodes :<br /><br /> setColumnEncryptionTrustedMasterKeyPaths de void statique public (Map < String, liste\<chaîne >> trustedKeyPaths)<br /><br /> updateColumnEncryptionTrustedMasterKeyPaths de void statique public (liste du serveur de chaîne\<chaîne > trustedKeyPaths)<br /><br /> public static void removeColumnEncryptionTrustedMasterKeyPaths (serveur de chaîne)|Vous permet de définir/mise à jour/supprimer une liste de chemins d’accès de clés approuvés pour un serveur de base de données. Si, pendant le traitement d’une requête d’application, le pilote reçoit un chemin d’accès de clé qui ne figure pas dans la liste, la requête échoue. Cette propriété fournit une protection supplémentaire contre les attaques de sécurité durant lesquelles un ordinateur SQL Server compromis fournit des chemins d’accès de clés factices, qui peuvent entraîner une fuite des informations d’identification du magasin de clés.|  
|Nouvelle méthode :<br /><br /> Carte statique publique < chaîne, liste\<chaîne >> getColumnEncryptionTrustedMasterKeyPaths()|Retourne une liste de chemins d’accès de clés approuvés pour un serveur de bases de données.|  
|Nouvelle méthode :<br /><br /> registerColumnEncryptionKeyStoreProviders de void statique public (carte\<String, SQLServerColumnEncryptionKeyStoreProvider > clientKeyStoreProviders)|Vous permet d’inscrire des fournisseurs de magasins de clés personnalisés. Il s’agit d’un dictionnaire qui mappe des noms de fournisseurs de magasins de clés à des implémentations de fournisseurs de magasins de clés.<br /><br /> Pour utiliser le magasin de clés JVM, vous devez instancier un objet SQLServerColumnEncryptionJVMKeyStoreProvider avec des informations d’identification de magasins de clés JVM et l’inscrire auprès du pilote. Le nom de ce fournisseur doit être « MSSQL_JVM_KEYSTORE ».<br /><br /> Pour utiliser le magasin de coffre de clés Azure, vous devez instancier un objet SQLServerColumnEncryptionAzureKeyStoreProvider et l’inscrire auprès du pilote. Le nom de ce fournisseur doit être 'AZURE_KEY_VAULT'.|
|public getSendTimeAsDatetime() de booléenne finale|Retourne le paramètre de la propriété de connexion sendTimeAsDatetime.|
|setSendTimeAsDatetime void publique (boolean sendTimeAsDateTimeValue)|Modifie le paramètre de la propriété de connexion sendTimeAsDatetime.|

 **Classe de SQLServerConnectionPoolProxy**
|Nom| Description|  
|----------|-----------------|  
|public getSendTimeAsDatetime() de booléenne finale|Retourne le paramètre de la propriété de connexion sendTimeAsDatetime.|
|setSendTimeAsDatetime void publique (boolean sendTimeAsDateTimeValue)|Modifie le paramètre de la propriété de connexion sendTimeAsDatetime.|
     
  
 **SQLServerDataSource, classe**  
  
|Nom| Description|  
|----------|-----------------|  
|setColumnEncryptionSetting void publique (chaîne columnEncryptionSetting)|Active/désactive la fonctionnalité Always Encrypted pour l’objet de source de données.<br /><br /> La valeur par défaut est Disabled.|  
|public getColumnEncryptionSetting() de chaîne|Récupère le paramètre de fonctionnalité Always Encrypted pour l’objet de source de données.|
|setKeyStoreAuthentication void publique (chaîne keyStoreAuthentication)|Définit le nom qui identifie un magasin de clés. Seule valeur prise en charge est « JavaKeyStorePassword » pour le magasin de clés Java de manutention.<br/><br/>La valeur par défaut est null.|
|public getKeyStoreAuthentication() de chaîne|Obtient la valeur du paramètre keyStoreAuthentication pour l’objet de source de données.|
|setKeyStoreSecret void publique (chaîne keyStoreSecret)|Définit le mot de passe pour le keystore Java. Notez que, pour le fournisseur de magasin de clés Java le mot de passe pour le magasin de clés et de la clé doit correspondre à. Notez que, keyStoreAuthentication doit être définie avec « JavaKeyStorePassword ».|
|setKeyStoreLocation void publique (chaîne keyStoreLocation)|Définit l’emplacement, y compris le nom de fichier pour le keystore Java. Notez que, keyStoreAuthentication doit être définie avec « JavaKeyStorePassword ».|
|public getKeyStoreLocation() de chaîne|Récupère la keyStoreLocation pour le magasin de clés de Java.|
  
 **Classe de SQLServerColumnEncryptionJavaKeyStoreProvider**  
  
 L’implémentation du fournisseur de magasin de clés pour le magasin de clés de Java. Cette classe permet à l’aide de certificats stockés dans le magasin de clés Java en tant que clés principales de colonne.  
  
 Constructeurs  
  
|Nom| Description|  
|----------|-----------------|  
|public SQLServerColumnEncryptionJavaKeyStoreProvider (chaîne keyStoreLocation, char [] keyStoreSecret)|Fournisseur de magasin de clés pour le magasin de clés de Java.|  
  
 Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|public byte [] decryptColumnEncryptionKey (masterKeyPath de chaîne, chaîne encryptionAlgorithm, byte [] encryptedColumnEncryptionKey)|Déchiffre la valeur chiffrée spécifiée d’une clé CEK. La valeur chiffrée est censée être chiffrée à l’aide du certificat avec le chemin d’accès de clé spécifié et à l’aide de l’algorithme spécifié.<br /><br /> **Le format du chemin d’accès de clé doit être une des opérations suivantes :**<br /><br /> Thumbprint:<empreinte_certificate><br /><br /> Alias:<alias_certificat><br /><br /> (Substitue SQLServerColumnEncryptionKeyStoreProvider. decryptColumnEncryptionKey (chaîne, chaîne, Byte[]).)|  
|public byte [] encryptColumnEncryptionKey (masterKeyPath de chaîne, chaîne encryptionAlgorithm, byte [] plainTextColumnEncryptionKey)|Chiffre une clé CEK à l’aide du certificat avec le chemin d’accès de clé spécifié et à l’aide de l’algorithme spécifié.<br /><br /> **Le format du chemin d’accès de clé doit être une des opérations suivantes :**<br /><br /> Thumbprint:<empreinte_certificate><br /><br /> Alias:<alias_certificat><br /><br /> (Substitue SQLServerColumnEncryptionKeyStoreProvider. encryptColumnEncryptionKey (chaîne, chaîne, Byte[]).)|  
|setName void publique (nom de chaîne)|Définit le nom de ce fournisseur de magasin de clés.|
|public chaîne getName ()|Obtient le nom de ce fournisseur de magasin de clés.|
  
 **Classe de SQLServerColumnEncryptionAzureKeyVaultProvider**  
  
 L’implémentation du fournisseur de magasin de clés pour le coffre de clés Azure. Cette classe permet à l’aide des clés stockées dans le coffre de clés Azure en tant que clés principales de colonne.  
  
 Constructeurs  
  
|Nom| Description|  
|----------|-----------------|  
|public SQLServerColumnEncryptionAzureKeyVaultProvider (clientId de chaîne, chaîne clientKey)|Fournisseur de magasin de clés pour le coffre de clés Azure.  Vous devez fournir l’identificateur et la clé du client demande le jeton pour s’authentifier auprès d’Azure Key Vault.|  
  
 Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|public byte [] decryptColumnEncryptionKey (masterKeyPath de chaîne, chaîne encryptionAlgorithm, byte [] encryptedColumnEncryptionKey)|Déchiffre la valeur chiffrée spécifiée d’une clé CEK. La valeur chiffrée est censée être chiffrée à l’aide de la clé de la colonne spécifiée IDmaster clé et à l’aide de l’algorithme spécifié. <br />(Substitue SQLServerColumnEncryptionKeyStoreProvider. decryptColumnEncryptionKey (chaîne, chaîne, Byte[]).)|  
|public byte [] encryptColumnEncryptionKey (masterKeyPath de chaîne, chaîne encryptionAlgorithm, byte [] columnEncryptionKey)|Chiffre une clé de chiffrement de colonne à l’aide de la clé principale de colonne spécifiée et à l’aide de l’algorithme spécifié. <br />(Substitue SQLServerColumnEncryptionKeyStoreProvider. encryptColumnEncryptionKey (chaîne, chaîne, Byte[]).)|  
|setName void publique (nom de chaîne)|Définit le nom de ce fournisseur de magasin de clés.|
|public chaîne getName ()|Obtient le nom de ce fournisseur de magasin de clés.|  
  
  
 **Interface de SQLServerKeyVaultAuthenticationCallback**  
  
 Cette interface contient une méthode pour l’authentification du coffre de clés Azure, qui doit être implémentée par l’utilisateur.  
  
 Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|public getAccessToken de chaîne (autorité de chaîne, les ressources de chaîne, étendue de la chaîne) ;|La méthode doit être substituée. La méthode est utilisée pour accéder à l’émission de jeton de coffre de clés Azure.|  
  
 **Classe SQLServerColumnEncryptionKeyStoreProvider**  
  
 Étendez cette classe pour implémenter un fournisseur de magasins de clés personnalisé.  
  
|Nom| Description|  
|----------|-----------------|  
|SQLServerColumnEncryptionKeyStoreProvider|Classe de base pour tous les fournisseurs de magasins de clés. Un fournisseur personnalisé doit dériver de cette classe et substituer ses fonctions membres, puis inscrivez-le à l’aide de SQLServerConnection. registerColumnEncryptionKeyStoreProviders().|  
  
 Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|public abstract byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte [] encryptedColumnEncryptionKey)|Méthode de classe de base pour déchiffrer la valeur chiffrée spécifiée d’une clé CEK. La valeur chiffrée est censée être chiffrée à l’aide de la clé CMK avec le chemin d’accès de clé spécifié et à l’aide de l’algorithme spécifié.|  
|public abstract byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[]  columnEncryptionKey)|Méthode de classe de base pour chiffrer une clé CEK à l’aide de la clé CMK avec le chemin d’accès de clé spécifié et à l’aide de l’algorithme spécifié.|
|setName public void abstraite (nom de chaîne)|Définit le nom de ce fournisseur de magasin de clés.|
|public abstract getName() de chaîne|Obtient le nom de ce fournisseur de magasin de clés.|  
  
 Les méthodes nouvelles ou surchargées dans **SQLServerPreparedStatement** classe  
  
|Nom| Description|  
|----------|-----------------|  
|setBigDecimal void publique (int parameterIndex, BigDecimal x, int, précision, échelle d’int)<br /><br /> public setObject void (int parameterIndex, objet x, int targetSqlType, précision entier, montée en puissance int)<br /><br /> public setObject void (int parameterIndex, objet x, SQLType targetSqlType, précision entier, l’échelle de nombre entier)<br /><br /> public setTime void (parameterIndex d’int, java.sql.Time, int échelle x)<br /><br /> public setTimestamp void (parameterIndex d’int, java.sql.Timestamp, int échelle x) <br />public setDateTimeOffset void (int parameterIndex, microsoft.sql.DateTimeOffset, int échelle x)|Ces méthodes sont surchargées avec une précision ou un argument d’échelle ou les deux pour prendre en charge le chiffrement intégral pour les types de données spécifiques qui requièrent la précision et l’échelle plus d’informations.|  
|setMoney void publique (int parameterIndex, BigDecimal x)<br /><br /> setSmallMoney void publique (int parameterIndex, BigDecimal x)<br /><br /> public setUniqueIdentifier void (int parameterIndex, guid de chaîne)<br /><br /> setDateTime void publique (parameterIndex d’int, java.sql.Timestamp x)<br /><br /> setSmallDateTime void publique (parameterIndex d’int, java.sql.Timestamp x)|Ces méthodes sont ajoutées pour prendre en charge le chiffrement intégral des types de données money, smallmoney, uniqueidentifier, datetime et smalldatetime. <br/><br/>Notez que la méthode setTimestamp() existant est utilisée pour envoyer des valeurs de paramètre dans la colonne chiffrée datetime2. Pour « DateTime » chiffrées et colonnes smalldatetime utiliser les nouvelles méthodes setDateTime() setSmallDateTime() respectivement.|  
|setBigDecimal finale public void (int parameterIndex, BigDecimal x int précision, échelle int, boolean forceEncrypt)<br /><br /> setMoney public void finale (int parameterIndex, BigDecimal x forceEncrypt booléenne)<br /><br /> setSmallMoney public void finale (int parameterIndex, BigDecimal x forceEncrypt booléenne)<br /><br /> setBoolean final public void (int parameterIndex, boolean x, boolean forceEncrypt)<br /><br /> setByte final public void (int parameterIndex, byte x forceEncrypt booléenne)<br /><br /> setBytes final public void (int parameterIndex, octet x[], boolean forceEncrypt)<br /><br /> setUniqueIdentifier finale public void (int parameterIndex, chaîne guid, boolean forceEncrypt)<br /><br /> setDouble final public void (int parameterIndex, double x, boolean forceEncrypt)<br /><br /> setFloat final public void (parameterIndex d’int, float x, boolean forceEncrypt)<br /><br /> setInt final public void (int parameterIndex, la valeur int, boolean forceEncrypt)<br /><br /> setLong final public void (parameterIndex d’int, long x, boolean forceEncrypt)<br /><br /> public setObject finale (int parameterIndex, objet x, int targetSqlType, précision d’entier, échelle int, boolean forceEncrypt)<br /><br /> setObject final public void (int parameterIndex, objet x, SQLType targetSqlType, entier précision, entier échelle, boolean forceEncrypt)<br /><br /> setShort final public void (int parameterIndex, x court, boolean forceEncrypt)<br /><br /> setString public void finale (forceEncrypt booléenne parameterIndex, chaîne str, int)<br /><br /> setNString finale public void (int parameterIndex, valeur de chaîne, booléen forceEncrypt)<br /><br /> public setTime de void finale (parameterIndex int, java.sql.Time x, l’échelle du type int, boolean forceEncrypt)<br /><br /> public setTimestamp de void finale (parameterIndex int, java.sql.Timestamp, int échelle x, boolean forceEncrypt)<br /><br /> public setDateTimeOffset de void finale (parameterIndex int, microsoft.sql.DateTimeOffset x, l’échelle du type int, boolean forceEncrypt)<br /><br /> setDateTime final public void (parameterIndex d’int, java.sql.Timestamp x forceEncrypt booléenne)<br /><br /> setSmallDateTime finale public void (parameterIndex d’int, java.sql.Timestamp x forceEncrypt booléenne)<br /><br /> setDate public de void finale (parameterIndex int, java.sql.Date x java.util.Calendar cal, boolean forceEncrypt)<br /><br /> setTime de void final public (parameterIndex int, java.sql.Time x java.util.Calendar cal, boolean forceEncrypt)<br /><br /> public setTimestamp de void finale (parameterIndex int, java.sql.Timestamp x java.util.Calendar cal, boolean forceEncrypt)|Définit le paramètre désigné selon la valeur java donnée.<br /><br /> Si le forceEncrypt booléenne est définie sur true, la requête paramètre n’a la valeur si la désignation de colonne est chiffrée et Always Encrypted est activé sur la connexion ou l’instruction.<br /><br /> Si le forceEncrypt booléenne est définie sur false, le pilote ne force pas le chiffrement sur les paramètres.|  
  
 Les méthodes nouvelles ou surchargées dans **SQLServerCallableStatement** classe  
  
|Nom| Description|  
|----------|-----------------|  
|public registerOutParameter void (int parameterIndex, sqlType d’int, int précision, l’échelle du type int)<br /><br /> public registerOutParameter void (int parameterIndex SQLType sqlType, int précision, échelle d’int)<br /><br /> public registerOutParameter void (String parameterName, int sqlType, int précision, échelle d’int)<br /><br /> public registerOutParameter void (String parameterName, SQLType sqlType, int précision, échelle d’int)<br />public setBigDecimal void (String parameterName, BigDecimal bd, int précision, échelle d’int)<br /><br /> public setTime void (String parameterName, java.sql.Time t, l’échelle d’int)<br /><br /> public setTimestamp void (String parameterName, java.sql.Timestamp t, l’échelle d’int)<br /><br /> public setDateTimeOffset void (String parameterName, microsoft.sql.DateTimeOffset t, l’échelle d’int)<br/><br/>setObject final public void (sCol chaîne, objet x, int targetSqlType, précision entier, montée en puissance int)|Ces méthodes sont surchargées avec une précision ou un argument d’échelle ou les deux pour prendre en charge le chiffrement intégral pour les types de données spécifiques qui requièrent la précision et l’échelle plus d’informations.|  
|setDateTime void publique (String parameterName, java.sql.Timestamp x)<br /><br /> setSmallDateTime void publique (String parameterName, java.sql.Timestamp x)<br /><br /> public setUniqueIdentifier void (String parameterName, guid de chaîne)<br /><br /> public setMoney void (String parameterName, BigDecimal bd)<br /><br /> public setSmallMoney void (String parameterName, BigDecimal bd)<br/><br/>public Timestamp getDateTime (int index)<br/><br/>public Timestamp getDateTime (chaîne sCol)<br/><br/>public getDateTime d’horodatage (int index, calendrier cal)<br/><br/>public getSmallDateTime Timestamp (int index)<br/><br/>public getSmallDateTime Timestamp (chaîne sCol)<br/><br/>public getSmallDateTime d’horodatage (int index, calendrier cal)<br/><br/>public getSmallDateTime d’horodatage (nom de chaîne, calendrier cal)<br/><br/>public BigDecimal getMoney (int index)<br/><br/>public BigDecimal getMoney (chaîne sCol)<br/><br/>public BigDecimal getSmallMoney (int index)<br/><br/>public BigDecimal getSmallMoney (chaîne sCol)|Ces méthodes sont ajoutées pour prendre en charge le chiffrement intégral des types de données money, smallmoney, uniqueidentifier, datetime et smalldatetime. <br/><br/>Notez que la méthode setTimestamp() existant est utilisée pour envoyer des valeurs de paramètre dans la colonne chiffrée datetime2. Pour « DateTime » chiffrées et colonnes smalldatetime utiliser les nouvelles méthodes setDateTime() setSmallDateTime() respectivement.|  
|public setObject void (String parameterName, Object o, int n, m d’int, boolean forceEncrypt)<br /><br /> public setObject void (String parameterName, objet obj, SQLType jdbcType, montée en puissance int, boolean forceEncrypt)<br /><br /> setDate void publique (String parameterName, java.sql.Date x calendrier c, boolean forceEncrypt)<br /><br /> public setTime void (String parameterName, java.sql.Time t, mise à l’échelle du type int, boolean forceEncrypt)<br /><br /> setTime void publique (String parameterName, java.sql.Time x calendrier c, boolean forceEncrypt)<br /><br /> public setDateTime void (String parameterName, java.sql.Timestamp x forceEncrypt booléenne)<br /><br /> public setDateTimeOffset void (String parameterName, microsoft.sql.DateTimeOffset t, mise à l’échelle du type int, boolean forceEncrypt)<br /><br /> public setSmallDateTime void (String parameterName, java.sql.Timestamp x forceEncrypt booléenne)<br /><br /> public setTimestamp void (String parameterName, java.sql.Timestamp t, mise à l’échelle du type int, boolean forceEncrypt)<br /><br /> public setTimestamp void (String parameterName, java.sql.Timestamp x forceEncrypt booléenne)<br /><br /> public setUniqueIdentifier void (String parameterName, chaîne guid, boolean forceEncrypt)<br /><br /> public setBytes void (String parameterName, byte [] b, boolean forceEncrypt)<br /><br /> public setByte void (String parameterName, octet b, boolean forceEncrypt)<br /><br /> public setString void (String parameterName, mappage de chaîne, boolean forceEncrypt)<br /><br /> setNString finale public void (String parameterName, valeur de chaîne, booléen forceEncrypt)<br /><br /> public setMoney void (String parameterName, BigDecimal bd, boolean forceEncrypt)<br /><br /> public setSmallMoney void (String parameterName, BigDecimal bd, boolean forceEncrypt)<br /><br /> public setBigDecimal void (String parameterName, BigDecimal bd, int précision, échelle d’int, forceEncrypt booléenne)<br /><br /> public setDouble void (String parameterName, double d, boolean forceEncrypt)<br /><br /> public setFloat void (String parameterName, float f, boolean forceEncrypt)<br /><br /> setInt void publique (String parameterName, int i, boolean forceEncrypt)<br /><br /> public setLong void (String parameterName, l de type long, boolean forceEncrypt)<br /><br /> public setShort void (String parameterName, short s, boolean forceEncrypt)<br /><br /> public setBoolean void (parameterNames de chaîne, booléen b, boolean forceEncrypt)<br/><br/>setTimeStamp void publique (chaîne sCol, java.sql.Timestamp x calendrier c, Boolean forceEncrypt)|Définit le paramètre désigné selon la valeur java donnée.<br /><br /> Si le forceEncrypt booléenne est définie sur true, la requête paramètre n’a la valeur si la désignation de colonne est chiffrée et Always Encrypted est activé sur la connexion ou l’instruction.<br /><br /> Si le forceEncrypt booléenne est définie sur false, le pilote ne force pas le chiffrement sur les paramètres.|
 

 Les méthodes nouvelles ou surchargées dans **SQLServerResultSet** classe  
  
|Nom| Description|  
|----------|-----------------|  
|getUniqueIdentifier chaîne public (columnIndex int)<br/><br/>public getUniqueIdentifier String (chaîne columnLabel)<br/><br/>   public java.sql.Timestamp getDateTime (columnIndex int) <br/><br/> public java.sql.Timestamp getDateTime (nom de colonne de chaîne)   <br/><br/> getDateTime java.sql.Timestamp public (int columnIndex, calendrier cal)   <br/><br/>getDateTime java.sql.Timestamp public (nom de colonne de chaîne, calendrier cal)    <br/><br/>public java.sql.Timestamp getSmallDateTime (columnIndex int)    <br/><br/> public java.sql.Timestamp getSmallDateTime (nom de colonne de chaîne)   <br/><br/> getSmallDateTime java.sql.Timestamp public (int columnIndex, calendrier cal)   <br/><br/> getSmallDateTime java.sql.Timestamp public (nom de colonne de chaîne, calendrier cal)   <br/><br/>  public BigDecimal getMoney (columnIndex int)  <br/><br/> public BigDecimal getMoney (nom de colonne de chaîne)   <br/><br/> public BigDecimal getSmallMoney (columnIndex int)   <br/><br/>  public BigDecimal getSmallMoney (nom de colonne de chaîne)  <br/><br/>updateMoney void publique (nom de colonne de chaîne, BigDecimal x)    <br/><br/>  updateSmallMoney void publique (nom de colonne de chaîne, BigDecimal x)  <br/><br/>     updateDateTime void publique (index de type int, java.sql.Timestamp x) <br/><br/> updateSmallDateTime void publique (index de type int, java.sql.Timestamp x) |Ces méthodes sont ajoutées pour prendre en charge le chiffrement intégral des types de données money, smallmoney, uniqueidentifier, datetime et smalldatetime. <br/><br/>Notez que la méthode updateTimestamp() existant est utilisée pour mettre à jour les colonnes datetime2 chiffré. Pour « DateTime » chiffrées et colonnes smalldatetime utiliser les nouvelles méthodes updateDateTime() updateSmallDateTime() respectivement.|
|public updateBoolean void (int index, boolean x, boolean forceEncrypt)  <br/><br/>  public updateByte void (index de type int, byte x forceEncrypt booléenne)  <br/><br/>  public updateShort void (int index, x court, boolean forceEncrypt)  <br/><br/> public updateInt void (index de type int, int x forceEncrypt booléenne)   <br/><br/>  public updateLong void (index int, long x, boolean forceEncrypt)  <br/><br/> public updateFloat void (index int, float x, boolean forceEncrypt)   <br/><br/> public updateDouble void (int index, double x, boolean forceEncrypt)   <br/><br/> updateMoney void publique (int index, BigDecimal x forceEncrypt booléenne)   <br/><br/>  updateMoney void publique (chaîne columnName, BigDecimal x forceEncrypt booléenne)  <br/><br/> updateSmallMoney void publique (int index, BigDecimal x forceEncrypt booléenne)   <br/><br/>  updateSmallMoney void publique (chaîne columnName, BigDecimal x forceEncrypt booléenne)  <br/><br/> public updateBigDecimal void (int index, BigDecimal x, la précision de l’entier, échelle entier, boolean forceEncrypt)   <br/><br/>  public updateString void (int columnIndex, chaîne stringValue, boolean forceEncrypt)  <br/><br/>  public updateNString void (int columnIndex, nString de chaîne, booléen forceEncrypt)  <br/><br/>  public updateNString void (columnLabel de chaîne, chaîne nString, boolean forceEncrypt)  <br/><br/> public updateBytes void (int index, octet x[], boolean forceEncrypt)   <br/><br/>  public updateDate void (index de type int, java.sql.Date x forceEncrypt booléenne)  <br/><br/> updateTime void publique (index int, java.sql.Time, entier échelle x, boolean forceEncrypt)   <br/><br/> updateTimestamp void publique (index int, java.sql.Timestamp, int échelle x, boolean forceEncrypt)   <br/><br/> updateDateTime void publique (index int, java.sql.Timestamp x, l’échelle entier, boolean forceEncrypt)   <br/><br/> updateSmallDateTime void publique (index int, java.sql.Timestamp x, l’échelle entier, boolean forceEncrypt)   <br/><br/>  updateDateTimeOffset void publique (int index, microsoft.sql.DateTimeOffset x, l’échelle entier, boolean forceEncrypt)  <br/><br/> updateUniqueIdentifier void publique (int index, la chaîne x forceEncrypt booléenne)    <br/><br/>  public updateObject void (int index, objet x, int précision, échelle int, boolean forceEncrypt)  <br/><br/>  public updateObject void (int index, objet obj, SQLType targetSqlType, mise à l’échelle du type int, boolean forceEncrypt)  <br/><br/> public updateBoolean void (nom de colonne de chaîne, booléen x, forceEncrypt booléenne)    <br/><br/>  public updateByte void (nom de colonne de chaîne, octet x forceEncrypt booléenne)  <br/><br/>  public updateShort void (nom de colonne de chaîne, x court, boolean forceEncrypt)  <br/><br/> public updateInt void (nom de colonne de chaîne, int x forceEncrypt booléenne)   <br/><br/>   public updateLong void (nom de colonne de chaîne, long x, boolean forceEncrypt) <br/><br/>  public updateFloat void (nom de colonne de chaîne, float x, boolean forceEncrypt)  <br/><br/>  public updateDouble void (nom de colonne de chaîne, double x, boolean forceEncrypt)  <br/><br/> updateBigDecimal void publique (chaîne columnName, BigDecimal x forceEncrypt booléenne)   <br/><br/>  public updateBigDecimal void (nom de colonne de chaîne, BigDecimal x, la précision de l’entier, échelle entier, boolean forceEncrypt)  <br/><br/> updateString void publique (columnName de chaîne, chaîne x forceEncrypt booléenne)   <br/><br/>  public updateBytes void (nom de colonne de chaîne, octet x[], boolean forceEncrypt)  <br/><br/> public updateDate void (nom de colonne de chaîne, java.sql.Date x forceEncrypt booléenne)   <br/><br/>  updateTime void publique (nom de colonne de chaîne, java.sql.Time x, l’échelle int, boolean forceEncrypt)  <br/><br/>  updateTimestamp void publique (nom de colonne de chaîne, java.sql.Timestamp, int échelle x, boolean forceEncrypt)  <br/><br/> updateDateTime void publique (nom de colonne de chaîne, java.sql.Timestamp, int échelle x, boolean forceEncrypt)   <br/><br/>  updateSmallDateTime void publique (nom de colonne de chaîne, java.sql.Timestamp, int échelle x, boolean forceEncrypt)  <br/><br/>  updateDateTimeOffset void publique (nom de colonne de chaîne, microsoft.sql.DateTimeOffset x, l’échelle int, boolean forceEncrypt)  <br/><br/>  updateUniqueIdentifier void publique (columnName de chaîne, chaîne x forceEncrypt booléenne)<br/><br/>public updateObject void (nom de colonne de chaîne, objet x, int précision, échelle int, boolean forceEncrypt)<br/><br/>public updateObject void (columnName de chaîne, objet obj, SQLType targetSqlType, montée en puissance int, boolean forceEncrypt)|Mettre à jour la colonne désignée à la valeur java donnée.<br/><br/>Si le forceEncrypt booléenne est définie sur true, la colonne n’a la valeur si elle est chiffrée et Always Encrypted est activé sur la connexion ou l’instruction.<br/><br/>Si le forceEncrypt booléenne est définie sur false, le pilote ne force pas le chiffrement sur les paramètres.|

  
Nouveaux types dans **microsoft.sql.Types** classe
|Nom| Description|  
|----------|-----------------|  
|DATETIME, SMALLDATETIME, MONEY, SMALLMONEY, GUID|Utilisez ces types comme types SQL cible lors de l’envoi des valeurs de paramètre à **chiffrées** datetime, smalldatetime, money, smallmoney, les colonnes uniqueidentifier à l’aide des méthodes de setObject()/updateObject() API.|  
  
  
 **SQLServerStatementColumnEncryptionSetting Enum**  
  
 Spécifie comment les données sont envoyées et reçues lors de la lecture et l’écriture des colonnes chiffrées. Selon votre requête spécifique, l’impact sur les performances peut être réduit en ignorant le pilote Always Encrypted traitement lorsque les colonnes non chiffrées sont utilisés. Notez que ces paramètres ne peuvent pas être utilisés pour contourner le chiffrement et accéder aux données en texte brut.  
  
 **Syntaxe**  
  
```  
Public enum  SQLServerStatementColumnEncryptionSetting  
```  
  
 **Membres**  
  
|Nom| Description|  
|----------|-----------------|  
|UseConnectionSetting|Spécifie que la commande doit utiliser par défaut pour le paramètre Always Encrypted dans la chaîne de connexion.|  
|Activé|Active le chiffrement intégral pour la requête.|  
|ResultSetOnly|Spécifie que seuls les résultats de la commande doivent être traités par la routine Always Encrypted dans le pilote. Utilisez cette valeur lors de la commande n’a pas de paramètres qui exigent un chiffrement.|  
|Désactivé|Désactive le chiffrement intégral pour la requête.|  
  
 Le paramètre de niveau instruction pour AE sont ajoutés à la classe SQLServerConnection et à la classe SQLServerConnectionPoolProxy. Les méthodes suivantes dans ces classes sont surchargées avec le nouveau paramètre.  
  
|Nom| Description|  
|----------|-----------------|  
|public createStatement d’instruction (%nLes d’int, int nConcur, int statementHoldability, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)|Crée un objet d’instruction qui génère les objets de jeu de résultats avec le paramètre de chiffrement de type, d’accès concurrentiel, mise en attente et de colonne donné.|  
|public prepareCall de CallableStatement (chaîne sql, %nLes d’int, int nConcur, int statementHoldability, SQLServerStatementColumnEncryptionSetting stmtColEncSetiing)|Crée un objet de CallableStatement avec le paramètre de chiffrement de colonne donnée qui génère les objets de jeu de résultats avec le type donné, la concurrence et la mise en attente.|  
|public prepareStatement de PreparedStatement (chaîne sql, int autogeneratedKeys, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)|Crée un objet PreparedStatement avec le paramètre de chiffrement de colonne spécifiée qui a la possibilité de récupérer les clés générées automatiquement.|  
|public prepareStatement de PreparedStatement (sql de la chaîne, chaîne [] columnNames, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)|Crée un objet PreparedStatement avec le paramètre de chiffrement de colonne donnée qui génère les objets de jeu de résultats avec les noms de colonne donnée.|  
|public prepareStatement de PreparedStatement (chaîne sql, int [] columnIndexes, SQLServerStatementColumnEncryptionSetting stmtColEncSetting|Crée un objet PreparedStatement avec le paramètre de chiffrement de colonne donnée qui génère les objets de jeu de résultats avec les index de colonne donnée.|  
|public prepareStatement de PreparedStatement (chaîne sql, %nLes d’int, int nConcur, int nHold, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)|Crée un objet PreparedStatement avec le paramètre de chiffrement de colonne donnée qui génère les objets de jeu de résultats avec le type donné, la concurrence et la mise en attente.|  
  
> [!NOTE]  
>  Si Always Encrypted est désactivé pour une requête et la requête contient des paramètres qui doivent être chiffrées (les paramètres qui correspondent aux colonnes chiffrées), la requête échoue.  
>   
>  Si Always Encrypted est désactivé pour une requête et la requête retourne des résultats des colonnes chiffrées, la requête retournera les valeurs chiffrées. Les valeurs chiffrées aura le type de données varbinary.  
  
 ## <a name="see-also"></a>Voir aussi  
 [Utilisation du chiffrement intégral avec le pilote JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)  
  

