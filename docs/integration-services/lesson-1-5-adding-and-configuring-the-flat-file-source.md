---
title: 'Étape 5 : Ajout et configuration de la source de fichier plat | Microsoft Docs'
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
ms.assetid: 5c95ce51-e0fe-4fc5-95eb-2945929f2b13
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 996c74d9810464c1da294343d4deb1bf5fe1dd8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-5---adding-and-configuring-the-flat-file-source"></a>Leçon 1-5 : Ajout et configuration de la source de fichier plat
Au cours de cette tâche, vous allez ajouter et configurer une source de fichier plat à votre package. Une source de fichier plat est un composant de flux de données qui utilise des métadonnées définies par un Gestionnaire de connexions de fichiers plats pour spécifier le format et la structure des données à extraire à partir du fichier plat par un processus de transformation. La source de fichier plat peut être configurée pour extraire les données à partir d'un seul fichier plat en utilisant la définition de format de fichier fournie par le Gestionnaire de connexions de fichiers plats.  
  
Au cours de ce didacticiel, vous allez configurer la source de fichier plat pour qu'elle utilise le Gestionnaire de connexions de l' **exemple de données source de fichier plat** que vous venez de créer.  
  
### <a name="to-add-a-flat-file-source-component"></a>Pour ajouter un composant source de fichier plat  
  
1.  Ouvrez le Concepteur **Flux de données** en double-cliquant sur la tâche de flux de données **Extract Sample Currency Data** ou en cliquant sur l’onglet **Flux de données**.  
  
2.  Dans la **Boîte à outils SSIS**, développez **Autres sources**, puis faites glisser une **source de fichier plat** sur l’aire de conception de l’onglet **Flux de données** .  
  
3.  Dans la zone de conception **Flux de données** , cliquez avec le bouton droit sur la nouvelle **Source du fichier plat**, cliquez sur **Renommer**et affectez le nom **Extract Sample Currency Data**.  
  
4.  Double-cliquez sur la source de fichier plat pour ouvrir la boîte de dialogue Éditeur de source de fichier plat.  
  
5.  Dans la zone **Gestionnaire de connexions de fichiers plats** , sélectionnez **Sample Flat File Source Data**.  
  
6.  Sélectionnez **Colonnes** et vérifiez si les noms des colonnes sont corrects.  
  
7.  Cliquez sur **OK**.  
  
8.  Cliquez avec le bouton droit sur la source de fichier plat, puis cliquez sur **Propriétés**.  
  
9. Dans la fenêtre Propriétés, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 6 : Ajout et configuration des transformations de recherche](../integration-services/lesson-1-6-adding-and-configuring-the-lookup-transformations.md)  
  
## <a name="see-also"></a> Voir aussi  
[source de fichier plat](../integration-services/data-flow/flat-file-source.md)  
[Éditeur du gestionnaire de connexions de fichiers plats &#40;page Général&#41;](../integration-services/connection-manager/flat-file-connection-manager-editor-general-page.md)  
  
  
  
