---
title: 'Étape 1 : Création d’un projet Integration Services | Microsoft Docs'
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
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: f14521b5-941e-443b-8f5e-385f98e37fbf
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 77213cdbe63ddb87e970269e2276bafc545a4470
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-1---creating-a-new-integration-services-project"></a>Leçon 1-1 : Création d’un projet Integration Services
La première étape de la création d'un package dans [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] consiste à créer un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Ce projet comprend les modèles des objets (sources de données, vues de source de données et packages) que vous utilisez dans une solution de transformation de données.  
  
Les packages que vous allez créer dans ce didacticiel [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] interprètent les valeurs des données de paramètres régionaux. Si votre ordinateur n'est pas configuré pour l'utilisation du paramètre Anglais (États-Unis), vous devez définir des propriétés supplémentaires dans le package. Les packages que vous utilisez dans les leçons 2 à 5 sont copiés à partir du package créé dans la leçon 1 ; vous n'avez pas besoin de mettre à jour les propriétés des paramètres régionaux dans les packages copiés.  
  
> [!NOTE]  
> Ce didacticiel nécessite Microsoft SQL Server Data Tools.  
>   
> Pour plus d'informations sur l'installation de SQL Server Data Tools, consultez [Téléchargement de SQL Server Data Tools](http://msdn.microsoft.com/data/hh297027).  
  
### <a name="to-create-a-new-integration-services-project"></a>Pour créer un nouveau projet Integration Services  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, puis sur **Microsoft SQL Server**et cliquez sur **SQL Server Data Tools**.  
  
2.  Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet** pour créer un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
3.  Dans la boîte de dialogue **Nouveau projet** , développez le nœud **Business Intelligence** sous **Modèles installés**, puis sélectionnez **Projet Integration Services** dans le volet **Modèles** .  
  
4.  Dans la zone **Nom** , remplacez le nom par défaut par **SSIS Tutorial**. Vous pouvez éventuellement désactiver la case à cocher **Créer le répertoire pour la solution** .  
  
5.  Acceptez l'emplacement par défaut ou cliquez sur **Parcourir** pour rechercher et accéder au dossier que vous souhaitez utiliser. Dans la boîte de dialogue **Emplacement du projet** , cliquez sur le dossier, puis sur **Sélectionner le dossier**.  
  
6.  Cliquez sur **OK**.  
  
    Par défaut, un package vide nommé **Package.dtsx**est créé et ajouté à votre projet sous Packages SSIS.  
  
7.  Dans la barre d’outils de **l’Explorateur de solutions** , cliquez avec le bouton droit sur **Package.dtsx**, choisissez **Renommer**, puis attribuez au package par défaut le nom **Lesson 1.dtsx**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 2 : ajout et configuration d'un gestionnaire de connexions de fichiers plats](../integration-services/lesson-1-2-adding-and-configuring-a-flat-file-connection-manager.md)  
  
