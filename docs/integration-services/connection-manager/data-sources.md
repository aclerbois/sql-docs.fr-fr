---
title: Sources de données | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data sources [Integration Services], about data sources
ms.assetid: 7ac81612-9822-470f-8d0f-a1dc96142fe3
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5e459c6a81e65c50031bc07e5f8845f7e44fac5e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-sources"></a>Sources de données
  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] inclut un objet au moment de la conception que vous pouvez utiliser dans les packages [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] : la source de données.  
  
 Un objet de source de données est une référence à une connexion et comprend au minimum une chaîne de connexion et un identificateur de source de données. Il peut également inclure des métadonnées supplémentaires comme une description, un nom, un nom d'utilisateur et un mot de passe.  
  
> **REMARQUE :** vous pouvez ajouter des sources de données uniquement aux projets qui sont configurés pour utiliser le modèle de déploiement de package. Si un projet est configuré pour utiliser le modèle de déploiement de projet, vous utilisez les gestionnaires de connexions créés au niveau du projet pour partager les connexions, au lieu d'utiliser les sources de données.  
>   
>  Pour plus d'informations sur les modèles de déploiement, consultez [Deployment of Projects and Packages](../packages/deploy-integration-services-ssis-projects-and-packages.md). Pour plus d’informations sur la conversion d’un projet en modèle de déploiement de projet, consultez [Déployer des projets sur le serveur Integration Services](https://msdn.microsoft.com/library/hh231102.aspx).  
  
 L'utilisation de sources de données dans les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] présente les avantages suivants :  
  
-   Une source de données a une étendue de projet, ce qui signifie qu’une source de données créée dans un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est accessible à tous les packages de ce projet. Une source de données peut être définie une fois, puis référencée par des gestionnaires de connexions dans plusieurs packages.  
  
-   Une source de données permet la synchronisation entre l'objet de source de données et ses références dans les packages. Si la source de données et les packages qui la référencent se trouvent dans le même projet, la propriété de chaîne de connexion des références de la source de données est automatiquement mise à jour lorsque la source de données change.  
  
## <a name="reference-data-sources"></a>Référencer des sources de données  
 Pour ajouter un objet de source de données à un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , cliquez avec le bouton droit sur le dossier **Sources de données** dans **l’Explorateur de solutions** , puis cliquez sur **Nouvelle source de données**. L’élément est ajouté au dossier **Sources de données** . Si vous souhaitez utiliser des objets sources de données créés dans d'autres projets, vous devez d'abord les ajouter au projet.  
  
 Pour utiliser un objet de source de données dans un package, vous devez ajouter un gestionnaire de connexions qui référence cet objet dans le package. Vous pouvez l'ajouter au package avant ou pendant la construction du flux de contrôle et des flux de données du package.  
  
 Un objet source de données représente une connexion simple à une source de données et fournit un accès aux objets de la banque de données qu'il référence. Par exemple, un objet de source de données qui se connecte à l’exemple de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]AdventureWorks inclut les 60 tables de cette base de données.  
  
 Il n'existe aucune dépendance entre une source de données et les gestionnaires de connexions qui la référencent. Si une source de données ne fait plus partie d'un projet, les packages restent valides car les informations relatives à la source de données, comme son type de connexion et sa chaîne de connexion, sont incluses dans la définition du package.  
  
  
