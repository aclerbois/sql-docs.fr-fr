---
title: Exigences et considérations pour Analysis Services de déploiement | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8fc21b64be49a74359dcde41e10be5524bc1d9ab
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="requirements-and-considerations-for-analysis-services-deployment"></a>Configuration requise et considérations relatives au déploiement d'Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les performances et la disponibilité d'une solution dépendent de nombreux facteurs, notamment des capacités du matériel sous-jacent, de la topologie de votre déploiement de serveur, des caractéristiques de votre solution (présence, par exemple, de partitions distribuées sur plusieurs serveurs ou utilisation du stockage ROLAP qui requiert l'accès direct au moteur relationnel), des contrats de niveau de service et de la complexité de votre modèle de données.  
  
## <a name="memory-and-processor-requirements"></a>Besoins en mémoire et de traitement  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]nécessite plus de ressources processeur et de mémoire dans les cas suivants :  
  
-   Lors du traitement de cubes volumineux ou complexes. Ces cubes nécessitent plus de ressources mémoire et de traitement que les petits cubes et les cubes simples.  
  
-   Lorsque le nombre de cubes dans une base de données augmente.  
  
-   Lorsque le nombre de bases de données dans une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] augmente.  
  
-   Lorsque le nombre d'instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur un ordinateur augmente.  
  
-   Lorsque le nombre d'utilisateurs qui accèdent simultanément aux ressources [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] augmente.  
  
 La quantité de ressources mémoire et la capacité de traitement disponibles pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] varient en fonction de l'édition de SQL Server, du système d'exploitation, des capacités matérielles et selon que vous utilisez des processeurs virtuels ou physiques. Pour plus d'informations, suivez ces liens :  
  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
  
 [Limites de capacité de calcul par l'édition de SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)  
  
 [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md)  
  
 [Caractéristiques de capacité maximale &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/olap-physical/maximum-capacity-specifications-analysis-services.md)  
  
## <a name="disk-space-requirements"></a>Espace disque nécessaire  
 Différents éléments de l'installation [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et les tâches associées au traitement des objets nécessitent une quantité d'espace disque différente. Le tableau suivant décrit ces besoins.  
  
 Cubes  
 Les cubes comportant de grandes tables de faits nécessitent plus d'espace disque que les cubes composés de petites tables de faits. De même, à un moindre degré, les cubes qui disposent d'un grand nombre de dimensions volumineuses nécessitent plus d'espace disque que les cubes ayant un nombre moindre de membres de dimension. En règle générale, une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] nécessite environ 20 % de l'espace nécessaire aux mêmes données stockées dans la base de données relationnelle sous-jacente.  
  
 Aggregations  
 Les agrégations nécessitent un espace supplémentaire proportionnel aux agrégations ajoutées ; plus les agrégations sont nombreuses, plus il convient d'ajouter de l'espace. Si vous évitez de créer des agrégations inutiles, l'espace disque supplémentaire nécessaire aux agrégations ne doit généralement pas dépasser 10 % environ de la taille des données stockées dans la base de données relationnelle sous-jacente.  
  
 Exploration de données  
 Par défaut, les structures d'exploration de données placent dans le cache du disque le dataset utilisé pour leur apprentissage. Pour supprimer ces données du cache du disque, vous pouvez utiliser l'option de traitement **Traiter l'effacement de la structure** sur l'objet de structure d'exploration de données. Pour plus d’informations, consultez [Exigences et considérations concernant le traitement &#40;exploration de données&#41;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md).  
  
 Traitement des objets  
 Pendant le traitement, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] copie sur le disque les objets qu’il traite dans la transaction de traitement jusqu’à la fin du traitement. Une fois le traitement terminé, les copies traitées des objets remplacent les objets d'origine. Par conséquent, vous devez disposer d'un espace disque supplémentaire suffisant pour une seconde copie de chaque objet à traiter. Si, par exemple, vous envisagez de traiter l'ensemble d'un cube dans une seule transaction, vous devez disposer d'un espace disque suffisant pour pouvoir stocker une seconde copie de l'ensemble du cube.  
  
##  <a name="BKMK_Availability"></a> Considérations relatives à la disponibilité  
 Dans un environnement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , un cube ou un modèle d'exploration de données peut être indisponible pour effectuer des requêtes suite à une erreur matérielle ou logicielle. Un cube peut également être indisponible car il doit être traité.  
  
### <a name="providing-availability-in-the-event-of-hardware-or-software-failures"></a>Maintien de la disponibilité en cas d'erreurs matérielles ou logicielles  
 Le matériel ou le logiciel peut être défaillant pour diverses raisons. Toutefois, le maintien de la disponibilité de l'installation [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne se limite pas à éliminer la source des problèmes, mais il vise également à fournir des ressources alternatives qui permettent aux utilisateurs de continuer à utiliser le système en cas d'erreur. Le clustering des serveurs et l'équilibrage de charge des serveurs sont généralement utilisés pour fournir les ressources alternatives nécessaires au maintien de la disponibilité en cas d'erreur matérielle ou logicielle.  
  
 Pour maintenir la disponibilité en cas d'erreur matérielle ou logicielle, déployez [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans un cluster de basculement. Dans un cluster de basculement, le clustering [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows bascule vers un nœud secondaire en cas de défaillance du nœud principal ou s'il est nécessaire de le redémarrer. Après le basculement, qui s'effectue très rapidement, les utilisateurs accèdent à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui est exécutée sur le nœud secondaire lorsqu'ils exécutent des requêtes. Pour plus d'informations sur les clusters de basculement, consultez [Technologies Windows Server : clusters de basculement](http://technet.microsoft.com/library/cc732488\(v=WS.10\).aspx).  
  
 Une autre solution pour maintenir la disponibilité consiste à déployer le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur au moins deux serveurs de production. Vous pouvez utiliser la fonction d'équilibrage de la charge réseau des serveurs Windows pour placer les serveurs de production dans un seul cluster. Dans un cluster avec équilibrage de la charge réseau, le service d'équilibrage de la charge réseau envoie les requêtes des utilisateurs aux serveurs toujours disponibles lorsqu'un serveur du cluster devient indisponible à la suite d'une erreur matérielle ou logicielle.  
  
### <a name="providing-availability-while-processing-structural-changes"></a>Maintien de la disponibilité lors du traitement des modifications structurelles  
 Certaines modifications d'un cube peuvent rendre le cube indisponible jusqu'à ce qu'il soit retraité. Si, par exemple, vous apportez des modifications structurelles à une dimension d'un cube, les cubes qui utilisent la dimension modifiée doivent également être retraités, même si vous retraitez la dimension. Les utilisateurs ne peuvent pas interroger ces cubes, ni les modèles d'exploration de données basés sur un cube dont la dimension est modifiée jusqu'à ce que vous retraitiez les cubes.  
  
 Pour maintenir la disponibilité lorsque vous traitez des modifications structurelles qui peuvent affecter un ou plusieurs cubes d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , intégrez un serveur de test et utilisez l'Assistant Synchronisation de la base de données. Cette fonction permet de mettre à jour les données et les métadonnées sur un serveur de test, puis de synchroniser en ligne le serveur de production et le serveur de test. Pour plus d’informations, consultez [Synchroniser des bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md).  
  
 Pour traiter de manière transparente les mises à jour incrémentielles des données sources, activez la mise en cache proactive. La mise en cache proactive met à jour les cubes avec les nouvelles données sources sans avoir à procéder à un traitement manuel et sans affecter la disponibilité des cubes. Pour plus d’informations, consultez [Mise en cache proactive &#40;partitions&#41;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-proactive-caching.md).  
  
##  <a name="BKMK_Scalability"></a> Considérations relatives à la extensibilité  
 L'existence de plusieurs instances [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur un même ordinateur peut affecter les performances. Pour résoudre ce problème, vous pouvez augmenter les ressources de traitement, mémoire et disque sur le serveur. Toutefois, il se peut que vous deviez également faire monter en charge les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur plusieurs ordinateurs.  
  
### <a name="scaling-analysis-services-across-multiple-computers"></a>Montée en charge d'Analysis Services sur plusieurs ordinateurs  
 Vous pouvez procéder de différentes manières pour faire monter en charge une installation [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur plusieurs ordinateurs. Ces options sont décrites dans le tableau suivant.  
  
-   S'il existe plusieurs instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur un même ordinateur, vous pouvez transférer une ou plusieurs instances vers un autre ordinateur.  
  
-   S'il existe plusieurs bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur un même ordinateur, vous pouvez transférer une, ou plusieurs, base de données vers sa propre instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur un ordinateur distinct.  
  
-   Si une ou plusieurs bases de données relationnelles fournissent des données à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous pouvez transférer ces bases de données vers un ordinateur distinct. Avant de transférer les bases de données, tenez compte de la vitesse et de la bande passante réseau qui existent entre la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et les bases de données sous-jacentes. Si le réseau est lent ou encombré et que vous transférez les bases de données sous-jacentes vers un autre ordinateur, les performances s'en trouvent affectées.  
  
-   Si le traitement affecte les performances des requêtes et que vous ne pouvez pas traiter les données lors de la réduction de la charge des requêtes, transférez les tâches de traitement vers un serveur de test, puis synchronisez en ligne le serveur de production et le serveur de test. Pour plus d’informations, consultez [Synchroniser des bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md). Vous pouvez également distribuer le traitement sur plusieurs instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en utilisant des partitions distantes. Le traitement des partitions distantes utilise les ressources de traitement et mémoire du serveur distant et non celles de l'ordinateur local. Pour plus d’informations sur la gestion des partitions distantes, consultez [Créer et gérer une partition distante &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-remote-partition-analysis-services.md).  
  
-   Si les performances des requêtes sont dégradées et que vous ne pouvez pas augmenter les ressources de traitement et mémoire de l'ordinateur local, déployez un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur au moins deux serveurs de production. Ainsi, vous pouvez utiliser l'équilibrage de la charge réseau pour placer les serveurs dans un cluster. Dans un cluster avec équilibrage de la charge réseau, les requêtes sont distribuées automatiquement vers tous les serveurs du cluster avec équilibrage de la charge réseau.  
  
  
