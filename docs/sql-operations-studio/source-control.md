---
title: "Contrôle de code source dans SQL Operations Studio (version préliminaire) | Documents Microsoft"
description: "Découvrez comment configurer le contrôle de code source dans SQL Operations Studio (version préliminaire)."
ms.custom: tools|sos
ms.date: 11/15/2017
ms.prod: sql-non-specified
ms.reviewer: alayu; erickang; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: 
ms.topic: article
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 4f28199262b087ad5362da0ddf56827216aec748
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
#  <a name="using-source-control-in-includename-sosincludesname-sos-shortmd"></a>Utiliser le contrôle à la source dans [!INCLUDE[name-sos](../includes/name-sos-short.md)]

[!INCLUDE[name-sos](../includes/name-sos-short.md)]prend en charge Git pour le contrôle de version/code source.

## <a name="git-support-in-includename-sosincludesname-sos-shortmd"></a>Prise en charge de Git dans [!INCLUDE[name-sos](../includes/name-sos-short.md)]

[!INCLUDE[name-sos](../includes/name-sos-short.md)] est fourni avec un gestionnaire de contrôle de code source Git (SCM), mais vous devez toujours [installer Git (version 2.0.0 ou version ultérieure)](https://git-scm.com/download) avant que ces fonctionnalités soient disponibles. 

## <a name="open-an-existing-git-repository"></a>Ouvrir un dépôt Git existant

1. Sous le menu **Fichier** , sélectionnez **Ouvrir un dossier...**
2. Accédez au dossier qui contient vos fichiers de suivi par Git, puis cliquez sur **Sélectionner le dossier**. Les sous-dossiers dans votre référentiel local sont OK sélectionner ici.


## <a name="initialize-a-new-git-repository"></a>Initialiser un nouveau dépôt Git

1. Sélectionnez **Contrôle de code Source**, puis sélectionnez l’icône Git.

   ![Icône du contrôle de code source git](media/source-control/source-control.png)

2. Entrez le chemin d’accès au dossier que vous souhaitez initialiser un dépôt Git, puis appuyez sur **Entrée**.

   ![initialiser le référentiel Git](media/source-control/initialize-git-repository.png)

## <a name="working-with-git-repositories"></a>Utilisation des référentiels Git

[!INCLUDE[name-sos](../includes/name-sos-short.md)] hérite son implémentation Git de Visual Studio Code, mais ne prend pas actuellement en charge d'autres fournisseurs SCM supplémentaires. Pour plus d’informations sur l’utilisation de Git après avoir ouvert ou initialiser un dépôt, consultez [prise en charge de Git dans VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support).


## <a name="additional-resources"></a>Ressources supplémentaires
- [Documentation de Git](https://git-scm.com/documentation)
