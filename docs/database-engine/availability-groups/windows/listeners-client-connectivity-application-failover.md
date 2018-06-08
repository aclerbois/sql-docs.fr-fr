---
title: Écouteurs, connectivité client et basculement d'application | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
caps.latest.revision: 48
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: be1ab43de5f792d2ff4aeac7d73c361b047b897b
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769905"
---
# <a name="listeners-client-connectivity-application-failover"></a>Écouteurs, connectivité client et basculement d'application
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique contient des informations sur les éléments à prendre en compte en matière de connectivité client [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et de fonctionnalité de basculement d'application.  
  
> [!NOTE]  
>  Pour la majorité des configurations habituelles d'écouteur, vous pouvez créer le premier écouteur de groupe de disponibilité simplement à l'aide d'instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou d'applets de commande PowerShell. Pour plus d'informations, consultez [Tâches associées](#RelatedTasks), plus loin dans cette rubrique.  
  
 **Dans cette rubrique :**  
  
-   [Écouteurs de groupe de disponibilité](#AGlisteners)  
  
-   [Utilisation d'un écouteur pour se connecter au réplica principal](#ConnectToPrimary)  
  
-   [Utilisation d'un écouteur pour se connecter à un réplica secondaire en lecture seule (routage en lecture seule)](#ConnectToSecondary)  
  
    -   [Pour configurer des réplicas de disponibilité pour le routage en lecture seule](#ConfigureARsForROR)  
  
    -   [Intention de l'application en lecture seule et routage en lecture seule](#ReadOnlyAppIntent)  
  
-   [Contournement des écouteurs de groupe de disponibilité](#BypassAGl)  
  
-   [Comportement des connexions clientes lors du basculement](#CCBehaviorOnFailover)  
  
-   [Prise en charge de basculements de sous-réseaux multiples de groupe de disponibilité](#SupportAgMultiSubnetFailover)  
  
-   [Écouteurs de groupe de disponibilité et certificats SSL](#SSLcertificates)  
  
-   [Écouteurs de groupe de disponibilité et noms de principal de serveur (SPN)](#SPNs)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="AGlisteners"></a> Écouteurs de groupe de disponibilité  
 Vous pouvez fournir la connectivité client à la base de données d'un groupe de disponibilité donné en créant un écouteur de groupe de disponibilité. Un écouteur de groupe de disponibilité est un nom de réseau virtuel (VNN) auquel les clients peuvent se connecter afin d’accéder à une base de données dans un réplica principal ou secondaire d’un groupe de disponibilité Always On. Un écouteur de groupe de disponibilité permet à un client de se connecter à un réplica de disponibilité sans connaître le nom de l'instance physique de SQL Server à laquelle il se connecte.  Il est inutile de modifier la chaîne de connexion du client pour se connecter à l'emplacement actuel du réplica principal.  
  
 Un écouteur de groupe de disponibilité consiste en un nom d'écouteur DNS (Domain Name System), en une désignation de port d'écoute et en une ou plusieurs adresses IP. Seul le protocole TCP est pris en charge par l'écouteur de groupe de disponibilité.  Le nom DNS de l'écouteur doit également être unique dans le domaine et dans NetBIOS.  Lorsque vous créez un nouvel écouteur de groupe de disponibilité, il devient une ressource de cluster avec un nom de réseau virtuel (VNN) associé, une adresse IP virtuelle (VIP) et une dépendance de groupe de disponibilité. Un client utilise le nom DNS pour résoudre le nom VNN en plusieurs adresses IP et tente ensuite de se connecter à chaque adresse, jusqu'à ce qu'une demande de connexion réussisse ou jusqu'à ce que la requête de connexion expire.  
  
 Si le routage en lecture seule est configuré pour un ou plusieurs [réplicas secondaires accessibles en lecture](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md), les connexions clientes d’intention de lecture du réplica principal sont redirigées vers un réplica secondaire accessible en lecture. Par ailleurs, si le réplica principal est mis hors connexion sur une instance de SQL Server et un nouveau réplica principal passe en ligne sur une autre instance de SQL Server, l'écouteur de groupe de disponibilité permet aux clients de se connecter au nouveau réplica principal.  
  
 Pour obtenir des informations essentielles sur les écouteurs de groupe de disponibilité, consultez [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Dans cette section :**  
  
-   [Configuration de l'écouteur de groupe de disponibilité](#AGlConfig)  
  
-   [Sélection d'un port d'écoute de groupe de disponibilité](#SelectListenerPort)  
  
###  <a name="AGlConfig"></a> Configuration de l'écouteur de groupe de disponibilité  
 Un écouteur de groupe de disponibilité est défini par les élément suivants :  
  
 Un nom DNS unique.  
 Ce nom est aussi appelé un nom de réseau virtuel (VNN). Les règles d'attribution de noms Active Directory pour les noms d'hôte DNS s'appliquent. Pour plus d'informations, consultez l'article [Conventions d'affectation de noms dans Active Directory pour les ordinateurs, les domaines, les sites et les unités d'organisation](http://support.microsoft.com/kb/909264) de la Base de connaissances.  
  
 Une ou plusieurs adresses IP virtuelles (VIP)  
 Les adresses IP virtuelles sont configurées pour un ou plusieurs sous-réseaux vers lesquels le groupe de disponibilité peut basculer.  
  
 Configuration de l'adresse IP  
 Pour un écouteur de groupe de disponibilité donné, l'adresse IP utilise le protocole DHCP (Dynamic Host Configuration Protocol) ou une ou plusieurs adresses IP statiques.  
  
-   Protocole DHCP (Dynamic Host Configuration Protocol)  
  
     Vous pouvez configurer toutes les adresses IP d'écouteur de groupe de disponibilité en vue d'utiliser le protocole DHCP (Dynamic Host Configuration Protocol) si un groupe de disponibilité réside sur un même sous-réseau. Pour les environnements de préproduction, le protocole DHCP permet une installation facile pour un groupe de disponibilité qui n'a pas besoin de la fonctionnalité de récupération d'urgence vers un site distant sur un sous-réseau distinct. Toutefois, nous déconseillons d'utiliser DHCP en même temps qu'un écouteur de groupe de disponibilité dans un environnement de production. En effet, en cas d'arrêt du système, si le bail IP DHCP expire, il faudra consacrer du temps supplémentaire pour réinscrire la nouvelle adresse IP DHCP associée au nom DNS de l'écouteur. Ce temps supplémentaire va provoquer l'échec de la connexion cliente.  
  
-   Adresses IP statiques  
  
     Dans les environnements de production, nous conseillons que les écouteurs de groupe de disponibilité utilisent des adresses IP statiques. De plus, dans le cas où vos groupes de disponibilité s'étendent sur plusieurs sous-réseaux dans un domaine de plusieurs sous-réseaux, un écouteur de groupe de disponibilité doit utiliser des adresses IP statiques.  
  
 Les configurations réseau hybrides et le recours au protocole DHCP sur plusieurs sous-réseaux ne sont pas pris en charge pour les écouteurs de groupe de disponibilité. Cela est dû au fait que, lorsqu'un basculement a lieu, une adresse IP dynamique peut expirer ou être libérée, ce qui compromet la haute disponibilité globale.  
  
###  <a name="SelectListenerPort"></a> Sélection d'un port d'écoute de groupe de disponibilité  
 Lors de la configuration d'un écouteur de groupe de disponibilité, vous devez indiquer un port.  Vous pouvez configurer le port par défaut sur 1433, afin de permettre de simplifier les chaînes de connexion du client. Si vous utilisez 1433, vous n'avez pas besoin d'indiquer un numéro de port dans une chaîne de connexion.   De plus, étant donné que chaque écouteur de groupe de disponibilité portera un nom de réseau virtuel distinct, chaque écouteur de groupe de disponibilité configuré sur un même WSFC pourra être configuré pour référencer le même port par défaut 1433.  
  
 Vous pouvez également indiquer un port d'écoute non standard ; toutefois, cela signifie que vous devrez également spécifier explicitement un port cible dans votre chaîne de connexion lors de chaque connexion à l'écouteur de groupe de disponibilité.  Vous devrez également ouvrir l'autorisation sur le pare-feu pour le port non standard.  
  
 Si vous utilisez le port par défaut 1433 comme nom VNN de l'écouteur du groupe de disponibilité, vous devez toujours vérifier qu'aucun autre service sur le nœud de cluster n'utilise ce port ; dans le cas contraire, cela provoquerait un conflit de ports.  
  
 Si l'une des instances de SQL Server écoute déjà sur le port TCP 1433 par l'intermédiaire de l'écouteur d'instance et qu'il n'existe aucun autre service (instances supplémentaires de SQL Server comprises) sur l'ordinateur écoutant sur le port 1433, cela ne provoque pas un conflit de ports avec l'écouteur du groupe de disponibilité.  Cela est dû au fait que l'écouteur du groupe de disponibilité peut partager le même port TCP au sein du même processus de service.  Toutefois, plusieurs instances de SQL Server (côte à côte) ne doivent pas être configurées pour écouter sur le même port.  
  
##  <a name="ConnectToPrimary"></a> Utilisation d'un écouteur pour se connecter au réplica principal  
 Pour utiliser un écouteur de groupe de disponibilité pour la connexion au réplica principal en vue de l'accès en lecture-écriture, la chaîne de connexion spécifie le nom DNS de l'écouteur de groupe de disponibilité.  Si un réplica principal de groupe de disponibilité se transforme en nouveau réplica, les connexions existantes qui utilisent le nom réseau d'un écouteur de groupe de disponibilité sont déconnectées.  Les nouvelles connexions à l'écouteur du groupe de disponibilité sont ensuite dirigées vers le nouveau réplica principal. Voici un exemple de chaîne de connexion de base pour le fournisseur ADO.NET (System.Data.SqlClient) :  
  
```  
Server=tcp: AGListener,1433;Database=MyDB;IntegratedSecurity=SSPI  
```  
  
 Vous pouvez néanmoins choisir de référencer directement l'instance du nom SQL Server des réplicas principaux ou secondaires au lieu d'utiliser le nom du serveur de l'écouteur du groupe de disponibilité ; toutefois, si vous choisissez d'agir ainsi, les nouvelles connexions ne seront plus dirigées automatiquement vers le réplica principal actuel.  Vous perdrez également l'avantage du routage en lecture seule.  
  
##  <a name="ConnectToSecondary"></a> Utilisation d'un écouteur pour se connecter à un réplica secondaire en lecture seule (routage en lecture seule)  
 Le*routage en lecture seule* fait référence à la capacité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’acheminer les connexions entrantes à un écouteur de groupe de disponibilité vers un réplica secondaire qui est configuré pour autoriser des charges de travail en lecture seule. Une connexion entrante faisant référence à un nom d'écouteur de groupe de disponibilité peut automatiquement être acheminée vers un réplica en lecture seule si les conditions suivantes sont réunies :  
  
-   Au moins un réplica secondaire est défini sur l'accès en lecture seule, et chaque réplica secondaire en lecture seule et le réplica principal sont configurés pour prendre en charge le routage en lecture seule. Pour plus d’informations, consultez [Pour configurer des réplicas de disponibilité pour le routage en lecture seule](#ConfigureARsForROR), plus loin dans cette section.  

-   La chaîne de connexion référence une base de données impliquée dans le groupe de disponibilité. Une alternative serait que la connexion utilisée ait la base de données configurée comme base de données par défaut. Pour plus d’informations, consultez [cet article sur le fonctionnement de l’algorithme avec le routage en lecture seule](https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/).

-   La chaîne de connexion fait référence à un écouteur de groupe de disponibilité, et l’intention de l’application de la connexion entrante est définie en lecture seule (par exemple, à l’aide du mot clé **Application Intent=ReadOnly** dans les chaînes de connexion ODBC ou OLEDB, ou dans les attributs ou les propriétés de connexion). Pour plus d’informations, consultez [Intention de l’application en lecture seule et routage en lecture seule](#ReadOnlyAppIntent), plus loin dans cette section.  
  
###  <a name="ConfigureARsForROR"></a> Pour configurer des réplicas de disponibilité pour le routage en lecture seule  
 Un administrateur de base de données doit configurer les réplicas de disponibilité comme suit :  
  
1.  Pour chaque réplica de disponibilité à configurer comme réplica secondaire accessible en lecture, un administrateur de base de données doit configurer les paramètres suivants, qui prennent effet uniquement sous le rôle secondaire :  
  
    -   L'accès à la connexion doit avoir la valeur « tous » ou « lecture seule ».  
  
    -   L'URL de routage en lecture seule doit être spécifiée.  
  
2.  Pour chacun de ces réplicas, une liste de routage en lecture seule doit être spécifiée pour le rôle principal. Spécifiez un ou plusieurs noms de serveur comme cibles de routage.  
  
####  <a name="RelatedTasksROR"></a> Tâches associées  
  
-   [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
###  <a name="ReadOnlyAppIntent"></a> Intention de l'application en lecture seule et routage en lecture seule  
 La propriété de chaîne de connexion de l'intention d'application exprime le souhait de l'application cliente d'être redirigée vers une version en lecture-écriture ou en lecture seule d'une base de données de groupe de disponibilité. Pour utiliser le routage en lecture seule, un client doit utiliser une intention d'application en lecture seule dans la chaîne de connexion pour la connexion à l'écouteur de groupe de disponibilité. Sans intention d'application en lecture seule, les connexions à l'écouteur de groupe de disponibilité sont dirigées vers la base de données sur le réplica principal.  
  
 L'attribut d'intention d'application est stocké dans la session du client lors de la connexion ; l'instance de SQL Server traite ensuite cette intention et détermine ce qu'il faut faire, selon la configuration du groupe de disponibilité et l'état en lecture-écriture actuel de la base de données cible dans le réplica secondaire.  
  
 Voici un exemple de chaîne de connexion pour le fournisseur ADO.NET (System.Data.SqlClient) qui indique l'intention d'application en lecture seule :  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly  
```  
  
 Dans cet exemple de chaîne de connexion, le client tente de se connecter à la base de données AdventureWorks par le biais d’un écouteur de groupe de disponibilité nommé `AGListener` sur le port 1433 (vous pouvez aussi omettre le port si l’écouteur du groupe de disponibilité écoute sur le port 1433).  La chaîne de connexion a la propriété **ApplicationIntent** définie sur **ReadOnly**, ce qui en fait une *chaîne de connexion d’intention de lecture*.  Sans ce paramètre, le serveur n'aurait pas essayé un routage en lecture seule de la connexion.  
  
 La base de données primaire du groupe de disponibilité traite la demande de routage en lecture seule entrante et tente de localiser un réplica en ligne et en lecture seule joint au réplica principal et configuré pour le routage en lecture seule.  Le client reçoit les informations de connexion depuis le serveur de réplica principal et se connecte au réplica en lecture seule identifié.  
  
 Notez que l'intention de l'application peut être envoyée depuis un pilote client vers une instance de bas niveau de SQL Server.  Dans ce cas, l'intention de l'application en lecture seule est ignorée et la connexion se déroule normalement.  
  
 Vous pouvez contourner le routage en lecture seule en ne définissant pas la propriété de connexion d’intention d’application avec la valeur **ReadOnly** (en l’absence de spécification, la valeur par défaut est **ReadWrite** pendant la connexion) ou en effectuant une connexion directe à l’instance du réplica principal de SQL Server au lieu d’utiliser le nom de l’écouteur du groupe de disponibilité.  Le routage en lecture seule n'a pas lieu non plus si vous vous connectez directement à un réplica en lecture seule.  
  
####  <a name="RelatedTasksApps"></a> Tâches associées  
  
-   [Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
##  <a name="BypassAGl"></a> Contournement des écouteurs de groupe de disponibilité  
 Alors que les écouteurs de groupe de disponibilité permettent la prise en charge de la redirection de basculement et le routage en lecture seule, les connexions clientes ne sont pas requises pour les utiliser. Une connexion cliente peut également faire directement référence à l'instance de SQL Server au lieu de se connecter à l'écouteur du groupe de disponibilité.  
  
 Pour l'instance de SQL Server, cela ne fait aucune différence si une connexion est établie à l'aide de l'écouteur du groupe de disponibilité ou avec un autre point de terminaison d'instance.  L'instance de SQL Server vérifie l'état de la base de données ciblée, puis autorise ou interdit la connectivité, selon la configuration du groupe de disponibilité et l'état actuel de la base de données sur l'instance.  Par exemple, si une application cliente se connecte directement à une instance de port SQL Server et se connecte à une base de données cible hébergée dans un groupe de disponibilité, et que la base de données cible se trouve dans l'état primaire et en ligne, la connectivité aboutit.  Si la base de données cible est hors connexion ou dans un état de transition, la connectivité à la base de données échoue.  
  
 Autrement, lors d'une migration depuis une mise en miroir de bases de données vers [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], les applications peuvent spécifier la chaîne de connexion de mise en miroir de bases de données dans la mesure où il existe un seul réplica secondaire et qu'il n'autorise pas les connexions utilisateur. Pour plus d’informations, consultez [Utilisation des chaînes de connexion de mise en miroir de bases de données avec des groupes de disponibilité](#DbmConnectionString), plus loin dans cette section.  
  
###  <a name="DbmConnectionString"></a> Utilisation des chaînes de connexion de mise en miroir de bases de données avec des groupes de disponibilité  
 Si un groupe de disponibilité possède uniquement un réplica secondaire et est configuré avec ALLOW_CONNECTIONS = READ_ONLY ou ALLOW_CONNECTIONS = NONE pour le réplica secondaire, les clients peuvent se connecter au réplica principal à l'aide d'une chaîne de connexion de mise en miroir de bases de données. Cette approche peut être utile lors de la migration d'une application existante depuis la mise en miroir de bases de données vers un groupe de disponibilité, tant que vous limitez le groupe de disponibilité à deux réplicas de disponibilité (un réplica principal et un réplica secondaire). Si vous ajoutez des réplicas secondaires supplémentaires, vous devrez créer un écouteur de groupe de disponibilité pour le groupe de disponibilité et mettre à jour vos applications pour utiliser le nom DNS de l'écouteur du groupe de disponibilité.  
  
 Lors de l'utilisation de chaînes de connexion de mise en miroir de bases de données, le client peut soit utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, soit le fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La chaîne de connexion fournie par un client doit fournir au minimum le nom d'une instance de serveur, le *nom du partenaire initial*, pour identifier l'instance de serveur qui héberge initialement le réplica de disponibilité auquel vous envisagez de vous connecter. Éventuellement, la chaîne de connexion peut également fournir le nom d'une autre instance de serveur, le *nom du partenaire de basculement*, pour identifier l'instance de serveur qui héberge initialement le réplica secondaire comme nom de partenaire de basculement.  
  
 Pour plus d’informations sur les chaînes de connexion de mise en miroir de base de données, consultez [Connecter des clients à une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md).  
  
##  <a name="CCBehaviorOnFailover"></a> Comportement des connexions clientes lors du basculement  
 Lorsqu'un basculement de groupe de disponibilité se produit, les connexions persistantes existantes au groupe de disponibilité prennent fin et le client doit établir une nouvelle connexion afin de continuer à utiliser la même base de données primaire ou base de données secondaire en lecture seule.  Lorsqu'un basculement se produit côté serveur, la connectivité au groupe de disponibilité peut échouer, forçant l'application cliente à réessayer une nouvelle connexion jusqu'à ce que le serveur principal soit entièrement remis en ligne.  
  
 Si le groupe de disponibilité revient en ligne pendant une tentative de connexion d'une application cliente, mais avant l'expiration du délai d'attente de connexion, le pilote client peut se connecter avec succès au cours de l'une de ses tentatives de reprise interne et aucune erreur ne sera visible dans l'application dans ce cas.  
  
##  <a name="SupportAgMultiSubnetFailover"></a> Prise en charge de basculements de sous-réseaux multiples de groupe de disponibilité  
 Si vous utilisez des bibliothèques clientes qui prennent en charge l'option de connexion MultiSubnetFailover dans la chaîne de connexion, vous pouvez optimiser le basculement du groupe de disponibilité vers un sous-réseau différent en définissant MultiSubnetFailover sur « True » ou « Yes », selon la syntaxe du fournisseur que vous utilisez.  
  
> [!NOTE]  
>  Nous vous recommandons de définir ce paramètre à la fois pour les connexions à un seul ou à plusieurs sous-réseaux aux écouteurs de groupes de disponibilité et aux noms d'instance de cluster de basculement SQL Server.  L'activation de cette option ajoute des optimisations supplémentaires, même pour les scénarios de sous-réseau unique.  
  
 L’option de connexion **MultiSubnetFailover** fonctionne uniquement avec le protocole réseau TCP et elle est prise en charge uniquement lors de la connexion à un écouteur de groupe de disponibilité et pour n’importe quel nom de réseau virtuel se connectant à [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
 Voici un exemple de chaîne de connexion du fournisseur ADO.NET (System.Data.SqlClient) qui permet le basculement de plusieurs sous-réseaux :  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;IntegratedSecurity=SSPI; MultiSubnetFailover=True  
```  
  
 L’option de connexion **MultiSubnetFailover** doit prendre la valeur **True** même si le groupe de disponibilité s’étend sur un seul sous-réseau.  Cela vous permet de préconfigurer de nouveaux clients afin de prendre en charge la future couverture de sous-réseaux, sans recourir à de futures modifications de la chaîne de connexion du client. Par ailleurs, cela permet d'optimiser les performances de basculement pour les basculements au sein d'un seul sous-réseau.  Alors que l’option de connexion **MultiSubnetFailover** n’est pas obligatoire, elle offre l’avantage d’un basculement de sous-réseau plus rapide.  Cela est dû au fait que le pilote client essaie d'ouvrir un socket TCP pour chaque adresse IP en parallèle associée au groupe de disponibilité.  Le pilote client attend une réponse correcte de la première adresse IP et, ceci fait, l'utilise pour la connexion.  
  
##  <a name="SSLcertificates"></a> Écouteurs de groupe de disponibilité et certificats SSL  
 Lors de la connexion à un écouteur de groupe de disponibilité, si les instances participantes de SQL Server utilisent des certificats SSL en même temps que le chiffrement de session, le pilote client de la connexion doit prendre en charge le nom SAN (Subject Alternate Name) dans le certificat SSL afin de forcer le chiffrement.  La prise en charge de pilotes SQL Server pour le nom SAN de certificat est planifiée pour ADO.NET (SqlClient), Microsoft JDBC et SQL Native Client (SNAC).  
  
 Un certificat X.509 doit être configuré pour chaque nœud serveur participant dans le cluster de basculement, avec une liste de tous les écouteurs de groupe de disponibilité définis dans le nom SAN du certificat.  
  
 Par exemple, si le WSFC dispose de trois écouteurs de groupe de disponibilité avec les noms `AG1_listener.Adventure-Works.com`, `AG2_listener.Adventure-Works.com`et `AG3_listener.Adventure-Works.com`, le nom SAN du certificat doit être défini comme suit :  
  
```  
CN = ServerFQDN  
SAN = ServerFQDN,AG1_listener.Adventure-Works.com, AG2_listener.Adventure-Works.com, AG3_listener.Adventure-Works.com  
```  
  
##  <a name="SPNs"></a> Écouteurs de groupe de disponibilité et noms de principal de serveur (SPN)  
 Un nom de principal de serveur (SPN) doit être configuré dans Active Directory par un administrateur de domaine pour chaque nom d'écouteur de groupe de disponibilité afin d'activer Kerberos pour la connexion cliente à l'écouteur du groupe de disponibilité. Lors de l’inscription du nom SPN, vous devez utiliser le compte de service de l’instance de serveur qui héberge le réplica de disponibilité.  Pour que le SPN fonctionne sur tous les réplicas, le même compte de service doit être utilisé pour toutes les instances du cluster WSFC qui héberge le groupe de disponibilité.  
  
 Utilisez l’outil de ligne de commande **setspn** Windows pour configurer le SPN.  Par exemple, pour configurer un SPN pour un groupe de disponibilité nommé `AG1listener.Adventure-Works.com` hébergé dans un ensemble d'instances de SQL Server, toutes configurées pour s'exécuter sous le compte de domaine `corp/svclogin2`:  
  
```  
setspn -A MSSQLSvc/AG1listener.Adventure-Works.com:1433 corp/svclogin2  
```  
  
 Pour plus d'informations sur l'inscription manuelle d'un SPN pour SQL Server, consultez [Register a Service Principal Name for Kerberos Connections](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Afficher les propriétés d’écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
-   [Supprimer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](http://go.microsoft.com/fwlink/?LinkId=227600)  
  
-   [Présentation de l’écouteur du groupe de disponibilité](https://blogs.msdn.microsoft.com/sqlalwayson/2012/01/16/introduction-to-the-availability-group-listener/) (blog de l’équipe SQL Server Always On)  
  
-   [Blog de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](https://blogs.msdn.microsoft.com/sqlalwayson/)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)   
 [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Secondaires actifs : réplicas secondaires lisibles &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Connecter des clients à une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)  
  
  
