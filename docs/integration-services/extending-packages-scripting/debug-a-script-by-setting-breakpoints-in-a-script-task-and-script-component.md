---
title: Déboguer un script en définissant des points d’arrêt dans une tâche de script et un composant de script| Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- breakpoints [Integration Services]
- scripts [Integration Services], breakpoints
ms.assetid: 6c03464f-3f7d-4882-b7f8-8e396f8e2944
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cdec733e418e63f769a1b4d4a097b9714e45eada
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="debug-a-script-by-setting-breakpoints-in-a-script-task-and-script-component"></a>Déboguer un script en définissant des points d'arrêt dans une tâche de script et un composant de script
  Cette section décrit la procédure de définition des points d'arrêt dans les scripts utilisés dans la tâche de script et le composant de script.  
  
 Après avoir défini les points d’arrêt dans le script, la boîte de dialogue **Définir les points d’arrêt - \<nom_objet>** donne la liste des points d’arrêt avec les points d’arrêt intégrés.  
  
> [!IMPORTANT]  
>  Dans certains cas, les points d'arrêt de la tâche de script et le composant de script sont ignorés. Pour plus d’informations, consultez la section **Débogage de la tâche de script** dans [Codage et débogage de la tâche de script](../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md) et la section **Débogage du composant Script** dans [Codage et débogage du composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="to-set-a-breakpoint-in-script"></a>Pour définir un point d'arrêt dans un script  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Double-cliquez sur le package qui contient le script dans lequel vous souhaitez définir des points d'arrêt.  
  
3.  Pour ouvrir la tâche de script, cliquez sur l’onglet **Flux de contrôle**, puis double-cliquez sur la tâche de script.  
  
4.  Pour ouvrir le composant de script, cliquez sur l’onglet **Flux de données**, puis double-cliquez sur le composant de script.  
  
5.  Cliquez sur **Script**, puis sur **Modifier le script**.  
  
6.  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA), recherchez la ligne de script sur laquelle vous voulez définir un point d’arrêt, cliquez dessus avec le bouton droit, pointez sur **Point d’arrêt**, puis sélectionnez **Insérer un point d’arrêt**.  
  
     L'icône du point d'arrêt apparaît sur la ligne de code.  
  
7.  Dans le menu **Fichier** , cliquez sur **Quitter**.  
  
8.  Cliquez sur **OK**.  
  
9. Pour enregistrer le package, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
  
