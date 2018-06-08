---
title: Utilisateurs et groupes (Master Data Services) | Microsoft Docs
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
helpviewer_keywords:
- users [Master Data Services]
- groups [Master Data Services]
- users [Master Data Services], about users
- groups [Master Data Services], about groups
ms.assetid: ed08dd2d-248e-4b68-91d4-e9961cb50eed
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: c754dee32b5ab9046d60f42d0b47dc15fdf9893c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="users-and-groups-master-data-services"></a>Utilisateurs et groupes (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Pour accéder à l'application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , un utilisateur doit disposer d'un compte de domaine Windows ou d'un compte sur le serveur où [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] est installé. Pour accorder l'accès à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , vous pouvez, au choix :  
  
-   Ajouter le compte d'utilisateur à un domaine ou à un groupe local, puis ajouter le groupe à la liste des groupes dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   Ajouter le compte d'utilisateur à la liste des utilisateurs dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
    > [!NOTE]  
    >  Lorsqu’un utilisateur appartient à un groupe qui a accès à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], le nom de l’utilisateur est ajouté automatiquement à la liste la première fois où l’utilisateur accède à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] ou à MDS [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)].  
  
 Pour agir dans la zone fonctionnelle **Explorateur** de l'interface utilisateur, le groupe ou l'utilisateur doit avoir accès à la zone fonctionnelle **Explorateur** et disposer d'une autorisation sur les objets modèle.  
  
 Si un utilisateur ou un groupe a besoin d’un accès à d’autres zones fonctionnelles, l’utilisateur ou le groupe doit se voir attribuer un accès à ladite zone fonctionnelle.  
  
## <a name="best-practice"></a>Meilleure pratique  
 Pour simplifier l'administration, créez des groupes et affectez à chaque groupe l'autorisation sur des zones fonctionnelles et objets modèles. Vous pouvez ensuite ajouter et supprimer des utilisateurs dans les groupes sans accéder à l'interface utilisateur de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
 N'affectez pas d'autorisations supplémentaires à un utilisateur individuel et n'incluez pas un utilisateur dans plusieurs groupes qui ont accès à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]. De plus, n'utilisez pas d'autorisations des membres de la hiérarchie à moins que vous ne souhaitiez qu'un groupe ait un accès limité à des membres spécifiques.  
  
## <a name="see-also"></a> Voir aussi  
 [Ajouter un utilisateur &#40;Master Data Services&#41;](../master-data-services/add-a-user-master-data-services.md)   
 [Ajouter un groupe &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)   
 [Supprimer des utilisateurs ou des groupes &#40;Master Data Services&#41;](../master-data-services/delete-users-or-groups-master-data-services.md)   
 [Tester les autorisations d’un utilisateur &#40;Master Data Services&#41;](../master-data-services/test-a-user-s-permissions-master-data-services.md)  
  
  
