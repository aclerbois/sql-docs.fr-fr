---
title: Afficher les propriétés de Resource Governor | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.rg.properties.f1
helpviewer_keywords:
- Resource Governor, properties
ms.assetid: de3510df-f792-4a9d-80fa-f198fd36cdc8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f345af10cd9f76b12e7f9b6e4871b11d4f953771
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="view-resource-governor-properties"></a>Afficher les propriétés du gouverneur de ressources
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Vous pouvez créer ou configurer des entités de Resource Governor, telles que des pools de ressources et des groupes de charge de travail, en utilisant la page Propriétés de Resource Governor dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 ##  <a name="BeforeYouBegin"></a> Rubriques connexes 
 Outre la consultation des propriétés des entités de Resource Governor, vous pouvez effectuer plusieurs tâches de configuration à l'aide de la page **ropriétés de Resource Governor** . Pour plus d'informations, consultez ces rubriques :  
  
-   [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)  
  
-   [Désactiver Resource Governor](../../relational-databases/resource-governor/disable-resource-governor.md)  
  
-   [Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)  
  
-   [Créer un groupe de charge de travail](../../relational-databases/resource-governor/create-a-workload-group.md)  
  
-   [Modifier les paramètres de pool de ressources](../../relational-databases/resource-governor/change-resource-pool-settings.md)  
  
-   [Modifier les paramètres de groupe de charge de travail](../../relational-databases/resource-governor/change-workload-group-settings.md)  
  
-   [Déplacer un groupe de charge de travail](../../relational-databases/resource-governor/move-a-workload-group.md)  
  
 Lorsque vous cliquez sur **OK** après avoir ajouté, supprimé ou déplacé un groupe de charge de travail ou un pool de ressources, l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE s'exécute.  
  
 Si l'opération de création ou de reconfiguration du pool de ressources ou du groupe de charge de travail échoue, un message d'erreur récapitulatif apparaît sous le titre de la page de propriétés. Pour consulter le message d'erreur détaillé, cliquez sur la flèche vers le bas du message d'erreur.  
  
 Vous pouvez déterminer s’il existe une configuration en attente en interrogeant la vue de gestion dynamique [sys.dm_resource_governor_configuration](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md) pour obtenir l’état en cours d’is_configuration_pending.  
  
##  <a name="Permissions"></a> Permissions  
 L'affichage des propriétés de Resource Governor nécessite l'autorisation VIEW SERVER STATER. Les tâches de configuration de Resource Governor nécessitent l'autorisation CONTROL SERVER.  
  
##  <a name="ViewRGProp"></a> Page Propriétés de Resource Governor  
 **Pour afficher les propriétés de Resource Governor à l’aide de la page Propriétés de Resource Governor dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez l'Explorateur d'objets et développez de manière récursive le nœud **Gestion** jusqu'au **Resource Governor**.  
  
2.  Cliquez avec le bouton droit sur **Resource Governor** , puis sélectionnez **Propriétés**afin d’ouvrir la page **Propriétés de Resource Governor** .  
  
3.  Pour obtenir les descriptions des champs de la page, consultez [Propriétés de Resource Governor](#RGProp).  
  
4.  Cliquez sur **OK**pour enregistrer les éventuelles modifications.  
  
##  <a name="RGProp"></a> Resource Governor properties  
 **Nom de la fonction classifieur**  
 Spécifiez la fonction classifieur en la sélectionnant dans la liste.  
  
 **Activer Resource Governor**  
 Activez ou désactivez Resource Governor en activant ou désactivant la case à cocher.  
  
 **Pools de ressources**  
 Créez ou modifiez la configuration des pools de ressources et des pools de ressources externes à l’aide de la grille fournie. Cette grille est remplie à l'aide des informations des pools internes et par défaut prédéfinis. Sélectionnez un pool à utiliser en cliquant sur la première colonne de la ligne du pool. Pour créer un pool de ressources, cliquez sur la ligne comportant le préfixe**\***(astérisque).  
  
 **Nom**  
 Spécifiez le nom du pool de ressources.  
  
 **% processeur minimal**  
 Spécifiez la bande passante de l'UC moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention de l'UC. La plage est comprise entre 0 et 100.  
  
 **% processeur maximal**  
 Spécifiez la bande passante de l'UC moyenne maximale que toutes les demandes dans ce pool de ressources recevront en cas de contention de l'UC. La plage est comprise entre 0 et 100. La valeur par défaut est 100.  
  
 **% mémoire minimal**  
 Spécifiez la quantité de mémoire minimale réservée à ce pool de ressources qui ne peut pas être partagée avec d'autres pools de ressources. La plage est comprise entre 0 et 100.  
  
 **% mémoire maximal**  
 Spécifiez la mémoire totale du serveur qui peut être utilisée par les demandes dans ce pool de ressources. La plage est comprise entre 0 et 100. La valeur par défaut est 100.  
  
 Pour plus d’informations, consultez [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md) et [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md).  
  
 **Groupes de charge de travail pour le pool de ressources**  
 Créez ou modifiez la configuration du groupe de charges de travail à l'aide de la grille fournie. Cette grille est remplie à l'aide des informations des groupes internes et par défaut prédéfinis. Sélectionnez un groupe à utiliser en cliquant sur la première colonne de la ligne du pool. Pour créer un groupe de charges de travail, cliquez sur la ligne comportant le préfixe**\***(astérisque).  
  
 **Nom**  
 Spécifiez le nom du groupe de charges de travail.  
  
 **Importance**  
 Spécifiez l'importance relative d'une demande dans le groupe de charges de travail. Les paramètres disponibles sont Faible, Moyenne et Haute.  
  
 **Demandes maximales**  
 Spécifiez le nombre maximal de demandes simultanées autorisées à s'exécuter dans le groupe de charges de travail. Doit être égal à 0 ou un entier positif.  
  
 **Temps processeur (s)**  
 Spécifiez la quantité de temps maximale de temps processeur qu'une demande peut utiliser. Doit être égal à 0 ou un entier positif. Si 0, le temps est illimité.  
  
 **% allocation mémoire**  
 Spécifiez la quantité de mémoire maximale qu'une demande unique peut prendre du pool. La plage est comprise entre 0 et 100.  
  
 **Délai d'expiration de l'allocation (s)**  
 Spécifiez le délai maximal pendant lequel une requête peut attendre qu'une ressource devienne disponible avant d'échouer. Doit être égal à 0 ou un entier positif.  
  
 **Degré de parallélisme**  
 Spécifiez le degré maximal de parallélisme (DOP) pour les demandes parallèles. La plage est comprise entre 0 et 64.  
  
 Pour plus d’informations, consultez [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md).  
  
## <a name="view-resource-governor-properties-using-transact-sql"></a>Afficher les propriétés de Resource Governor à l’aide de Transact-SQL  
 **Afficher les propriétés de Resource Governor à l'aide de Transact-SQL**  
  
1.  Pour afficher les définitions des entités de Resource Governor, utilisez les [Affichages catalogue de Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md).  
  
2.  Pour consulter la configuration actuelle des entités de Resource Governor, utilisez les [Vues de gestion dynamique liées à Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md).  
  
## <a name="more-information"></a>Informations complémentaires
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Fonction classifieur de Resource Governor](../../relational-databases/resource-governor/resource-governor-classifier-function.md)  
  
  
