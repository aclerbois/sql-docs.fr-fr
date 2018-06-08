---
title: 'Étape 1 : Copie du package de la leçon 5 | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
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
ms.assetid: a25fcc13-987e-4f3d-8f0c-76f7e6e59920
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 54e8f15d4cb049fa6dd79041668e3db998d196af
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-6-1---copying-the-lesson-5-package"></a>Leçon 6-1 : Copie du package de la leçon 5
Dans cette tâche, vous allez créer une copie du package Lesson 5.dtsx que vous avez créé à la leçon 5. Vous pouvez également ajouter au projet le package final de la leçon 5 inclus avec le didacticiel, puis le copier à la place. Vous allez utiliser cette nouvelle copie tout au long de la leçon 6.  
  
### <a name="to-copy-the-lesson-5-package"></a>Pour copier le package de la leçon 5  
  
1.  Si SQL Server Data Tools n'est pas déjà ouvert, dans le menu Démarrer, pointez sur Tous les programmes, puis sur Microsoft SQL Server 2012 et cliquez sur SQL Server Data Tools.  
  
2.  Dans le menu Fichier, cliquez sur Ouvrir, cliquez sur Projet/Solution, sélectionnez Didacticiel SSIS, cliquez sur Ouvrir, puis double-cliquez sur SSIS Tutorial.sln.  
  
3.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur Lesson 5.dtsx et choisissez Copier.  
  
4.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur Packages SSIS et choisissez Coller.  
  
    Par défaut, le package copié est nommé Lesson 6.dtsx.  
  
5.  Dans l'Explorateur de solutions, double-cliquez avec le bouton droit sur Lesson 6.dtsx pour l'ouvrir.  
  
6.  Cliquez n'importe où dans l'arrière-plan de l'onglet Flux de contrôle, puis cliquez sur Propriétés.  
  
7.  Dans la fenêtre Propriétés, mettez la propriété Name à jour vers la leçon 6 (Lesson 6).  
  
8.  Cliquez sur la zone de la propriété ID, puis sur la flèche déroulante et sur <Generate New ID>.  
  
### <a name="to-add-the-completed-lesson-5-package"></a>Pour ajouter le package final de la leçon 5  
  
1.  Ouvrez SQL Server Data Tools et ouvrez le projet Didacticiel SSIS.  
  
2.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur Packages SSIS, puis cliquez sur Ajouter le package existant.  
  
3.  Dans la boîte de dialogue Ajouter une copie des packages existants, sous Emplacement du package, sélectionnez Système de fichiers.  
  
4.  Cliquez sur le bouton de navigation (…), accédez à Lesson 5.dtsx sur votre ordinateur, puis cliquez sur **Ouvrir**.  
  
    Pour télécharger tous les packages de leçons de ce didacticiel, procédez comme suit.  
  
    1.  Accédez à [Exemples de produits Integration Services](http://go.microsoft.com/fwlink/?LinkId=275027)  
  
    2.  Cliquez sur l'onglet **DOWNLOADS** (Téléchargements).  
  
    3.  Cliquez sur le fichier SQL2012.Integration_Services.Create_Simple_ETL_Tutorial.Sample.zip.  
  
5.  Copiez et collez le package de la leçon 5 selon les étapes 3 à 8 décrites dans la procédure précédente.  
  
    Après la copie du package de la leçon 5, si vous avez des packages des leçons précédentes dans votre solution, cliquez avec le bouton droit sur chaque package des leçons 1 à 5 et cliquez sur Exclure du projet. Lorsque vous avez terminé, vous devez avoir uniquement la leçon 6.dtsx dans votre solution.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 2 : Conversion du projet en modèle de déploiement de projet](../integration-services/lesson-6-2-converting-the-project-to-the-project-deployment-model.md)  
  
