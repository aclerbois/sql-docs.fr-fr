---
title: Boîte de dialogue Versions du projet | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: service
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isprojectprop.versions.f1
ms.assetid: a48a387c-2e70-45bc-be2e-26e57a9bb2c4
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9e85129c3f8c08db8d1df1304d81f73e90c28b20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-versions-dialog-box"></a>Boîte de dialogue de versions du projet
  Utilisez la boîte de dialogue **Versions du projet** pour afficher les versions d'un projet et pour restaurer une version précédente.  
  
 Vous pouvez également afficher les versions antérieures dans la vue [catalog.object_versions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md) et utiliser la procédure stockée [catalog.restore_project &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md) pour les restaurer.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Versions du projet](#open_dialog)  
  
-   [Restaurer une version de projet](#restore)  
  
##  <a name="open_dialog"></a> Ouvrir la boîte de dialogue Versions du projet  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Autrement dit, connectez-vous à l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **Catalogues Integration Services** pour afficher le nœud **SSISDB** .  
  
4.  Le nœud **SSISDB** contient un ou plusieurs dossiers, qui contiennent chacun un ou plusieurs projets. Développez le dossier qui contient le projet qui vous intéresse.  
  
5.  Cliquez avec le bouton droit sur le projet, puis cliquez sur **Versions**.  
  
 Dans la boîte de dialogue **Versions du projet** , le tableau **Versions** affiche la liste des versions de projet déployées sur le serveur, avec la date et l’heure de déploiement de la version, la date et l’heure de restauration de la version (si elle a été restaurée), la description de la version et un identificateur de version. La version active est indiquée par une coche dans la colonne **En cours** du tableau.  
  
##  <a name="restore"></a> Restaurer une version de projet  
 Pour restaurer une version antérieure d'un projet, sélectionnez la version dans le tableau **Versions** , puis cliquez sur **Restaurer la version sélectionnée**. Le projet est restauré à la version sélectionnée et cette dernière est indiquée par une coche dans la colonne **En cours** du tableau **Versions** .  
  
  
