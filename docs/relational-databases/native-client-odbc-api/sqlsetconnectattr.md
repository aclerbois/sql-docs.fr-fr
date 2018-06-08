---
title: SQLSetConnectAttr | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetConnectAttr function
ms.assetid: d21b5cf1-3724-43f7-bc96-5097df0677b4
caps.latest.revision: 106
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5ad5c5425427f9bf5b8f7e6177379d91ebd10d6f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sqlsetconnectattr"></a>SQLSetConnectAttr
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ignore le paramètre de SQL_ATTR_CONNECTION_TIMEOUT.  
  
 SQL_ATTR_TRANSLATE_LIB est également ignoré ; la spécification d'une autre bibliothèque de traduction n'est pas prise en charge. Pour permettre aux applications d'être aisément déplacées pour utiliser un pilote Microsoft ODBC pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], toute valeur définie avec SQL_ATTR_TRANSLATE_LIB sera copiée vers et depuis une mémoire tampon dans le gestionnaire de pilotes.  
  
 Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client implémente l'isolement des transactions de lecture renouvelée comme sérialisable.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit la prise en charge d'un nouvel attribut d'isolation des transactions, SQL_COPT_SS_TXN_ISOLATION. L'attribution de la valeur SQL_TXN_SS_SNAPSHOT à SQL_COPT_SS_TXN_ISOLATION indique que la transaction aura lieu avec le niveau d'isolement d'instantané.  
  
> [!NOTE]  
>  SQL_ATTR_TXN_ISOLATION peut être utilisé pour définir tous les autres niveaux d'isolement à l'exception de SQL_TXN_SS_SNAPSHOT. Si vous souhaitez utiliser le niveau d'isolement d'instantané, vous devez définir SQL_TXN_SS_SNAPSHOT par le biais de SQL_COPT_SS_TXN_ISOLATION. Toutefois, vous pouvez récupérer le niveau d'isolement en utilisant SQL_ATTR_TXN_ISOLATION ou SQL_COPT_SS_TXN_ISOLATION.  
  
 La promotion des attributs d'instruction ODBC en attributs de connexion peut avoir des conséquences inattendues. Les attributs d'instruction qui demandent des curseurs côté serveur pour le traitement du jeu de résultats peuvent être promus pour la connexion. Par exemple, affecter à l'attribut d'instruction ODBC SQL_ATTR_CONCURRENCY une valeur plus restrictive que la valeur SQL_CONCUR_READ_ONLY par défaut conduit le pilote à utiliser des curseurs dynamiques pour toutes les instructions soumises sur la connexion. L'exécution d'une fonction de catalogue ODBC sur une instruction sur la connexion retourne SQL_SUCCESS_WITH_INFO et un enregistrement de diagnostic qui indique que le comportement du curseur a été converti en lecture seule. La tentative d'exécution d'une instruction Transact-SQL SELECT contenant une clause COMPUTE sur la même connexion échoue.  
  
 Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prend en charge plusieurs extensions spécifiques au pilote des attributs de connexion ODBC définis dans sqlncli.h. Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client peut requérir que l'attribut soit défini avant la connexion, ou il peut ignorer l'attribut si celui-ci est déjà défini. Le tableau ci-dessous répertorie les restrictions.  
  
|Attribut SQL Server|Défini avant ou après la connexion au serveur|  
|--------------------------|----------------------------------------------|  
|SQL_COPT_SS_ANSI_NPW|Avant|  
|SQL_COPT_SS_APPLICATION_INTENT|Avant|  
|SQL_COPT_SS_ATTACHDBFILENAME|Avant|  
|SQL_COPT_SS_BCP|Avant|  
|SQL_COPT_SS_BROWSE_CONNECT|Avant|  
|SQL_COPT_SS_BROWSE_SERVER|Avant|  
|SQL_COPT_SS_CONCAT_NULL|Avant|  
|SQL_COPT_SS_CONNECTION_DEAD|After|  
|SQL_COPT_SS_ENCRYPT|Avant|  
|SQL_COPT_SS_ENLIST_IN_DTC|After|  
|SQL_COPT_SS_ENLIST_IN_XA|After|  
|SQL_COPT_SS_FALLBACK_CONNECT|Avant|  
|SQL_COPT_SS_FAILOVER_PARTNER|Avant|  
|SQL_COPT_SS_INTEGRATED_SECURITY|Avant|  
|SQL_COPT_SS_MARS_ENABLED|Avant|  
|SQL_COPT_SS_MULTISUBNET_FAILOVER|Avant|  
|SQL_COPT_SS_OLDPWD|Avant|  
|SQL_COPT_SS_PERF_DATA|After|  
|SQL_COPT_SS_PERF_DATA_LOG|After|  
|SQL_COPT_SS_PERF_DATA_LOG_NOW|After|  
|SQL_COPT_SS_PERF_QUERY|After|  
|SQL_COPT_SS_PERF_QUERY_INTERVAL|After|  
|SQL_COPT_SS_PERF_QUERY_LOG|After|  
|SQL_COPT_SS_PRESERVE_CURSORS|Avant|  
|SQL_COPT_SS_QUOTED_IDENT|Avant ou après|  
|SQL_COPT_SS_TRANSLATE|Avant ou après|  
|SQL_COPT_SS_TRUST_SERVER_CERTIFICATE|Avant|  
|SQL_COPT_SS_TXN_ISOLATION|Avant ou après|  
|SQL_COPT_SS_USE_PROC_FOR_PREP|Avant ou après|  
|SQL_COPT_SS_USER_DATA|Avant ou après|  
|SQL_COPT_SS_WARN_ON_CP_ERROR|Avant|  
  
 L'utilisation d'un attribut de préconnexion et la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] équivalente pour la même session, base de données ou état [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut produire un comportement inattendu. Par exemple :  
  
```  
SQLSetConnectAttr(SQL_COPT_SS_QUOTED_IDENT, SQL_QI_ON) // turn ON via attribute  
SQLDriverConnect(…);  
SQLExecDirect("SET QUOTED_IDENTIFIER OFF") // turn OFF via Transact-SQL  
SQLSetConnectAttr(SQL_ATTR_CURRENT_CATALOG, …) // restores to pre-connect attribute value  
```  
  
## <a name="sqlcoptssansinpw"></a>SQL_COPT_SS_ANSI_NPW  
 SQL_COPT_SS_ANSI_NPW active ou désactive l'utilisation de la gestion ISO des valeurs NULL dans les comparaisons et la concaténation, le remplissage des types de données caractère et les avertissements. Pour plus d'informations, consultez SET ANSI_NULLS, SET ANSI_PADDING, SET ANSI_WARNINGS et SET CONCAT_NULL_YIELDS_NULL.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_AD_ON|Valeur par défaut. La connexion utilise le comportement par défaut ANSI pour gérer les comparaisons NULL, le remplissage, les avertissements et les concaténations NULL.|  
|SQL_AD_OFF|La connexion utilise la gestion définie par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de NULL, le remplissage des types de données caractère et les avertissements.|  
  
 Si vous utilisez le regroupement de connexions, SQL_COPT_SS_ANSI_NPW doit être défini dans la chaîne de connexion, plutôt qu’avec SQLSetConnectAttr. Après avoir établi une connexion, toute tentative de modification de cet attribut échouera silencieusement lorsque le regroupement de connexions est utilisé.  
  
## <a name="sqlcoptssapplicationintent"></a>SQL_COPT_SS_APPLICATION_INTENT  
 Déclare le type de la charge de travail de l'application lors de la connexion à un serveur. Les valeurs possibles sont **Readonly** et **ReadWrite**. Par exemple :  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_APPLICATION_INTENT, TEXT("Readonly"), SQL_NTS)  
```  
  
 La valeur par défaut est **ReadWrite**. Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prise en charge Native Client pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] groupes de disponibilité, consultez [SQL Server Native Client prend en charge pour la haute disponibilité, la récupération d’urgence](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
## <a name="sqlcoptssattachdbfilename"></a>SQL_COPT_SS_ATTACHDBFILENAME  
 SQL_COPT_SS_ATTACHDBFILENAME spécifie le nom du fichier primaire d'une base de données pouvant être attachée. Cette base de données est attachée et devient la base de données par défaut de la connexion. Pour utiliser sql_copt_ss_attachdbfilename, vous devez spécifier le nom de la base de données en tant que la valeur de l’attribut de connexion SQL_ATTR_CURRENT_CATALOG ou dans la base de données = le paramètre d’un [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md). Si la base de données a été attachée précédemment, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne l'attachera pas de nouveau.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQLPOINTER vers une chaîne de caractères|La chaîne contient le nom du fichier primaire de la base de données à attacher. Incluez le chemin d'accès complet du fichier.|  
  
## <a name="sqlcoptssbcp"></a>SQL_COPT_SS_BCP  
 SQL_COPT_SS_BCP active les fonctions de copie en bloc sur une connexion. Pour plus d’informations, consultez [les fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md).  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_BCP_OFF|Valeur par défaut. Les fonctions de copie en bloc ne sont pas disponibles sur la connexion.|  
|SQL_BCP_ON|Les fonctions de copie en bloc sont disponibles sur la connexion.|  
  
## <a name="sqlcoptssbrowseconnect"></a>SQL_COPT_SS_BROWSE_CONNECT  
 Cet attribut est utilisé pour personnaliser le jeu de résultats retourné par [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md). SQL_COPT_SS_BROWSE_CONNECT active ou désactive le retour d'informations supplémentaires à partir d'une instance énumérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela peut inclure des informations, comme par exemple si le serveur est un cluster, les noms d'instances différentes et le numéro de version.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_MORE_INFO_NO|Valeur par défaut. Retourne une liste de serveurs.|  
|SQL_MORE_INFO_YES|**SQLBrowseConnect** retourne une chaîne étendue de propriétés du serveur.|  
  
## <a name="sqlcoptssbrowseserver"></a>SQL_COPT_SS_BROWSE_SERVER  
 Cet attribut est utilisé pour personnaliser le jeu de résultats retourné par **SQLBrowseConnect**. SQL_COPT_SS_BROWSE_SERVER Spécifie le nom du serveur pour lequel **SQLBrowseConnect** retourne les informations.  
  
|Valeur| Description|  
|-----------|-----------------|  
|computername|**SQLBrowseConnect** retourne une liste d’instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur spécifié. Doubles barres obliques inverses (\\\\) ne doit pas être utilisée pour le nom du serveur (par exemple, au lieu de \\\MyServer, MyServer doit être utilisé).|  
|NULL|Valeur par défaut. **SQLBrowseConnect** renvoie des informations pour tous les serveurs dans le domaine.|  
  
## <a name="sqlcoptssconcatnull"></a>SQL_COPT_SS_CONCAT_NULL  
 SQL_COPT_SS_CONCAT_NULL active ou désactive l'utilisation de la gestion ISO des valeurs NULL lors de la concaténation de chaînes. Pour plus d'informations, consultez SET CONCAT_NULL_YIELDS_NULL.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_CN_ON|Valeur par défaut. La connexion utilise le comportement par défaut ISO pour gérer les valeurs NULL lors de la concaténation de chaînes.|  
|SQL_CN_OFF|La connexion utilise le comportement défini par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour gérer les valeurs NULL lors de la concaténation de chaînes.|  
  
## <a name="sqlcoptssencrypt"></a>SQL_COPT_SS_ENCRYPT  
 Contrôle le chiffrement pour une connexion.  
  
 Le chiffrement utilise le certificat sur le serveur. Cela doit être vérifié par une autorité de certification, à moins que l'attribut de connexion SQL_COPT_SS_TRUST_SERVER_CERTIFICATE ait la valeur SQL_TRUST_SERVER_CERTIFICATE_YES ou que la chaîne de connexion contienne "TrustServerCertificate=yes". Si l'une ou l'autre de ces conditions est vraie, un certificat généré et signé par le serveur peut être utilisé pour chiffrer la connexion si aucun certificat n'est présent sur le serveur.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_EN_ON|La connexion sera chiffrée.|  
|SQL_EN_OFF|La connexion ne sera pas chiffrée. Il s'agit du paramètre par défaut.|  
  
## <a name="sqlcoptssenlistindtc"></a>SQL_COPT_SS_ENLIST_IN_DTC  
 Le client appelle le service Microsoft Distributed Transaction Coordinator (MS DTC) OLE DB **ITransactionDispenser::BeginTransaction** méthode pour commencer une transaction MS DTC et créer un objet de transaction MS DTC qui représente la transaction. L’application appelle ensuite **SQLSetConnectAttr** avec l’option SQL_COPT_SS_ENLIST_IN_DTC pour associer l’objet de transaction de la connexion ODBC. Toute l'activité de base de données connexe sera effectuée sous la protection de la transaction MS DTC. L’application appelle **SQLSetConnectAttr** avec SQL_DTC_DONE pour terminer l’association de DTC de la connexion.  
  
|Valeur| Description|  
|-----------|-----------------|  
|Objet DTC*|Objet de transaction OLE de MS DTC qui spécifie la transaction à exporter vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SQL_DTC_DONE|Délimite la fin d'une transaction DTC.|  
  
## <a name="sqlcoptssenlistinxa"></a>SQL_COPT_SS_ENLIST_IN_XA  
 Pour commencer une transaction XA avec un processeur compatible XA Transaction (TP), le client appelle la méthode Open Group **tx_begin** (fonction). L’application appelle ensuite **SQLSetConnectAttr** avec un paramètre sql_copt_ss_enlist_in_xa ayant la valeur TRUE pour associer la transaction XA à la connexion ODBC. Toute l'activité de base de données connexe sera effectuée sous la protection de la transaction XA. Pour mettre fin à une association XA avec une connexion ODBC, le client doit appeler **SQLSetConnectAttr** avec un paramètre sql_copt_ss_enlist_in_xa ayant la valeur FALSE. Pour plus d'informations, consultez la documentation sur Microsoft Distributed Transaction Coordinator.  
  
## <a name="sqlcoptssfallbackconnect"></a>SQL_COPT_SS_FALLBACK_CONNECT  
 Cet attribut n'est plus pris en charge.  
  
## <a name="sqlcoptssfailoverpartner"></a>SQL_COPT_SS_FAILOVER_PARTNER  
 Utilisé pour spécifier ou récupérer le nom du serveur partenaire de basculement utilisé pour la mise en miroir de bases de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et il s'agit d'une chaîne de caractères terminée par le caractère NULL qui doit être définie avant que la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soit effectuée initialement.  
  
 Après avoir établi la connexion, l’application peut interroger cet attribut à l’aide de [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) pour déterminer l’identité du partenaire de basculement. Si le serveur principal n'a pas de partenaire de basculement, cette propriété retourne une chaîne vide. Cela permet à une application intelligente de mettre en cache le serveur de sauvegarde le plus récent ; toutefois, de telles applications doivent prendre en compte le fait que les informations sont mises à jour uniquement lorsque la connexion est établie au préalable, ou réinitialisée en cas de regroupement, et que ces informations peuvent devenir obsolètes dans le cas de connexions à long terme.  
  
 Pour plus d’informations, consultez [à l’aide de la mise en miroir de la base de données](../../relational-databases/native-client/features/using-database-mirroring.md).  
  
## <a name="sqlcoptssintegratedsecurity"></a>SQL_COPT_SS_INTEGRATED_SECURITY  
 SQL_COPT_SS_INTEGRATED_SECURITY force l'utilisation de l'authentification Windows pour la validation de l'accès sur la connexion au serveur. Lorsque l’authentification Windows est utilisée, le pilote ignore les valeurs d’identificateur et le mot de passe utilisateur fournis dans le cadre de **SQLConnect**, [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md), ou [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) de traitement.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_IS_OFF|Valeur par défaut. L'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisée pour valider l'identificateur d'utilisateur et le mot de passe lors de la connexion.|  
|SQL_IS_ON|Le mode d'authentification Windows est utilisé pour valider les droits d'accès d'un utilisateur à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="sqlcoptssmarsenabled"></a>SQL_COPT_SS_MARS_ENABLED  
 Cet attribut active ou désactive MARS (Multiple Active Result Sets). MARS est désactivé par défaut. Cet attribut doit être défini avant l'établissement d'une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une fois que la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est ouverte, MARS reste activé ou désactivé durant toute la durée de vie de la connexion.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_MARS_ENABLED_NO|Valeur par défaut. MARS (Multiple Active Result Sets) est désactivé.|  
|SQL_MARS_ENABLED_YES|MARS est activé.|  
  
 Pour plus d’informations sur MARS, consultez [à l’aide de jeux de résultats actifs multiples &#40;MARS&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
## <a name="sqlcoptssmultisubnetfailover"></a>SQL_COPT_SS_MULTISUBNET_FAILOVER  
 Si votre application se connecte à un groupe de disponibilité [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] sur des sous-réseaux différents, cette propriété de connexion configure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client afin de permettre une détection plus rapide du serveur (actuellement) actif et la connexion à ce dernier. Par exemple :  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_MULTISUBNET_FAILOVER, SQL_IS_ON, SQL_IS_INTEGER)  
```  
  
 Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prise en charge Native Client pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] groupes de disponibilité, consultez [SQL Server Native Client prend en charge pour la haute disponibilité, la récupération d’urgence](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_IS_ON|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client permet une reconnexion plus rapide en cas de basculement.|  
|SQL_IS_OFF|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ne permet pas une reconnexion plus rapide en cas de basculement.|  
  
## <a name="sqlcoptssoldpwd"></a>SQL_COPT_SS_OLDPWD  
 L'expiration du mot de passe pour l'authentification SQL Server a été introduite dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. L'attribut SQL_COPT_SS_OLDPWD a été ajouté pour permettre au client de fournir à la fois l'ancien et le nouveau mots de passe pour la connexion. Lorsque cette propriété est définie, le fournisseur n'utilisera pas le pool de connexions pour la première connexion ou pour les connexions suivantes, puisque la chaîne de connexion contiendra l'« ancien mot de passe » qui a maintenant changé.  
  
 Pour plus d’informations, consultez [la modification des mots de passe par programmation](../../relational-databases/native-client/features/changing-passwords-programmatically.md).  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_COPT_SS_OLD_PASSWORD|SQLPOINTER vers une chaîne de caractères contenant l'ancien mot de passe. Cette valeur est en écriture seule et doit être définie avant la connexion au serveur.|  
  
## <a name="sqlcoptssperfdata"></a>SQL_COPT_SS_PERF_DATA  
 SQL_COPT_SS_PERF_DATA lance ou arrête la journalisation des données de performances. Il convient de définir le nom du fichier journal des données avant de commencer la journalisation des données. Consultez SQL_COPT_SS_PERF_DATA_LOG ci-dessous.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_PERF_START|Démarre le pilote qui échantillonne les données de performances.|  
|SQL_PERF_STOP|Arrête les compteurs d'échantillonnage des données de performances.|  
  
 Pour plus d’informations, consultez [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
## <a name="sqlcoptssperfdatalog"></a>SQL_COPT_SS_PERF_DATA_LOG  
 SQL_COPT_SS_PERF_DATA_LOG attribue le nom du fichier journal utilisé pour enregistrer les données de performances. Le nom du fichier journal est une chaîne ANSI ou Unicode terminée par le caractère Null qui dépend de la compilation de l'application. Le *StringLength* argument doit être SQL_NTS.  
  
## <a name="sqlcoptssperfdatalognow"></a>SQL_COPT_SS_PERF_DATA_LOG_NOW  
 SQL_COPT_SS_PERF_DATA_LOG_NOW indique au pilote d'écrire une entrée de journal de statistiques sur le disque. Le *StringLength* argument doit être SQL_NTS.  
  
## <a name="sqlcoptssperfquery"></a>SQL_COPT_SS_PERF_QUERY  
 SQL_COPT_SS_PERF_QUERY démarre ou arrête la journalisation pour les requêtes longues. Il convient de fournir le nom du fichier journal des requêtes avant de commencer la journalisation. L'application peut définir "longues" en définissant l'intervalle pour la journalisation.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_PERF_START|Démarre la journalisation des requêtes longues.|  
|SQL_PERF_STOP|Arrête la journalisation des requêtes longues.|  
  
 Pour plus d’informations, consultez [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
## <a name="sqlcoptssperfqueryinterval"></a>SQL_COPT_SS_PERF_QUERY_INTERVAL  
 SQL_COPT_SS_PERF_QUERY_INTERVAL définit le seuil de journalisation des requêtes en millisecondes. Les requêtes qui ne sont pas résolues dans cette limite sont enregistrées dans le fichier journal des requêtes longues. Le seuil de requête ne présente pas de limite supérieure. Une valeur de seuil de requête égale à zéro entraîne la consignation de toutes les requêtes.  
  
## <a name="sqlcoptssperfquerylog"></a>SQL_COPT_SS_PERF_QUERY_LOG  
 SQL_COPT_SS_PERF_QUERY_LOG attribue le nom d'un fichier journal pour la journalisation des données des requêtes longues. Le nom du fichier journal est une chaîne ANSI ou Unicode terminée par le caractère Null qui dépend de la compilation de l'application. Le *StringLength* argument doit être SQL_NTS ou la longueur de la chaîne en octets.  
  
## <a name="sqlcoptsspreservecursors"></a>SQL_COPT_SS_PRESERVE_CURSORS  
 Cet attribut vous permet d'interroger et de définir si la connexion conservera ou non le ou les curseurs lorsque vous validez/annulez une transaction. Le paramètre est SQL_PC_ON ou SQL_PC_OFF. La valeur par défaut est SQL_PC_OFF. Ce paramètre contrôle si le pilote fermera l’ou les curseurs pour vous lorsque vous appelez [SQLEndTran](../../relational-databases/native-client-odbc-api/sqlendtran.md) (ou SQLTransact).  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_PC_OFF|Valeur par défaut. Les curseurs sont fermés lorsque la transaction est validée ou annulée à l’aide **SQLEndTran**.|  
|SQL_PC_ON|Les curseurs ne sont pas fermés lors de la transaction est validée ou annulée à l’aide **SQLEndTran**, sauf lors de l’utilisation d’un curseur statique ou jeu de clés en mode asynchrone. Si une annulation est effectuée alors que le remplissage du curseur n'est pas complet, le curseur est fermé.|  
  
## <a name="sqlcoptssquotedident"></a>SQL_COPT_SS_QUOTED_IDENT  
 SQL_COPT_SS_QUOTED_IDENT autorise les identificateurs entre guillemets dans les instructions ODBC et Transact-SQL soumises sur la connexion. En fournissant des identificateurs entre guillemets, le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client autorise des noms d'objets qui seraient autrement non valides, tels que "My Table", qui contient un espace. Pour plus d'informations, consultez SET QUOTED_IDENTIFIER.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_QI_OFF|La connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas les identificateurs entre guillemets dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] soumises.|  
|SQL_QI_ON|Valeur par défaut. La connexion autorise les identificateurs entre guillemets dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] soumises.|  
  
## <a name="sqlcoptsstranslate"></a>SQL_COPT_SS_TRANSLATE  
 SQL_COPT_SS_TRANSLATE conduit le pilote à traduire les caractères entre les pages de codes client et serveur lorsque des données MBCS sont échangées. L’attribut affecte uniquement les données stockées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **char**, **varchar**, et **texte** colonnes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_XL_OFF|Le pilote ne traduit pas les caractères d'une page de codes à une autre dans les données de type caractère échangées entre le client et le serveur.|  
|SQL_XL_ON|Valeur par défaut. Le pilote traduit les caractères d'une page de codes à une autre dans les données de type caractère échangées entre le client et le serveur. Le pilote configure automatiquement la traduction du caractère, déterminant la page de codes installée sur le serveur et celle utilisée par le client.|  
  
## <a name="sqlcoptsstrustservercertificate"></a>SQL_COPT_SS_TRUST_SERVER_CERTIFICATE  
 SQL_COPT_SS_TRUST_SERVER_CERTIFICATE conduit le pilote à activer ou désactiver la validation de certificat lors de l'utilisation du chiffrement. Cet attribut est une valeur en lecture/écriture, mais le définir après l'établissement d'une connexion n'a aucun effet.  
  
 Les applications clientes peuvent interroger cette propriété après qu'une connexion a été ouverte pour déterminer le chiffrement effectif et les paramètres de validation utilisés.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_TRUST_SERVER_CERTIFICATE_NO|Valeur par défaut. Le chiffrement sans validation de certificat n'est pas activé.|  
|SQL_TRUST_SERVER_CERTIFICATE_YES|Le chiffrement sans validation de certificat est activé.|  
  
## <a name="sqlcoptsstxnisolation"></a>SQL_COPT_SS_TXN_ISOLATION  
 SQL_COPT_SS_TXN_ISOLATION définit l'attribut du niveau d'isolement d'instantané spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le niveau d'isolement d'instantané ne peut pas être défini à l'aide de SQL_ATTR_TXN_ISOLATION car la valeur est spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutefois, elle peut être extraite à l'aide de SQL_ATTR_TXN_ISOLATION ou de SQL_COPT_SS_TXN_ISOLATION.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_TXN_SS_SNAPSHOT|Indique qu'il n'est pas possible de voir à partir d'une transaction les modifications apportées dans d'autres transactions, et cela même si vous réexécutez la requête.|  
  
 Pour plus d’informations sur l’isolation d’instantané, consultez [Working with Snapshot Isolation](../../relational-databases/native-client/features/working-with-snapshot-isolation.md).  
  
## <a name="sqlcoptssuseprocforprep"></a>SQL_COPT_SS_USE_PROC_FOR_PREP  
 Cet attribut n'est plus pris en charge.  
  
## <a name="sqlcoptssuserdata"></a>SQL_COPT_SS_USER_DATA  
 SQL_COPT_SS_USER_DATA définit le pointeur de données utilisateur. Les données utilisateur correspondent à la mémoire détenue par le client et enregistrée par connexion.  
  
 Pour plus d’informations, consultez [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
## <a name="sqlcoptsswarnoncperror"></a>SQL_COPT_SS_WARN_ON_CP_ERROR  
 Cet attribut détermine si vous obtenez un avertissement en cas de perte de données lors de la conversion d'une page de codes. Cela s'applique uniquement aux données provenant du serveur.  
  
|Valeur| Description|  
|-----------|-----------------|  
|SQL_WARN_YES|Générer des avertissements lorsqu'une perte de données est constatée pendant la conversion de page de codes.|  
|SQL_WARN_NO|(Par défaut) Ne pas générer d'avertissements lorsqu'une perte de données est constatée pendant la conversion de page de codes.|  
  
## <a name="sqlsetconnectattr-support-for-service-principal-names-spns"></a>Prise en charge de SQLSetConnectAttr pour les noms de principaux du service (SPN)  
 SQLSetConnectAttr peut être utilisé pour définir la valeur de nouveaux attributs de connexion SQL_COPT_SS_SERVER_SPN et SQL_COPT_SS_FAILOVER_PARTNER_SPN. Ces attributs ne peuvent pas être définis lorsqu'une connexion est ouverte ; si vous essayez de définir ces attributs lorsqu'une connexion est ouverte, l'erreur HY011 est retournée avec le message « Opération actuellement non valide ». (SQLSetConnectOption peut également être utilisée pour définir ces valeurs.)  
  
 Pour plus d’informations sur les SPN, consultez [noms principaux de Service & #40 ; Noms principaux de service & #41 ; dans les connexions clientes & #40 ; ODBC & #41 ; ](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md).  
  
## <a name="sqlcoptssconnectiondead"></a>SQL_COPT_SS_CONNECTION_DEAD  
 Ceci est un attribut de lecture seule.  
  
 Pour plus d’informations sur SQL_COPT_SS_CONNECTION_DEAD, consultez [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) et [la connexion à une Source de données &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/connecting-to-a-data-source-odbc.md).  
  
## <a name="example"></a>Exemple  
 Cet exemple consigne les données de performances.  
  
```  
SQLPERF*     pSQLPERF;  
SQLINTEGER   nValue;  
  
// See if you are already logging. SQLPERF* will be NULL if not.  
SQLGetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA, &pSQLPERF,  
    sizeof(SQLPERF*), &nValue);  
  
if (pSQLPERF == NULL)  
    {  
    // Set the performance log file name.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG,  
        (SQLPOINTER) "\\My LogDirectory\\MyServerLog.txt", SQL_NTS);  
  
    // Start logging...  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
        (SQLPOINTER) SQL_PERF_START, SQL_IS_INTEGER);  
    }  
else  
    {  
    // Take a snapshot now so that your performance statistics are discernible.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
    }  
  
    // ...perform some action...  
  
// ...take a performance data snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
    // ...perform more actions...  
  
// ...take another snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
// ...and disable logging.  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
    (SQLPOINTER) SQL_PERF_STOP, SQL_IS_INTEGER);  
  
// Continue on...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction SQLSetConnectAttr](http://go.microsoft.com/fwlink/?LinkId=59368)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)   
 [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET QUOTED_IDENTIFIER & #40 ; Transact-SQL & #41 ;](../../t-sql/statements/set-quoted-identifier-transact-sql.md)   
 [Fonction SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360)   
 [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md)  
  
  
