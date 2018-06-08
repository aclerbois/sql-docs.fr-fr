---
title: Administrateurs (Master Data Services) | Microsoft Docs
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
- administrators [Master Data Services], about administrators
- administrators [Master Data Services]
- models [Master Data Services], administrators
ms.assetid: d330aa4e-6ade-4b09-b376-1b15d6c78f7d
caps.latest.revision: 14
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: c1a75f5d3c6f4a9f9eadcec5ad6f48e809f9f3e1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="administrators-master-data-services"></a>Administrateurs (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cet article décrit les types d’administrateurs dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]: administrateurs de modèle, administrateurs d’entité et super utilisateur.  
  
## <a name="model-administrators"></a>Administrateurs de modèle  
 Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], un administrateur de modèle est un utilisateur qui a l’autorisation **d’administrateur** sur l’objet modèle de niveau supérieur sous l’onglet **Objets de modèle** . Quand un utilisateur a une autorisation d’administrateur sur un modèle particulier, toutes les autres autorisations sur les objets enfants du modèle (à la fois les autorisations de membre et d’objet de modèle) sont ignorées et remplacées par l’autorisation **d’administrateur** du modèle.  
  
-   Si l'utilisateur a accès à la zone fonctionnelle **Explorateur** , il peut ajouter, supprimer et mettre à jour toutes les données de référence dans cette zone.  
  
-   Si l'utilisateur a accès à d'autres zones fonctionnelles, il peut effectuer toutes les tâches d'administration disponibles dans la zone fonctionnelle.  
  
 Chaque modèle peut avoir plusieurs administrateurs. Chaque utilisateur peut être un administrateur de modèle pour un, plusieurs ou tous les modèles du déploiement [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 Un utilisateur peut être configuré en tant qu'administrateur de modèle dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] ou par programme. Pour plus d’informations, consultez [Créer un administrateur de modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md).  
  
## <a name="entity-administrators"></a>Administrateurs d’entité  
 Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], un administrateur d’entité est un utilisateur qui a des autorisations d’administrateur sur l’objet entité, sous l’onglet Objets de modèle. Lorsqu’un utilisateur a des autorisations d’administrateur sur une entité, toutes les autres autorisations sur les objets enfants de l’entité (à la fois les autorisations de membre et d’objet de modèle) sont ignorées et remplacées par les autorisations d’administrateur.  
  
-   Si l'utilisateur a accès à la zone fonctionnelle **Explorateur** , il peut ajouter, supprimer et mettre à jour toutes les données de référence dans cette zone.  
  
-   Si des modifications de l’entité requièrent l’approbation d’un administrateur, un administrateur d’entité peut examiner puis approuver ou rejeter les ensembles de modifications.  
  
 Chaque entité peut avoir plusieurs administrateurs. Chaque utilisateur peut être un administrateur d’entité pour un, plusieurs ou toutes les entités.  
  
 Un utilisateur peut être configuré en tant qu’administrateur d’entité dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ou par programme. Pour plus d’informations, consultez [Créer un administrateur d’entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-administrator-master-data-services.md).  
  
## <a name="master-data-services-super-user"></a>Super utilisateur Master Data Services  
 Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez affecter des autorisations utilisateur à la zone fonctionnelle Super utilisateur. Un utilisateur disposant d’autorisations sur la zone fonctionnelle Super utilisateur a en réalité l’autorisation d’administrateur sur tous les modèles et des autorisations pour toutes les autres zones fonctionnelles. Pour plus d’informations sur les autorisations pour les zones fonctionnelles, consultez [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 Le super utilisateur par défaut est spécifié pour le **compte d’administrateur** quand vous créez la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] à l’aide de [l’Assistant Création d’une base de données &#40;Gestionnaire de configuration Master Data Services&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
 Le super utilisateur peut effectuer les opérations suivantes :  
  
-   Accéder à toutes les zones fonctionnelles  
  
-   Ajouter, supprimer et mettre à jour toutes les données de référence pour tous les modèles dans la zone fonctionnelle **Explorateur** .  
  
 Vous pouvez affecter des autorisations de super utilisateur à plusieurs utilisateurs et/ou groupes d’utilisateurs.  
  
## <a name="comparing-administrator-types"></a>Comparaison des types d'administrateur  
  
|Type d'administrateur|Description|  
|------------------------|-----------------|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Super utilisateur|Les autorisations affectées dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] n'ont aucun effet sur l'accès de l'administrateur.<br /><br /> Peut être super utilisateur en fonction des autorisations de zones fonctionnelles qui lui sont affectées explicitement ou de celles héritées d’un groupe.<br /><br /> Dispose automatiquement de toutes les autorisations sur tous les modèles.<br /><br /> Accès automatique à toutes les zones fonctionnelles.|  
|Administrateur de modèle|Peut être administrateur de modèle en fonction des autorisations d’administrateur qui lui sont affectées explicitement ou de celles héritées d’un groupe.<br /><br /> Accès uniquement aux zones fonctionnelles auxquelles l'accès est accordé.<br /><br /> Dispose automatiquement de toutes les autorisations sur tous les objets et membres dans le modèle spécifique.|  
|Administrateur d’entité|Peut être administrateur d’entité en fonction des autorisations d’administrateur qui lui sont affectées explicitement ou de celles héritées d’un groupe.<br /><br /> Accès uniquement aux zones fonctionnelles auxquelles l'accès est accordé.<br /><br /> Dispose automatiquement de toutes les autorisations sur tous les objets et membres dans l’entité spécifique.<br /><br /> Peut approuver les ensembles de modifications en attente si les modifications de l’entité doivent être approuvées.|  
  
## <a name="external-resources"></a>Ressources externes  
 Billet de blog, [Améliorations de sécurité](http://go.microsoft.com/fwlink/p/?LinkId=615376), sur msdn.com.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un administrateur de modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)   
 [Créer une base de données Master Data Services](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Notifications &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
  
