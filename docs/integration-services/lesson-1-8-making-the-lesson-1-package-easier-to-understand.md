---
title: 'Étape 8 : Comment rendre le package de la leçon 1 plus facile à assimiler | Microsoft Docs'
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
ms.assetid: e3751e53-77c7-47d0-8fe8-73ed1a53413a
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 71188d8cfdf91bb6ea00b43b7c4a49cd4014af10
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-8---making-the-lesson-1-package-easier-to-understand"></a>Leçon 1-8 : Comment rendre le package de la leçon 1 plus facile à assimiler
Une fois la configuration du package de la leçon 1 terminée; il peut être judicieux de mettre un peu d'ordre dans la disposition du package. Si les formes dans la disposition des flux de contrôle et de données affichent des tailles aléatoires, ou si elles ne sont pas alignées ou groupées, la maîtrise des fonctionnalités du package peut s'avérer plus délicate.  
  
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools fournit des outils qui facilitent et accélèrent le processus de mise en forme de la disposition des packages. Les fonctionnalités de mise en forme offrent notamment la possibilité d'attribuer la même taille aux formes, de les aligner et de manipuler l'espace horizontal et vertical entre elles.  
  
Une autre manière d'améliorer la compréhension des fonctionnalités des packages est d'ajouter des annotations décrivant ces fonctionnalités.  
  
Au cours de cette tâche, vous allez exploiter les fonctionnalités de mise en forme disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools pour améliorer la disposition du flux de données et ajouter une annotation à ce dernier.  
  
### <a name="to-format-the-layout-of-the-data-flow"></a>Pour mettre en forme la disposition du flux de données  
  
1.  Si le package de la leçon 1 n'est pas encore ouvert, double-cliquez sur le fichier Lesson 1.dtsx dans l'Explorateur de solutions.  
  
2.  Cliquez sur l'onglet **Flux de données** .  
  
3.  Placez le curseur en haut et à droite de la transformation Extract Sample Currency, cliquez, puis faites glisser le curseur sur tous les composants du flux de données.  
  
4.  Dans le menu **Format** , pointez sur **Uniformiser la taille**, puis cliquez sur **Les deux**.  
  
5.  Avec les objets du flux de données sélectionnés, dans le menu **Format** , pointez sur **Aligner**, puis cliquez sur **Gauche**.  
  
### <a name="to-add-an-annotation-to-the-data-flow"></a>Pour ajouter une annotation au flux de données  
  
1.  Cliquez avec le bouton droit n’importe où sur l’arrière-plan de la zone de conception du flux de données, puis cliquez sur **Ajouter une annotation**.  
  
2.  Tapez ou collez le texte suivant dans la zone de l'annotation.  
  
    **Le flux de données extrait les données d'un fichier, recherche des valeurs dans la colonne CurrencyKey de la table DimCurrency et dans la colonne DateKey de la table DimDate, puis écrit les données dans la table NewFactCurrencyRate.**  
  
    Pour renvoyer à la ligne le texte de la zone d'annotation, placez le curseur à l'endroit où vous souhaitez démarrer une nouvelle ligne, puis appuyez sur la touche Entrée.  
  
    Si vous n'ajoutez aucun texte dans la zone de l'annotation, cette dernière disparaît lorsque vous cliquez en dehors.  
  
## <a name="next-steps"></a>Next Steps  
[Étape 9 : Test de la leçon 1 du Package du didacticiel](../integration-services/lesson-1-9-testing-the-lesson-1-tutorial-package.md)  
  
  
  
