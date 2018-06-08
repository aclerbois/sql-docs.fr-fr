---
title: Prise en charge du pilote JDBC pour la haute disponibilité, la récupération d’urgence | Documents Microsoft
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 62de4be6-b027-427d-a7e5-352960e42877
caps.latest.revision: 40
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 77ebc4b570d74fca0ec5bfbde5fc8d34c3f3aa30
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jdbc-driver-support-for-high-availability-disaster-recovery"></a>Pilote JDBC pour la prise en charge de la haute disponibilité et de la récupération d'urgence
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cette rubrique traite [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prise en charge de la récupération d’urgence haute disponibilité-- [!INCLUDE[ssHADR](../../includes/sshadr_md.md)]. Pour plus d'informations sur [!INCLUDE[ssHADR](../../includes/sshadr_md.md)], consultez la documentation en ligne de [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)] .  
  
 À compter de la version 4.0 de la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], vous pouvez spécifier l’écouteur du groupe de disponibilité d’une (haute disponibilité, récupération d’urgence) le groupe de disponibilité (AG) dans la propriété de connexion. Si un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] application est connectée à une base de données AlwaysOn qui bascule, la connexion d’origine est rompue et l’application doit ouvrir une nouvelle connexion pour continuer à fonctionner après le basculement. Les éléments suivants [propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md) ont été ajoutées dans [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)]:  
  
-   **multiSubnetFailover**  
  
-   **applicationIntent**
 
Spécifiez multiSubnetFailover = true lors de la connexion à l’écouteur de groupe de disponibilité d’un groupe de disponibilité ou d’une Instance de Cluster de basculement. Notez que **multiSubnetFailover** a la valeur false par défaut. Utilisez **applicationIntent** pour déclarer le type de charge de travail d’application. Consultez les sections ci-dessous pour plus de détails.
 
Depuis la version 6.0 du pilote JDBC Microsoft pour SQL Server, une nouvelle propriété de connexion **transparentNetworkIPResolution** (TNIR) est ajoutée pour la connexion transparente aux groupes de disponibilité Always On ou à un serveur qui a plusieurs adresses IP associées. Lorsque **transparentNetworkIPResolution** a la valeur true, le pilote tente de se connecter à la première adresse IP disponible. Si la première tentative échoue, le pilote tente de se connecter à toutes les adresses IP en parallèle jusqu'à ce que le délai expire, en ignorant les tentatives de connexion en attente lorsqu’un d’eux réussisse.   

Veuillez noter que :
* transparentNetworkIPResolution a la valeur true par défaut
* transparentNetworkIPResolution est ignorée si multiSubnetFailover a la valeur true
* transparentNetworkIPResolution est ignorée si la mise en miroir de base de données est utilisé.
* transparentNetworkIPResolution est ignorée s’il y a plus de 64 adresses IP
* Lorsque transparentNetworkIPResolution est true, la première tentative de connexion utilise une valeur de délai d’attente de 500 ms. Reste de tentatives de connexion suit la même logique que dans la fonction multiSubnetFailover. 

> [!NOTE]  
Si vous utilisez Microsoft JDBC Driver 4.2 (ou réduire) pour SQL Server et si **multiSubnetFailover** a la valeur false, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] essaie de se connecter à la première adresse IP. Si le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ne peut pas établir une connexion avec la première adresse IP, la connexion échoue. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ne tente pas de se connecter à l’adresse IP suivante associé au serveur. 

  
> [!NOTE]  
>  L'augmentation du délai de connexion et l'implémentation de la logique de tentative de connexion augmente la probabilité qu'une application se connecte à un groupe de disponibilité. En raison du risque d'échec de connexion en cas de basculement d'un groupe de disponibilité, il est également nécessaire d'implémenter la logique de déclenchement de nouvelles tentatives de connexion, afin de multiplier les tentatives jusqu'à ce qu'une connexion soit établie.  
  
 
  
## <a name="connecting-with-multisubnetfailover"></a>Connexion à MultiSubnetFailover  
 Toujours spécifier **multiSubnetFailover = true** lors de la connexion à l’écouteur de groupe de disponibilité d’un [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)] groupe de disponibilité ou un [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)] Instance de Cluster de basculement. **multiSubnetFailover** permet un basculement plus rapide pour tous les groupes de disponibilité et les instances de cluster de basculement dans [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)] et réduit considérablement le temps de basculement pour les topologies AlwaysOn de sous-réseaux uniques et multiples. Lors d'un basculement de sous-réseaux multiples, le client tente les connexions en parallèle. Lors d’un basculement de sous-réseau, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] sera intensément d’établir la connexion TCP.  
  
 Le **multiSubnetFailover** propriété de connexion indique que l’application est déployée dans un groupe de disponibilité ou Instance de Cluster de basculement et que le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] essaient de se connecter à la base de données sur le serveur principal [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] traite de l’instance en essayant de se connecter à tous les l’adresse IP. Lorsque **MultiSubnetFailover = true** est spécifié pour une connexion, le client tente de tentatives de connexion TCP plus rapidement que les intervalles de retransmission TCP par défaut du système d’exploitation. Cela permet une reconnexion plus rapide après le basculement d'un groupe de disponibilité AlwaysOn ou d'une instance de cluster de basculement AlwaysOn et s'applique aux groupes de disponibilité et aux instances de cluster de basculement uniques et à plusieurs sous-réseaux.  
  
 Pour plus d’informations sur les mots clés de chaîne de connexion dans le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
 Spécification de **multiSubnetFailover = true** lorsque la connexion à un élément autre qu’un écouteur du groupe de disponibilité ou d’une Instance de Cluster de basculement peut provoquer une diminution des performances et n’est pas pris en charge.  
  
 Si le gestionnaire de sécurité n'est pas installé, la machine virtuelle Java met en cache les adresses IP virtuelles pour une durée définie par défaut par votre implémentation JDK et les propriétés Java networkaddress.cache.ttl et networkaddress.cache.negative.ttl. Si le gestionnaire de sécurité JDK est installé, la machine virtuelle Java mettra en cache les adresses IP virtuelles et n'actualisera pas le cache par défaut. Vous devez définir la durée de vie (TTL, time-to-live – networkaddress.cache.ttl) sur un jour pour le cache de la machine virtuelle Java. Si vous ne modifiez pas la valeur par défaut en attribuant une valeur égale à un jour (environ), l'ancienne valeur ne sera pas supprimée définitivement du cache de la machine virtuelle Java lorsqu'une adresse IP virtuelle sera ajoutée ou mise à jour. Pour plus d’informations sur networkaddress.cache.ttl et networkaddress.cache.negative.ttl, consultez [ http://download.oracle.com/javase/6/docs/technotes/guides/net/properties.html ](http://download.oracle.com/javase/6/docs/technotes/guides/net/properties.html).  
  
 Utilisez les instructions suivantes pour la connexion à un serveur dans un groupe de disponibilité ou dans une instance de cluster de basculement :  
  
-   Le pilote génère une erreur si le **instanceName** propriété de connexion est utilisée dans la même chaîne de connexion que le **multiSubnetFailover** propriété de connexion. Cela révèle que SQL Browser n'est pas utilisé dans un groupe de disponibilité. Toutefois, si le **numéro_port** propriété de connexion est également spécifiée, le pilote ignorera **instanceName** et utiliser **numéro_port**.  
  
-   Utilisez le **multiSubnetFailover** propriété de connexion lors de la connexion à un sous-réseau unique ou plusieurs sous-réseaux, il améliore les performances pour les deux.  
  
-   Pour vous connecter à un groupe de disponibilité, spécifiez l'écouteur du groupe de disponibilité en tant que serveur dans votre chaîne de connexion. Par exemple, jdbc:sqlserver://VNN1.  
  
-   La connexion à une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] configurée avec plus de 64 adresses IP provoque un échec de connexion.  
  
-   Comportement d’une application qui utilise le **multiSubnetFailover** propriété de connexion n’est pas affectée selon le type d’authentification : [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, l’authentification Kerberos ou l’authentification Windows.  
  
-   Augmentez la valeur de **loginTimeout** pour tenir compte des temps de basculement et de réduire le nombre de tentatives de connexion application.  
  
-   Les transactions distribuées ne sont pas prises en charge.  
  
 Si le routage en lecture seule n'est pas appliqué, la connexion à un emplacement de réplica secondaire dans un groupe de disponibilité échoue dans les situations suivantes :  
  
1.  Si l'emplacement du réplica secondaire n'est pas configuré pour accepter des connexions.  
  
2.  Si une application utilise **applicationIntent = ReadWrite** (voir ci-dessous) et l’emplacement du réplica secondaire est configuré pour l’accès en lecture seule.  
  
 Une connexion échoue si un réplica principal est configuré pour rejeter des charges de travail en lecture seule et si la chaîne de connexion contient **ApplicationIntent=ReadOnly**.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Mise à niveau pour utiliser des clusters de sous-réseaux multiples à partir de la mise en miroir de bases de données  
 Si vous mettez à niveau un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] application qui utilise actuellement la mise en miroir de base de données à un scénario de sous-réseaux multiples, vous devez supprimer la **failoverPartner** propriété de connexion et remplacez-la par **multiSubnetFailover**  la valeur **true** et remplacez le nom du serveur dans la chaîne de connexion avec un écouteur de groupe de disponibilité. Si une chaîne de connexion utilise **failoverPartner** et **multiSubnetFailover = true**, le pilote génère une erreur. Toutefois, si une chaîne de connexion utilise **failoverPartner** et **multiSubnetFailover = false** (ou **ApplicationIntent = ReadWrite**), l’application utilise la base de données mise en miroir.  
  
 Le pilote retournera une erreur si la mise en miroir de base de données est utilisée sur la base de données primaire dans le groupe de disponibilité et si **multiSubnetFailover = true** est utilisé dans la chaîne de connexion qui se connecte à une base de données principale au lieu d’un groupe de disponibilité écouteur.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="new-methods-supporting-multisubnetfailover-and-applicationintent"></a>Nouvelles méthodes prenant en charge multiSubnetFailover et applicationIntent  
 Les méthodes suivantes vous donnent un accès à la **multiSubnetFailover**, **applicationIntent** et **transparentNetworkIPResolution** chaîne de connexion mots clés :  
  
-   [SQLServerDataSource.getApplicationIntent](../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)  
  
-   [SQLServerDataSource.setApplicationIntent](../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)  
  
-   [SQLServerDataSource.getMultiSubnetFailover](../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)  
  
-   [SQLServerDataSource.setMultiSubnetFailover](../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)  
  
-   [SQLServerDriver.getPropertyInfo](../../connect/jdbc/reference/getpropertyinfo-method-sqlserverdriver.md)  

-   SQLServerDataSource.setTransparentNetworkIPResolution

-   SQLServerDataSource.getTransparentNetworkIPResolution
  
 Le **getMultiSubnetFailover**, **setMultiSubnetFailover**, **getApplicationIntent**, **setApplicationIntent**, **getTransparentNetworkIPResolution** et **setTransparentNetworkIPResolution** méthodes sont également ajoutés à [classe SQLServerDataSource](../../connect/jdbc/reference/sqlserverdatasource-class.md), [ Classe SQLServerConnectionPoolDataSource](../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md), et [classe SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md).  
  
## <a name="ssl-certificate-validation"></a>Validation du certificat SSL  
 Un groupe de disponibilité se compose de plusieurs serveurs physiques. [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)] prise en charge de **autre nom du sujet** dans les certificats SSL pour plusieurs hôtes peuvent être associés avec le même certificat. Pour plus d’informations sur SSL, consultez [prise en charge de SSL compréhension](../../connect/jdbc/understanding-ssl-support.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)   
 [Définition des propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md)  
  
  
