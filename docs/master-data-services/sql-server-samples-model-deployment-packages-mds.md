---
title: 'Exemples SQL Server : packages de déploiement de modèle (MDS) | Microsoft Docs'
ms.custom: ''
ms.date: 07/28/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords:
- master data services
- sample
ms.assetid: 9b31b7b6-319b-4840-b67d-eb383e7762b1
caps.latest.revision: 21
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 277ff7e499e64ee9c88ce452dae2c06d54818284
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-examples-model-deployment-packages-mds"></a>Exemples SQL Server : packages de déploiement de modèle (MDS)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Des exemples de packages de modèles contenant des données sont inclus avec l’installation de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. L’emplacement par défaut de ces packages est \<lecteur>\Program Files\Microsoft SQL Server\130\Master Data Services\Samples\Packages.  
  
 Pour obtenir des instructions sur la façon de déployer les exemples de packages de modèle, consultez [Déployer les exemples de modèles et de données](../master-data-services/master-data-services-installation-and-configuration.md#deploySample). Vous déployez les exemples de packages de package à l’aide de [l’outil MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
> [!IMPORTANT]  
>  **Mises à jour des exemples dans [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]**  
>   
>  Les exemples de packages ont été mis à jour pour prendre en charge les nouvelles fonctionnalités suivantes.  
>   
>  -   Afficher les relations plusieurs-à-plusieurs.  
>   
>      Pour plus d’informations, consultez [Relation M2M dans l’exemple de modèle](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md#M2MSample).  

> -   Valeurs limite autorisées pour les attributs basés sur un domaine.  
>   
>      Pour plus d’informations, consultez [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md).  
> -   Demander l’approbation des modifications de l’entité.  
>   
>      Pour plus d’informations, consultez [Approbation requise &#40;Master Data Services&#41;](../master-data-services/approval-required-master-data-services.md).  
> -   Utiliser les opérateurs Not et Else dans les règles d’entreprise  
>   
>      Pour plus d’informations, consultez [Exemples de règles d’entreprise](../master-data-services/business-rule-examples-master-data-services.md).  
> -   Implémenter un index personnalisé  
>   
>      Pour plus d’informations, consultez [Index personnalisé &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md).  
 

 
 Dans Master Data Services, un package est un fichier XML qui contient une structure de modèle déployable et, éventuellement, les données du modèle. Utilisez les packages de modèle pour déplacer des copies de modèles d’un environnement MDS vers un autre, ou pour créer de nouveaux modèles dans votre environnement [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] existant.  
  
## <a name="see-also"></a> Voir aussi  
 [Déployer un package de déploiement de modèle à l’aide de MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
  
