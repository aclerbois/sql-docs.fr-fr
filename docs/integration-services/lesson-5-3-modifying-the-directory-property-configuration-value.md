---
title: 'Étape 3 : Modification de la valeur de configuration de la propriété Directory | Microsoft Docs'
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
ms.assetid: ba2a091f-361c-4331-afe2-53b465164c36
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0489101f98bcb4815e0c42e61763c894e61aeb00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-5-3---modifying-the-directory-property-configuration-value"></a>Leçon 5-3 : Modification de la valeur de configuration de la propriété Directory
Dans cette tâche, vous modifiez le paramètre de configuration (stocké dans le fichier SSISTutorial.dtsConfig) pour la propriété Value de la variable de niveau package `User::varFolderName`. Cette variable met à jour la propriété Directory du conteneur de boucles Foreach. La valeur modifiée pointe sur le dossier **New Sample Data** que vous avez créé dans la tâche précédente. Une fois le paramètre de configuration modifié et le package exécuté, la propriété Directory est mise à jour par la variable, en utilisant la valeur récupérée dans le fichier de configuration au lieu de la valeur de la propriété Directory configurée au départ dans le package.  
  
### <a name="to-modify-the-configuration-setting-of-the-directory-property"></a>Pour modifier le paramétrage de configuration de la propriété Directory  
  
1.  Dans le Bloc-notes ou dans tout autre éditeur de texte, recherchez et ouvrez le fichier de configuration SSISTutorial.dtsConfig que vous avez créé à l'aide de l'Assistant Configuration de package au cours de la tâche précédente.  
  
2.  Modifiez la valeur de l’élément **ConfiguredValue** pour qu’elle corresponde au chemin du dossier **New Sample Data** que vous avez créé dans la tâche précédente. N'encadrez pas le chemin d'accès de guillemets. Si le dossier **New Sample Data** se trouve à la racine de votre lecteur (par exemple C:\\), le code XML mis à jour doit être semblable à l’exemple suivant :  
  
    `<?xml version="1.0"?><DTSConfiguration><DTSConfigurationHeading><DTSConfigurationFileInfo GeneratedBy="DOMAIN\UserName" GeneratedFromPackageName="Lesson 5" GeneratedFromPackageID="{F4475E73-59E3-478F-8EB2-B10AFA61D3FA}" GeneratedDate="6/10/2012 8:16:50 AM"/></DTSConfigurationHeading><Configuration ConfiguredType="Property" Path="\Package.Variables[User::varFolderName].Properties[Value]" ValueType="String"><ConfiguredValue></ConfiguredValue></Configuration></DTSConfiguration>`  
  
    Les informations d’en-tête, **GeneratedBy**, **GeneratedFromPackageID**et **GeneratedDate** sont différentes dans votre fichier, bien évidemment. L’élément à noter est l’élément **Configuration** . La propriété **Value** de la variable `User::varFolderName`contient maintenant C:\New Sample Data.  
  
3.  Enregistrez les modifications, puis fermez l'éditeur de texte.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 4 : Test de la leçon 5 du Package du didacticiel](../integration-services/lesson-5-4-testing-the-lesson-5-tutorial-package.md)  
  
  
  
