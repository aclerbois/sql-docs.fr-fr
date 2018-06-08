---
title: Terminal intégrée dans Studio des opérations SQL (version préliminaire) | Documents Microsoft
description: En savoir plus sur le terminal intégré dans Studio des opérations SQL (version préliminaire).
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
ms.openlocfilehash: 0754c66c182acefd2fdff799b791fbf135356d7b
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="integrated-terminal"></a>Terminal Server intégré

Dans [!INCLUDE[name-sos](../includes/name-sos-short.md)], vous pouvez ouvrir un terminal intégré, initialement en commençant à la racine de votre espace de travail. Cela peut être pratique lorsque vous n’êtes pas obligé de passer de windows ou de modifier l’état d’un terminal existant pour effectuer une tâche de ligne de commande rapide.

Pour ouvrir le terminal :

* Utilisez le **Ctrl +'** raccourci clavier avec l’accent grave.
* Utilisez le **vue** | **Terminal Server intégré** commande de menu.
* À partir de la **Palette de commandes** (**Ctrl + Maj + P**), utilisez la **Terminal Server intégré à activer/désactiver :** commande.

![Terminal Server](media/integrated-terminal/terminal-screen.png)

> [!NOTE]
> Vous pouvez toujours ouvrir un interpréteur de commandes externe avec l’Explorateur de **ouvrir dans l’invite de commandes** commande (**ouvrir dans Terminal** sur Mac ou Linux) si vous préférez travailler sans [!INCLUDE[name-sos](../includes/name-sos-short.md)].

## <a name="managing-multiple-terminals"></a>La gestion de plusieurs terminaux

Vous pouvez créer plusieurs terminaux ouvert à différents emplacements et de naviguer entre eux. Instances Terminal Server peuvent être ajoutées en appuyant sur l’icône plus dans l’angle supérieur droit de la **TERMINAL** panneau ou en déclenchant le **Ctrl + Maj +'** commande. Cette opération crée une autre entrée dans la liste déroulante qui peut être utilisée pour passer à l’autre.

![Plusieurs terminaux](media/integrated-terminal/terminal-multiple-instances.png)

Le bouton peuvent supprimer des instances Terminal Server en appuyant sur la Corbeille.

> [!TIP]
> Si vous utilisez plusieurs terminaux largement, vous pouvez ajouter des liaisons de clés pour le `focusNext`, `focusPrevious` et `kill` commandes décrites dans le [section des liaisons de clé](#key-bindings) de permettre la navigation entre eux à l’aide du clavier.

## <a name="configuration"></a>Configuration

L’interpréteur de commandes utilisé par défaut, `$SHELL` sur Linux et macOS, PowerShell sur Windows 10 et cmd.exe dans les versions antérieures de Windows. Celles-ci peuvent être substituées manuellement en définissant `terminal.integrated.shell.*` dans [paramètres](settings.md). Arguments peuvent être passés à l’interface de Terminal Server sur Linux et macOS à l’aide de le `terminal.integrated.shellArgs.*` paramètres.

### <a name="windows"></a>Windows

Configurer correctement votre interpréteur de commandes sur Windows est une question de localiser le fichier exécutable à droite et le paramètre de mise à jour. Voici une liste des exécutables de shell courants et leurs emplacements par défaut :

```json
// 64-bit cmd if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe"
// 64-bit PowerShell if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
// Git Bash
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
// Bash on Ubuntu (on Windows)
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\bash.exe"
```

> [!NOTE]
> Pour être utilisé comme un terminal intégré, l’exécutable de l’interpréteur de commandes doit être une application console afin que `stdin/stdout/stderr` peut être redirigé.

> [!TIP]
> L’environnement intégré de Terminal Server est en cours d’exécution avec les autorisations de [!INCLUDE[name-sos](../includes/name-sos-short.md)]. Si vous avez besoin exécuter une commande d’interpréteur de commandes avec élévation de privilèges (administrateur) ou des autorisations différentes, vous pouvez utiliser les utilitaires de plateforme comme `runas.exe` au sein d’un terminal.

### <a name="shell-arguments"></a>Arguments de l’interpréteur de commandes

Vous pouvez passer des arguments à l’interpréteur de commandes lorsqu’elle est lancée.

Par exemple, pour activer un interpréteur de commandes en cours d’exécution en tant que connexion shell (qui s’exécute `.bash_profile`), passez la `-l` argument (avec des guillemets doubles) :

```json
// Linux
"terminal.integrated.shellArgs.linux": ["-l"]
```

## <a name="terminal-display-settings"></a>Paramètres d’affichage de Terminal Server

Vous pouvez personnaliser la police terminal intégrée et la hauteur de ligne avec les paramètres suivants :

* `terminal.integrated.fontFamily`
* `terminal.integrated.fontSize`
* `terminal.integrated.lineHeight`

## <a id="key-bindings"></a>Combinaisons de touches Terminal Server

Le **vue : bascule intégré Terminal** commande est liée à **Ctrl +'** rapidement basculer le panneau de configuration de Terminal Server intégré et.

Voici les raccourcis clavier pour naviguer rapidement dans le terminal intégré :

Key|Command
---|---
**CTRL +'**|Terminal Server intégré à afficher
**CTRL + MAJ +'**|Créer nouveau terminal
**CTRL + haut**|Défiler vers le haut
**CTRL + bas**|Faites défiler vers le bas
**CTRL + PG. préc**|Page de défilement précédente
**CTRL + PG. suiv**|Page de défilement vers le bas
**CTRL + origine**|Faites défiler vers le haut
**CTRL + fin**|Faites défiler vers le bas
**CTRL + K**|Désactivez le terminal

Autres commandes Terminal Server sont disponibles et peuvent être liés à vos raccourcis clavier par défaut.

Celles-ci sont les suivantes :

* `workbench.action.terminal.focus`: Vous concentrer le terminal. Cela ressemble à activer/désactiver mais concentre le Terminal Server au lieu de masquer, s’il est visible.
* `workbench.action.terminal.focusNext`: Se concentre l’instance suivante de Terminal Server.
* `workbench.action.terminal.focusPrevious`: Se concentre l’instance précédente de Terminal Server.
* `workbench.action.terminal.kill`: Supprimer l’instance actuelle de Terminal Server.
* `workbench.action.terminal.runSelectedText`: Exécuter le texte sélectionné dans l’instance de Terminal Server.
* `workbench.action.terminal.runActiveFile`: Exécutez le fichier actif dans l’instance de Terminal Server.

### <a name="run-selected-text"></a>Exécuter le texte sélectionné

Pour utiliser le `runSelectedText` de commande, sélectionner du texte dans un éditeur et exécutez la commande **Terminal Server : exécuter le texte sélectionné dans Terminal Server actives** via la **Palette de commandes** (**Ctrl + Maj + P**). Le terminal tente d’exécuter le texte sélectionné :

![Exécuter le texte sélectionné](media/integrated-terminal/terminal_run_selected.png)

Si aucun texte n’est sélectionné dans l’éditeur actif, la ligne qui se trouve le curseur est exécutée dans le terminal.

### <a name="copy--paste"></a>Copier et coller

Les combinaisons de touches pour copier et coller respecter les normes de plateforme :

* Linux : **Ctrl + Maj + C** et **Ctrl + Maj + V**
* Mac : **Cmd + C** et **Cmd + V**
* Windows : **Ctrl + C** et **Ctrl + V**

### <a name="find"></a>Rechercher

Le Terminal intégré a des fonctionnalités de recherche de base qui peuvent être déclenchée avec **Ctrl + F**.

Si vous souhaitez **Ctrl + F** pour accéder à l’interpréteur de commandes au lieu de lancer le widget de rechercher sur Linux et Windows, vous devez supprimer la combinaison de touches comme suit :

```js
{ "key": "ctrl+f", "command": "-workbench.action.terminal.focusFindWidget",
                      "when": "terminalFocus" },
```

### <a name="rename-terminal-sessions"></a>Renommer des sessions Terminal Server

Les sessions Terminal Server intégrées peuvent être renommées maintenant à l’aide de la **Terminal Server : renommer** (`workbench.action.terminal.rename`) commande. Le nouveau nom s’affiche dans la liste déroulante de sélection Terminal Server.

### <a name="forcing-key-bindings-to-pass-through-the-terminal"></a>Forcer les combinaisons de touches à passer par le biais du terminal

Alors que le focus est dans le terminal intégré, nombre de liaisons de clé ne fonctionne pas, car les séquences de touches sont passés à et consommés par le terminal lui-même. Le `terminal.integrated.commandsToSkipShell` paramètre peut être utilisé pour contourner ce problème. Il contient un tableau de noms de commande dont les liaisons de clés ignorer le traitement par l’interpréteur de commandes et à la place d’être traités par le [!INCLUDE[name-sos](../includes/name-sos-short.md)] système de liaison de clé. Par défaut, cela inclut toutes les combinaisons de touches Terminal Server en plus une sélection combinaisons de touches quelques couramment utilisés.

