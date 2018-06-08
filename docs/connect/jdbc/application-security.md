---
title: Sécurité des applications | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 940879b4-aa0f-41ce-a369-6cfc0e78e01d
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8226badb1031792badc1601cd12c2a0e2f13c9bd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="application-security"></a>Sécurité des applications
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Lorsque vous utilisez le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], il est important de prendre des précautions pour garantir la sécurité de votre application. Les sections suivantes contiennent des informations sur les diverses mesures que vous pouvez prendre pour sécuriser votre application.  
  
## <a name="using-java-policy-permissions"></a>Utilisation d'autorisations de stratégie Java  
 Lorsque vous utilisez le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], il est important de spécifier les autorisations de stratégie Java requises par le pilote JDBC. Java Runtime Environment (JRE) intègre un modèle de sécurité étendu que vous pouvez utiliser en cours d'exécution pour déterminer si un thread a accès à une ressource. Les fichiers de politique de sécurité peuvent contrôler cet accès. Les fichiers de politique sont gérés par le déployeur et l'administrateur système pour le conteneur ; cependant, les autorisations répertoriées dans cette rubrique sont celles qui affectent le fonctionnement du pilote JDBC.  
  
 Voici à quoi ressemble une autorisation de politique dans le fichier de politique.  
  
```  
// Example policy file entry.  
grant [signedBy <signer>,] [codeBase <code source>] {  
   permission  <class>  [<name> [, <action list>]];  
};  
```  
  
 Le code base suivant doit être limité au code base du pilote JDBC afin d'octroyer le moins possible de privilèges.  
  
```  
grant codeBase "file:/install_dir/lib/-" {  
  
// Grant access to data source.  
permission java.util.PropertyPermission "java.naming.*", "read,write";  
  
// Specify which hosts can be connected to.  
permission java.net.socketPermission "host:port", "connect";  
  
// Logger permission to take advantage of logging.  
permission java.util.logging.LoggingPermission;  
  
// Grant listen/connect/accept permissions to the driver if   
// connecting to a named instance as the client driver.   
// This connects to a udp service and listens for a response.  
permission java.net.SocketPermission "*", "listen, connect, accept";   
};   
```  
  
> [!NOTE]  
>  Le code « file:/install_dir/lib/- » fait référence au répertoire d'installation du pilote JDBC.  
  
## <a name="protecting-server-communication"></a>Protection de la communication avec le serveur  
 Lorsque vous utilisez le pilote JDBC pour communiquer avec un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données, vous pouvez sécuriser le canal de communication à l’aide de la sécurité du protocole Internet (IPSEC) ou couche de Sockets sécurisée (SSL) ; ou vous pouvez utiliser les deux.  
  
 La prise en charge du protocole SSL peut être utilisée pour fournir un niveau supplémentaire de protection conjointement avec la sécurité IPSEC. Pour plus d’informations sur l’utilisation de SSL, consultez [le chiffrement SSL à l’aide de](../../connect/jdbc/using-ssl-encryption.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurisation des applications de pilote JDBC](../../connect/jdbc/securing-jdbc-driver-applications.md)  
  
  
