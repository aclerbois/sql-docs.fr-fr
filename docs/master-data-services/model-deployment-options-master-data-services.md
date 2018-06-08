---
title: Options de déploiement de modèle (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: cf1b17b4-47d5-4eba-83f9-fb0555806867
caps.latest.revision: 6
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 82bdae63648dcd579090b7fa418709a6c48258be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="model-deployment-options-master-data-services"></a>Options de déploiement de modèle (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], lorsque vous déployez un fichier de package de modèle, vous devez décider soit de déployer un modèle nouveau ou cloné, soit de mettre à jour un modèle précédemment cloné.  
  
## <a name="workflows"></a>Flux de travail  
 Lorsque vous travaillez avec des packages de modèle, il existe deux flux de travail principaux.  
  
-   Créez un package d'un modèle dans un environnement de test et déployez un clone du modèle dans un environnement de production. Au fil du temps, déployez les mises à jour de l'environnement de test vers l'environnement de production.  
  
-   Créez un package d'un modèle et déployez-le en tant que nouveau modèle dans le même environnement. Dans ce cas, vous devez attribuer un nouveau nom au modèle.  
  
## <a name="options"></a>Options  
 Dans la base de données MDS, chaque objet de modèle a un identificateur (ID) unique. Ces ID sont inclus dans les packages de déploiement de modèle. Lorsque vous déployez le package, vous devez choisir comment utiliser ces ID.  
  
 Le tableau suivant doit vous aider à déterminer quel choix opérer lors du déploiement d'un modèle à l'aide de l'Assistant Déploiement de modèle de l'administration de système ou de l'outil MDSModelDeploy.  
  
|Option|Description|Remarques|  
|------------|-----------------|-----------|  
|Nouvelle|Crée un nouveau modèle avec un nom unique. Les nouveaux identificateurs sont créés pour tous les objets de modèle.|Si vous créez un modèle avec de nouveaux identificateurs, vous ne pouvez pas utiliser les outils de déploiement de modèle pour appliquer des mises à jour ultérieures au modèle. Si vous utilisez l'Assistant de l'application Web pour déployer un package de modèle, vous avez la possibilité de créer un modèle uniquement s'il existe déjà un modèle portant le même nom ou le même ID.|  
|Clone|Crée un modèle qui est un clone exact du modèle dans le package. Cela fonctionne uniquement si le modèle (nom ou identificateur) n'existe pas dans l'environnement cible. Utilisez Clone lorsque vous souhaitez utiliser le même modèle dans plusieurs environnements et mettre à jour le modèle cloné au fil du temps.|Il s'agit du comportement par défaut de l'Assistant dans l'application Web. S'il existe déjà un modèle avec le même nom ou le même ID, vous êtes invité à créer un modèle à la place.|  
|Update|Met à jour un modèle existant avec le modèle dans le package. Les identificateurs doivent être identiques dans les deux modèles. Cela permet de mettre à jour un modèle que vous avez précédemment cloné.|Vous pouvez uniquement mettre à jour les modèles qui ont été précédemment clonés. (Les noms et les ID doivent correspondre.)|  
  
## <a name="see-also"></a> Voir aussi  
 [Déployer un package de déploiement de modèle à l'aide de MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)   
 [Déployer un package de déploiement de modèle à l’aide de l’Assistant](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)   
 [Déploiement de modèles &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
  
