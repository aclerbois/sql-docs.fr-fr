---
title: Événements étendus | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- extended events [SQL Server]
- xe
ms.assetid: bf3b98a6-51ed-4f2d-9c26-92f07f1fa947
caps.latest.revision: 48
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e5dfdc3c08bd548530e4ba2deb50871808efc00f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34706857"
---
# <a name="extended-events"></a>Événements étendus
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Les événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bénéficient d'une architecture extrêmement évolutive et configurable qui permet aux utilisateurs de recueillir le maximum ou le minimum d'informations nécessaires pour le dépannage ou l'identification d'un problème de performance.  

Pour plus d’informations sur les événements étendus, consultez :

- [Démarrage rapide : événements étendus dans SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)
- Blogs : [Utilisation des événements étendus SQL Server](http://blogs.msdn.com/b/extended_events/)

  
## <a name="benefits-of-includessnoversionincludesssnoversion-mdmd-extended-events"></a>Avantages des événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Les événements étendus sont un système léger d'analyse des performances qui utilise très peu de ressources de performances. Les événements étendus fournissent deux interfaces utilisateur graphiques (**Assistant Nouvelle session** et **Nouvelle session**) permettant de créer, modifier, afficher et analyser vos données de session.  
  
## <a name="extended-events-concepts"></a>Concepts liés aux événements étendus  
 Les événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’appuient sur des concepts existants, tels qu’un événement ou un consommateur d’événements, utilisent les concepts provenant du suivi d’événements pour Windows et introduisent de nouveaux concepts.  
  
 Le tableau suivant décrit les concepts des événements étendus.  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Packages d’événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-packages.md)|Décrit les packages des Événements étendus qui contiennent les objets utilisés pour obtenir et traiter les données au cours d'une session Événements étendus.|  
|[Cibles des événements étendus SQL Server](http://msdn.microsoft.com/library/e281684c-40d1-4cf9-a0d4-7ea1ecffa384)|Décrit les consommateurs d'événements qui peuvent recevoir des données au cours d'une session d'événements.|  
|[Moteur des événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-engine.md)|Décrit le moteur qui implémente et gère une session Événements étendus.|  
|[Sessions d’événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md)|Décrit la session d'événements étendus.|  
  
## <a name="extended-events-architecture"></a>Architecture des événements étendus  
 Les événements étendus représentent un système de gestion d'événements général pour les systèmes serveur. L'infrastructure des événements étendus prend en charge la corrélation de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et, sous certaines conditions, la corrélation de données provenant du système d'exploitation et d'applications de base de données. Dans ce dernier cas, la sortie des Événements étendus doit être dirigée vers le Suivi d'événements pour Windows (ETW) afin de mettre en corrélation les données d'événement avec les données d'événement du système d'exploitation ou des applications.  
  
 Toutes les applications ont des points d'exécution qui sont utiles aussi bien à l'intérieur qu'à l'extérieur d'une application. Au sein de l'application, le traitement asynchrone peut être mis en attente à l'aide d'informations collectées au cours de l'exécution initiale d'une tâche. En dehors de l'application, les points d'exécution fournissent des utilitaires d'analyse avec des informations sur les caractéristiques comportementales et de performances de l'application analysée.  
  
 Les événements étendus prennent en charge l'utilisation de données d'événement à l'extérieur d'un processus. Ces données sont utilisées en général par :  
  
-   des outils de suivi, tels que Trace SQL et le Moniteur système ;  
  
-   des outils de journalisation, tels que le journal des événements Windows ou le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   des utilisateurs qui administrent un produit ou développent des applications sur un produit.  
  
 Les événements étendus présentent les aspects de conception clés suivants :  
  
-   Le moteur d'événements étendus est agnostique en termes d'événements, ce qui permet au moteur de lier tout événement à toute cible, car le moteur n'est pas contraint par le contenu des événements. Pour plus d'informations sur le moteur d'événements étendus, consultez [SQL Server Extended Events Engine](../../relational-databases/extended-events/sql-server-extended-events-engine.md).  
  
-   Les événements sont séparés des consommateurs d'événements, appelés *cibles* dans les événements étendus. Cela signifie que toute cible peut recevoir tout événement. De plus, tout événement déclenché peut être automatiquement consommé par la cible, qui peut enregistrer dans le journal ou fournir un contexte d'événement supplémentaire. Pour plus d'informations, consultez [SQL Server Extended Events Targets](http://msdn.microsoft.com/library/e281684c-40d1-4cf9-a0d4-7ea1ecffa384).  
  
-   Les événements sont distincts de l'action à entreprendre lorsqu'un événement se produit. Par conséquent, toute action peut être associée à tout événement.  
  
-   Les prédicats peuvent filtrer dynamiquement lorsque les données d'événement doivent être capturées. Cela s'ajoute à la flexibilité de l'infrastructure des événements étendus. Pour plus d'informations, consultez [SQL Server Extended Events Packages](../../relational-databases/extended-events/sql-server-extended-events-packages.md).  
  
 Les événements étendus peuvent générer de façon synchrone des données d'événement (et traiter de façon asynchrone ces données), ce qui fournit une solution flexible de gestion des événements. De plus, les événements étendus fournissent les fonctionnalités suivantes :  
  
-   une approche unifiée de la gestion des événements sur le système serveur, tout en permettant aux utilisateurs d'isoler des événements spécifiques dans le but de résoudre des problèmes ;  
  
-   l'intégration avec les outils existants de suivi ETW et la prise en charge de ces derniers ;  
  
-   un mécanisme de gestion des événements entièrement configurable, basé sur [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   la capacité de surveiller dynamiquement les processus actifs tout en ayant un impact minime sur ces processus.  
  
-   une session de l'intégrité du système par défaut qui s'exécute sans effet de performance notable ; Elle recueille des données système qui peuvent vous aider à résoudre des problèmes de performances. Pour plus d’informations, consultez [Utiliser la session system_health](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="extended-events-tasks"></a>Tâches relatives aux événements étendus  

En utilisant [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] pour exécuter des instructions DDL (Data Definition Language) [!INCLUDE[tsql](../../includes/tsql-md.md)] , des vues et des fonctions de gestion dynamiques ou des affichages catalogue, vous pouvez créer des solutions de dépannage des Événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] simples ou complexes pour votre environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Utilisez l' **Explorateur d'objets** pour gérer les sessions d'événements.|[Gérer les sessions d’événements dans l’Explorateur d’objets](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)|  
|Explique comment créer une session d'événements étendus.|[Créer une session d’événements étendus](http://msdn.microsoft.com/library/34b1e95a-a80e-4aca-9201-abde47f2ca74)|  
|Explique comment afficher et actualiser des données cibles.| [Affichage avancé des données cibles d’événements étendus dans SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)|  
|Explique comment utiliser des outils d'événements étendus pour créer et gérer vos sessions d'événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Outils associés aux événements étendus](../../relational-databases/extended-events/extended-events-tools.md)|  
|Explique comment altérer une session d'événements étendus.|[Modifier une session d’événements étendus](../../relational-databases/extended-events/alter-an-extended-events-session.md)|  
|Explique comment obtenir des informations sur les champs associés aux événements.|[Obtenir les champs pour tous les événements](http://msdn.microsoft.com/library/4e4ee03f-5bca-42ed-a37c-db1c82e3aad2)|  
|Explique comment déterminer quels sont les événements disponibles dans les packages enregistrés.|[Consulter les événements pour les packages enregistrés](http://msdn.microsoft.com/library/9a90b1a2-aa69-43f6-bdeb-cc5f57a26c6f)|  
|Explique comment déterminer quelles cibles d'événements étendus sont disponibles dans les packages enregistrés.|[Afficher les cibles d’événements étendus pour les packages enregistrés](http://msdn.microsoft.com/library/4985aa5f-ac99-49f6-852c-9d25916549e9)|  
|Explique comment afficher les événements Événements étendus et les actions qui sont équivalents à chaque événement SQL Trace et à ses colonnes associées.|[Consulter les événements étendus équivalents aux classes d’événements Trace SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)|  
|Explique comment rechercher les paramètres que vous pouvez définir lorsque vous utilisez l'argument ADD TARGET dans CREATE EVENT SESSION ou ALTER EVENT SESSION.|[Obtenir les paramètres configurables pour l’argument ADD TARGET](http://msdn.microsoft.com/library/08454543-c5c8-4ca3-9af9-f1d82264471c)|  
|Explique comment convertir un script Trace SQL existant en session d'événements étendus.|[Convertir un script Trace SQL existant en session d’événements étendus](../../relational-databases/extended-events/convert-an-existing-sql-trace-script-to-an-extended-events-session.md)|  
|Explique comment déterminer quelles requêtes détiennent le verrou, le plan de la requête et la pile [!INCLUDE[tsql](../../includes/tsql-md.md)] au moment où le verrou a été mis.|[Déterminer quelles requêtes détiennent des verrous](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)|  
|Explique comment identifier la source des verrous qui gênent les performances de la base de données.|[Trouver les objets comportant le plus de verrous](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)|  
|Explique comment utiliser les événements étendus avec le suivi d'événements pour Windows pour surveiller l'activité système.|[Surveiller l’activité système à l’aide d’événements étendus](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)|  
| Utilisation des affichages catalogue et des vues de gestion dynamique pour les événements étendus | [SELECT et JOIN à partir de vues système pour les événements étendus dans SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) |

  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Prise en charge DAC pour les objets et versions SQL Server](../../relational-databases/data-tier-applications/dac-support-for-sql-server-objects-and-versions.md)   
 [Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)   
 [Analyser les applications de la couche Données](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)   
 [Vues de gestion dynamique des Événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)  
  
  
