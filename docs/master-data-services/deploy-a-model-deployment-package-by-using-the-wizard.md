---
title: Déployer un package de déploiement de modèle à l’aide de l’Assistant | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- deployment packages [Master Data Services], deploying
- models [Master Data Services], deploying a package
ms.assetid: 4f65dc60-0ff8-46e6-9988-5bc5b9603ad0
caps.latest.revision: 16
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: d1e0be14ede33fe4867d2d554572cb56d2d8f7fc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploy-a-model-deployment-package-by-using-the-wizard"></a>Déployer un package de déploiement de modèle à l'aide de l'Assistant

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Utilisez l'Assistant Déploiement de modèle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] pour déployer les packages qui contiennent uniquement des objets de modèle. Si vous avez besoin de déployer un package avec des données, consultez [Déployer un package de déploiement de modèle à l’aide de MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
> [!IMPORTANT]  
>  Les packages peuvent être déployés uniquement dans l'édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans laquelle ils ont été créés. Cela signifie que les packages créés dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] ne peuvent pas être déployés sur [!INCLUDE[ssSQL14](../includes/sssql14-md.md)].  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** dans l'environnement [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] cible.  
  
-   Un package de déploiement de modèle doit exister. Pour plus d’informations, consultez [Créer un package de déploiement de modèle à l’aide de l’Assistant](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md).  
  
-   Vous devez être administrateur dans l'environnement où vous déployez le modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-deploy-a-model-deployment-package-of-model-objects-only"></a>Pour déployer un package de déploiement de modèle d'objets de modèle uniquement  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Vue du modèle** , dans la barre de menus, pointez sur **Système** , puis cliquez sur **Déploiement**.  
  
3.  Dans l' **Assistant Déploiement de modèle**, cliquez sur **Déployer**.  
  
4.  Cliquez sur **Parcourir**.  
  
5.  Recherchez votre package de déploiement (fichier .pkg), puis cliquez sur **Ouvrir**.  
  
6.  Cliquez sur **Suivant**.  
  
7.  Une fois le package chargé, cliquez sur **Suivant**.  
  
8.  Si le modèle existe déjà, vous pouvez le mettre à jour en sélectionnant **Mettre à jour le modèle existant**. Pour créer un modèle, sélectionnez **Créer un modèle** , cliquez sur **Suivant** , puis tapez un nom pour le nouveau modèle.  
  
9. Cliquez sur **Terminer** pour quitter l'Assistant.  
  
 **Remarques :**  
  
-   Si une vue d’abonnement dans le package a le même nom qu’une vue d’abonnement dans un modèle existant, l’avertissement suivant s’affiche : **La vue d’abonnement du système de déploiement a été renommée**. Par ailleurs, la vue est créée en tant que *modelname.subscriptionviewname*. Si ce nom existe déjà, la vue d'abonnement n'est pas créée.  
  
-   Le processus de déploiement comporte quatre étapes :  
  
    1.  Les objets de modèle sont créés.  
  
    2.  Les vues d'abonnement sont créées.  
  
    3.  Les règles d'entreprise sont créées.  
  
-   Lorsque vous créez un modèle nouveau ou cloné, si le processus échoue au cours d'une étape, le modèle est supprimé.  
  
     Lorsque vous mettez à jour un modèle, si le processus échoue au cours des trois premières étapes, il s'arrête ; toutefois, les modifications qui sont déjà effectuées ne sont pas annulées.  
  
## <a name="next-steps"></a>Next Steps  
 Les attributs de fichier et les autorisations d’accès ne sont pas inclus dans les packages de déploiement de modèle. Après avoir déployé un modèle, vous devez les mettre à jour manuellement. Pour plus d'informations, consultez :  
  
-   [Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Déploiement de modèles &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
  
