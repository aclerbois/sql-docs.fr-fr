---
title: Collections (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services]
- collections [Master Data Services], about collections
ms.assetid: 5aa1d1e0-b4e5-4897-8e74-01dcf418df73
caps.latest.revision: 14
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 55562a106650890b5fff491e00a5e23c62e878f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="collections-master-data-services"></a>Collections (services de données de référence)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Une collection est un groupe de membres feuille et de membres consolidés d'une entité unique. Utilisez des collections lorsque vous n'avez pas besoin d'une hiérarchie complète et que vous souhaitez afficher des regroupements différents de vos membres pour la création de rapports ou l'analyse, ou lorsque vous devez créer une taxonomie.  
  
> [!NOTE]  
>  Les collections sont déconseillées.  
  
## <a name="what-collections-contain"></a>Contenu des collections  
 Les collections ne limitent pas le nombre ou le type de membres que vous pouvez inclure, tant que les membres se trouvent dans la même entité. Une collection peut contenir des membres feuille et consolidés de plusieurs hiérarchies explicites obligatoires et non obligatoires.  
  
 Lorsque vous créez une collection, vous ne créez pas une structure hiérarchique ; vous créez une liste plate de membres. Lorsque vous sélectionnez un nœud d'une hiérarchie et l'ajouter à la collection, le membre consolidé sélectionné est le seul membre ajouté à la collection.  
  
 Une collection peut également contenir d'autres collections. Vous pouvez utiliser des collections de collections pour créer des taxonomies.  
  
 Lorsque vous créez une collection, vous apparaissez automatiquement comme propriétaire. Si vous êtes administrateur, vous pouvez créer d'autres attributs pour votre collection si nécessaire.  
  
## <a name="subscription-views-for-collections"></a>Vues d'abonnement pour les collections  
 Il existe deux types de vues d'abonnement qui affichent des collections. Le format **Attributs de collection** affiche la liste des collections et des attributs associés aux collections (comme la description ou le propriétaire). Le format **Collections** affiche tous les membres de toutes les collections, ainsi que chaque poids et ordre de tri des membres. Pour plus d’informations, consultez [Vue d’ensemble : exportation de données &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md).  
  
 Si vous définissez des valeurs de pondération pour des membres spécifiques d'une collection, ces valeurs sont disponibles dans les vues associées d'abonnement.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer une collection.|[Créer une collection &#40;Master Data Services&#41;](../master-data-services/create-a-collection-master-data-services.md)|  
|Ajouter des membres à une collection existante.|[Ajouter des membres à une collection &#40;Master Data Services&#41;](../master-data-services/add-members-to-a-collection-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [Vue d’ensemble : exportation de données &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)  
  
  
