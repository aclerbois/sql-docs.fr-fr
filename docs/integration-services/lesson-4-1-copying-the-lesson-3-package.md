---
title: 'Étape 1 : Copie du package de la leçon 3 | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 0d053786-5203-43f3-a613-27a8dd2bc44a
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7620b1ce0a6c92417a78e2c203a6599b0d678313
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-4-1---copying-the-lesson-3-package"></a>Leçon 4-1 : Copie du package de la leçon 3
Dans cette tâche, vous allez créer une copie du package Lesson 3.dtsx que vous avez créé à la leçon 3. Si vous n'avez pas terminé la leçon 3, vous pouvez également ajouter au projet le package final de la leçon 3 inclus avec le didacticiel, puis le copier pour l'utiliser. Vous allez utiliser cette nouvelle copie tout au long de la leçon 4.  
  
### <a name="to-create-the-lesson-4-package"></a>Pour créer le package de la leçon 4  
  
1.  Si [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools n’est pas déjà ouvert, dans le menu **Démarrer**, pointez sur **Tous les programmes**, puis sur **Microsoft SQL Server**et cliquez sur **SQL Server Data Tools**.  
  
2.  Dans le menu **Fichier**, cliquez sur **Ouvrir**, cliquez sur **Projet/Solution**, sélectionnez **Didacticiel SSIS**, cliquez sur **Ouvrir**, puis double-cliquez sur **SSIS Tutorial.sln**.  
  
3.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur **Lesson 3.dtsx**et choisissez **Copier**.  
  
4.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur **Packages SSIS**et choisissez **Coller**.  
  
    Par défaut, le package copié est nommé Lesson 4.dtsx.  
  
5.  Dans l’Explorateur de solutions, double-cliquez sur **Lesson 4.dtsx** pour ouvrir le package.  
  
6.  Cliquez avec le bouton droit dans l’arrière-plan de l’onglet **Flux de contrôle** , puis cliquez sur **Propriétés**.  
  
7.  Dans la fenêtre Propriétés, mettez à jour la propriété **Nom** en lui affectant la valeur **Leçon 4**.  
  
8.  Cliquez sur la zone de la propriété **ID** , puis dans la liste, cliquez sur **<Generate New ID>**.  
  
### <a name="to-add-the-completed-lesson-3-package"></a>Pour ajouter le package final de la leçon 3  
  
1.  Ouvrez [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] , puis le projet SSIS Tutorial.  
  
2.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur **Packages SSIS**, puis cliquez sur **Ajouter le package existant**.  
  
3.  Dans la boîte de dialogue **Ajouter une copie des packages existants** , dans **Emplacement du package**, sélectionnez **Système de fichiers**.  
  
4.  Cliquez sur le bouton de navigation **(...)** , accédez à Lesson 3.dtsx sur votre ordinateur, puis cliquez sur **Ouvrir**.  
  
    Pour télécharger tous les packages de leçons de ce didacticiel, procédez comme suit.  
  
    1.  Accédez à [Exemples de produits Integration Services](http://go.microsoft.com/fwlink/?LinkId=275027)  
  
    2.  Cliquez sur l'onglet **DOWNLOADS** (Téléchargements).  
  
    3.  Cliquez sur le fichier SQL2012.Integration_Services.Create_Simple_ETL_Tutorial.Sample.zip.  
  
5.  Copiez et collez le package de la leçon 3 selon les étapes 3 à 8 décrites dans la procédure précédente.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 2 : Création d'un fichier corrompu](../integration-services/lesson-4-2-creating-a-corrupted-file.md)  
  
