---
title: Prise en charge SQL Server Native Client pour la haute disponibilité, la récupération d’urgence | Documents Microsoft
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 2b06186b-4090-4728-b96b-90d6ebd9f66f
caps.latest.revision: 35
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 193226bd9ef0b21771479e245c69cd4ea2402a08
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-native-client-support-for-high-availability-disaster-recovery"></a>Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Cette rubrique décrit la prise en charge de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (ajouté dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]) pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Pour plus d’informations sur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clustering de basculement et groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md), et [Secondaires actifs : réplicas secondaires lisibles actifs &#40;groupes de disponibilité AlwaysOn&#41;](~/database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 Vous pouvez spécifier l’écouteur d’un groupe de disponibilité donné dans la chaîne de connexion. Si une application [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est connectée à une base de données dans un groupe de disponibilité qui bascule, la connexion d'origine est rompue et l'application doit ouvrir une nouvelle connexion pour reprendre le travail après le basculement.  
  
 Si vous ne vous connectez pas à un écouteur du groupe de disponibilité et si plusieurs adresses IP sont associées à un nom d'hôte, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client effectue une itération de façon séquentielle parmi toutes les adresses IP associées à l'entrée DNS. Cette opération peut prendre du temps si la première adresse IP retournée par le serveur DNS n'est liée à aucune carte d'interface réseau (NIC). Lors de la connexion à un écouteur du groupe de disponibilité, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client tente d'établir des connexions à toutes les adresses IP en parallèle et, si une tentative de connexion réussit, le pilote ignore toutes les tentatives de connexion en attente.  
  
> [!NOTE]  
>  L'augmentation du délai de connexion et l'implémentation de la logique de tentative de connexion augmente la probabilité qu'une application se connecte à un groupe de disponibilité. En raison du risque d'échec de connexion en cas de basculement d'un groupe de disponibilité, il est également nécessaire d'implémenter la logique de déclenchement de nouvelles tentatives de connexion, afin de multiplier les tentatives jusqu'à ce qu'une connexion soit établie.  
  
## <a name="connecting-with-multisubnetfailover"></a>Connexion à MultiSubnetFailover  
 Spécifiez toujours **MultiSubnetFailover=Yes** lors de la connexion à un écouteur du groupe de disponibilité SQL Server 2012 ou à une instance de cluster de basculement SQL Server 2012. **MultiSubnetFailover** permet un basculement plus rapide pour tous les groupes de disponibilité et de cluster de basculement de l’instance de SQL Server 2012 et réduisent considérablement le temps de basculement de sous-réseaux uniques et multiples Always On topologies. Lors d'un basculement de sous-réseaux multiples, le client tente les connexions en parallèle. Lors d'un basculement de sous-réseau, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client retente de façon intensive d'établir la connexion TCP.  
  
 La propriété de connexion **MultiSubnetFailover** indique que l’application est déployée sur un groupe de disponibilité ou une instance de cluster de basculement et que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client tente de se connecter à la base de données sur l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] principale en essayant toutes les adresses IP du groupe de disponibilité. Quand **MultiSubnetFailover=Yes** est spécifié dans le cadre d’une connexion, le client retente d’établir une connexion TCP plus rapidement que les intervalles de retransmission TCP par défaut du système d’exploitation. Cela permet une reconnexion plus rapide après le basculement d’un groupe de disponibilité AlwaysOn ou d’une toujours sur Instance Cluster de basculement et s’applique à la fois unique et à plusieurs sous-réseaux groupes de disponibilité et Instances de Cluster de basculement.  
  
 Pour plus d’informations sur les mots clés de chaîne de connexion, consultez [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 La spécification de **MultiSubnetFailover=Yes** quand la connexion n’est pas établie avec un écouteur de groupe de disponibilité ou une instance de cluster de basculement peut avoir un impact négatif sur les performances et n’est pas prise en charge.  
  
 Utilisez les instructions suivantes pour la connexion à un serveur dans un groupe de disponibilité ou dans une instance de cluster de basculement :  
  
-   Utilisez la propriété de connexion**MultiSubnetFailover** quand vous vous connectez à un sous-réseau unique ou à des sous-réseaux multiples pour améliorer leurs performances.  
  
-   Pour vous connecter à un groupe de disponibilité, spécifiez l'écouteur du groupe de disponibilité en tant que serveur dans votre chaîne de connexion.  
  
-   La connexion à une instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurée avec plus de 64 adresses IP provoque un échec de connexion.  
  
-   Le comportement d’une application qui utilise la propriété de connexion **MultiSubnetFailover** peut ne pas être affecté en fonction du type d’authentification : authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], authentification Kerberos ou authentification Windows.  
  
-   Vous pouvez augmenter la valeur de **loginTimeout** pour tenir compte du temps de basculement et réduire les nouvelles tentatives de connexion de l’application.  
  
-   Les transactions distribuées ne sont pas prises en charge.  
  
 Si le routage en lecture seule n'est pas appliqué, la connexion à un emplacement de réplica secondaire dans un groupe de disponibilité échoue dans les situations suivantes :  
  
1.  Si l'emplacement du réplica secondaire n'est pas configuré pour accepter des connexions.  
  
2.  Si une application utilise **ApplicationIntent=ReadWrite**(voir ci-dessous) et si l’emplacement de réplica secondaire est configuré pour un accès en lecture seule.  
  
 Une connexion échoue si un réplica principal est configuré pour rejeter des charges de travail en lecture seule et si la chaîne de connexion contient **ApplicationIntent=ReadOnly**.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Mise à niveau pour utiliser des clusters de sous-réseaux multiples à partir de la mise en miroir de bases de données  
 Une erreur de connexion se produit si les mots clés de connexion **MultiSubnetFailover** et **Failover_Partner** sont présents dans la chaîne de connexion. Une erreur se produit également si **MultiSubnetFailover** est utilisé et si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] retourne une réponse de partenaire de basculement qui indique qu’il fait partie d’une paire de mise en miroir de bases de données.  
  
 Si vous mettez à niveau une application [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client qui utilise actuellement la mise en miroir de bases de données vers un scénario de sous-réseaux multiples, vous devez supprimer la propriété de connexion **Failover_Partner** et la remplacer par **MultiSubnetFailover** avec la valeur **Yes** et remplacer le nom du serveur dans la chaîne de connexion par un écouteur du groupe de disponibilité. Si une chaîne de connexion utilise **Failover_Partner** et **MultiSubnetFailover=Yes**, le pilote génère une erreur. Toutefois, si une chaîne de connexion utilise **Failover_Partner** et **MultiSubnetFailover=No** (ou **ApplicationIntent=ReadWrite**), l’application utilise la mise en miroir de bases de données.  
  
 Le pilote retournera une erreur si la mise en miroir de bases de données est utilisée pour la base de données principale au sein du groupe de disponibilité, et si **MultiSubnetFailover=Yes** est utilisé dans la chaîne de connexion qui établit une connexion avec une base de données principale au lieu d’un écouteur de groupe de disponibilité.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="odbc"></a>ODBC  
 Deux mots clés de chaîne de connexion ODBC ont été ajoutés pour prendre en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client :  
  
-   **ApplicationIntent**  
  
-   **MultiSubnetFailover**  
  
 Pour plus d’informations sur les mots clés de chaîne de connexion ODBC dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, consultez [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Les propriétés de connexion équivalentes sont :  
  
-   **SQL_COPT_SS_APPLICATION_INTENT**  
  
-   **SQL_COPT_SS_MULTISUBNET_FAILOVER**  
  
 Pour plus d’informations sur les propriétés de connexion ODBC dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, consultez [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
 Les fonctionnalités des mots clés **ApplicationIntent** et **MultiSubnetFailover** sont exposées dans l’Administrateur de sources de données ODBC pour les noms de source de données (DSN) qui utilisent le pilote [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, à compter de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 Une application ODBC [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client peut utiliser l'une de ces trois fonctions pour établir la connexion :  
  
|Fonction| Description|  
|--------------|-----------------|  
|[SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)|La liste de serveurs retournée par **SQLBrowseConnect** n’inclut pas de VNN. Vous verrez seulement une liste de serveurs sans indication si ces derniers sont des serveurs autonomes, ou un serveur principal ou secondaire dans un cluster de Clustering de basculement Windows Server (WSFC) qui contient deux instances ou plus de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] activées pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Si la connexion à un serveur se solde par un échec, c’est peut-être parce que le paramètre **ApplicationIntent** n’est pas compatible avec la configuration de ce serveur.<br /><br /> Étant donné que **SQLBrowseConnect** ne reconnaît pas de serveurs dans un cluster Clustering de basculement Windows Server (WSFC) qui contient deux d’instances ou plus de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] activées pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], **SQLBrowseConnect** ignore le mot clé de chaîne de connexion **MultiSubnetFailover**.|  
|[SQLConnect](../../../relational-databases/native-client-odbc-api/sqlconnect.md)|**SQLConnect** prend en charge **ApplicationIntent** et **MultiSubnetFailover** par le biais d’un nom de source de données ou de propriétés de connexion.|  
|[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)|**SQLDriverConnect** prend en charge **ApplicationIntent** et **MultiSubnetFailover** par le biais de mots clés de chaîne de connexion, de propriétés de connexion ou d’un nom de source de données.|  
  
## <a name="ole-db"></a>OLE DB  
 OLE DB dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne prend pas en charge le mot clé **MultiSubnetFailover**.  
  
 OLE DB dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prendra en charge l'intention de l'application. L'intention de l'application se comporte de manière identique pour les applications OLE DB et les applications ODBC (voir ci-dessus).  
  
 Un mot clé de chaîne de connexion OLE DB a été ajouté pour prendre en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client :  
  
-   **Intention de l’application**  
  
 Pour plus d’informations sur les mots clés de chaîne de connexion dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, consultez [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Les propriétés de connexion équivalentes sont :  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
 Une application OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client peut utiliser l'une de ces méthodes pour spécifier l'intention de l'application :  
  
 **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** utilise le jeu de propriétés configuré précédemment pour initialiser la source de données et créer l’objet source de données. Spécifiez l'intention de l'application en tant que propriété de fournisseur ou dans le cadre de la chaîne de propriétés étendues.  
  
 **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** accepte une chaîne de connexion d’entrée qui peut contenir le mot clé **Application Intent**.  
  
 **IDBProperties::GetProperties**  
 **IDBProperties::GetProperties** récupère la valeur de la propriété définie actuellement sur la source de données.  Vous pouvez récupérer la valeur **Application Intent** au moyen des propriétés DBPROP_INIT_PROVIDERSTRING et SSPROP_INIT_APPLICATIONINTENT.  
  
 **IDBProperties::SetProperties**  
 Pour définir la valeur de propriété **ApplicationIntent**, appelez **IDBProperties::SetProperties** qui passe la propriété **SSPROP_INIT_APPLICATIONINTENT** avec la valeur « **ReadWrite** » ou « **ReadOnly** » ou **DBPROP_INIT_PROVIDERSTRING** avec la valeur qui contient « **ApplicationIntent=ReadOnly** » ou « **ApplicationIntent=ReadWrite** ».  
  
 Vous pouvez spécifier l’intention de l’application dans le champ Propriétés de l’intention de l’application de l’onglet Tous de la boîte de dialogue **Propriétés de liaison de données**.  
  
 Lorsque des connexions implicites sont établies, la connexion implicite utilise le paramètre d'intention de l'application de la connexion parente. De la même façon, plusieurs sessions créées à partir de la même source de données héritent du paramètre d'intention de l'application de la source de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [À l’aide de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
  
