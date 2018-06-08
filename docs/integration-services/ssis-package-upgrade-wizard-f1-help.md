---
title: Aide sur l’Assistant Mise à niveau de packages SSIS via la touche F1 | Microsoft Docs
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
ms.topic: reference
f1_keywords:
- sql13.is.upgradewizard.ssisupgradewizard.f1
- sql13.is.upgradewizard.selectsourcelocation.f1
- sql13.is.upgradewizard.selectdestinationlocation.f1
- sql13.is.upgradewizard.selectpackagemgtoptions.f1
- sql13.is.upgradewizard.selectpackages.f1
- sql13.is.upgradewizard.completewizard.f1
- sql13.is.upgradewizard.upgradingpackage.f1
ms.assetid: 7fe886ff-1ea5-48d5-9d20-d5da36dd1cd7
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 3d5a99d26e02695c8c805e30f7a3bc9afb972178
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ssis-package-upgrade-wizard-f1-help"></a>Aide sur l'Assistant Mise à niveau de packages SSIS via la touche F1
  Utilisez l’Assistant Mise à niveau de packages SSIS pour mettre à niveau des packages créés par des versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vers le format de package utilisé par la version actuelle de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
 **Pour exécuter l'Assistant Mise à niveau de packages SSIS**  
  
-   [Mettre à niveau des packages Integration Services à l’aide de l’Assistant Mise à niveau de packages SSIS](../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md)  

## <a name="ssis-upgrade-wizard"></a>Assistant Mise à niveau de SSIS
  
### <a name="options"></a>Options  
 **Ne plus afficher cette page.**  
 Ignorer la page d'accueil lors de l'ouverture suivante de l'Assistant.  
 
## <a name="select-source-location-page"></a>Page Sélectionner l’emplacement source
 Utilisez la page **Sélectionner l’emplacement source** pour spécifier la source à partir de laquelle effectuer la mise à niveau de packages.  
  
> [!NOTE]  
>  Cette page est disponible uniquement quand vous exécutez l’Assistant Mise à niveau de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] à partir de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ou de l’invite de commandes.  
  
### <a name="static-options"></a>Options statiques  
 **Source du package**  
 Sélectionnez l'emplacement de stockage qui contient les packages à mettre à niveau. Cette option a les valeurs répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**File System**|Indique que les packages à mettre à niveau se trouvent dans un dossier sur l'ordinateur local.<br /><br /> Pour que l'Assistant sauvegarde les packages d'origine avant de les mettre à niveau, les packages d'origine doivent être stockés dans le système de fichiers. Pour plus d'informations, consultez la rubrique de procédure.|  
|**Magasin de packages SSIS**|Indique que les packages à mettre à niveau se trouvent dans le magasin de packages. Le magasin de packages se compose de l’ensemble des dossiers du système de fichiers gérés par le service [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Pour plus d’informations, consultez [Gestion de packages &#40;Service SSIS&#41;](../integration-services/service/package-management-ssis-service.md).<br /><br /> La sélection de cette valeur affiche les options dynamiques **Source du package** correspondantes.|  
|**Microsoft SQL Server**|Indique que les packages à mettre à niveau proviennent d’une instance existante de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> La sélection de cette valeur affiche les options dynamiques **Source du package** correspondantes.|  
  
 **Dossier**  
 Tapez le nom d’un dossier qui contient les packages à mettre à niveau, ou cliquez sur **Parcourir** et recherchez le dossier.  
  
 **Parcourir**  
 Recherchez le dossier qui contient les packages à mettre à niveau.  
  
### <a name="package-source-dynamic-options"></a>Options dynamiques de la source du package  
  
#### <a name="package-source--ssis-package-store"></a>Source du package = Magasin de packages SSIS  
 **Server**  
 Tapez le nom du serveur sur lesquels se trouvent les packages à mettre à niveau ou sélectionnez ce serveur dans la liste.  
  
#### <a name="package-source--microsoft-sql-server"></a>Source du package = Microsoft SQL Server  
 **Server**  
 Tapez le nom du serveur sur lesquels se trouvent les packages à mettre à niveau ou sélectionnez ce serveur dans la liste.  
  
 **Utiliser l’authentification Windows**  
 Permet d'utiliser l'authentification Windows pour se connecter au serveur.  
  
 **Utiliser l'authentification SQL Server**  
 Permet d’utiliser l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour se connecter au serveur. Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vous devez fournir un nom d’utilisateur et un mot de passe.  
  
 **User name**  
 Tapez le nom d’utilisateur que l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilisera pour se connecter au serveur.  
  
 **Mot de passe**  
 Tapez le mot de passe que l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilisera pour se connecter au serveur.  
 
## <a name="select-destination-location-page"></a>Page Sélectionner l’emplacement de destination
 Utilisez la page **Sélectionner l’emplacement de destination** pour spécifier la destination dans laquelle enregistrer les packages mis à niveau.  
  
> [!NOTE]  
>  Cette page est disponible uniquement quand vous exécutez l’Assistant Mise à niveau de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] à partir de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ou de l’invite de commandes.  
 
### <a name="static-options"></a>Options statiques  
 **Enregistrer à l’emplacement source**  
 Enregistrez les packages mis à niveau dans le même emplacement que celui spécifié dans la page **Sélectionner l’emplacement source** de l’Assistant.  
  
 Si vous voulez que l’Assistant sauvegarde les packages d’origine quand ils sont stockés dans le système de fichiers, sélectionnez l’option **Enregistrer à l’emplacement source** . Pour plus d’informations, consultez [Mettre à niveau des packages Integration Services à l’aide de l’Assistant Mise à niveau de packages SSIS](../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md).  
  
 **Sélectionner le nouvel emplacement de destination**  
 Enregistrez les packages mis à niveau dans l'emplacement de destination qui est spécifié sur cette page.  
  
 **Source du package**  
 Spécifiez l'emplacement où les packages de mise à niveau doivent être stockés. Cette option a les valeurs répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**File System**|Indique que les packages mis à niveau doivent être enregistrés dans un dossier sur l'ordinateur local.|  
|**Magasin de packages SSIS**|Indique que les packages mis à niveau doivent être enregistrés dans le magasin de packages Integration Services. Le magasin de packages se compose de l'ensemble des dossiers du système de fichiers gérés par Integration Services. Pour plus d’informations, consultez [Gestion de packages &#40;Service SSIS&#41;](../integration-services/service/package-management-ssis-service.md).<br /><br /> La sélection de cette valeur affiche les options dynamiques **Source du package** correspondantes.|  
|**Microsoft SQL Server**|Indique que les packages mis à niveau doivent enregistrés dans une instance existante de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> La sélection de cette valeur affiche les options **Source du package** dynamiques correspondantes.|  
  
 **Dossier**  
 Tapez le nom d’un dossier dans lequel enregistrer les packages mis à niveau, ou cliquez sur **Parcourir** et recherchez le dossier.  
  
 **Parcourir**  
 Recherchez le dossier dans lequel les packages mis à niveau seront enregistrés.  
  
### <a name="package-source-dynamic-options"></a>Options dynamiques de la source du package  
  
#### <a name="package-source--ssis-package-store"></a>Source du package = Magasin de packages SSIS  
 **Server**  
 Tapez le nom du serveur sur lequel les packages de mise à niveau seront enregistrés, ou sélectionnez un serveur dans la liste.  
  
#### <a name="package-source--microsoft-sql-server"></a>Source du package = Microsoft SQL Server  
 **Server**  
 Tapez le nom du serveur sur lequel les packages de mise à niveau seront enregistrés, ou sélectionnez ce serveur dans la liste.  
  
 **Utiliser l’authentification Windows**  
 Permet d'utiliser l'authentification Windows pour se connecter au serveur.  
  
 **Utiliser l'authentification SQL Server**  
 Permet d’utiliser l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour se connecter au serveur. Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vous devez fournir un nom d’utilisateur et un mot de passe.  
  
 **User name**  
 Tapez le nom d’utilisateur à utiliser pendant l’utilisation de l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour se connecter au serveur.  
  
 **Mot de passe**  
 Tapez le mot de passe à utiliser pendant l’utilisation de l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour se connecter au serveur.  
 
## <a name="select-package-management-options-page"></a>Page Sélectionner les options de gestion des packages
  Utilisez la page **Sélectionner les options de gestion des packages** pour spécifier des options permettant de mettre à niveau des packages.  
  
 **Pour exécuter l'Assistant Mise à niveau de packages SSIS**  
  
-   [Mettre à niveau des packages Integration Services à l’aide de l’Assistant Mise à niveau de packages SSIS](../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md)  
  
### <a name="options"></a>Options  
 **Mettre à jour les chaînes de connexion pour l'utilisation des nouveaux noms de fournisseurs**  
 Mettez à jour les chaînes de connexion afin d'utiliser les noms des fournisseurs suivants pour la version actuelle d' [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]:  
  
-   Fournisseur OLE DB pour [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client  
  
 L'Assistant Mise à niveau de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] met à jour uniquement les chaînes de connexion qui sont stockées dans des gestionnaires de connexions. Il ne met pas à jour les chaînes de connexion qui sont construites dynamiquement à l'aide du langage d'expression [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] ou en utilisant du code dans une tâche de script.  
  
 **Valider les packages mis à niveau**  
 Validez les packages de mise à niveau et enregistrez uniquement ceux dont la validation a réussi.  
  
 Si vous ne sélectionnez pas cette option, l'Assistant ne validera pas les packages de mise à niveau. Par conséquent, il les enregistrera tous, qu'ils soient valides ou non. L’Assistant enregistre les packages de mise à niveau dans la destination spécifiée dans la page **Sélectionner l’emplacement de destination** de l’Assistant.  
  
 La validation ralentit le processus de mise à niveau. Nous vous recommandons de ne pas sélectionner cette option pour les packages volumineux qui sont susceptibles d'être mis à niveau avec succès.  
  
 **Créer des ID de package**  
 Créez de nouveaux ID de package pour les packages de mise à niveau.  
  
 **Continuer le processus de mise à niveau lorsque la mise à niveau d’un package échoue**  
 Spécifiez que, quand un package ne peut pas être mis à niveau, l’Assistant Mise à niveau de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] continue à mettre à niveau les packages restants.  
  
 **Conflits de noms de packages**  
 Spécifiez la façon dont l'Assistant doit gérer les packages qui portent le même nom. Cette option a les valeurs répertoriées dans le tableau suivant.  
  
 **Remplacer les fichiers de packages existants**  
 Remplace le package existant par le package de mise à niveau du même nom.  
  
 **Ajouter des suffixes numériques pour mettre à niveau les noms de packages**  
 Ajoute un suffixe numérique au nom du package de mise à niveau.  
  
 **Ne pas mettre à niveau les packages**  
 Arrête la mise à niveau des packages et affiche une erreur à la fin de l'Assistant.  
  
 Ces options ne sont pas disponibles quand vous sélectionnez l’option **Enregistrer à l’emplacement source** dans la page **Sélectionner l’emplacement de destination** de l’Assistant.  
  
 **Ignorer les configurations**  
 Ne charge pas les configurations de package pendant la mise à niveau des packages. L'activation de cette option réduit le temps requis pour mettre à niveau le package.  
  
 **Sauvegarder les packages d’origine**  
 Cette option demande à l’Assistant de sauvegarder les packages d’origine dans un dossier **SSISBackupFolder** . L’Assistant crée le dossier **SSISBackupFolder** en tant que sous-dossier du dossier qui contient les packages d’origine et les packages mis à niveau.  
  
> [!NOTE]  
>  Cette option est disponible uniquement lorsque vous spécifiez que les packages d'origine et les packages mis à niveau sont stockés dans le système de fichiers et dans le même dossier.  

## <a name="select-packages-page"></a>Page Sélectionner les packages
  Utilisez la page **Sélectionnez les packages** pour sélectionner les packages à mettre à niveau. Cette page répertorie les packages qui sont stockés dans l'emplacement spécifié sur la page **Sélectionner l'emplacement source** de l'Assistant.  
  
### <a name="options"></a>Options  
 **Nom de package existant**  
 Sélectionnez un ou plusieurs packages à mettre à niveau.  
  
 **Nom du package de mise à niveau**  
 Indiquez le nom du package de destination, ou utilisez le nom par défaut fourni par l'Assistant.  
  
> [!NOTE]  
>  Vous pouvez également modifier le nom du package de destination après avoir mis à niveau le package. Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] ou [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], ouvrez le package mis à niveau et modifiez son nom.  
  
 **Mot de passe**  
 Indiquez le mot de passe utilisé pour déchiffrer les packages de mise à niveau sélectionnés.  
  
 **Appliquer à la sélection**  
 Appliquez le mot de passe spécifié pour déchiffrer les packages de mise à niveau sélectionnés.  
 
## <a name="complete-the-wizard-page"></a>Page Fin de l’Assistant
  Utilisez la page **Terminer l'Assistant** pour vérifier et confirmer les options de mise à niveau des packages que vous avez sélectionnées. Il s'agit de la dernière page de l'Assistant dans laquelle vous pouvez revenir aux pages précédentes et modifier les options pour cette session de l'Assistant.  
  
### <a name="options"></a>Options  
 **Récapitulatif des options**  
 Vérifiez les options de mise à niveau que vous avez sélectionnées dans l'Assistant. Pour modifier ces options, cliquez sur **Précédent** pour retourner aux pages précédentes de l'Assistant.
 
## <a name="upgrading-the-packages-page"></a>Page Mise à niveau des packages
  Utilisez la page **Mise à niveau des packages** pour afficher la progression de la mise à niveau de packages et pour interrompre le processus de mise à niveau. L'Assistant Mise à niveau de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] met à niveau, un par un, les packages sélectionnés.  
  
### <a name="options"></a>Options  
 **Volet Message**  
 Affiche des messages de progression et des informations de résumé pendant le processus de mise à niveau.  
  
 **Action**  
 Permet d'afficher les actions de la mise à niveau.  
  
 **État**  
 Permet d'afficher le résultat de chaque action.  
  
 **Message**  
 Permet d'afficher les messages d'erreur générés par chaque action.  
  
 **Arrêter**  
 Permet d'arrêter la mise à niveau de packages.  
  
 **Rapport**  
 Sélectionnez ce que vous voulez faire avec le rapport qui contient les résultats de la mise à niveau de packages :  
  
-   Afficher le rapport en ligne.  
  
-   Enregistrer le rapport dans un fichier.  
  
-   Copier le rapport dans le Presse-papiers.  
  
-   Envoyer le rapport sous forme de message électronique.  

## <a name="view-upgraded-packages"></a>Afficher les packages mis à niveau
### <a name="view-upgraded-packages-that-were-saved-to-a-sql-server-database-or-to-the-package-store"></a>Afficher les packages mis à niveau qui ont été enregistrés dans une base de données SQL Server ou dans le magasin de packages
  
Dans l'Explorateur d'objets de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], connectez-vous à l'instance locale d' [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], puis développez le nœud **Packages stockés** pour voir les packages mis à niveau.  
  
### <a name="view-upgraded-packages-that-were-upgraded-from-sql-server-data-tools"></a>Afficher les packages mis à niveau à partir de SQL Server Data Tools  
  
Dans l'Explorateur de solutions de [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , puis développez le nœud **Packages SSIS** pour voir les packages mis à niveau.  
  
## <a name="see-also"></a> Voir aussi  
 [Mettre à niveau des packages Integration Services](../integration-services/install-windows/upgrade-integration-services-packages.md)  
  
  
