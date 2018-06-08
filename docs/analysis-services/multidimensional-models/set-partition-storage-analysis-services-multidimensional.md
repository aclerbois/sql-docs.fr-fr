---
title: Définir le stockage de Partition (Analysis Services - multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3410a8b26b9b9e26046a39f8ed5250ae9b82e67d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="set-partition-storage-analysis-services---multidimensional"></a>Définir un stockage de partitions (Analysis Services - Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit plusieurs configurations de stockage standard pour les modes de stockage et les options de mise en cache. Celles-ci fournissent des configurations fréquemment utilisées pour la notification des mises à jour, la latence et la reconstruction des données.  
  
 Vous pouvez spécifier le stockage de partitions dans l'onglet Partitions du cube dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ou dans la page de propriétés de partition de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="guidelines-for-choosing-a-storage-mode"></a>Instructions pour le choix d'un mode de stockage  
 Pour un groupe de mesures important, il est courant de configurer le stockage différemment pour les différentes partitions. Respectez les consignes suivantes :  
  
-   Utiliser le mode ROLAP en temps réel pour les données actuelles qui sont mises à jour en continu.  
  
-   Utiliser la mise en cache proactive avec une latence faible ou une latence moyenne pour les partitions basées sur des sources de données qui sont mises à jour moins souvent.  
  
-   Utiliser le mode MOLAP automatique pour les sources de données dont les utilisateurs requièrent de hautes performances mais peuvent supporter une certaine latence des données.  
  
-   Utiliser le mode MOLAP planifié pour les sources de données pour lesquelles les utilisateurs doivent être en mesure d'accéder en permanence aux données mais voient les modifications seulement périodiquement.  
  
-   Utiliser le stockage MOLAP sans mise en cache proactive pour les partitions qui changent rarement ou jamais, pour les partitions pour lesquelles les utilisateurs n'ont pas besoin de parcourir les données les plus récentes, et si les données ne doivent pas être constamment disponibles pour les utilisateurs, au cours de tous les traitements et mises à jour nécessaires.  
  
 Il s'agit là de consignes générales et une analyse et des tests précis peuvent être nécessaires pour développer le meilleur plan de stockage possible pour vos données. Vous pouvez également configurer manuellement les paramètres de stockage pour une partition si aucune des configurations standard ne répond à vos besoins.  
  
## <a name="storage-settings-descriptions"></a>Descriptions des paramètres de stockage  
  
|Paramétrage standard de stockage|Description|  
|------------------------------|-----------------|  
|ROLAP en temps réel|OLAP est en temps réel. Les données de détail et les agrégations sont stockées dans un format relationnel. Le serveur est à l'écoute des notifications lorsque des données changent et que toutes les requêtes reflètent l'état actuel des données (latence nulle).<br /><br /> Ce paramétrage doit généralement être utilisé pour une source de données avec des mises à jour très fréquentes et continues, lorsque les utilisateurs requièrent toujours les données les plus récentes. En fonction des types des requêtes générées par les applications clientes, cette méthode est responsable des temps de réponse les plus longs.|  
|HOLAP en temps réel|OLAP est en temps réel. Les données de détail sont stockées dans un format relationnel, tandis que les agrégations sont stockées dans un format multidimensionnel. Le serveur est à l'écoute des notifications lorsque des données changent et il actualise les agrégations OLAP multidimensionnel (MOLAP), selon les besoins. Aucun cache MOLAP n'est créé. À chaque mise à jour de la source de données, le serveur bascule en mode OLAP relationnel (ROLAP) en temps réel, jusqu'à ce que les agrégations soient actualisées. Toutes les requêtes reflètent l'état actuel des données (latence nulle).<br /><br /> Ce paramétrage doit généralement être utilisé pour une source de données avec mises à jour fréquentes et continues (mais pas assez fréquentes pour requérir le mode ROLAP en temps réel) et dont les utilisateurs requièrent toujours les données les plus récentes. Cette méthode fournit normalement de meilleures performances globales que le stockage ROLAP. Les utilisateurs peuvent obtenir des performances MOLAP grâce à ce paramétrage, si la source de données demeure silencieuse assez longtemps.|  
|MOLAP de latence faible|Les données de détail et les agrégations sont stockées dans un format multidimensionnel. Le serveur est à l'écoute des notifications de modification des données et bascule en mode ROLAP en temps réel, alors que les objets MOLAP sont traités de nouveau dans un cache. Un intervalle de latence d'au moins 10 secondes est requis avant la mise à jour du cache. Il existe une valeur de remplacement de l’intervalle de latence de 10 minutes, si l'intervalle de latence n'est pas atteint. Le traitement s’effectue automatiquement pendant les modifications de données avec une latence de trente minutes sur la cible, après la première modification.<br /><br /> Ce paramétrage doit généralement être utilisé pour une source de données avec mises à jour fréquentes, lorsque les performances des requêtes sont un peu plus importantes que le fait de fournir toujours les données les plus récentes. Ce paramétrage entraîne le traitement automatique des objets MOLAP chaque fois qu'ils sont requis après l'intervalle de latence. Les performances sont plus lentes lorsque les objets MOLAP sont traités de nouveau.|  
|MOLAP de latence moyenne|Les données de détail et les agrégations sont stockées dans un format multidimensionnel. Le serveur est à l’écoute des notifications de modification des données et bascule en mode ROLAP en temps réel, alors que les objets MOLAP sont traités de nouveau dans le cache. Un intervalle de latence d'au moins 10 secondes est requis avant la mise à jour du cache. Il existe une valeur de remplacement de l’intervalle de latence de 10 minutes, si l'intervalle de latence n'est pas atteint. Le traitement s'effectue automatiquement lors de modifications de données avec une latence de quatre heures sur la cible.<br /><br /> Ce paramétrage est généralement utilisé pour une source de données avec mises à jour fréquentes (ou moins fréquentes), lorsque les performances des requêtes sont plus importantes que le fait de fournir toujours les données les plus récentes. Ce paramétrage entraîne le traitement automatique des objets MOLAP chaque fois qu'ils sont requis après l'intervalle de latence. Les performances sont plus lentes lorsque les objets MOLAP sont traités de nouveau.|  
|MOLAP automatique|Les données de détail et les agrégations sont stockées dans un format multidimensionnel. Le serveur est à l'écoute des notifications mais conserve le cache MOLAP actuel alors qu'il en construit un nouveau. Le serveur ne bascule jamais en mode OLAP en temps réel et les requêtes peuvent être obsolètes alors que le nouveau cache est en cours de construction.<br /><br /> Un intervalle de latence d'au moins 10 secondes est requis avant la création du nouveau cache MOLAP. Il existe une valeur de remplacement de l’intervalle de latence de 10 minutes, si l'intervalle de latence n'est pas atteint. Le traitement s'effectue automatiquement lors de modifications de données avec une latence de deux heures sur la cible.<br /><br /> Ce paramétrage est généralement utilisé pour une source de données lorsque les performances des requêtes ont une importance stratégique. Ce paramétrage entraîne le traitement automatique des objets MOLAP chaque fois qu'ils sont requis après l'intervalle de latence. Les requêtes ne retournent pas les données les plus récentes alors que le nouveau cache est en cours de construction et de traitement.|  
|MOLAP planifié|Les données de détail et les agrégations sont stockées dans un format multidimensionnel. Le serveur ne reçoit pas de notification lorsque les données sont modifiées. Le traitement s’effectue automatiquement toutes les 24 heures.<br /><br /> Ce paramétrage est généralement utilisé pour une source de données lorsque seules des mises à jour quotidiennes sont requises. Les requêtes sont toujours exécutées sur les données du cache MOLAP, lesquelles ne sont pas supprimées tant qu'un nouveau cache n'est pas construit et que ses objets ne sont pas traités.|  
|MOLAP|La mise en cache proactive n'est pas activée. Les données de détail et les agrégations sont stockées dans un format multidimensionnel. Le serveur ne reçoit pas de notification lorsque les données sont modifiées. Le traitement doit être planifié ou effectué manuellement.<br /><br /> Ce paramétrage est généralement utilisé pour une source de données dans laquelle des mises à jour régulières ne sont pas nécessaires pour les applications clientes, mais pour laquelle de hautes performances sont essentielles.<br /><br /> Le stockage MOLAP sans mise en cache proactive fournit les meilleures performances possibles si vos applications ne requièrent pas les données les plus récentes. Il requiert une certaine indisponibilité pour le traitement des objets mis à jour, bien que les temps d'arrêt puissent être réduits au minimum par la mise à jour et le traitement des cubes sur un serveur de test et l'utilisation de la synchronisation des bases de données pour copier les objets MOLAP mis à jour et traités sur le serveur de production.|  
  
## <a name="custom-storage-options"></a>Options de stockage personnalisées  
 Au lieu d'utiliser l'un des paramètres de stockage standard, vous pouvez configurer manuellement le stockage et la mise en cache proactive. Avant de créer des paramètres de stockage personnalisés, vous pouvez d’abord cliquer sur l’option **Paramètres standard** et déplacer le curseur sur le paramètre standard qui se rapproche le plus de la configuration que vous voulez utiliser. Ensuite, pour créer une configuration personnalisée, cliquez sur l’option **Paramètres personnalisés** , puis sur **Options**.  
  
-   Vous pouvez spécifier si les modifications effectuées dans une source de données déclenchent la mise à jour du cache. Pour atteindre un niveau d'actualisation raisonnable, vous pouvez spécifier un intervalle de latence minimal après les mises à jour de la source de données. Vous pouvez également spécifier une valeur de remplacement de l’intervalle de latence qui met à jour le cache après une période spécifiée si l'intervalle entre les modifications de la source de données n'atteint jamais la valeur minimale.  
  
-   Vous pouvez spécifier si le cache obsolète doit être supprimé lors des mises à jour. Si vous sélectionnez cette option, lorsque la latence spécifiée est dépassée, le serveur passe en mode ROLAP (OLAP relationnel) en temps réel lors de la mise à jour du cache. Si vous ne sélectionnez pas cette option, le serveur continue à interroger le cache MOLAP (OLAP multidimensionnel) périmé lors de la génération du nouveau cache.  
  
     Vous pouvez spécifier l'intervalle de latence nécessaire entre les modifications et la suppression d'un cache obsolète. Cet intervalle correspond à la durée pendant laquelle les utilisateurs peuvent continuer à parcourir les données d'un cache obsolète avant que celui-ci ne soit supprimé. Si des modifications sont effectuées et que le cache est toujours en cours de mise à jour ou de traitement à la fin de cet intervalle, les requêtes sont redirigées vers ROLAP.  
  
-   Vous pouvez planifier des mises à jour forcées du cache si vous voulez mettre régulièrement à jour les objets MOLAP en cache, indépendamment des modifications de la source de données. Les avantages d'OLAP en temps réel varient en fonction de la taille de votre base de données et de la période de latence affectée par la fréquence des modifications des données sources. Vous avez intérêt à ce que les utilisateurs envoient aussi souvent que possible des requêtes vers un cache, et non vers ROLAP.  
  
 Si vous cochez la case **Appliquer les paramètres aux dimensions** , les mêmes paramètres de stockage sont appliqués aux dimensions associées au groupe de mesures. Les valeurs des dimensions sont initialement les mêmes que les valeurs des partitions.  
  
## <a name="see-also"></a>Voir aussi  
 [Partitions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/partitions-in-multidimensional-models.md)  
  
  
