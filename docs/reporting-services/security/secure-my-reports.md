---
title: Sécuriser Mes rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- denying My Reports folder access
- private folders [Reporting Services]
- user workspace [Reporting Services]
- security [Reporting Services], My Reports folder
- My Reports folder [Reporting Services]
ms.assetid: 3b23a382-13b8-4196-9a93-7fe62d03a63c
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: db7ab6718c89132a0aabbfdda826db786628dc68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="secure-my-reports"></a>Sécuriser Mes Rapports
  La fonctionnalité Mes Rapports offre un espace géré par l'utilisateur pour utiliser des rapports. Pour remplir sa fonction, le dossier Mes Rapports nécessite des autorisations moins restrictives que d'autres dossiers d'utilisation générale. Les utilisateurs qui disposent uniquement d'autorisations d'affichage et d'exécution de rapports dans d'autres dossiers peuvent nécessiter un ensemble étendu d'autorisations pour gérer leurs dossiers Mes rapports et leurs contenus. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] offre une attribution et une définition de rôles spécialisées à cette fin.  
  
> [!NOTE]  
>  Le dossier Mes Rapports est uniquement disponible dans le Gestionnaire de rapports. Il n’est pas disponible dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
## <a name="role-assignment-for-my-reports"></a>Attribution de rôle pour Mes Rapports  
 L'attribution de rôle pour Mes Rapports contient des éléments prédéfinis et est automatiquement créée pour chaque utilisateur qui active un dossier Mes Rapports. Compter sur le serveur de rapports pour attribuer automatiquement la sécurité est particulièrement utile pour les organisations qui utilisent beaucoup Mes Rapports puisque les administrateurs ne sont plus ainsi tenus d'activer l'accès pour chaque utilisateur de Mes Rapports.  
  
 Une attribution de rôle **Mes Rapports** se compose des éléments suivants :  
  
-   Le dossier Mes rapports de l’utilisateur, qui se trouve dans Dossiers des utilisateurs\\*\<nom_utilisateur>* \Mes Rapports.  
  
-   Le compte d'utilisateur, qui est déterminé lors de l'activation du dossier Mes Rapports. Un dossier est activé lorsqu'un utilisateur clique sur un dossier Mes Rapports dans le Gestionnaire de rapports ou lorsqu'il publie un rapport dans un dossier Mes Rapports depuis le Gestionnaire de rapports. Ce dossier est également activé lorsqu'un utilisateur demande des propriétés sur le lien Mes Rapports.  
  
-   La définition de rôle prédéfinie pour Mes rapports.  
  
## <a name="role-definition-for-my-reports"></a>Définition de rôle pour Mes Rapports  
 La définition de rôle **Mes Rapports** comprend des tâches qui prennent en charge la gestion du contenu d’un dossier Mes Rapports. Le rôle **Mes Rapports** est conçu pour être un rôle à fonction unique. Bien que vous puissiez le choisir pour n'importe quelle stratégie de sécurité au niveau élément, vous devez éviter de le faire pour réduire le risque de le modifier en fonction d'autres dossiers. Réserver le rôle **Mes Rapports** pour la fonctionnalité Mes Rapports peut vous permettre d’assurer une expérience utilisateur cohérente.  
  
 Par défaut, seuls les administrateurs des serveurs de rapports modifient le rôle **Mes Rapports** . Personnalisez le rôle **Mes Rapports** en modifiant les tâches qu’il contient. Vous pouvez également le remplacer par un autre rôle.  
  
## <a name="denying-access-to-my-reports"></a>Refus d'accès à Mes Rapports  
 Vous pouvez empêcher les utilisateurs d'accéder à Mes Rapports comme suit :  
  
-   Désactivation de Mes Rapports dans la page Paramètres du site. Pour plus d’informations, consultez [Activer et désactiver Mes rapports](../../reporting-services/report-server/enable-and-disable-my-reports.md).  
  
-   Suppression de toutes les tâches du rôle **Mes Rapports** .  
  
 Lorsque vous désactivez Mes Rapports, tout lien vers un dossier Mes Rapports est supprimé du Gestionnaire de rapports. La structure de dossiers sous-jacente qui prend en charge Mes Rapports (c'est-à-dire, le dossier Dossiers des utilisateurs et ses sous-dossiers) reste disponible et l'utilisateur peut y accéder s'il connaît le chemin d'accès au dossier. La suppression de toutes les tâches du rôle **Mes Rapports** garantit le refus de l’accès.  
  
## <a name="see-also"></a> Voir aussi  
 [Sécuriser des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md)   
 [Dossiers sécurisés](../../reporting-services/security/secure-folders.md)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
