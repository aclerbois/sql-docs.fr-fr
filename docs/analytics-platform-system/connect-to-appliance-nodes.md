---
title: Se connecter à l’appliance nœuds - système de plateforme Analytique | Documents Microsoft
description: Cet article explique les différentes façons de se connecter à chaque nœud au niveau du matériel de système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 2d7d634023c5fc3d0a6f522b5f60933ce3b96272
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="connect-to-appliance-nodes-in-analytics-platform-system"></a>Se connecter à des nœuds de matériel dans le système de plateforme Analytique
Cet article explique les différentes façons de se connecter à chaque nœud au niveau du matériel de système de plateforme Analytique.  
  
## <a name="connecting-with-hadoop"></a>Connexion à Hadoop  
Avant d’utiliser Hadoop avec SQL Server PDW, demandez à votre administrateur de matériel pour installer l’environnement d’exécution Java sur SQL Server PDW. Pour obtenir des instructions, consultez [configurer la connectivité PolyBase pour données externes &#40;système de plateforme Analytique&#41; ](configure-polybase-connectivity-to-external-data.md) dans le dispositif Operations Guide.  
  
## <a name="ConnectingToIndividualNodes"></a>Connexion à des nœuds de matériel  
Chaque nœud de la solution est accessible directement que dans les scénarios d’utilisation spécifique et par les types d’utilisateur spécifique. Le tableau suivant répertorie chaque nœud de matériel et les scénarios dans lesquels les utilisateurs se connectent directement à ce nœud.  
  
<!-- MISSING LINKS For information on the purpose of each node, see [Understanding SQL Server PDW &#40;SQL Server PDW&#41;](../sqlpdw/understanding-sql-server-pdw-sql-server-pdw.md).  -->  
  
|||  
|-|-|  
|**Nœud**|**Scénarios d’accès**|  
|Nœud de contrôle|Utilisez un navigateur web pour accéder à la Console d’administration, qui s’exécute sur le nœud du contrôle. Pour plus d’informations, consultez [contrôler le matériel à l’aide de la Console d’administration &#40;système de plateforme Analytique&#41;](monitor-the-appliance-by-using-the-admin-console.md).<br /><br />Tous les outils et les applications clientes se connectent au nœud de contrôle, indépendamment de si la connexion utilise Ethernet ou InfiniBand.<br /><br />Pour configurer une connexion Ethernet au nœud de contrôle, utilisez l’adresse IP de Cluster du nœud contrôle et le port **17001**. Par exemple, « 192.168.0.1,17001 ».<br /><br />Pour configurer une connexion InfiniBand au nœud de contrôle, utilisez ***appliance_domain *-SQLCTL01** et le port **17001**. À l’aide de ***appliance_domain *-SQLCTL01**, le serveur DNS se connecte à votre serveur au réseau InfiniBand actif. Pour configurer votre serveur autre que l’appliance pour l’utiliser, consultez [configurer des cartes réseau InfiniBand](configure-infiniband-network-adapters.md).<br /><br />L’administrateur de l’application se connecte au nœud de contrôle pour effectuer des opérations de gestion. Par exemple, l’administrateur de l’application effectue les opérations suivantes à partir du nœud de contrôle :<br /><br />Configurer le système de plateforme Analytique avec le **dwconfig.exe** outil de configuration.|  
|Nœud de calcul|Calcul des connexions de nœud sont dirigées par le nœud de contrôle. Les adresses IP des nœuds de calcul ne sont jamais entrés dans les commandes de l’application en tant que paramètres.<br /><br />Pour le chargement, sauvegarde, copie de la Table distante et Hadoop, SQL Server PDW envoyer ou recevoir des données directement en parallèle entre les nœuds de calcul et les serveurs non-dispositif de nœuds. Ces applications se connectent avec SQL Server PDW en vous connectant au nœud de contrôle et le nœud contrôle dirige ensuite SQL Server PDW pour établir la communication entre les nœuds de calcul et le serveur autre que l’appliance.<br /><br />Par exemple, ces opérations de transfert de données se produisent en parallèle avec des connexions directes aux nœuds de calcul :<br /><br />Chargement à partir du serveur lors du chargement de SQL Server PDW.<br /><br />Sauvegarde une base de données à partir de SQL Server PDW dans le serveur de sauvegarde.<br /><br />Restauration d’une base de données à partir de la sauvegarde du serveur SQL Server PDW.<br /><br />Interrogation des données Hadoop à partir de SQL Server PDW.<br /><br />Exportation de données à une table Hadoop externe à partir de SQL Server PDW.<br /><br />Copie d’une table SQL Server PDW pour une base de données SMP SQL Server distant.|  
  
## <a name="connecting-to-the-ethernet-and-infiniband-networks"></a>Connexion à la carte Ethernet et les réseaux InfiniBand  
Serveurs distants peuvent se connecter via le réseau InfiniBand appliance ou via le réseau Ethernet. Pour des raisons de performances, le chargement des serveurs, les serveurs de sauvegarde et de recevoir des données via des serveurs **CREATE distant TABLE AS SELECT** généralement, les instructions, transférer des données via le réseau InfiniBand de matériel.  
  
Vous pouvez configurer des serveurs non-appliance fasse partie de votre propre client de groupe de travail ou un domaine et ensuite utiliser votre propre réseau de client pour se connecter à ces serveurs et le transfert de données sur les. En outre, les serveurs non-matériel connectés à l’appliance InfiniBand ont la possibilité pour transférer des données entre eux via le réseau InfiniBand de matériel. à utiliser avec prudence, car elle peut ralentir les performances de SQL Server PDW.  
  
Par exemple, cette instruction ajoute des informations d’identification réseau pour effectuer des sauvegardes via InfiniBand vers un serveur de sauvegarde qui a une adresse InfiniBand IP 192.168.0.1. Le domaine d’application est *mypdw*, et l’instruction est exécutée à partir de la sauvegarde du serveur. Avant d’exécuter cette instruction, vous devez entrer vos propres paramètres.  
  
```  
sqlcmd -S "mypdw-sqlctl01,17001" -U sa -P password -I -Q "exec sp_pdw_add_network_credentials '192.168.0.1', 'mypdw\Administrator', 'password'"  
```  
  
Par exemple, une commande de chargement démarre avec les éléments suivants :  
  
```  
dwloader –S mypdw-SQLCTL01  
```  
  
<!-- MISSING LINKS ## See Also  
[Configure an External Windows System To Receive Remote Table Copies Using InfiniBand &#40;SQL Server PDW&#41;](../sqlpdw/configure-an-external-windows-system-to-receive-remote-table-copies-using-infiniband-sql-server-pdw.md)  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
  
