---
title: Classe SQLServerXADataSource | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 95fc7b07-2498-4a7e-8f7f-ee0d86b598b4
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b69edd13e84599e4e3e11ba56a3a1c34ee1d36ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverxadatasource-class"></a>Classe SQLServerXADataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente une fabrique pour [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) objets qui est utilisée en interne.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
 **Implémente :** javax.sql.XADataSource  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public class SQLServerXADataSource  
```  
  
## <a name="remarks"></a>Notes  
 Un objet qui implémente l’interface SQLServerXADataSource est généralement inscrit avec un service d’affectation de noms qui utilise le Java Naming and Directory Interface JNDI ().  
  
 La classe SQLServerXADataSource fournit des connexions de base de données pour une utilisation dans des transactions distribuées (XA). La classe SQLServerXADataSource prend également en charge le regroupement de connexions physiques. Les interfaces SQLServerXADataSource et SQLServerXAConnection, qui sont définies dans le package javax.sql, sont implémentées par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
 Un objet SQLServerXAConnection est une connexion regroupée qui peut participer à une transaction distribuée. Plus précisément, SQLServerXAConnection étend l’interface SQLServerPooledConnection en ajoutant la méthode [getXAResource](../../../connect/jdbc/reference/getxaresource-method-sqlserverxaconnection.md). Cette méthode produit un [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) objet qui peut être utilisé par un gestionnaire de transactions pour coordonner le travail effectué sur la connexion avec les autres participants de la transaction distribuée. Fait qu’ils étendent l’interface SQLServerPooledConnection, les objets SQLServerXAConnection prend en charge toutes les méthodes d’objets de SQLServerPooledConnection. Il s'agit de connexions physiques réutilisables à une source de données sous-jacente qui produisent des handles de connexions logiques pouvant être renvoyés à une application JDBC.  
  
 Les objets SQLServerXAConnection sont produites par un objet SQLServerXADataSource. Objets de SQLServerConnectionPoolDataSource et SQLServerXADataSource sont similaires, car ils sont implémentés sous une couche de source de données qui est visible par l’application JDBC. Cette architecture permet [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] prend en charge les transactions distribuées de façon transparente pour l’application. SQLServerXADataSource peut être configuré pour s’intégrer avec [!INCLUDE[msCoName](../../../includes/msconame_md.md)] coordinateur de transactions distribuées (DTC) pour fournir la valeur true, de traitement des transactions distribuées.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [Référence d’API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
