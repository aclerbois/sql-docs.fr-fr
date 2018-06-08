---
title: Suppression des éléments du catalogue (Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.deleteitems.f1
ms.assetid: b0599e01-6dc3-4484-80d4-022a412e0ebd
caps.latest.revision: 16
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d3d92d7345862149380565911def8eb3a52454fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="delete-catalog-items-management-studio"></a>Suppression des éléments du catalogue (Management Studio)
  Utilisez cette page pour supprimer des planifications partagées et des définitions de rôle.  
  
 Si vous supprimez une planification partagée utilisée par plusieurs rapports et abonnements, le serveur de rapports créera des planifications individuelles pour chaque rapport et abonnement qui a précédemment utilisé la planification partagée. Chaque nouvelle planification individuelle contiendra la date, l'heure et la périodicité spécifiée dans la planification partagée. Notez que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne fournit pas de gestion centrale des planifications individuelles. Si vous supprimez une planification partagée, vous devez désormais gérer les informations de planification pour chaque élément individuel. Avant de supprimer une planification partagée, utilisez la [page Rapports](../../reporting-services/tools/schedule-properties-reports-page.md) pour déterminer les rapports qui l'utilisent actuellement.  
  
 Pour les définitions de rôle, vous ne pouvez supprimer que celles qui ne sont pas utilisées activement dans une attribution de rôle. Si vous essayez de supprimer un rôle qui est actuellement en cours d'utilisation, le serveur de rapports ne supprimera pas le rôle et un message d'erreur s'affichera à cet effet. Si cette page contient une définition de rôle unique qui n'est pas actuellement en cours d'utilisation, elle est supprimée lorsque vous cliquez sur **OK**. Si cette page contient plusieurs rôles, vous ne pouvez pas sélectionner les rôles à conserver ou supprimer. Toutes les définitions de rôle inutilisées sont supprimées lorsque vous cliquez sur **OK**.  
  
 Vous ne pouvez pas annuler une opération de suppression. Pour récupérer un élément supprimé, vous devez le recréer ou restaurer une copie de sauvegarde de la base de données du serveur de rapports.  
  
## <a name="options"></a>Options  
 **Nom**  
 Spécifie le nom de l'élément à supprimer.  
  
 **Type**  
 Affiche le type d'élément à supprimer.  
  
 **Propriétaire**  
 Affiche le nom du propriétaire. Il s'agit de Système dans la plupart des cas.  
  
 **État**  
 Affiche les informations de progression d'une opération de suppression.  
  
 **Erreur**  
 Affiche un code d'erreur si une erreur se produit pendant la suppression d'un élément.  
  
## <a name="see-also"></a> Voir aussi  
 [Supprimer un élément &#40;Management Studio&#41;](../../reporting-services/tools/delete-an-item-management-studio.md)   
 [Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Créer, modifier et supprimer des planifications](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md)  
  
  
