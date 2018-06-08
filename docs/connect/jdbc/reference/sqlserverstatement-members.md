---
title: Membres de SQLServerStatement | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 828cbaa9-ea7a-4986-95c3-5ba0d7d01d83
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 77161ec9693615eb50d4e62fb9cd1f6f185d23fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverstatement-members"></a>Membres de SQLServerStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Nom| Description|  
|----------|-----------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverstatement.md)|Ajoute la commande SQL donnée à la liste actuelle des commandes pour ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[annuler](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|Annule l’instruction SQL en cours d’exécution par ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverstatement.md)|Vide la liste actuelle des commandes SQL pour ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|Efface tous les avertissements signalés sur cet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[Fermer](../../../connect/jdbc/reference/close-method-sqlserverstatement.md)|Cela libère [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) l’objet de base de données et des ressources JDBC immédiatement au lieu de patienter jusqu'à leur libération automatique.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)|Exécute l'instruction SQL donnée, qui peut retourner plusieurs résultats.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverstatement.md)|Envoie un lot de commandes à exécuter à la base de données. En cas d'exécution correcte de toutes les commandes, un tableau des nombres de mises à jour est retourné.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverstatement.md)|Exécute l’instruction SQL fournie et retourne un seul [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)|Exécute l'instruction SQL fournie, qui peut être une instruction INSERT, UPDATE, MERGE ou DELETE ; ou une instruction SQL qui ne retourne rien, telle qu'une instruction SQL DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|Récupère le [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet qui a généré ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|Récupère la direction de l’extraction de lignes à partir des tables de base de données qui est la valeur par défaut pour les jeux de résultats générés à partir de ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|Récupère le nombre de résultats du jeu de lignes qui est la taille d’extraction par défaut pour les objets générés à partir de ce jeu de résultats [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|Récupère toute clé générée automatiquement qui est créés à la suite de l’exécution de ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|Récupère le nombre maximal d’octets qui peuvent être retournées pour les caractères et les valeurs de colonne binaire dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet qui est généré par ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|Récupère le nombre maximal de lignes qui une [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet qui est généré par ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet peut contenir.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|Passe au résultat suivant de ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|Récupère le nombre de secondes pendant lesquelles le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] attendra pour ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet s’exécute.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|Récupère la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|Récupère le résultat actuel en tant qu’un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|Récupère le jeu de résultats d’accès concurrentiel pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|Récupère le jeu de résultats mise en attente pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|Récupère le jeu de résultats type pour [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets générés par ce [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|Récupère le résultat actuel en tant que nombre de mises à jour.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|Récupère le premier avertissement signalé par des appels sur cet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet.|  
|[IsClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|Indique si cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet a été fermé.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|Retourne une valeur qui indique s'il est possible d'ajouter une instruction au regroupement d'instructions fourni par l'utilisateur.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)|Indique si cet objet d'instruction est un wrapper pour l'interface spécifiée.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|Définit le nom de curseur SQL selon la chaîne donnée, qui est exécutée par les méthodes d'exécution suivantes.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|Définit le mode de traitement d'échappement.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|Fournit un conseil au pilote JDBC concernant la direction de traitement des lignes du jeu de résultats.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites depuis la base de données, lorsque davantage de lignes sont nécessaires. |  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|Définit la limite pour le nombre maximal d’octets dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) colonne qui stocke des valeurs de caractères ou binaires pour le nombre d’octets spécifié.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|Définit la limite pour le nombre maximal de lignes que tout [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet peut contenir le nombre donné.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|Demande qu'une instruction soit regroupée ou non.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|Définit le nombre de secondes pendant lesquelles le pilote doit attendre un [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet à exécuter pour le nombre de secondes donné.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|Définit la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet à la casse **chaîne complète** ou **adaptive**.|  
|[Unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md)|Retourne un objet qui implémente l’interface spécifiée pour autoriser l’accès à la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-des méthodes spécifiques.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
