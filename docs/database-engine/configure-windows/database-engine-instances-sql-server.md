---
title: Instances du moteur de base de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: af9ae643-9866-4014-b36f-11ab556a773e
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 34db3b2fe4d33fd7680fe22ecca31f5885742916
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-engine-instances-sql-server"></a>Instances du moteur de base de données (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est une copie de l’exécutable **sqlservr.exe** qui s’exécute en tant que service du système d’exploitation. Chaque instance gère plusieurs bases de données système et une ou plusieurs bases de données utilisateur. Chaque ordinateur peut exécuter plusieurs instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Les applications se connectent à l'instance afin d'effectuer des travaux dans une base de données gérée par l'instance.  
  
## <a name="instances"></a>Instances  
 Une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] s'exécute en tant que service, celui-ci gérant toutes les demandes des applications concernant l'utilisation des données dans les bases de données gérées par cette instance. Il s'agit de la cible des demandes de connexion émanant des applications. La connexion passe par une connexion réseau si l'application et l'instance se trouvent sur des ordinateurs différents. Si l'application et l'instance se trouvent sur le même ordinateur, la connexion SQL Server peut s'exécuter en tant que connexion réseau ou en tant que connexion en mémoire. Une fois qu'une connexion est établie, une application envoie des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sur la connexion à l'instance. L'instance résout les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en opérations sur les données et les objets dans les bases de données. Si les autorisations requises ont été accordées aux informations d'identification de la connexion, l'instance effectue le travail. Toutes les données récupérées sont retournées à l'application, de même que les éventuels messages (notamment les erreurs).  
  
 Vous pouvez exécuter plusieurs instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur un même ordinateur. Une instance peut être l'instance par défaut. L'instance par défaut n'a pas de nom. Si une demande de connexion spécifie uniquement le nom de l'ordinateur, la connexion est établie à l'instance par défaut. Une instance nommée se caractérise par la spécification d'un nom d'instance au moment de son installation. Une demande de connexion doit spécifier le nom de l'ordinateur et le nom de l'instance pour pouvoir se connecter à l'instance. L'installation d'une instance par défaut n'est soumise à aucune condition particulière ; toutes les instances s'exécutant sur un ordinateur peuvent être des instances nommées.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Explique comment configurer les propriétés d'une instance. Explique comment configurer les valeurs par défaut telles que l'emplacement des fichiers et les formats de date, ou comment l'instance utilise les ressources du système d'exploitation, telles que la mémoire ou les threads.|[Configurer des instances du moteur de base de données &#40;SQL Server&#41;](../../database-engine/configure-windows/configure-database-engine-instances-sql-server.md)|  
|Explique comment gérer le classement pour une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Les classements définissent les séries de bits utilisées pour représenter les caractères, les comportements associés tels que le tri, ainsi que le respect de la case ou des accents dans les opérations de comparaison.|[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)|  
|Explique comment configurer des définitions de serveur lié qui permettent aux instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées dans une instance d'utiliser des données stockées dans des sources de données OLE DB distinctes.|[Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)|  
|Explique comment créer un déclencheur LOGON qui spécifie les actions à entreprendre après la validation d'une tentative de connexion et avant l'utilisation des ressources dans l'instance. Les déclencheurs LOGON prennent en charge diverses actions telles que la journalisation des activités de connexion ou la restriction de connexions selon une logique donnée, et ce en plus de l'authentification des informations d'identification effectuée par Windows et SQL Server.|[Déclencheurs de connexion](../../relational-databases/triggers/logon-triggers.md)|  
|Explique comment gérer le service associé à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Sont traitées des actions telles que le démarrage et l'arrêt du service, ou la configuration des options de démarrage.|[Gérer les services du moteur de base de données](../../database-engine/configure-windows/manage-the-database-engine-services.md)|  
|Indique comment effectuer des tâches de configuration réseau du serveur, telles que l'activation des protocoles, la modification du port ou du canal utilisé par un protocole, la configuration du chiffrement, la configuration du service SQL Server Browser, l'affichage ou le masquage du moteur de base de données SQL Server sur le réseau et l'inscription du nom SPN.|[Configuration réseau du serveur](../../database-engine/configure-windows/server-network-configuration.md)|  
|Explique comment effectuer des tâches de configuration réseau client, telles que la configuration des protocoles clients et la création ou la suppression d'un alias de serveur.|[Configuration du réseau client](../../database-engine/configure-windows/client-network-configuration.md)|  
|Décrit les éditeurs de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] que vous pouvez utiliser pour concevoir, déboguer et exécuter des scripts tels que des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] . Explique également comment coder des scripts Windows PowerShell de manière à ce qu'ils fonctionnent avec les composants SQL Server.|[Scripts du moteur de base de données](../../relational-databases/scripting/database-engine-scripting.md)|  
|Explique comment utiliser des plans de maintenance pour spécifier un flux de travail concernant les tâches d'administration communes pour une instance. Les flux de travail incluent des tâches telles que la sauvegarde des bases de données et la mise à jour des statistiques pour améliorer les performances.|[Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)|  
|Explique comment utiliser Resource Governor pour gérer la consommation des ressources et les charges de travail en limitant le temps processeur et la mémoire à disposition des demandes d'applications.|[Resource Governor](../../relational-databases/resource-governor/resource-governor.md)|  
|Explique comment les applications de base de données peuvent utiliser la messagerie de base de données pour envoyer des messages électroniques à partir du [!INCLUDE[ssDE](../../includes/ssde-md.md)].|[Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)|  
|Explique comment utiliser les événements étendus pour capturer des données de performances afin de générer des lignes de base de performances ou diagnostiquer des problèmes de performances. Les événements étendus sont un système léger très évolutif permettant de collecter des données de performances.|[Événements étendus](../../relational-databases/extended-events/extended-events.md)|  
|Explique comment utiliser Trace SQL pour générer un système personnalisé pour capturer et enregistrer les événements dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)].|[Trace SQL](../../relational-databases/sql-trace/sql-trace.md)|  
|Explique comment utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler pour capturer les traces de demandes d'applications arrivant à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Ces traces peuvent être relues pour des activités telles que le test des performances ou le diagnostic de problèmes.|[SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)|  
|Décrit les fonctionnalités de capture de données modifiées (CDC) et de suivi des modifications, ainsi que la manière d'utiliser ces fonctionnalités pour effectuer le suivi des modifications apportées aux données d'une base de données.|[Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)|  
|Explique comment utiliser la visionneuse du fichier journal pour rechercher et afficher les erreurs et les messages de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans différents journaux, tels que l'historique des travaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les journaux SQL Server et les journaux d'événements Windows.|[Visionneuse du fichier journal](../../relational-databases/logs/log-file-viewer.md)|  
|Explique comment utiliser l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour analyser les bases de données et faire des recommandations en vue de résoudre les problèmes potentiels liés aux performances.|[Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/database-engine-tuning-advisor.md)|  
|Explique comment les administrateurs de base de données de production peuvent établir une connexion de diagnostic aux instances lorsque les connexions standard ne sont pas acceptées.|[Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)|  
|Explique comment utiliser la fonctionnalité des serveurs distants déconseillés pour autoriser l'accès d'une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à une autre. Le mécanisme recommandé pour cette fonctionnalité est un serveur lié.|[Serveurs distants](../../database-engine/configure-windows/remote-servers.md)|  
|Décrit les fonctionnalités de Service Broker pour les applications de messagerie et de file d'attente et fournit des pointeurs vers la documentation de Service Broker.|[Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)|  
|Décrit comment utiliser l'extension du pool de mémoires tampons pour permettre l'intégration transparente du stockage d'accès aléatoire non volatile (disques SSD) au pool de mémoires tampons du moteur de base de données de façon à améliorer considérablement le débit d'E/S.|[Fichier d'extension du pool de mémoires tampons](../../database-engine/configure-windows/buffer-pool-extension.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Application sqlservr](../../tools/sqlservr-application.md)   
 [Fonctionnalités de base de données](../../relational-databases/database-features.md)   
 [Fonctionnalités entre les instances du moteur de base de données](../../relational-databases/database-engine-cross-instance-features.md)  
  
  
