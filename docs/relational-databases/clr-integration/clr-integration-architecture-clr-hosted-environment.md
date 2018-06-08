---
title: Environnement de CLR hébergé | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- type-safe code [CLR integration]
- UNSAFE permission set
- run-time environments [CLR integration]
- common language runtime [SQL Server], about CLR integration
- application domains [CLR integration]
- host protection attributes [CLR integration]
- managed code [SQL Server], common language runtime
- permission sets [CLR integration]
- reliability [CLR integration]
- SAFE permission set
- code access security [CLR integration]
- EXTERNAL_ACCESS permission set
- verifying type safety
- scalability [CLR integration]
- hosted environments [CLR integration]
- HPAs [CLR integration]
ms.assetid: d280d359-08f0-47b5-a07e-67dd2a58ad73
caps.latest.revision: 60
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 603b0d66a4a8b7f406708442f18e9c98c4414b26
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-integration-architecture---clr-hosted-environment"></a>Architecture d’intégration CLR - environnement hébergé CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'intégration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le Common Language Runtime (CLR) .NET Framework permet aux programmeurs de base de données d'utiliser des langages tels que Visual C#, Visual Basic .NET et Visual C++. Les fonctions, procédures stockées, déclencheurs, types de données et agrégats sont parmi les types de logique métier que les programmeurs peuvent écrire avec ces langages.  
  
  Le CLR propose une mémoire récupérée par le garbage collector, un threading préemptif, des services de métadonnées (réflexion de type), la vérifiabilité du code et la sécurité d'accès du code. Le CLR utilise les métadonnées pour rechercher et charger des classes, placer des instances en mémoire, résoudre des appels de méthode, générer un code natif, appliquer la sécurité et définir les limites du contexte d'exécution.  
  
 Le CLR et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diffèrent comme environnements d'exécution dans leur façon de gérer la mémoire, les threads et la synchronisation. Cette rubrique décrit la manière dont ces deux runtimes sont intégrés afin que toutes les ressources système soient gérées uniformément. Cette rubrique couvre également la manière dont la sécurité d'accès du code CLR et la sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont intégrées pour fournir un environnement d'exécution fiable et sécurisé pour le code utilisateur.  
  
## <a name="basic-concepts-of-clr-architecture"></a>Concepts de base de l'architecture du CLR  
 Dans le .NET Framework, un programmeur écrit dans un langage de haut niveau qui implémente une classe définissant sa structure (par exemple, les champs ou les propriétés de la classe) et ses méthodes. Certaines de ces méthodes peuvent être des fonctions statiques. La compilation du programme produit un fichier appelé assembly qui contient le code compilé en langage MSIL ([!INCLUDE[msCoName](../../includes/msconame-md.md)] intermediate language), ainsi qu'un manifeste qui contient toutes les références aux assemblys dépendants.  
  
> [!NOTE]  
>  Les assemblys sont un élément essentiel dans l'architecture du CLR. Ils représentent les unités d'empaquetage, de déploiement et de contrôle de version du code d'application dans le .NET Framework. Grâce aux assemblys, vous pouvez déployer le code d'application au sein de la base de données et fournir une méthode uniforme pour administrer, sauvegarder et restaurer des applications de base de données complètes.  
  
 Le manifeste d'assembly contient les métadonnées sur l'assembly, décrivant toutes les structures, les champs, les propriétés, les classes, les relations d'héritage, les fonctions et les méthodes définis dans le programme. Le manifeste établit l'identité de l'assembly, spécifie les fichiers constituant l'implémentation de l'assembly, spécifie les types et les ressources de l'assembly, détaille les dépendances lors de la compilation par rapport aux autres assemblys et précise l'ensemble des autorisations requises pour que l'assembly fonctionne correctement. Ces informations sont utilisées au moment de l'exécution pour résoudre les références, appliquer la stratégie de liaison des versions et valider l'intégrité des assemblys chargés.  
  
 Le .NET Framework prend en charge les attributs personnalisés pour annoter les classes, les propriétés, les fonctions et les méthodes avec des informations supplémentaires que l'application peut capturer dans les métadonnées. Tous les compilateurs .NET Framework consomment ces annotations sans interprétation et les stockent comme des métadonnées d'assembly. Ces annotations peuvent être examinées de la même façon que d'autres métadonnées quelconques.  
  
 Le code managé correspond à du langage MSIL exécuté dans le CLR, plutôt que directement par le système d'exploitation. Les applications de code managé acquièrent les services CLR, tels que le garbage collection automatique, la vérification des types au moment de l'exécution et la prise en charge de la sécurité. Grâce à ces services, les applications de code managé ont un comportement uniforme, indépendant de la plateforme et du langage.  
  
## <a name="design-goals-of-clr-integration"></a>Objectifs de conception de l'intégration du CLR  
 Lorsque le code utilisateur s'exécute au sein de l'environnement hébergé CLR dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (appelé intégration du CLR), les objectifs de conception suivants s'appliquent :  
  
###### <a name="reliability-safety"></a>Fiabilité (sécurité)  
 Le code utilisateur ne doit pas être autorisé à effectuer des opérations qui compromettent l'intégrité du processus du moteur de base de données, telles que l'affichage d'une boîte de message demandant une réponse de l'utilisateur ou la sortie du processus. Le code utilisateur ne doit pas être en mesure de remplacer les mémoires tampons ou les structures de données internes du moteur de base de données.  
  
###### <a name="scalability"></a>Extensibilité  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR ont des modèles internes différents de planification et de gestion de la mémoire. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un modèle de thread coopératif et non préemptif, dans lequel les threads suspendent volontairement leur exécution périodiquement, ou lorsqu'ils attendent des verrous ou des E/S. Le CLR prend en charge un modèle de thread préemptif. Si le code utilisateur qui s'exécute au sein de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut appeler directement les primitives de thread du système d'exploitation, il ne s'intègre pas correctement dans le planificateur de tâches [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peut dégrader l'évolutivité du système. Le CLR ne distingue pas la mémoire virtuelle de la mémoire physique, mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère directement la mémoire physique et doit utiliser la mémoire physique dans une limite configurable.  
  
 Les modèles différents de threading, de planification et de gestion de la mémoire présentent une difficulté d'intégration pour un système de gestion de base de données relationnelle (SGBDR) qui évolue pour prendre en charge des milliers de sessions utilisateur simultanées. L'architecture doit garantir que l'évolutivité du système n'est pas compromise lorsque le code d'utilisateur appelle des interfaces de programmation d'applications (API) directement pour des primitives de thread, de mémoire et de synchronisation.  
  
###### <a name="security"></a>Sécurité  
 Le code utilisateur qui s'exécute dans la base de données doit suivre les règles d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d'autorisation lors de l'accès à des objets de base de données tels que les tables et les colonnes. De plus, les administrateurs de base de données doivent être en mesure de contrôler l'accès aux ressources du système d'exploitation, tel que l'accès aux fichiers et au réseau, à partir du code utilisateur qui s'exécute dans la base de données. Cela devient important alors que les langages de programmation managés (contrairement aux langages non managés tels que Transact-SQL) fournissent des API pour accéder à ces ressources. Le système doit fournir une méthode sécurisée pour que le code utilisateur accède aux ressources d'ordinateur à l'extérieur du processus du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour plus d’informations, consultez [Sécurité de l’intégration du CLR](../../relational-databases/clr-integration/security/clr-integration-security.md).  
  
###### <a name="performance"></a>Performance  
 Le code utilisateur managé qui s'exécute dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit avoir des performances de calcul comparables au même code exécuté hors du serveur. L'accès à la base de données à partir du code utilisateur managé n'est pas aussi rapide que pour [!INCLUDE[tsql](../../includes/tsql-md.md)] natif. Pour plus d’informations, consultez [performances d’intégration du CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
  
## <a name="clr-services"></a>Services CLR  
 Le CLR fournit plusieurs services pour atteindre les objectifs de conception de l'intégration du CLR avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###### <a name="type-safety-verification"></a>Vérification de la cohérence des types  
 Un code de type sécurisé est un code qui accède aux structures de mémoire uniquement de façons bien définies. Prenons par exemple une référence d'objet valide, le code de type sécurisé peut accéder à la mémoire à des offsets fixes correspondant aux membres de champ réels. Cependant, si le code accède à la mémoire à des offsets arbitraires dans ou hors de la plage de mémoire qui appartient à l'objet, il n'est pas de type sécurisé. Lorsque des assemblys sont chargés dans le CLR, avant que le langage MSIL soit compilé à l'aide de la compilation juste-à-temps (JIT), le runtime effectue une phase de vérification qui examine le code pour déterminer s'il est de type sécurisé. Le code qui réussit cette vérification est appelé code de type sécurisé vérifié.  
  
###### <a name="application-domains"></a>Domaines d’application  
 Le CLR prend en charge la notion de domaines d'application comme des zones d'exécution au sein d'un processus hôte dans lesquelles des assemblys de code managé peuvent être chargés et exécutés. La limite du domaine d'application assure l'isolement entre les assemblys. Les assemblys sont isolés quant à la visibilité des variables statiques et des membres de données et à la capacité d'appeler dynamiquement le code. Les domaines d'application correspondent également au mécanisme de chargement et de déchargement du code. Le code peut être déchargé à partir de la mémoire seulement en déchargeant le domaine d'application. Pour plus d’informations, consultez [domaines d’Application et de sécurité de l’intégration CLR](http://msdn.microsoft.com/library/54ee904e-e21a-4ee7-b4ad-a6f6f71bd473).  
  
###### <a name="code-access-security-cas"></a>Sécurité d'accès du code  
 Le système de sécurité du CLR offre un moyen pour à contrôler quels types d'opérations le code managé peut effectuer en assignant des autorisations au code. Les autorisations d'accès au code sont assignées en fonction de l'identité du code (par exemple, la signature de l'assembly ou l'origine du code).  
  
 Le CLR prévoit une stratégie de l'ordinateur qui peut être définie par l'administrateur de l'ordinateur. Cette stratégie définit les attributions d'autorisations pour tout code managé qui s'exécute sur l'ordinateur. De plus, il existe une stratégie de sécurité au niveau de l'hôte qui peut être utilisée par les hôtes tels que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour spécifier des restrictions supplémentaires sur le code managé.  
  
 Si une API managée dans le .NET Framework expose des opérations sur les ressources protégées par une autorisation d'accès au code, l'API demandera cette autorisation avant d'accéder à la ressource. Cette demande conduit le système de sécurité du CLR à déclencher une vérification complète de chaque unité de code (assembly) dans la pile d'appels. L'accès à la ressource est accordé seulement si la chaîne d'appel entière a l'autorisation.  
  
 Notez que la capacité à générer dynamiquement du code managé, à l'aide de l'API Reflection.Emit, n'est pas prise en charge au sein de l'environnement hébergé CLR dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un tel code n'aurait pas les autorisations de sécurité d'accès du code pour s'exécuter et échouerait par conséquent au moment de l'exécution. Pour plus d’informations, consultez [sécurité d’accès du Code CLR Integration](../../relational-databases/clr-integration/security/clr-integration-code-access-security.md).  
  
###### <a name="host-protection-attributes-hpas"></a>Attributs de protection de l'hôte (HPA)  
 Le CLR fournit un mécanisme pour annoter les API managées qui font partie du .NET Framework avec certains attributs qui peuvent avoir un intérêt pour un hôte du CLR. Voici des exemples de tels attributs :  
  
-   SharedState, qui indique si l'API expose la capacité à créer ou gérer l'état partagé (par exemple, champs de classe statique).  
  
-   Synchronization, qui indique si l'API expose la capacité à réaliser la synchronisation entre des threads.  
  
-   ExternalProcessMgmt, qui indique si l'API expose une méthode pour contrôler le processus hôte.  
  
 Étant donné ces attributs, l'hôte peut spécifier une liste de HPA, tels que l'attribut SharedState, qui doivent être rejetés dans l'environnement hébergé. Dans ce cas, le CLR refuse les tentatives d'appel par code utilisateur des API annotées par les HPA dans la liste interdite. Pour plus d’informations, consultez [attributs de Protection d’hôte et de la programmation de l’intégration CLR](../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md).  
  
## <a name="how-sql-server-and-the-clr-work-together"></a>Comment SQL Server et le CLR fonctionnent-ils ensemble ?  
 Cette section explique comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre le threading, la planification, la synchronisation et les modèles de gestion de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et du CLR. En particulier, cette section examine l'intégration du point de vue de l'évolutivité, de la fiabilité et des objectifs en matière de sécurité. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agit essentiellement comme un système d'exploitation pour le CLR lorsque celui-ci est hébergé au sein de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le CLR appelle les routines de bas niveau implémentées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le threading, la planification, la synchronisation et la gestion de la mémoire. Ce sont les mêmes primitives que le reste du moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise. Cette approche fournit plusieurs avantages en termes d'évolutivité, de fiabilité et de sécurité.  
  
###### <a name="scalability-common-threading-scheduling-and-synchronization"></a>Évolutivité : threading, planification et synchronisation courants  
 Le CLR appelle des API [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour créer des threads, à la fois pour exécuter le code utilisateur et pour sa propre utilisation interne. Afin de synchroniser plusieurs threads, le CLR appelle des objets de synchronisation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela permet au planificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de planifier d'autres tâches lorsqu'un thread attend un objet de synchronisation. Par exemple, lorsque le CLR lance le garbage collection, tous ses threads attendent que le garbage collection se termine. Comme les threads du CLR et les objets de synchronisation qu'ils attendent sont connus du planificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut planifier des threads qui exécutent d'autres tâches de base de données n'impliquant pas le CLR. Cela permet également à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de détecter les blocages qui impliquent des verrous pris par les objets de synchronisation du CLR et d'employer des techniques traditionnelles de suppression des blocages.  
  
 Le code managé s'exécute de façon préemptive dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le planificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la capacité de détecter et d'arrêter les threads qui n'ont pas été soumis pendant une période significative. La capacité à accrocher des threads du CLR aux threads [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implique que le planificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut identifier des threads « fugitifs » dans le CLR et gérer leur priorité. De tels threads fugitifs sont suspendus et placés en arrière dans la file d'attente. Les threads identifiés à plusieurs reprises comme des threads fugitifs ne sont pas autorisés à s'exécuter pendant un intervalle de temps donné afin que les autres travaux en cours puissent s'exécuter.  
  
> [!NOTE]  
>  Le code managé de longue durée qui accède aux données ou alloue suffisamment de mémoire pour déclencher le garbage collection sera soumis automatiquement. Un code managé de longue durée qui n'accède pas aux données ou n'alloue pas suffisamment de mémoire managée pour déclencher le garbage collection doit être soumis explicitement en appelant la fonction System.Thread.Sleep() du .NET Framework.  
  
###### <a name="scalability-common-memory-management"></a>Évolutivité : gestion de la mémoire courante  
 Le CLR appelle des primitives [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour allouer et libérer sa mémoire. Comme la mémoire utilisée par le CLR est comptabilisée dans l'utilisation de la mémoire totale du système, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut rester dans ses limites de mémoire configurées et garantir que le CLR et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne rivalisent pas l'un avec l'autre pour la mémoire. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut également rejeter les demandes de mémoire CLR lorsque la mémoire système est contrainte, et demander au CLR de réduire son utilisation de la mémoire lorsque d'autres tâches ont besoin de mémoire.  
  
###### <a name="reliability-application-domains-and-unrecoverable-exceptions"></a>Fiabilité : domaines d'application et exceptions irrécupérables  
 Lorsqu'un code managé dans les API du .NET Framework rencontre des exceptions critiques, telles qu'une mémoire insuffisante ou un dépassement de capacité de la pile, il n'est pas toujours possible de récupérer après de tels échecs et de garantir une sémantique cohérente et correcte pour leur implémentation. Ces API déclenchent une exception d'abandon de thread en réponse à ces échecs.  
  
 En cas d'hébergement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], de tels abandons de thread sont gérés comme suit : le CLR détecte tout état partagé dans le domaine d'application dans lequel l'abandon de thread se produit. Le CLR accomplit cela en vérifiant la présence d'objets de synchronisation. S'il existe un état partagé dans le domaine d'application, le domaine d'application lui-même est déchargé. Le déchargement du domaine d'application arrête les transactions de base de données qui sont actuellement en cours d'exécution dans ce domaine d'application. Comme la présence d'un état partagé peut élargir l'impact de telles exceptions critiques aux sessions utilisateur autres que celle qui déclenche l'exception, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR ont pris des mesures pour réduire la probabilité d'un état partagé. Pour plus d'informations, consultez la documentation sur le .NET Framework.  
  
###### <a name="security-permission-sets"></a>Sécurité : jeux d'autorisations  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet aux utilisateurs de spécifier les exigences de fiabilité et de sécurité du code déployé dans la base de données. Lorsque les assemblys sont téléchargés dans la base de données, l'auteur de l'assembly peut spécifier trois jeux d'autorisations au choix pour cet assembly : SAFE, EXTERNAL_ACCESS ou UNSAFE.  
  
|||||  
|-|-|-|-|  
|Jeu d'autorisations|SAFE|EXTERNAL_ACCESS|UNSAFE|  
|Sécurité d’accès du code|Exécution uniquement|Exécution + accès aux ressources externes|Illimité|  
|Restrictions du modèle de programmation|Oui|Oui|Aucune restriction|  
|Vérifiabilité requise|Oui|Oui|non|  
|Possibilité d'appeler du code natif|non|Non|Oui|  
  
 SAFE est le mode le plus fiable et sécurisé avec des restrictions associées quant au modèle de programmation autorisé. Les assemblys SAFE bénéficient d'autorisations suffisantes pour s'exécuter, effectuer des calculs et avoir accès à la base de données locale. Les assemblys SAFE doivent être de type sécurisé vérifié et ne sont pas autorisés à appeler du code non managé.  
  
 UNSAFE est réservé à du code hautement approuvé qui peut être créé uniquement par les administrateurs de base de données. Ce code approuvé n'a pas de restrictions de sécurité d'accès du code et il peut appeler du code non managé (natif).  
  
 EXTERNAL_ACCESS fournit une option de sécurité intermédiaire. Il permet au code d'accéder à des ressources externes à la base de données, mais possède néanmoins les garanties de fiabilité de SAFE.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la couche de stratégie de sécurité d'accès du code de niveau hôte pour définir une stratégie hôte qui accorde l'un des trois jeux d'autorisations en fonction du jeu d'autorisations stocké dans les catalogues [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le code managé qui s'exécute au sein de la base de données obtient toujours l'un de ces jeux d'autorisations d'accès du code.  
  
### <a name="programming-model-restrictions"></a>Restrictions du modèle de programmation  
 Le modèle de programmation du code managé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implique l'écriture de fonctions, de procédures et de types qui ne nécessitent généralement pas l'utilisation d'un état maintenu d'un appel à un autre ni le partage de l'état sur plusieurs sessions utilisateur. Par ailleurs, comme décrit précédemment, la présence d'un état partagé peut provoquer des exceptions critiques qui affectent l'évolutivité et la fiabilité de l'application.  
  
 Dans de telles considérations, nous déconseillons d'utiliser des variables statiques et des membres de données statiques de classes utilisées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour les assemblys SAFE et EXTERNAL_ACCESS, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] examine les métadonnées de l'assembly au moment de la création de l'assembly et échoue dans la création de tels assemblys s'il détecte l'utilisation de membres de données et de variables statiques.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette également les appels aux API .NET Framework qui sont annotés avec les **SharedState**, **synchronisation** et **ExternalProcessMgmt** les attributs de protection de l’hôte. Cela empêche les assemblys SAFE et EXTERNAL_ACCESS d'appeler des API qui activent l'état de partage, d'effectuer une synchronisation et d'affecter l'intégrité du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Restrictions du modèle de programmation CLR Integration](../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité d’intégration du CLR](../../relational-databases/clr-integration/security/clr-integration-security.md)   
 [Performances de l’intégration du CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md)  
  
  
