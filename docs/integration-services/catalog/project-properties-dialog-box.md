---
title: Boîte de dialogue Propriétés du projet | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- sql13.ssis.ssms.isprojectprop.general.f1
- sql13.ssis.ssms.isprojectprop.permissions.f1
ms.assetid: d5cf52f5-1fe2-438a-98a3-fe117360acf8
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 34d4bcf0ab2e6db44ddf3c710a9d88a291c6a9f9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-properties-dialog-box"></a>Propriétés du projet, boîte de dialogue
  Un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est une unité de déploiement. Chaque projet peut contenir des packages, des paramètres et des références d'environnement. Un projet est un objet sécurisable et peut définir des autorisations pour les principaux de base de données. Quand un projet est redéployé, la version précédente du projet peut être stockée dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Les paramètres du projet et les paramètres du package peuvent être utilisés pour affecter des valeurs aux propriétés dans des packages au moment de l'exécution. Certains paramètres requièrent des valeurs avant que le package puisse être exécuté. Les valeurs de paramètre qui référencent des variables d'environnement requièrent que le projet utilise la référence d'environnement correspondante avant l'exécution.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Propriétés du projet](#open_dialog)  
  
-   [Définir les options sur la page Général](#general)  
  
-   [Définir les options sur la page Autorisations](#permissions)  
  
##  <a name="open_dialog"></a> Ouvrir la boîte de dialogue Propriétés du projet  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous vous connectez à l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Développez le dossier qui contient le projet dont vous souhaitez définir les propriétés.  
  
5.  Cliquez avec le bouton droit sur le projet, puis cliquez sur **Propriétés**.  
  
##  <a name="general"></a> Définir les options sur la page Général  
 Utilisez la page Général pour afficher les propriétés du projet.  
  
 **Nom**  
 Indique le nom du projet.  
  
 **Identificateur**  
 Indique l'ID de projet.  
  
 **Description**  
 Affiche la description facultative du projet.  
  
 **Version du projet**  
 Indique la version du projet.  
  
 **Date du déploiement**  
 Indique la date et l'heure auxquelles le projet a été déployé ou redéployé.  
  
##  <a name="permissions"></a> Définir les options sur la page Autorisations  
 Utilisez la page **Autorisations** pour afficher et définir les autorisations explicites du projet.  
  
 ...  
 Cliquez sur **Parcourir** pour sélectionner les utilisateurs et les rôles auxquels vous souhaitez affecter des autorisations à l’aide de la boîte de dialogue **Parcourir tous les principaux** .  
  
 **Nom**  
 Indique le nom de l'utilisateur ou du rôle.  
  
 **Type**  
 Indique le type d'utilisateur ou de rôle.  
  
 **Autorisation**  
 Indique les autorisations.  
  
 **Fournisseur d'autorisations**  
 Indique l'utilisateur ou le rôle qui accorde l'autorisation.  
  
 **Octroyer**  
 Si **Accorder** est sélectionné, l’autorisation est accordée à l’utilisateur ou au rôle sélectionné.  
  
 **Refuser**  
 Si **Refuser** est sélectionné, l’autorisation est refusée à l’utilisateur ou au rôle sélectionné.  
  
  
