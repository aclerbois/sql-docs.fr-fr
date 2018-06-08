---
title: Gérer l’Éditeur et le mode d’affichage | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], managing window behavior
- workbench view modes [SQL Server Management Studio]
- full screen mode [SQL Server Management Studio]
- fonts [SQL Server Management Studio]
- word wraps [SQL Server Management Studio]
- virtual space mode [SQL Server Management Studio]
- splitting document views
- displaying line numbers
- view modes [SQL Server Management Studio]
ms.assetid: 25c58a14-9f94-4296-9770-7d84c6bc3969
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 62557aec41fcf47cee73acecad95b1cf8ee68443
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="manage-the-editor-and-view-mode"></a>Gérer l'Éditeur et le mode d'affichage
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  L'Éditeur offre plusieurs moyens de contrôler l'affichage du code.  
  
## <a name="changing-the-view-mode"></a>Changement du mode d'affichage  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] propose un mode d’affichage appelé **Documents avec onglet**, qui permet d’ouvrir plusieurs éditeurs et documents en même temps et d’y accéder au moyen des onglets situés dans la partie supérieure de l’Éditeur. Vous pouvez également ouvrir l'environnement [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] dans le mode MD, dans lequel il est possible d'attacher les fenêtres sans les onglets, de les organiser, de les réduire, etc.  
  
#### <a name="to-switch-between-view-modes"></a>Pour passer d'un mode d'affichage à un autre  
  
1.  Cliquez sur **Options** dans le menu **Outils** .  
  
2.  Cliquez sur **Environnement**. Cliquez sur **Général**.  
  
3.  Cliquez sur **Documents avec onglet** ou sur **Environnement MDI**.  
  
    > [!NOTE]  
    >  Les modifications sont prises en compte uniquement une fois [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] redémarré.  
  
## <a name="splitting-the-view"></a>Fractionnement de l'affichage  
 Une fenêtre d'Éditeur peut être fractionnée en deux volets distincts afin de faciliter les opérations de modification.  
  
#### <a name="to-split-a-window"></a>Pour fractionner une fenêtre  
  
1.  Cliquez sur la barre de fractionnement (située au-dessus de la barre de défilement).  
  
2.  Faites glisser la barre de fractionnement vers le bas.  
  
3.  Pour revenir à un affichage ne contenant qu'un seul volet, double-cliquez sur la barre qui scinde la fenêtre en deux volets.  
  
 Le nouveau volet contient le même document et toute modification apportée dans un volet est reflétée dans l'autre volet à condition que cet autre volet affiche la même partie du document.  
  
## <a name="word-wrap"></a>Retour automatique à la ligne  
 Lorsque vous activez le retour automatique à la ligne, la barre de défilement horizontale est supprimée et les lignes de code qui dépassent la largeur de la fenêtre de l'Éditeur sont renvoyées automatiquement à la ligne affichée suivante, au lieu de se poursuivre latéralement.  
  
#### <a name="to-activate-word-wrap"></a>Pour activer le retour automatique à la ligne  
  
1.  Cliquez sur **Options** dans le menu **Outils** .  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Ouvrez le dossier de langage approprié (ou **Tous les langages** pour que tous les langages soient pris en compte).  
  
4.  Sélectionnez **Retour automatique à la ligne**.  
  
## <a name="enabling-virtual-space-mode"></a>Activation du mode Espace virtuel  
 Quand le mode **Espace virtuel** est activé, l’Éditeur se comporte comme si l’espace après la fin de chaque ligne se composait d’un nombre infini d’espaces, ce qui permet aux lignes de code de se poursuivre en dehors de la zone latérale visible de l’écran.  
  
#### <a name="to-enable-virtual-space-mode"></a>Pour activer le mode Espace virtuel  
  
1.  Cliquez sur **Options** dans le menu **Outils** .  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Ouvrez le dossier de langage approprié (ou **Tous les langages** pour que tous les langages soient pris en compte).  
  
4.  Sélectionnez **Activer l’espace virtuel**.  
  
 Lorsque le mode Espace virtuel n'est pas activé, le curseur passe de la fin d'une ligne au premier caractère de la ligne suivante et inversement.  
  
## <a name="displaying-line-numbers"></a>affichage des numéros de ligne  
 Vous pouvez activer la numérotation des lignes dans votre code. Les numéros de ligne sont utiles pour se déplacer dans le code. Pour plus d’informations, consultez [Navigation dans le code et le texte](../../relational-databases/scripting/navigate-code-and-text.md).  
  
> [!NOTE]  
>  L'activation de la numérotation des lignes ne signifie pas que ces numéros soient repris sur le document imprimé. Pour imprimer les numéros de ligne, cochez la case **Numéros de ligne** dans la commande **Mise en page** du menu **Fichier** .  
  
#### <a name="to-display-line-numbers-in-code"></a>Pour afficher les numéros de ligne dans le code  
  
1.  Cliquez sur **Options** dans le menu **Outils** .  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Cliquez sur **Tous les langages**.  
  
4.  Cliquez sur **Général**.  
  
5.  Sélectionnez **Numéros de ligne**.  
  
 Pour choisir de numéroter les lignes uniquement pour certains langages de programmation, sélectionnez **Numéros de ligne** dans le dossier de langage approprié.  
  
## <a name="enabling-full-screen-mode"></a>Activation du mode Plein écran  
 Vous pouvez choisir de masquer toutes les fenêtres Outil et d'afficher uniquement les fenêtres de document en activant le mode Plein écran.  
  
#### <a name="to-enable-full-screen-mode"></a>Pour activer le mode Plein écran  
  
1.  Appuyez sur ALT+MAJ+ENTRÉE pour activer le mode Plein écran.  
  
## <a name="using-auto-hide-all"></a>Utilisation de l'option Masquer tout automatiquement  
  
#### <a name="to-hide-all-the-tool-windows-at-once"></a>Pour masquer toutes les fenêtres Outil en même temps  
  
1.  Dans le menu **Fenêtre** , sélectionnez **Masquer tout automatiquement** .  
  
  
