---
title: Propriétés du serveur (page Historique) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.serverproperties.history.f1
ms.assetid: be9d8018-a46f-4625-9ae1-138ebe6b38ba
caps.latest.revision: 30
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: e0c9f8065d9d66f52a86554bcb219eb4e4406cd9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-properties-history-page"></a>Propriétés du serveur (page Historique)
  Utilisez cette page [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] dans [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] pour définir le nombre de copies par défaut que l’historique de rapport doit conserver. La valeur par défaut fournit un paramètre initial qui établit les limites de l'historique de tous les rapports. Ces paramètres sont modifiables pour chaque rapport.  
  
 L'historique de rapport est une collection d'instantanés de rapport qui incluent des données de rapport et une mise en page qui s'appliquent au rapport au moment où l'instantané est créé. Vous pouvez utiliser l'historique de rapport pour garder une copie d'un rapport tel qu'il se présentait à une date ou heure spécifique. Vous pouvez créer et gérer l'historique de rapport pour des rapports individuels qui s'exécutent sur un serveur de rapports en mode natif ou configuré pour le mode intégré SharePoint.  
  
 Les instantanés d'historique de rapport sont stockés dans la base de données du serveur de rapports. Si vous conservez un nombre illimité d'instantanés, veillez à vérifier régulièrement la taille de la base de données pour vous assurer qu'elle n'augmente pas trop rapidement ni n'utilise trop d'espace disque.  
  
 Pour ouvrir cette page :
 1) Démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
 2) Connectez-vous à une instance de serveur de rapports.
 3) Cliquez avec le bouton droit sur le nom du serveur de rapports et sélectionnez **Propriétés**.
 4) Cliquez sur **Historique** pour ouvrir cette page.  
  
## <a name="options"></a>Options  
 **Conserver un nombre illimité d'instantanés dans l'historique de rapport**  
 Conservez tous les instantanés d'historique de rapport. Vous devez supprimer manuellement des instantanés pour réduire la taille de l'historique de rapport.  
  
 **Limiter les copies de l'historique de rapport**  
 Conservez un nombre défini d'instantanés d'historique de rapport. Lorsque la limite est atteinte, des copies anciennes sont supprimées de l'historique de rapport pour libérer de l'espace pour les nouvelles copies.  
  
 Si vous limitez l'historique de rapport ultérieurement, le serveur de rapports applique la nouvelle limite à l'historique de rapport existant dès que celui-ci la dépasse. Les instantanés de rapport les plus anciens sont supprimés en premier. Si l'historique de rapport est vide ou n'a pas atteint la limite, de nouveaux instantanés de rapport sont ajoutés. Lorsque la limite est atteinte, l'instantané le plus ancien est supprimé dès qu'un nouvel instantané est ajouté.  
  
## <a name="see-also"></a> Voir aussi  
 [Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Aide du serveur de rapports dans Management Studio via la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)  
  
  
