---
title: Décompresser un package DAC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: data-tier-applications
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-data-tier-apps
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- wizard [DAC], unpack
- data-tier application [SQL Server], unpack
- How to [DAC], unpack
- unpack DAC
ms.assetid: 697b69b3-f157-4e22-ac4e-f65c5fc2d0ad
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2e30fdb3d4fe97021ba89b710fd128ee9faaedec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unpack-a-dac-package"></a>Décompresser un package DAC
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la boîte de dialogue Décompresser une application de la couche Données pour décompresser les scripts et les fichiers d'un package d'application de la couche Données (DAC). Les scripts et les fichiers sont placés dans un dossier où ils peuvent être examinés avant que le package soit utilisé pour déployer la DAC dans un système de production. Le contenu d'une DAC peut également être comparé avec le contenu d'un autre package décompressé dans un autre dossier.  
  
1.  **Avant de commencer :**  [Sécurité](#Security)  
  
2.  **Pour décompresser une DAC, à l'aide de la boîte de dialogue**  [Décompresser une application de la couche Données](#UnpackDACDial), [Examen du contenu d'un package DAC](#ExamDACPack)  
  
##  <a name="Security"></a> Sécurité  
 Nous vous recommandons de ne pas déployer un package DAC provenant de sources inconnues ou non approuvées. Ces DAC peuvent contenir du code malveillant susceptible d'exécuter un code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma. Avant d'utiliser une DAC provenant d'une source inconnue ou non approuvée, déployez-la sur une instance de test isolée du [!INCLUDE[ssDE](../../includes/ssde-md.md)], décompressez la DAC et examinez le code, par exemple les procédures stockées ou autre code défini par l'utilisateur.  
  
##  <a name="UnpackDACDial"></a> Boîte de dialogue Décompresser une application de la couche Données  
 **Pour décompresser un fichier de package DAC**  
  
-   Dans l’ **Explorateur Windows**, accédez à l’emplacement d’un fichier de package DAC (.dacpac).  
  
-   Utilisez l'une des deux méthodes suivantes pour ouvrir la boîte de dialogue Décompresser une application de la couche Données :  
  
    1.  Cliquez avec le bouton droit sur le fichier de package DAC (.dacpac), puis sélectionnez **Décompresser**.  
  
    2.  Double-cliquez sur le fichier de package DAC.  
  
-   Renseignez les boîtes de dialogue :  
  
    -   [Décompresser un fichier de package DAC Microsoft SQL Server](#Unpack)  
  
    -   [Rechercher un dossier](#Browse)  
  
###  <a name="Unpack"></a> Décompresser un fichier de package DAC Microsoft SQL Server  
 Utilisez cette page pour spécifier le dossier de destination dans lequel placer les fichiers non compressés, puis exécuter l'opération de décompression.  
  
 **Les fichiers seront décompressés dans ce dossier :** - Spécifiez le chemin complet du dossier pour les fichiers non compressés. Si le dossier existe et que vous connaissez le chemin d'accès complet, tapez le chemin d'accès dans la zone. Autrement, cliquez sur le bouton **Parcourir** pour accéder à un dossier ou créer un dossier.  
  
 **Parcourir** - Ouvre la page **Rechercher un dossier** où vous pouvez choisir un dossier en parcourant la hiérarchie des fichiers ou créer un dossier.  
  
 **Décompresser** - Démarre l’opération de décompression.  
  
 **Annuler** - Ferme la boîte de dialogue sans décompresser le package DAC.  
  
###  <a name="Browse"></a> Rechercher un dossier  
 Utilisez cette page pour choisir le dossier de destination pour l'opération de décompression. Vous pouvez également créer un dossier.  
  
 **Liste de dossiers** - Affiche la hiérarchie des fichiers de votre ordinateur. Développez les nœuds pour accéder au dossier dans lequel décompresser le package DAC. Cliquez sur le dossier, puis cliquez sur **OK**.  
  
 **Créer un nouveau dossier** - Ouvre un dialogue dans lequel vous pouvez spécifier le nom d’un nouveau dossier qui sera créé dans le dossier actuellement sélectionné dans l’arborescence des dossiers.  
  
 **OK** - Place le chemin du dossier que vous avez sélectionné dans la zone **Les fichiers seront décompressés dans ce dossier** de la page **Décompresser un fichier de package DAC** et vous ramène à cette page.  
  
 **Annuler** - Ferme la boîte de dialogue sans sélectionner de dossier.  
  
##  <a name="ExamDACPack"></a> Examen du contenu d'un package DAC  
 Après avoir décompressé le package, vous pouvez examiner les fichiers produits par la boîte de dialogue **Décompresser une application de la couche Données** . La boîte de dialogue génère les fichiers suivants dans le dossier de destination sélectionné :  
  
1.  un script Transact-SQL qui contient les instructions pour la création des objets définis dans la DAC ; Le nom de fichier est *DACName*.sql, où *DACName* correspond au nom de la DAC.  
  
2.  tous les fichiers XML du package ;  
  
3.  tous les fichiers de la section Fichiers supplémentaires de la DAC, tels que les fichiers de pré-déploiement ou de post-déploiement de la DAC.  
  
 Pour plus d’informations, consultez [Validate a DAC Package](../../relational-databases/data-tier-applications/validate-a-dac-package.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)   
 [Mettre à niveau une application de la couche Données](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)  
  
  
