---
title: Contrôle de code source dans SQL Operations Studio (preview) | Documents Microsoft
description: Découvrez comment configurer le contrôle de code source dans SQL Operations Studio (version préliminaire).
ms.custom: tools|sos
ms.date: 11/15/2017
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e947bfbd96b5a098728ff423e3eb591544e5f453
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
#  <a name="using-source-control-in-includename-sosincludesname-sos-shortmd"></a>Utilisation du contrôle de code source dans[!INCLUDE[name-sos](../includes/name-sos-short.md)]

[!INCLUDE[name-sos](../includes/name-sos-short.md)] prend en charge Git pour le contrôle de version/source.


## <a name="git-support-in-includename-sosincludesname-sos-shortmd"></a>Prise en charge de Git dans[!INCLUDE[name-sos](../includes/name-sos-short.md)]

[!INCLUDE[name-sos](../includes/name-sos-short.md)] est fourni avec un gestionnaire de contrôle de code source Git (SCM), mais vous devez toujours [installer Git (version 2.0.0 ou version ultérieure)](https://git-scm.com/download) avant que ces fonctionnalités soient disponibles. 



## <a name="open-an-existing-git-repository"></a>Ouvrir un dépôt Git existant

1. Sous le menu **Fichier** , sélectionnez **Ouvrir un dossier...**
2. Accédez au dossier qui contient vos fichiers suivis par Git, puis cliquez sur **Sélectionner le dossier**.  Vous pouvez sélectionner ici les sous-dossiers de votre dépôt local.


## <a name="initialize-a-new-git-repository"></a>Initialiser un nouveau dépôt Git

1. Sélectionnez **Contrôle de code source**, puis sélectionnez l’icône Git.

   ![Icône du contrôle de code source Git](media/source-control/source-control.png)

1. Entrez le chemin du dossier que vous voulez initialiser comme dépôt Git, puis appuyez sur **Entrée**.

   ![initialiser le référentiel Git](media/source-control/initialize-git-repository.png)

## <a name="working-with-git-repositories"></a>Utilisation des référentiels Git

[!INCLUDE[name-sos](../includes/name-sos-short.md)]hérite son implémentation Git de Visual Studio Code, mais ne prend pas actuellement en charge d'autres fournisseurs SCM. Pour plus d’informations sur l’utilisation de Git après avoir ouvert ou initialisé un dépôt, consultez [prise en charge de Git dans VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support).


## <a name="additional-resources"></a>Ressources supplémentaires
- [Documentation de Git](https://git-scm.com/documentation)
