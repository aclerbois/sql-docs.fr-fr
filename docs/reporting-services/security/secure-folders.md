---
title: Dossiers sécurisés | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- high-security folders [Reporting Services]
- low-security folders
- folders [Reporting Services], security
- security [Reporting Services], folders
ms.assetid: 0fd91f77-0143-476b-9af0-87293be78e44
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9bb2d76a068d584e509c9a2d7697d6683964d9ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="secure-folders"></a>Dossiers sécurisés
  La sécurité des dossiers constitue la base de la sécurisation de tout autre contenu du serveur de rapports. Étant donné que la sécurité est héritée au sein de la structure de dossiers, vous pouvez désigner de grandes ou de petites sections de la hiérarchie de dossiers afin d'autoriser certains types d'accès.  
  
 Les dossiers de haute sécurité peuvent être utilisés pour stocker des rapports confidentiels ou comme des zones intermédiaires. Vous pouvez par exemple avoir un dossier qui sert à tester des rapports avant de les déplacer vers un emplacement final. Pour contrôler l'accès à cette zone, vous pouvez définir une attribution de rôle qui permet uniquement aux créateurs de rapports d'ajouter et de supprimer des éléments, et une deuxième attribution de rôle qui permet aux testeurs d'exécuter des rapports mais pas d'ajouter ou de supprimer des éléments. Étant donné que les attributions de rôles sont définies de manière explicite pour les testeurs et les auteurs de rapports, aucun autre utilisateur (à l'exception des administrateurs système locaux) ne peut accéder au dossier.  
  
 Les dossiers de basse sécurité peuvent être utilisés pour stocker des rapports dont vous voulez faciliter l'accès.  
  
 La sécurité des dossiers constitue la base de la sécurité au niveau élément, en commençant par le nœud racine de la hiérarchie de dossiers du serveur de rapports, le Dossier racine. Étant donné que la sécurité est héritée, il est conseillé de définir une stratégie de sécurité relativement restrictive sur le Dossier racine. C’est exactement à cela que sert le rôle **Visiteur** dans les attributions de rôles du dossier de base, en n’accordant qu’un accès en affichage seul.  
  
## <a name="tasks-and-folder-access"></a>Accès aux tâches et au dossier  
 Pour créer des attributions de rôles pour les dossiers, tenez compte des tâches répertoriées dans le tableau suivant.  
  
|Sélectionnez cette tâche|Pour autoriser les opérations suivantes|  
|----------------------|---------------------------|  
|Afficher les dossiers|Visualiser les propriétés de la hiérarchie et d'accès en lecture seule du dossier qui indiquent les dates de création et de modification de celui-ci.<br /><br /> Les utilisateurs ne peuvent afficher les éléments contenus dans le dossier que si des rôles qui incluent également les tâches « Afficher les rapports », « Afficher les modèles », « Afficher les ressources » et « Afficher les sources de données » leur ont été attribués.|  
|Gérer les dossiers|Visualiser les propriétés du dossier, modifier le nom ou la description ou déplacer le dossier. Cette tâche permet aux utilisateurs de créer des dossiers.|  
|Gérer les rapports|Ajouter des rapports depuis le système de fichiers à un dossier et publier les rapports depuis le Générateur de rapports sur le serveur de rapports.|  
|Gérer les sources de données|Ajouter de nouveaux éléments de source de données partagée à un dossier et modifier les sources de données partagées existantes.|  
|Définir la sécurité au niveau des éléments|Créer et modifier des attributions de rôles qui contrôlent l'accès au dossier. Cette tâche doit être utilisée avec les tâches « Afficher les dossiers » ou « Gérer les dossiers ». Si tel n'est pas le cas, elle n'aura aucun effet car l'utilisateur ne pourra pas sélectionner l'élément.|  
  
## <a name="see-also"></a> Voir aussi  
 [Sécuriser des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md)   
 [Sécuriser les éléments de source de données partagée](../../reporting-services/security/secure-shared-data-source-items.md)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
