---
title: Base de données Master Data Services | Microsoft Docs
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
- database [Master Data Services], about the database
- database [Master Data Services]
ms.assetid: 5f590cc1-6ec2-4b8c-a598-03de0f6051a0
caps.latest.revision: 6
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: a03e89095ec813cf522059a6ad00e2d34dd94ed0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="master-data-services-database"></a>Base de données Master Data Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  La base de données contient toutes les informations pour le système [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Elle est au centre d’un déploiement [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . La base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] :  
  
-   stocke les paramètres, objets de base de données et données requis par le système [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ;  
  
-   contient des tables intermédiaires utilisées pour traiter les données des systèmes sources ;  
  
-   fournit un schéma et des objets de base de données pour stocker les données de référence de systèmes sources ;  
  
-   prend en charge les fonctionnalités de contrôle de version, notamment la validation des règles d'entreprise et les notifications par courrier électronique ;  
  
-   fournit des vues pour les systèmes d'abonnement qui doivent récupérer des données de la base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Table de mise en lots des membres feuilles &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Table de mise en lots des membres consolidés &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Table de mise en lots des relations &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md)  
  
-   [Erreurs du processus de mise en lots &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Créer une base de données Master Data Services](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Sécurité de l’objet de base de données &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)   
 [Connexions, utilisateurs et rôles de base de données &#40;Master Data Services&#41;](../master-data-services/database-logins-users-and-roles-master-data-services.md)  
  
  
