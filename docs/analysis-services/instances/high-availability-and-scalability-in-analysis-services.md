---
title: Haute disponibilité et extensibilité dans Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 33270141fc67581adaeaeca9df8411c68fe0ac50
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="high-availability-and-scalability-in-analysis-services"></a>Haute disponibilité et extensibilité dans Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Cet article décrit les techniques couramment utilisées pour rendre les bases de données Analysis Services hautement disponibles et évolutives. Bien que ces deux objectifs pourraient être traités séparément, ils vont souvent de pair : un déploiement évolutif destiné à des charges de travail de requête ou de traitement volumineuses est généralement associé à certaines attentes en matière de haute disponibilité.  
  
 L’inverse n’est cependant pas toujours vrai. La haute disponibilité, sans mise à l’échelle, peut être le seul objectif lorsque des contrats SLA stricts sont établis pour des charges de travail de requête critiques mais modérées.  
  
 Les techniques permettant de rendre Analysis Services hautement disponible et évolutif sont sensiblement les mêmes pour tous les modes de serveur (multidimensionnel, tabulaire et mode intégré SharePoint). Sauf indication contraire, partez du principe que les informations présentes dans cet article s’appliquent à tous les modes.  
  
## <a name="key-points"></a>Points clés  
 Les techniques associées à la disponibilité et la mise à l’échelle étant différentes de celles du moteur de base de données relationnelle, un bref résumé des points clés permet de présenter clairement les techniques utilisées avec Analysis Services :  
  
-   Analysis Services utilise les mécanismes de haute disponibilité et d’extensibilité intégrés dans la plateforme serveur Windows : équilibrage de la charge réseau (NLB), clustering de basculement Windows Server (WSFC), ou les deux.  
  
    > [!NOTE]  
    >  La fonctionnalité Always On du moteur de base de données relationnelle ne s’étend pas à Analysis Services.  Vous ne pouvez pas configurer une instance Analysis Services pour qu’elle s’exécute dans un groupe de disponibilité Always On.  
    >   
    >  Bien qu’Analysis Services ne s’exécute pas dans des groupes de disponibilité Always On, il peut à la fois récupérer et traiter des données provenant de bases de données relationnelles Always On. Pour connaître les instructions de configuration d’une base de données relationnelle hautement disponible en vue de son utilisation par Analysis Services, consultez [Analysis Services avec les groupes de disponibilité AlwaysOn](../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md).  
  
-   La haute disponibilité, comme seul objectif, peut être obtenue via la redondance de serveur dans un cluster de basculement. Les nœuds de remplacement sont censés avoir la même configuration matérielle et logicielle que le nœud actif.  Le clustering de basculement Windows Server vous offre à lui seul la haute disponibilité, mais sans mise à l’échelle.  
  
-   L’extensibilité, avec ou sans disponibilité, est obtenue via l’équilibrage de la charge réseau sur les bases de données en lecture seule.  En général, l’extensibilité présente un intérêt lorsque les volumes de requêtes sont grands ou soumis à des pics soudains.  
  
     L’équilibrage de la charge, associé à plusieurs bases de données en lecture seule, procure la mise à l’échelle et la haute disponibilité, car tous les nœuds sont actifs, et en cas d’arrêt d’un serveur, les requêtes sont automatiquement distribuées entre les nœuds restants. Si vous avez besoin d’extensibilité et de disponibilité, le choix d’un cluster NLB s’impose.  
  
 Pour le traitement, les objectifs de haute disponibilité et d’extensibilité sont moins importants, car vous contrôlez la durée et l’étendue des opérations. Le traitement peut être à la fois partiel et incrémentiel sur des parties d’un modèle même si, à un moment donné, vous allez devoir traiter un modèle entièrement sur un serveur unique pour garantir la cohérence des données sur l’ensemble des index et agrégations. Une architecture évolutive solide repose sur du matériel capable de prendre en charge un traitement complet à la cadence requise. Pour les solutions volumineuses, ce travail est structuré comme une opération indépendante, avec ses propres ressources matérielles.  
  
##  <a name="bkmk_serverconfig"></a> Configurations sur un serveur et sur plusieurs serveurs  
 Dans un déploiement à serveur unique standard, les charges de travail de traitement et de requête s’exécutent simultanément, en supposant que les ressources système sont suffisantes pour les deux activités. Analysis Services laisse intactes les structures de données existantes pour la prise en charge des requêtes pendant qu’une version mise à jour est traitée en arrière-plan. Une quantité suffisante de mémoire et d’espace disque pour gérer les structures de données temporaires est un prérequis matériel applicable à tous les modes de serveur, bien que chacun observe des exigences différentes en matière de ressources système et possède des niveaux différents de reconnaissance NUMA.  
  
 **Serveurs uniques et extensibilité**  
  
 Un serveur multicœur haut de gamme unique peut offrir une mise à l’échelle suffisante. Sur les systèmes haut de gamme dotés d’un grand nombre de cœurs, et de suffisamment de mémoire RAM et d’espace disque, vous pouvez éventuellement mettre à l’échelle dans un seul système.  
  
 Pour les bases de données multidimensionnelles, vous pouvez ajuster les propriétés de la configuration du serveur pour créer des affinités entre les processus et les processeurs. Consultez la rubrique [Thread Pool Properties](../../analysis-services/server-properties/thread-pool-properties.md) (éventuellement en anglais) pour plus d'informations.  
  
 **Déploiements multiserveur**  
  
 Parfois, les spécifications opérationnelles exigent d’utiliser plusieurs serveurs. Par exemple, les clusters de basculement sont multiserveur par définition, chaque nœud étant exécuté sur des configurations matérielle et logicielle identiques.  
  
 De même, un besoin important de haute disponibilité des charges de travail de requête fait généralement appel à plusieurs serveurs. Dans ce scénario, la configuration recommandée pour Analysis Services est d’utiliser un mélange de bases de données en lecture seule et en lecture-écriture, exécutées sur des instances distinctes d’Analysis Services, sur du matériel dédié. Les bases de données en lecture seule gèrent les requête d’interrogation. Les bases de données accessibles en lecture-écriture servent au traitement. Une description approfondie de cette technique courante figure dans la section suivante.  
  
 **Machines virtuelles et haute disponibilité**  
  
 Pour répondre à une exigence de haute disponibilité, une autre stratégie consiste à utiliser des machines virtuelles. Si la disponibilité peut être obtenue en mettant en place un serveur de remplacement en plusieurs heures au lieu de quelques minutes, vous pouvez peut-être utiliser des machines virtuelles pouvant être démarrées à la demande, et chargées avec des bases de données mises à jour récupérées à partir d’un emplacement central.  
  
## <a name="scalability-using-read-only-and-read-write-databases"></a>Extensibilité à l’aide de bases de données en lecture seule et en lecture-écriture  
 L’équilibrage de la charge réseau est recommandé pour les charges de travail de traitement et de requête élevées ou intensifiées. Les bases de données Analysis Services dans une solution d’équilibrage de la charge réseau sont définies en tant que bases de données en lecture seule afin de garantir la cohérence entre les requêtes.  
  
 Bien que les instructions données dans l’article [Scale-out querying for Analysis Services using read-only databases](https://technet.microsoft.com/library/ff795582\(v=sql.100\).aspx) (Montée en charge des requêtes pour Analysis Services à l’aide de bases de données en lecture seule) datent de 2008, elles sont toujours valables. Les systèmes d’exploitation et le matériel des serveurs ont évolué, et les références à certaines plateformes et limites de processeur sont obsolètes, mais la technique de base d’utilisation des bases de données en lecture seule et en lecture-écriture pour de grands volumes de requêtes est inchangée.  
  
 L’approche peut être résumée comme suit :  
  
-   Utilisez un matériel et des instances d’Analysis Services dédiés pour traiter la base de données. Définissez la base de données sur lecture seule une fois le traitement terminé. Pour obtenir des instructions, consultez [Switch an Analysis Services database between ReadOnly and ReadWrite modes](../../analysis-services/multidimensional-models/switch-an-analysis-services-database-between-readonly-and-readwrite-modes.md) .  
  
-   Utilisez plusieurs serveurs de requêtes identiques pour exécuter des copies de la même base de données Analysis Services en lecture seule. Les serveurs sont déployés dans un cluster NLB, auquel vous accédez par un nom de serveur virtuel qui sert de point d’entrée unique vers le cluster.  
  
-   Utilisez robocopy pour copier un répertoire de données entier du serveur de traitement vers chaque serveur de requêtes et attacher la même base de données en lecture seule à tous les serveurs de requêtes. Vous pouvez également utiliser des instantanés SAN, la synchronisation ou tout autre outil ou méthode permettant de déplacer des bases de données de production.  
  
## <a name="resource-demands-for-tabular-and-multidimensional-workloads"></a>Demandes de ressources pour les charges de travail tabulaires et multidimensionnelles  
 Le tableau suivant offre un résumé général de l’utilisation des ressources système par Analysis Services pour les requêtes et le traitement, triées par mode de serveur et stockage. Ce résumé peut vous permettre de comprendre sur quoi vous devez mettre l’accent dans un déploiement multiserveur qui gère une charge de travail distribuée.  
  
|||  
|-|-|  
|**Serveur et mode de stockage**|**Impact sur les ressources système**|  
|Tabulaire en mémoire (par défaut), où les requêtes sont exécutées en tant qu’analyses de table de structures de données en mémoire.|Mettez l’accent sur la RAM et les processeurs avec des vitesses rapides.|  
|Tabulaire en mode DirectQuery, où les requêtes sont déchargées sur les serveurs de base de données relationnelle principaux et le traitement est limité à la construction des métadonnées du modèle.|Concentrez-vous sur les performances de la base de données relationnelle, réduisez la latence du réseau et optimisez le débit. Un processeur plus rapide permet également d’améliorer les performances du processeur de requêtes Analysis Services.|  
|Modèles multidimensionnels utilisant un stockage MOLAP|Choisissez une configuration équilibrée qui combine l’E/S disque pour charger des données rapidement et une mémoire RAM suffisante pour les données en cache.|  
|Modèles multidimensionnels utilisant un stockage ROLAP|Optimisez l’E/S disque et réduisez la latence du réseau au minimum.|  
  
## <a name="highly-availability-and-redundancy-through-wsfc"></a>Haute disponibilité et redondance via WSFC  
 Analysis Services peut être installé dans un cluster de basculement Windows Server (WSFC) existant pour obtenir une haute disponibilité qui restaure le service dans les plus brefs délais.  
  
 Les clusters de basculement fournissent un accès complet (lecture et écriture différée) à la base de données, mais un nœud à la fois. Les bases de données secondaires s’exécutent sur des nœuds supplémentaires dans le cluster, constituant des serveurs de remplacement si le premier nœud tombe en panne.  
  
 Le principal avantage du clustering de basculement est sa rapidité de récupération à la suite d’un échec du service. Cet avantage comporte toutefois certaines limites. Premièrement, si le basculement n’est jamais nécessaire, les ressources dédiées dans le cluster sont inactives. Deuxièmement, en cas de basculement, toutes les connexions sont perdues, entraînant la perte du travail non validé. La plupart des applications clientes doivent être en mesure de gérer cette situation ; souvent, l'activation du bouton de rafraîchissement de l'application retournera les résultats. 
 
 Quand vous envisagez un WSFC, gardez à l’esprit les points suivants :

- Actif/actif n’est pas pris en charge pour l’instant. Actif/passif (basculement) est la seule configuration WSFC prise en charge pour Analysis Services.
- En cas de mise en cluster d’Analysis Services, vérifiez que les nœuds participant au cluster s’exécutent sur du matériel identique ou très similaire, et que le contexte opérationnel de chaque nœud présente les mêmes caractéristiques : version du système d’exploitation et Service Packs, version d’Analysis Services et Service Packs (ou mises à jour cumulatives), ainsi que le mode serveur.
- Évitez de réaffecter un nœud passif comme nœud actif d’une autre charge de travail. Tous les gains à court terme en matière d’utilisation de l’ordinateur sont perdus en cas de basculement réel si le nœud ne peut pas gérer les deux charges de travail.
 
 Des instructions détaillées et des informations générales pour le déploiement d’Analysis Services dans un cluster de basculement sont fournies dans le livre blanc [How to Cluster SQL Server Analysis Services](https://msdn.microsoft.com/library/dn736073.aspx)(Comment mettre en cluster SQL Server Analysis Services). Bien que rédigées pour SQL Server 2012, ces instructions s’appliquent toujours aux versions plus récentes d’Analysis Services.  
  
## <a name="see-also"></a>Voir aussi  
 [Synchroniser des base de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)   
 [Forcer l’affinité NUMA pour les bases de données tabulaires Analysis Services](https://blogs.msdn.microsoft.com/sqlcat/2013/11/05/forcing-numa-node-affinity-for-analysis-services-tabular-databases/)   
 [Une étude de cas Analysis Services : Utilisation des modèles tabulaires dans une Solution commerciale à grande échelle](https://msdn.microsoft.com/library/dn751533.aspx)  
  
  
