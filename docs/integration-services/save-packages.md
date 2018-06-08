---
title: Enregistrer des packages | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.savecopyas.f1
helpviewer_keywords:
- Integration Services packages, saving
- packages [Integration Services], saving
- saving packages
- SSIS packages, saving
- SQL Server Integration Services packages, saving
ms.assetid: 17c1de2c-637f-45c2-a148-79294bae0af4
caps.latest.revision: 48
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f5fd030f90a6b0b08cf1a7e64e87825c44b8329e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="save-packages"></a>Enregistrer des packages
  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] , vous créez des packages à l’aide du concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] et vous les enregistrez dans le système de fichiers sous forme de fichiers XML (fichiers .dtsx). Vous pouvez également enregistrer des copies du fichier XML des packages dans la base de données msdb dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou dans le magasin de packages. Le magasin de packages représente les dossiers de l’emplacement du système de fichiers gérés par le service [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
 Si vous enregistrez un package dans le système de fichiers, vous pouvez utiliser par la suite le service [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour importer le package dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou dans le magasin de packages. Pour plus d’informations, consultez [Service Integration Services &#40;Service SSIS&#41;](../integration-services/service/integration-services-service-ssis-service.md).  
  
 Vous pouvez également utiliser un utilitaire d’invite de commandes, **dtutil**, pour copier un package entre le système de fichiers et msdb. Pour plus d’informations, consultez [dtutil Utility](../integration-services/dtutil-utility.md).  
## <a name="save-a-package-to-the-file-system"></a>Enregistrer un package dans le système de fichiers  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package à enregistrer dans un fichier.  
  
2.  Dans l'Explorateur de solutions, cliquez sur le package à enregistrer.  
  
3.  Dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  
  
    > [!NOTE]  
    >  Vous pouvez vérifier le chemin d'accès et le nom de fichier dans lequel le package a été enregistré dans la fenêtre Propriétés.  

## <a name="save-a-copy-of-a-package"></a>Enregistrer une copie d'un package
  Cette section décrit comment enregistrer une copie d’un package dans le système de fichiers, dans le magasin de packages ou dans la base de données **msdb** dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Lorsque vous spécifiez un emplacement dans lequel enregistrer la copie du package, vous pouvez également mettre à jour le nom de celui-ci.  
  
 Le magasin de packages peut comprendre à la fois la base de données **msdb** et les dossiers du système de fichiers, uniquement la base de données **msdb**, ou uniquement les dossiers du système de fichiers. Dans la base de données **msdb**, les packages sont enregistrés dans la table **sysssispackages** . Cette table comprend une colonne **folderid** qui identifie le dossier logique auquel le package appartient. Les dossiers logiques permettent de regrouper les packages enregistrés dans la base de données **msdb** , à l’image des dossiers du système de fichiers qui permettent de regrouper les packages enregistrés dans le système de fichiers. Les lignes de la table **sysssispackagefolders** de la base de données **msdb** définissent les dossiers.  
  
 Si la base de données **msdb** n’est pas définie dans le cadre du magasin de packages, vous pouvez néanmoins associer les packages aux dossiers logiques existants en sélectionnant SQL Server dans l’option **Chemin d’accès au package** .  
  
> [!NOTE]  
>  Pour pouvoir enregistrer une copie du package, le package doit être ouvert dans le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] .  
  
### <a name="to-save-a-copy-of-a-package"></a>Pour enregistrer une copie d'un package  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur le package dont vous voulez enregistrer une copie.  
  
2.  Dans le menu **Fichier**, cliquez sur **Enregistrer une copie de \<fichier_package> sous**.  
  
3.  Dans la boîte de dialogue **Enregistrer une copie du package**, sélectionnez l’emplacement d’un package dans la liste **Emplacement du package**. Les options suivantes sont disponibles :  
    -   SQL Server
    -   Système de fichiers 
    -   Magasin de packages SSIS 
  
4.  Si l’emplacement est **SQL Server** ou **Magasin de packages SSIS**, indiquez un nom de serveur.  
  
5.  Si vous enregistrez une copie du package dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], indiquez le type d'authentification et, si vous utilisez l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , indiquez un nom d'utilisateur et un mot de passe.  
  
6.  Pour spécifier le chemin du package, tapez le chemin ou cliquez sur le bouton Parcourir **(…)** pour indiquer l’emplacement du package. Le nom par défaut du package est Package. Le cas échéant, attribuez au package un nom qui réponde mieux à vos besoins.  
  
     Si vous sélectionnez **SQL Server** dans l’option **Chemin d’accès au package** , le chemin du package comprend les dossiers logiques enregistrés dans la base de données **msdb** et le nom du package. Par exemple, si le package DownloadMonthlyData est associé au sous-dossier Finance du dossier MSDB (nom par défaut du dossier logique racine dans la base de données **msdb**), le chemin du package nommé DownloadMonthlyData est MSDB/Finance/DownloadMonthlyData.  
  
     Si vous sélectionnez **Magasin de packages SSIS** dans l’option **Chemin d’accès au package** , le chemin du package comprend le dossier géré par le service Integration Services. Par exemple, si le package UpdateDeductions se trouve dans le sous-dossier HumanResources du dossier de système de fichiers géré par le service, le chemin d'accès au package est /File System/HumanResources/UpdateDeductions ; de même, si le package PostResumes est associé au sous-dossier HumanResources du dossier MSDB, le chemin d'accès au package est MSDB/HumanResources/PostResumes.  
  
     Si vous sélectionnez **Système de fichiers** dans l’option **Chemin d’accès au package** , le chemin du package se compose de l’emplacement dans le système de fichiers et du nom de fichier. Par exemple, si le nom du package est UpdateDemographics, le chemin d'accès au package est C:\HumanResources\Quarterly\UpdateDemographics.dtsx.  
  
7.  Vérifiez le niveau de protection du package.  
  
8.  Si vous le souhaitez, cliquez sur le bouton **(…)** en regard de la zone **Niveau de protection** pour modifier le niveau de protection.  
  
    -   Dans la boîte de dialogue **Niveau de protection du package** , sélectionnez un niveau de protection différent.  
  
    -   Cliquez sur **OK**.  
  
9. Cliquez sur **OK**.  

## <a name="save-a-package-as-a-package-template"></a>Enregistrer un package en tant que modèle de package
 Cette section montre comment désigner et utiliser des packages personnalisés comme modèles quand vous créez des packages Integration Services dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. Par défaut, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] utilise un modèle de package qui crée un package vide lorsque vous ajoutez un nouveau package à un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Vous ne pouvez pas remplacer ce modèle par défaut, mais vous pouvez en ajouter de nouveaux.  
  
 Vous pouvez désigner plusieurs packages à utiliser comme modèles. Avant de pouvoir implémenter de nouveaux packages personnalisés comme modèles, vous devez créer les packages.  
  
 Lorsque vous créez un package en utilisant des packages personnalisés comme modèles, les nouveaux packages portent le même nom et le même GUID que le modèle. Pour différencier les packages, vous devez mettre à jour la valeur de la propriété **Name** et générer un nouveau GUID pour la propriété **ID** . Pour plus d’informations, consultez [Créer des packages dans les outils de données SQL Server](../integration-services/create-packages-in-sql-server-data-tools.md) et [Définir les propriétés d’un package](../integration-services/set-package-properties.md).  
  
### <a name="to-designate-a-custom-package-as-a-package-template"></a>Pour désigner un package personnalisé comme modèle de package  
  
1.  Dans le système de fichiers, localisez le package que vous souhaitez utiliser comme modèle.  
  
2.  Copiez le package dans le dossier DataTransformationItems. Par défaut ce dossier se trouve dans C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies\ProjectItems\DataTransformationProject.  
  
3.  Recommencez les étapes 1 et 2 pour chaque package que vous souhaitez utiliser comme modèle.  
  
### <a name="to-use-a-custom-package-as-a-package-template"></a>Pour utiliser un package personnalisé comme modèle de package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] dans lequel vous souhaitez créer un package.  
  
2.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet, pointez sur **Ajouter**, puis cliquez sur **Nouvel élément**.  
  
3.  Dans la boîte de dialogue **Ajouter un nouvel élément -\<nom_projet>**, cliquez sur le package que vous voulez utiliser comme modèle.  
  
     La liste de modèles inclut le modèle de package par défaut nommé Nouveau package SSIS. L'icône de package identifie les modèles pouvant être utilisés comme modèles de packages.  
  
4.  Cliquez sur **Ajouter**.  
