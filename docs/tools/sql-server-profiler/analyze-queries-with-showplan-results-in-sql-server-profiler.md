---
title: Analyser des requêtes avec des résultats SHOWPLAN dans SQL Server Profiler | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: profiler
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- events [SQL Server], Showplan
- Profiler [SQL Server Profiler], Showplan results
- SQL Server Profiler, Showplan results
ms.assetid: 6a2f7727-141c-4f59-8613-2e452bc78467
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 26e3fcd0f7959f5b659468f943a321d30c75cce9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="analyze-queries-with-showplan-results-in-sql-server-profiler"></a>Analyser des requêtes avec des résultats SHOWPLAN dans SQL Server Profiler
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez ajouter des classes d'événements Showplan à une définition de trace afin que [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] rassemble et affiche les informations de plan de requête dans la trace. Vous pouvez également extraire les événements Showplan des autres événements collectés dans la trace et les enregistrer dans un fichier XML distinct.  
  
 Chacune des procédures suivantes permet d'extraire les événements Showplan de la trace :  
  
-   Au moment de la configuration de la trace, à l’aide de l’onglet **Paramètres d’extraction des événements** . Notez que cet onglet n’apparaît pas tant que vous n’avez pas sélectionné l’un des événements Showplan sous l’onglet **Sélection des événements** .  
  
-   À l’aide de l’option **Extraire les événements SQL Server** du menu **Fichier** .  
  
-   En extrayant et en enregistrant un événement donné en cliquant avec le bouton droit dessus et en choisissant **Extraire les données d’événement**.  
  
## <a name="showplan-events"></a>Événements Showplan  
 Le tableau suivant répertorie et décrit les événements de trace Showplan.  
  
|Nom d'événement|Description|  
|----------------|-----------------|  
|**Performance statistics**|Indique la première mise en cache d'un plan d'exécution compilé, à quel moment il est recompilé et à quel moment il est supprimé de la mémoire cache de plans. La colonne **TextData** contient le plan d’exécution au format XML. Pour plus d’informations, consultez [Classe d’événements Performance Statistics](../../relational-databases/event-classes/performance-statistics-event-class.md).|  
|**Showplan All**|Affiche le plan de requête avec le détail complet des informations de compilation de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutée. Par exemple, il peut afficher des estimations de coût et des listes de colonnes. Pour plus d’informations, consultez [Classe d’événements Showplan All](../../relational-databases/event-classes/showplan-all-event-class.md).|  
|**Showplan All For Query Compile**|Générée lorsqu'une requête est compilée ou recompilée sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il s’agit de l’équivalent de compilation de l’événement **Showplan All** . **Showplan All** est générée lors de l’exécution d’une requête. **Showplan All For Query Compile** est générée lors de la compilation d’une requête. Pour plus d’informations, consultez [Classe d’événements Showplan All for Query Compile](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md).|  
|**Showplan Statistics Profile**|Affiche le plan de requête avec le détail complet des informations de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] en cours d'exécution, dont le nombre réel de lignes traitées dans chaque opération. Pour plus d’informations, consultez [Classe d’événements Showplan Statistics Profile](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md).|  
|**Showplan Text**|Affiche, sous forme de données binaires, l'arborescence du plan de requête de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] en cours d'exécution. Pour plus d’informations, consultez [Classe d’événements Showplan Text](../../relational-databases/event-classes/showplan-text-event-class.md).|  
|**Showplan Text (non encodée)**|Affiche, sous forme de texte, l'arborescence du plan de requête de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] en cours d'exécution. Cette classe d'événements affiche les mêmes informations que la classe Showplan Text, à la différence que celle-ci affiche du texte au lieu de données binaires. Pour plus d’informations, consultez [Classe d’événements Showplan Text &#40;non encodée&#41;](../../relational-databases/event-classes/showplan-text-unencoded-event-class.md).|  
|**Showplan XML**|Affiche le plan de requête avec les données complètes collectées au cours de l'optimisation de la requête. Cet événement est généré seulement quand un plan de requête est optimisé. Pour plus d’informations, consultez [Classe d’événements Showplan XML](../../relational-databases/event-classes/showplan-xml-event-class.md).|  
|**Showplan XML For Query Compile**|Affiche le plan de requête lorsque la requête est compilée. Pour plus d’informations, consultez [Classe d’événements Showplan XML for Query Compile](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md).|  
|**Showplan XML Statistics Profile**|Affiche le plan de requête avec le détail complet des informations d'exécution au format XML. Par exemple, cette classe d'événements capture le nombre de lignes traitées par chaque opérateur de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutée. Pour plus d’informations, consultez [Classe d’événements Showplan XML Statistics Profile](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md).|  
  
## <a name="see-also"></a> Voir aussi  
 [Catégorie d'événements Performance](../../relational-databases/event-classes/performance-event-category.md)  
  
  
