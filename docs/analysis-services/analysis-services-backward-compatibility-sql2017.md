---
title: Compatibilité descendante de SQL Server 2017 Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7dc1581fd2940ec5bad7698985eeab2c8ed96b2c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-backward-compatibility-sql-2017"></a>Compatibilité descendante de Analysis Services (2017 SQL)
[!INCLUDE[ssas-appliesto-sql2017](../includes/ssas-appliesto-sql2017.md)]

Cet article décrit les modifications de la disponibilité des fonctionnalités et de comportement entre la version actuelle et la version précédente.

## <a name="deprecated-features"></a>Fonctionnalités déconseillées
A *fonctionnalité déconseillée* sera supprimé du produit dans une version ultérieure, mais est toujours pris en charge et incluse dans la version actuelle pour assurer la compatibilité descendante. Il est recommandé de que vous arrêter d’utiliser les fonctionnalités déconseillées dans les projets nouveaux et existants pour assurer la compatibilité avec les versions ultérieures.

Les fonctionnalités suivantes sont déconseillées dans cette version :
  
|||  
|-|-|  
|**En mode/une catégorie**|**Fonctionnalité**|
|(Multidimensionnel)|Exploration de données|
|(Multidimensionnel)|Groupes de mesures liés distants|
|Tabulaire|Au niveau de compatibilité 1100 et 1103 des modèles|
|Tabulaire|Propriétés du modèle d’objet tabulaires : Column.TableDetailPosition, Column.IsDefaultLabel, Column.IsDefaultImage|
|Outils|SQL Server Profiler pour la capture de traces<br /><br /> La solution consiste à utiliser le Générateur de profils d’événements étendus, intégré dans SQL Server Management Studio.  <br /> Consultez [Surveiller Analysis Services avec des événements étendus SQL Server](../analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events.md).|  
|Outils|Server Profiler pour Trace Replay <br />Remplacement. Il n’existe aucune solution de remplacement.|  
|Objets de gestion de trace et API de trace|Objets Microsoft.AnalysisServices.Trace (contenant les API des objets Analysis Services de trace et de relecture). La solution de remplacement est multiple :<br /><br /> -Configuration de trace : Microsoft.SqlServer.Management.XEvent<br />-Lecture de trace : Microsoft.SqlServer.XEvent.Linq<br />-   Relecture de trace : Aucune|  


## <a name="discontinued-features"></a>Fonctionnalités supprimées
A *abandonné fonctionnalité* a été déconseillée dans une version antérieure. Il peut continuer à être inclus dans la version actuelle, mais n’est plus pris en charge. Fonctionnalités supprimées peuvent être supprimées entièrement dans une future version ou mettre à jour.

Les fonctionnalités suivantes ont été déconseillées dans une version antérieure et ne sont plus prises en charge dans cette version.
  
|||  
|-|-|  
|**En mode/une catégorie**|**Fonctionnalité**|  
|Tabulaire|Valeur de la propriété mémoire VertipaqPagingPolicy (2), activer la pagination disque à l’aide de la mémoire des fichiers mappés.|
|(Multidimensionnel)|Partitions distantes|  
|(Multidimensionnel)|Groupes de mesures liés distants|  
|(Multidimensionnel)|Écriture différée dimensionnelle|  
|(Multidimensionnel)|Dimensions liées|


## <a name="breaking-changes"></a>Modifications avec rupture
A *modification avec rupture* provoque une fonctionnalité, le modèle de données, le code d’application ou le script ne fonctionne plus après la mise à niveau vers la version actuelle.

Il n’existe pas de modifications importantes dans cette version.

## <a name="behavior-changes"></a>Changements de comportement
A *changement de comportement* affecte le fonctionne de la même fonctionnalité dans la version actuelle par rapport à la version précédente. Seules les modifications de comportement importantes sont décrites. Modifications dans l’interface utilisateur ne sont pas incluses.

Modifications apportées aux MDSCHEMA_MEASUREGROUP_DIMENSIONS et DISCOVER_CALC_DEPENDENCY, détaillées dans le [quelles sont les nouveautés de SQL Server 2017 CTP 2.1 pour Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/2017/05/18/whats-new-in-sql-server-2017-ctp-2-1-for-analysis-services/) annonce.


## <a name="see-also"></a>Voir aussi
[Compatibilité descendante de Analysis Services (SQL Server 2016)](analysis-services-backward-compatibility.md)
