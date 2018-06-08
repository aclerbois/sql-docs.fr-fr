---
title: Boîte de dialogue Propriétés du package | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
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
- sql13.ssis.ssms.ispackageprop.general.f1
- sql13.ssis.ssms.packageproperties.f1
ms.assetid: a70acbf4-5f5c-4606-8ce4-8eb3684233de
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2d78c48635cdf47b6826f639494942d1b82d39a4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="package-properties-dialog-box"></a>Propriétés du package, boîte de dialogue
  Utilisez la boîte de dialogue **Propriétés du package** pour afficher et gérer les propriétés des packages stockés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Pour plus d’informations, consultez [Serveur Integration Services &#40;SSIS&#41;](../integration-services-ssis-packages.md).  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Propriétés du package](#open_dialog)  
  
-   [Configurer les options](#options)  
  
##  <a name="open_dialog"></a> Ouvrir la boîte de dialogue Propriétés du package  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous vous connectez à l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Développez le dossier qui contient le package dont vous souhaitez afficher les propriétés.  
  
5.  Cliquez avec le bouton droit sur le package, puis sélectionnez **Propriétés**.  
  
##  <a name="options"></a> Configurer les options  
 Utilisez la page **Général** pour afficher les propriétés du package sélectionné.  
  
 Toutes les propriétés affichées dans la page **Général** sont en lecture seule.  
  
 **Nom**  
 Affiche le nom du package.  
  
 **Identificateur**  
 Indique l'ID du package.  
  
 **Point d'entrée**  
 La valeur **True** indique que le package est démarré directement. La valeur **False** indique que le package est démarré par un autre package via la tâche d'exécution du package. La valeur par défaut est **True**.  
  
 Vous définissez cette propriété dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour les packages parent et enfant en cliquant avec le bouton droit sur le package dans l’Explorateur de solutions et en sélectionnant **Package de point d’entrée**.  
  
 **Description**  
 Affiche la description facultative du package.  
  
  
