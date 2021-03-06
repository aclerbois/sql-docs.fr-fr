---
title: "Sécurisation des Applications JDBC Driver | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: jdbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 90724ec6-a9cb-43ef-903e-793f89410bc0
caps.latest.revision: "19"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: On Demand
ms.openlocfilehash: 7e26574a8a11256136b376546e1f52533a986711
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="securing-jdbc-driver-applications"></a>Sécurisation des applications de pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Amélioration de la sécurité d’un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] application implique plus d’éviter les pièges de codage courants. Une application qui accède à des données présente de nombreux points de défaillance potentiels qu'un intrus peut exploiter en vue d'extraire, de manipuler ou de détruire des données sensibles. Il est important de comprendre tous les aspects de la sécurité, du processus de modélisation des menaces durant la phase de conception de votre application jusqu'à son déploiement final, sans oublier sa maintenance continue.  
  
 Les rubriques de cette section décrivent certains problèmes de sécurité courants, liés entre autres aux chaînes de connexion, à la validation de l'entrée d'utilisateur et à la sécurité générale des applications.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Sécurisation de chaînes de connexion](../../connect/jdbc/securing-connection-strings.md)|Décrit des techniques permettant d'aider à protéger les informations utilisées pour se connecter à une source de données.|  
|[Validation des entrées utilisateur](../../connect/jdbc/validating-user-input.md)|Décrit des techniques permettant de valider l'entrée utilisateur.|  
|[Sécurité des applications](../../connect/jdbc/application-security.md)|Décrit comment utiliser des autorisations de stratégie Java afin d'aider à sécuriser une application de pilote JDBC.|  
|[Utilisation du chiffrement SSL](../../connect/jdbc/using-ssl-encryption.md)|Décrit comment établir un canal de communication sécurisé avec un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] de base de données à l’aide de la couche de Sockets sécurisée (SSL).|  
|[Mode FIPS](../../connect/jdbc/fips-mode.md)|Décrit comment utiliser le pilote JDBC en mode conforme FIPS.| 
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
