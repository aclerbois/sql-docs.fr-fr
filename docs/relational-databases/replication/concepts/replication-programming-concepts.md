---
title: Concepts de programmation en matière de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- replication [SQL Server], planning
- programming [SQL Server replication], planning
- programming [SQL Server replication]
ms.assetid: 2cd846e7-5bf3-4144-8772-703c4f439a2a
caps.latest.revision: 43
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 03bedc038bd35a85cd12d08c695491d9bba8d9c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-programming-concepts"></a>Concepts de programmation en matière de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Avant de développer une application qui utilise des fonctionnalités de réplication, vous devez effectuer les opérations de planification générales indiquées ci-dessous :  
  
1.  Définissez votre topologie de réplication.  
  
2.  Définissez les fonctionnalités de l'application.  
  
3.  Planifiez la sécurité.  
  
4.  Choisissez un environnement de développement.  
  
5.  Choisissez l'interface de programmation de réplication appropriée.  
  
 Le reste de cette rubrique décrit ces opérations de manière plus détaillée. Un exemple a été inclus afin d'illustrer le processus de planification.  
  
## <a name="defining-the-replication-topology"></a>Définition de la topologie de réplication  
 Pour la programmation de la réplication, la première étape consiste à définir la topologie de réplication de votre application. Si vous écrivez une application qui utilisera une topologie de réplication existante, telle qu'une application cliente qui accède aux données d'un abonné existant, vous devez passer à l'étape suivante.  
  
> [!NOTE]  
>  Dans certains cas, l'application aura uniquement pour but de déployer la topologie de réplication.  
  
 La topologie de réplication que vous définissez dépend de nombreux facteurs, entre autres :  
  
-   la nécessité ou non de mettre à jour les données répliquées, et la personne chargée de leur mise à jour ;  
  
-   vos impératifs de distribution de données en matière de cohérence, d'autonomie et de latence ;  
  
-   l'environnement de réplication, y compris les utilisateurs professionnels, l'infrastructure technique, le réseau et la sécurité, ainsi que les caractéristiques des données ;  
  
-   les types de réplication et les options de réplication ;  
  
-   les topologies de réplication et leur adéquation aux types de réplication.  
  
 Si vous débutez avec la réplication [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Types de réplication](../../../relational-databases/replication/types-of-replication.md).  
  
## <a name="defining-application-functionality"></a>Définition des fonctionnalités de l'application  
 Une fois que la topologie de réplication a été définie, vous devez choisir les fonctionnalités que votre application offrira, qu'il s'agisse d'un simple script qui synchronise un abonnement ou d'une application comportant une interface utilisateur pour configurer la réplication. La réplication prend en charge les tâches de programmation générales indiquées ci-dessous :  
  
-   Configuration de la réplication  
  
-   Synchronisation des abonnés  
  
-   Gestion d'une topologie de réplication  
  
-   Analyse d’une topologie de réplication.  
  
-   Dépannage de la réplication.  
  
 Vous pouvez également étendre votre application en associant les fonctionnalités de réplication à d'autres fonctionnalités fournies par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le tableau suivant décrit certaines fonctionnalités étendues que vous pouvez fournir dans votre application de réplication.  
  
|Fonctionnalité| Exemple|  
|-------------------|-------------|  
|Administration de serveur à l'aide de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Objects (SMO)|Application qui permet à un administrateur de joindre et de configurer une base de données en tant que serveur de publication dans une topologie de réplication.|  
|Accès aux données à l'aide d'ADO.NET|Application qui permet aux utilisateurs d'accéder par programme aux données de ventes répliquées et de les modifier dans une base de données d'abonné locale en mode hors connexion, puis de se connecter et de synchroniser l'abonnement par extraction de données (pull) en cliquant sur un bouton.|  
  
## <a name="planning-for-security"></a>Planification de la sécurité  
 La sécurité est un impératif pour une application. Il est donc essentiel de prévoir les besoins en matière de sécurité avant l'écriture du code. La sécurité d'une application se décompose en trois parties principales : sécurisation de la base de données, sécurisation de la réplication, et écriture de code sécurisé.  
  
 Les rubriques suivantes contiennent des informations sur la sécurité :  
  
-   [Sécurité et protection &#40;réplication&#41;](../../../relational-databases/replication/security/security-and-protection-replication.md)  
  
-   [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
## <a name="choosing-a-development-environment"></a>Choix d'un environnement de développement  
 Lorsque vous développez une application de réplication, trois environnements de développement de base doivent être pris en compte. Chaque environnement de développement a accès aux mêmes fonctionnalités de réplication, à quelques exceptions près. Il est possible de développer des applications de réplication dans chacun des environnements suivants.  
  
-   **Code managé**  
  
     Il s'agit d'un environnement de développement orienté objet qui tire parti des avantages de la programmation du [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] et du Common Language Runtime (CLR) .NET. Le code managé est l'environnement de programmation recommandé pour le développement .NET et les applications [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les interfaces de réplication managées permettent de programmer l'administration de la réplication en mode orienté objet sans pour autant connaître le langage [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Elles fournissent également certaines fonctionnalités de rappel lors de l'exécution d'agents de réplication qui ne sont pas disponibles à partir de scripts. Le code managé constitue l'environnement idéal pour le développement de composants réutilisables et d'applications d'interface utilisateur.  
  
-   **Création de scripts**  
  
     Il s'agit d'applications simples qui exécutent une série de commandes sous forme de procédures stockées de réplication système dans des scripts [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou sous forme de commandes dans des fichiers de commandes. Il est possible d'exécuter des scripts dans un environnement managé à l'aide du fournisseur managé intra-processus [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Vous pouvez cependant obtenir les mêmes fonctionnalités en utilisant les interfaces de réplication managées, lesquelles fournissent également des fonctionnalités de rappel. La création de scripts constitue l'environnement idéal pour l'exécution de tâches peu fréquemment utilisées et lorsque les fonctionnalités de rappel ne sont pas requises (par exemple, pour l'installation d'un serveur de réplication).  
  
-   **Code natif**  
  
     Il s'agit d'un environnement de développement orienté objet qui utilise l'accès direct aux objets système ou COM de sorte que ce code ne soit pas géré par le CLR. Les interfaces de réplication en environnement de code natif sont déconseillées. Pour plus d’informations, consultez [Fonctionnalités déconseillées dans la réplication SQL Server](../../../relational-databases/replication/deprecated-features-in-sql-server-replication.md) ou [Compatibilité descendante de la réplication](../../../relational-databases/replication/replication-backward-compatibility.md).  
  
## <a name="choose-the-appropriate-replication-programming-interface"></a>Choix de l'interface de programmation de réplication appropriée  
 La dernière étape de la planification consiste à choisir l'interface de programmation de réplication appropriée qui implémente les fonctionnalités de réplication souhaitées pour l'environnement de développement choisi. Le tableau suivant répertorie les interfaces de programmation de réplication disponibles.  
  
|Interface|Environnement|Utilisations|  
|---------------|-----------------|----------|  
|[Concepts liés à Replication Management Objects](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)|Code managé|Administration, analyse et synchronisation.|  
|<xref:Microsoft.SqlServer.Replication>|Code managé|Synchronisation.|  
|<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|Code managé|Création de gestionnaires de logique métier pour intégrer la logique personnalisée au processus de synchronisation de fusion.|  
|[Procédures stockées de réplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)|Création de scripts|Administration et analyse.|  
|[Concepts des exécutables de l'agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)|Création de scripts|Synchronisation.|  
  
## <a name="example"></a> Exemple  
 Dans le contexte d'[!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)], les données doivent être publiées pour 200 représentants dans le monde. Les représentants voyagent fréquemment ; ils devront donc utiliser des ordinateurs portables ou des assistants numériques personnels (PDA) pour modifier les données client et ajouter de nouvelles commandes. Les modifications devront ensuite être synchronisées avec le serveur de publication lorsque le représentant connectera l'ordinateur portable au réseau.  
  
 Pour cette application, les étapes de la planification peuvent être les suivantes :  
  
1.  La topologie de réplication existe déjà pour cette application. Toutefois, un nouvel abonnement par extraction doit être créé sur le client. La publication doit utiliser des filtres paramétrables pour répliquer un jeu de données unique sur chaque représentant.  
  
2.  Cette application doit non seulement garantir l'accès aux données classique requis pour une application de vente, mais également permettre à un vendeur de synchroniser l'abonnement par extraction de données à la demande en cliquant sur un bouton. Étant donné qu'un représentant installera et exécutera l'application, l'application doit également être en mesure de configurer un abonnement et d'appliquer l'instantané initial au client. L'application utilisera éventuellement l'infrastructure fournie par Windows pour détecter la connectivité sans fil et synchroniser automatiquement l'abonnement lors de la détection d'une connexion.  
  
3.  Respectez toutes les consignes de sécurité pour la réplication, notamment le recours à l'authentification Windows et à un réseau privé virtuel (VPN) lors de la connexion au serveur de publication. Si vous mettez en œuvre la synchronisation Web, utilisez une connexion SSL (Secure Sockets Layer). Pour plus d’informations, consultez [Configurer la synchronisation Web](../../../relational-databases/replication/configure-web-synchronization.md).  
  
4.  Pour que l'application puisse exploiter les fonctionnalités du [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], elle est développée à l'aide d'un langage de code managé.  
  
5.  Conformément à ces spécifications, l'interface managée Replication Management Objects sera en mesure d'assurer toutes les fonctionnalités de réplication nécessaires pour cette application.  
  
 Cet exemple de scénario a été mis en œuvre dans l’exemple d’application AdventureWorks qui peut être téléchargé pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
  
