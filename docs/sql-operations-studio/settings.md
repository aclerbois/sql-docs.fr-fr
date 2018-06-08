---
title: Studio d’opérations SQL (aperçu) utilisateur et les paramètres de l’espace de travail | Documents Microsoft
description: Comment modifier les paramètres de l’espace de travail et les Studio des opérations SQL (aperçu) utilisateur.
ms.custom: tools|sos
ms.date: 11/15/2017
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: yualan
ms.author: alayu
manager: craigg
ms.openlocfilehash: 67f60d30693eeb60030f3a977ec1bcf5a1f98be1
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="user-and-workspace-settings"></a>Utilisateur et les paramètres de l’espace de travail

Il est facile à configurer [!INCLUDE[name-sos](../includes/name-sos-short.md)] à votre convenance via des paramètres. Presque toutes les parties de [!INCLUDE[name-sos](../includes/name-sos-short.md)]d’éditeur, l’interface utilisateur et le comportement fonctionnel propose les options que vous pouvez modifier.

[!INCLUDE[name-sos](../includes/name-sos-short.md)] fournit deux portées différentes pour les paramètres :

* **Utilisateur** ces paramètres s’appliquent globalement à une instance de [!INCLUDE[name-sos](../includes/name-sos-short.md)] vous ouvrez.
* **Espace de travail** paramètres de l’espace de travail sont des paramètres spécifiques à un dossier sur votre ordinateur et sont disponibles uniquement lorsque le dossier est ouvert dans la barre latérale de l’Explorateur. Paramètres définis sur cette étendue de remplacent l’étendue de l’utilisateur.

## <a name="creating-user-and-workspace-settings"></a>Création d’utilisateurs et les paramètres de l’espace de travail

La commande de menu **fichier** > **préférences** > **paramètres** (**Code**  >  **Préférences** > **paramètres** sur Mac) fournit le point d’entrée pour configurer les paramètres utilisateur et l’espace de travail. Vous trouverez une liste de paramètres par défaut. Copier les paramètres que vous souhaitez modifier approprié `settings.json` fichier. Les onglets à droite vous permettent de basculer rapidement entre les fichiers de paramètres utilisateur et l’espace de travail.

Vous pouvez également ouvrir les paramètres utilisateur et l’espace de travail à partir de la **Palette de commandes** (**Ctrl + Maj + P**) avec **préférences : ouvrir les paramètres utilisateur** et  **Préférences : Paramètres d’espace de travail ouvert** ou utilisez le raccourci clavier (**Ctrl +**).

L’exemple suivant désactive les numéros de ligne dans l’éditeur et configure les lignes de code pour être mises en retrait automatiquement.

![Exemples de paramètres](media/settings/sample-settings.png)

Modifications apportées aux paramètres sont rechargées par [!INCLUDE[name-sos](../includes/name-sos-short.md)] après avoir modifié `settings.json` est enregistré.

>**Remarque :** paramètres de l’espace de travail sont utiles pour le partage des paramètres spécifiques au projet dans une équipe.

## <a name="settings-file-locations"></a>Emplacements des fichiers de paramètres

Selon votre plateforme, le fichier de paramètres utilisateur se trouve ici :

* **Windows** `%APPDATA%\sqlops\User\settings.json`
* **Mac** `$HOME/Library/Application Support/sqlops/User/settings.json`
* **Linux** `$HOME/.config/sqlops/User/settings.json`

Le fichier de paramètres d’espace de travail se trouve sous le `.[!INCLUDE[name-sos](../includes/name-sos-short.md)]` dossier dans votre projet.

## <a name="hot-exit"></a>Sortie à chaud

Studio d’opérations SQL sera n’oubliez pas les modifications non enregistrées dans des fichiers lorsque vous quittez par défaut. Cela est identique à la fonctionnalité de sortie à chaud dans Visual Studio Code.

Par défaut, la sortie à chaud est désactivée. Permettre à chaud sortie en modifiant le `files.hotExit` paramètre. Pour plus d’informations, consultez [à chaud de sortie (dans la documentation de Visual Studio Code)](https://code.visualstudio.com/docs/editor/codebasics#_hot-exit).


## <a name="tab-color"></a>Couleur d’onglet

Pour simplifier l’identifiant les connexions que vous travaillez, onglets ouverts dans l’éditeur peuvent avoir leurs couleurs configurées pour correspondre à la couleur du groupe de serveurs auquel appartient la connexion. Par défaut, les couleurs d’onglet sont désactivées par défaut. Activer les couleurs d’onglet en modifiant le `sql.tabColorMode` paramètre.

## <a name="additional-resources"></a>Ressources supplémentaires

Étant donné que [!INCLUDE[name-sos](../includes/name-sos-short.md)] hérite ses paramètres de l’utilisateur et l’espace de travail à partir de Visual Studio Code, des informations détaillées sur les paramètres de la fonctionnalité est dans le [paramètres pour Visual Studio Code](https://code.visualstudio.com/docs/getstarted/settings) l’article.
