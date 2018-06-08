---
title: Tâches au niveau système | Microsoft Docs
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
- system-level tasks [Reporting Services]
ms.assetid: 7023b388-40b2-4590-b227-115cf380a1e7
caps.latest.revision: 36
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 704aba49823742f237171000f14c10a3156f26a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tasks-and-permissions---system-level-tasks"></a>Tâches et autorisations - Tâches au niveau système
  Une tâche de niveau système est une collection d'autorisations liées à des opérations qui s'appliquent au site du serveur de rapports dans son ensemble. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut aussi des tâches au niveau élément qui s’appliquent à des éléments spécifiques. Pour plus d’informations, consultez [Tâches au niveau élément](../../reporting-services/security/tasks-and-permissions-item-level-tasks.md). Pour plus d'informations sur les tâches et les autorisations en général, consultez [Tasks and Permissions](../../reporting-services/security/tasks-and-permissions.md).  
  
> [!NOTE]  
>  Si vous travaillez avec ces tâches par programmation, vous devez utiliser des méthodes qui prennent en charge les tâches au niveau système. Pour plus d’informations, consultez <xref:ReportService2010.ReportingService2010.ListTasks%2A> et <xref:ReportService2010.ReportingService2010.ListRoles%2A>.  
  
## <a name="permissions-in-system-level-tasks"></a>Autorisations dans les tâches de niveau système  
 Le tableau suivant identifie la collection d'autorisations pour chaque tâche système. Les autorisations sont répertoriées à titre d'informations uniquement, afin de fournir une description plus exacte de la fonctionnalité disponible via chaque tâche.  
  
|Tâche|Autorisations|  
|----------|-----------------|  
|Exécuter les définitions de rapport|Exécuter les définitions de rapport (l'autorisation et la tâche portent le même nom)|  
|Générer des événements|Générer des événements|  
|Gérer les travaux|Lire les propriétés système<br /><br /> Mettre à jour les propriétés système|  
|Gérer les propriétés du serveur de rapports|Lire les propriétés système<br /><br /> Mettre à jour les propriétés système|  
|Gérer les rôles|Créer les rôles<br /><br /> Supprimer les rôles<br /><br /> Lire les propriétés des rôles<br /><br /> Mettre à jour les propriétés des rôles|  
|Gérer les planifications partagées|Créer les planifications|  
|Gérer la sécurité du serveur de rapports|Lire les stratégies de sécurité du système<br /><br /> Mettre à jour les stratégies de sécurité du système|  
|Afficher les propriétés du serveur de rapports|Lire les propriétés système|  
|Afficher les planifications partagées|Lire les planifications|  
  
## <a name="see-also"></a> Voir aussi  
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
