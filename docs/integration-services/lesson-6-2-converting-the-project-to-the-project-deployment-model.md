---
title: 'Étape 2 : Conversion du projet en modèle de déploiement de projet | Microsoft Docs'
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
ms.assetid: 80964293-f1f5-4da7-b1fb-00ab8c30c1c5
caps.latest.revision: 4
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f7097d75078c8602172e2e16a26832ab4bb385a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-6-2---converting-the-project-to-the-project-deployment-model"></a>Leçon 6-2 : Conversion du projet en modèle de déploiement de projet
Dans cette tâche, vous allez utiliser l'Assistant Conversion de projet Integration Services pour convertir le projet en modèle de déploiement de projet.  
  
### <a name="converting-the-project-to-the-project-deployment-model"></a>Conversion du projet en modèle de déploiement de projet  
  
1.  Dans le menu Projet, cliquez sur Convertir en modèle de déploiement de projet.  
  
2.  Dans la page d'introduction de l'Assistant Conversion de projet Integration Services, passez en revue les étapes, puis cliquez sur Suivant.  
  
3.  Dans la page Sélectionner les packages, dans la liste Packages, décochez toutes les cases à l'exception de Lesson 6.dtsx, puis cliquez sur Suivant.  
  
4.  Dans la page Spécifier les propriétés du projet, cliquez sur Suivant.  
  
5.  Dans la page Mettre à jour la tâche d'exécution de package, cliquez sur Suivant.  
  
6.  Dans la page Sélectionner les configurations, assurez-vous que le package Lesson 6.dtsx est sélectionné dans la liste Configurations, puis cliquez sur Suivant.  
  
7.  Dans la page Créer des paramètres, assurez-vous que le package Lesson 6.dtsx est sélectionné et qu'Étendue est définie sur Package, dans la liste Propriétés de configuration, puis cliquez sur Suivant.  
  
8.  Dans la page Configurer les paramètres, vérifiez que les valeurs Nom et Valeur sont identiques à celles spécifiées dans la leçon 5 pour la variable et la valeur de configuration, puis cliquez sur Suivant.  
  
9. Dans la page Révision, dans le volet Résumé, notez que l'Assistant a utilisé les informations du fichier de configuration pour définir les propriétés à convertir.  
  
10. Cliquez sur Convertir.  
  
    Lorsque la conversion est terminée, un message avertit que les modifications ne sont pas enregistrées tant que le projet n'est pas enregistré dans Visual Studio. Cliquez sur OK dans la boîte de dialogue d'avertissement.  
  
11. Dans l'Assistant Conversion de projet Integration Services, cliquez sur Fermer.  
  
12. Dans SQL Server Data Tools, cliquez sur le menu Fichier, puis cliquez sur Enregistrer pour enregistrer le package converti.  
  
13. Cliquez sur l'onglet Paramètres et vérifiez que le package contient désormais un paramètre pour VarFolderName et que la valeur est le même chemin d'accès que celui spécifié pour le dossier New Sample Data du fichier de configuration de la leçon 5.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 3 : Test du package de la leçon 6](../integration-services/lesson-6-3-testing-the-lesson-6-package.md)  
  
  
  
