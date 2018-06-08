---
title: Guide d’architecture de thread et de tâche | Microsoft Docs
ms.custom: ''
ms.date: 10/26/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: relational-databases-misc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
caps.latest.revision: 3
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: dcb886bee40d358bf0b2815e8fb669876f32d3a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="thread-and-task-architecture-guide"></a>guide d’architecture de thread et de tâche
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Les threads sont une caractéristique du système d'exploitation qui permettent à la logique de l'application d'être séparée en plusieurs chemins d'exécution simultanés. Cette fonctionnalité est utile lorsque des applications complexes comprennent un grand nombre de tâches à exécuter simultanément. 

Lorsqu'un système d'exploitation exécute une instance d'application, il crée une unité appelée processus pour gérer cette instance. Ce processus est associé à un thread d'exécution. Il s'agit de la série d'instructions de programmation exécutées par le code de l'application. Par exemple, si une application simple comprend un seul jeu d'instructions pouvant être exécutées en série, il n'existe qu'un seul chemin d'exécution, ou thread, dans toute l'application. Les applications plus complexes peuvent exécuter plusieurs tâches en même temps, mais pas en série. Pour ce faire, l'application lance plusieurs processus distincts pour chaque tâche. Mais le démarrage d'un processus consomme beaucoup de ressources. Une application peut également lancer plusieurs threads. Les threads consomment relativement peu de ressources. De plus, l'exécution de chaque thread peut être programmée indépendamment des autres threads du même processus.

Les threads permettent aux applications complexes d'utiliser plus efficacement une UC, même sur des ordinateurs dotés d'une seul UC. Avec une UC, vous ne pouvez exécuter qu'un seul thread à la fois. Si un thread exécute une longue opération qui n'utilise pas l'UC, comme par exemple la lecture ou l'écriture sur le disque, un autre thread peut être exécuté avant la fin de la première opération. La possibilité d'exécuter des threads pendant que d'autres attendent la fin d'une opération permet à l'application d'optimiser l'utilisation de l'UC. Cela est particulièrement vrai pour les applications multi-utilisateurs qui utilisent beaucoup d'E/S disque, par exemple un serveur de bases de données. Les ordinateurs dotés de plusieurs microprocesseurs ou UC peuvent exécuter simultanément un thread par UC. Ainsi, si un ordinateur comprend huit UC, il peut exécuter huit threads en même temps.

## <a name="sql-server-batch-or-task-scheduling"></a>Planification de lots ou de tâches SQL Server

### <a name="allocating-threads-to-a-cpu"></a>Allocation de threads à une UC

Par défaut, chaque instance de SQL Server commence chaque thread. Si l'affinité a été activée, le système d'exploitation attribue chaque thread à une UC spécifique. Le système d’exploitation répartit les threads des instances de SQL Server entre tous les microprocesseurs ou UC sur un ordinateur en fonction de la charge. Parfois, le système d'exploitation peut également déplacer un thread d'une UC très utilisée vers une autre. Par opposition, le moteur de base de données SQL Server attribue des threads de travail aux planificateurs qui distribuent les threads de manière équitable entre les unités centrales.

L’option de masque d’affinité est définie à l’aide [d’ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md). Lorsque le masque d’affinité n’est pas défini, l’instance de SQL Server alloue les threads de travail de manière équitable entre les planificateurs qui n’ont pas été masqués

### <a name="using-the-lightweight-pooling-option"></a>Utilisation de l'option Regroupement léger

L'avantage du basculement des contextes de thread n'est pas significatif. La plupart des instances de SQL Server ne perçoivent aucune différence de performances entre les valeurs 0 et 1 de l’option Regroupement léger. Les seules instances de SQL Server susceptibles de tirer parti de l’option [Regroupement léger](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) sont celles dont l’ordinateur présente les caractéristiques suivantes :    
* le serveur multiprocesseur est grand ;
* tous les processeurs fonctionnent quasiment à leur capacité maximale ;
* la fréquence de basculement des contextes est importante.

Ces systèmes peuvent augmenter légèrement leurs performances si la valeur 1 est attribuée à l’option Regroupement léger.

Il n'est pas recommandé d'utiliser la planification du mode fibre pour les opérations courantes. Cela peut réduire les performances en bloquant les avantages habituels du basculement de contexte. Par ailleurs, certains composants de SQL Server ne peuvent pas fonctionner correctement en mode fibre. Pour plus d’informations, consultez Regroupement léger.

## <a name="thread-and-fiber-execution"></a>Exécution de threads et de fibres

Microsoft Windows utilise un système de priorité numérique qui va de 1 à 31 pour planifier l'exécution des threads. Le zéro est réservé pour le système d'exploitation. Lorsque plusieurs threads attendent d'être exécutés, Windows envoie le thread dont la priorité est la plus élevée.

Par défaut, chaque instance de SQL Server est une priorité 7 (priorité dite normale). Ceci octroie aux threads SQL Server une priorité suffisamment élevée pour obtenir les ressources UC dont ils ont besoin sans pénaliser les autres applications. 

L’option de configuration [Renforcement de priorité](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) peut être utilisée pour augmenter la priorité des threads d’une instance de SQL Server à 13. (priorité dite élevée). Cette valeur donne aux threads SQL Server une priorité plus élevée que la plupart des autres applications. Par conséquent, les threads SQL Server auront plutôt tendance à être distribués chaque fois qu’ils sont prêts à être exécutés et ne seront pas devancés par les threads d’autres applications. Ceci peut améliorer les performances lorsqu’un serveur n’exécute que des instances de SQL Server et aucune autre application. Cependant, si une opération nécessitant beaucoup de mémoire se produit dans SQL Server, les autres applications n’auront sans doute pas une priorité suffisamment élevée pour devancer le thread SQL Server. 

Si vous utilisez plusieurs instances de SQL Server sur un ordinateur et activez le renforcement de priorité pour certaines d’entre elles uniquement, les performances des instances exécutées avec une priorité normale peuvent être compromises. Les performances d’autres applications et composants sur le serveur peuvent aussi diminuer si l’option Renforcement de priorité est activée. C'est pourquoi elle doit être utilisée dans certaines conditions bien définies.


## <a name="hot-add-cpu"></a>Ajout d’un processeur à chaud

L'ajout d'un processeur à chaud est la capacité d'ajouter dynamiquement des processeurs à un système en cours d'exécution. L'ajout de processeurs peut s'effectuer physiquement en ajoutant du matériel, logiquement en partitionnant du matériel en ligne ou virtuellement par l'intermédiaire d'une couche de virtualisation. À partir de la version SQL Server 2008, SQL Server prend en charge l’ajout d’un processeur à chaud.

Spécifications pour l'ajout de processeurs à chaud :  
* Nécessite un matériel prenant en charge l'ajout de processeurs à chaud.
* Nécessite l'édition 64 bits de Windows Server 2008 Datacenter ou le système d'exploitation Windows Server 2008 Enterprise Edition pour systèmes basés sur Itanium.
* Nécessite SQL Server Entreprise.
* SQL Server ne peut pas être configuré pour utiliser la configuration NUMA logicielle. Pour plus d’informations sur la configuration NUMA logicielle, consultez [Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md).

SQL Server ne commence pas automatiquement à utiliser les processeurs une fois que ceux-ci ont été ajoutés. Cela empêche SQL Server d’utiliser des processeurs qui peuvent être ajoutés à d’autres fins. Après avoir ajouté des processeurs, exécutez l’instruction [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md) pour que SQL Server reconnaisse les nouveaux processeurs comme des ressources disponibles.

> [!NOTE]
> Si le [masque d’affinité 64](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) est configuré, il doit être modifié pour utiliser les nouveaux processeurs.
 

## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>Recommandations pour l'exécution de SQL Server sur des ordinateurs comportant plus de 64 unités centrales

### <a name="assigning-hardware-threads-with-cpus"></a>Affectation de threads matériels avec des unités centrales

N’utilisez pas les options de configuration de serveur masque d’affinité (affinity mask) et masque d’affinité 64 (affinity64 mask) pour lier les processeurs à des threads spécifiques. Ces options sont limitées à 64 unités centrales. Utilisez l’option SET PROCESS AFFINITY de [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) à la place.

### <a name="managing-the-transaction-log-file-size"></a>Gestion de la taille du fichier journal des transactions

Ne comptez pas sur la croissance automatique pour augmenter la taille du fichier journal de transactions. L'augmentation du journal des transactions doit être un processus série. L’extension du journal peut empêcher la poursuite d’opérations d’écriture de transactions jusqu’à ce que l’extension du journal soit terminée. Pré-allouez plutôt l'espace des fichiers journaux en définissant leur taille avec une valeur suffisamment élevée pour prendre en charge la charge de travail habituelle de l'environnement.

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>Définition du degré maximal de parallélisme pour les opérations d'index

Les performances des opérations d'index telles que la création ou la reconstruction d'index peuvent être améliorées sur les ordinateurs qui possèdent de nombreuses unités centrales en définissant temporairement le mode de récupération de la base de données sur le mode de récupération simple ou de journalisation en bloc. Ces opérations d’index peuvent générer une activité de journal significative et les contentions de journal peuvent affecter le meilleur choix de degré de parallélisme (DOP) effectué par SQL Server.

De plus, modifiez selon les besoins l’option de configuration serveur du **degré maximum de parallélisme (MAXDOP)** pour ces opérations. Les indications suivantes sont basées sur des tests internes et constituent des recommandations générales. Essayez plusieurs paramètres MAXDOP différents pour déterminer le paramètre optimal pour votre environnement.

* Pour le mode de récupération complète, limitez la valeur de l’option Degré maximal de parallélisme à huit ou une valeur inférieure.   
* Pour le modèle de journalisation en bloc ou le mode de récupération simple, essayez de définir la valeur de l’option Degré maximal de parallélisme sur une valeur supérieure à huit.   
* Pour les serveurs pour lesquels des nœuds NUMA sont configurés, le degré maximal de parallélisme ne doit pas dépasser le nombre d'unités centrales attribuées à chaque nœud NUMA. Cela vient du fait que la requête est plus susceptible d'utiliser la mémoire locale d'un nœud NUMA, ce qui peut améliorer le temps d'accès à la mémoire.  
* Sur les serveurs ou l’hyperthreading est activé et qui ont été fabriqués avant l’année 2009 incluse (c’est-à-dire avant l’amélioration de la fonctionnalité hyperthreading), la valeur MAXDOP ne doit pas dépasser le nombre de processeurs physiques plutôt que le nombre de processeurs logiques.

Pour plus d’informations sur l’option relative au degré maximal de parallélisme, consultez [Configurer l’option de configuration du serveur Degré maximal de parallélisme](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

### <a name="setting-the-maximum-number-of-worker-threads"></a>Configuration du nombre maximal de threads de travail

Définissez toujours le nombre maximal de threads de travail avec une valeur supérieure à celle du paramètre de degré maximal de parallélisme. Le nombre de threads de travail doit toujours être défini sur une valeur équivalant à au moins sept fois le nombre d'unités centrales qui sont présentes sur le serveur. Pour plus d’informations, consultez [Configurer l’option max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

### <a name="using-sql-trace-and-sql-server-profiler"></a>Utilisation de Trace SQL et du Générateur de profils SQL

 Nous vous recommandons de pas ne pas utiliser Trace SQL et SQL Server Profiler dans un environnement de production. La surcharge induite par l'exécution de ces outils augmente également proportionnellement au nombre d'unités centrales. Si vous devez utiliser Trace SQL dans un environnement de production, limitez le nombre d'événements de trace au minimum. Profilez et testez avec soin chaque événement de trace sous charge et évitez d'utiliser des combinaisons d'événements qui affectent les performances de façon significative.

### <a name="setting-the-number-of-tempdb-data-files"></a>Définition du nombre de fichiers de données tempdb

En général, le nombre de fichiers de données tempdb doit correspondre au nombre d’unités centrales. Toutefois, en prenant soigneusement en compte les besoins de concurrence de tempdb, vous pouvez réduire la gestion de la base de données. Par exemple, si un système comporte 64 unités centrales et qu'habituellement seules 32 requêtes utilisent tempdb, l'augmentation du nombre de fichiers tempdb à 64 n'améliorera pas les performances.

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>Composants SQL Server qui peuvent utiliser plus de 64 unités centrales

Le tableau suivant dresse la liste des composants de SQL Server et indique s’ils peuvent utiliser plus de 64 unités centrales.

|Nom du processus   |Programme exécutable |Utilisation de plus de 64 unités centrales |  
|----------|----------|----------|  
|Moteur de base de données SQL Server |Sqlserver.exe  |Oui |  
|Reporting Services |Rs.exe |non |  
|Analysis Services  |As.exe |non |  
|Integration Services   |Is.exe |non |  
|Service Broker |Sb.exe |non |  
|Recherche en texte intégral   |Fts.exe    |non |  
|SQL Server Agent   |Sqlagent.exe   |non |  
|SQL Server Management Studio   |Ssms.exe   |non |  
|Programme d'installation de SQL Server   |Setup.exe  |non |  


