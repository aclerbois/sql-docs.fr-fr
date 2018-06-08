---
title: L’installation de SQL Server Migration Assistant pour Access (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- installing SSMA
- instructions, installation
- instructions, upgrade
- licensing SSMA
- prerequisites for installing SSMA
- procedure, installation
- procedure, licensing
- procedure, upgrading
- removing SSMA
- Setup
- uninstalling SSMA
- upgrading SSMA
ms.assetid: dd50eebd-75df-4e0d-8c4d-88b511aae4c7
caps.latest.revision: 31
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 39c6aef71584fbc3a50ec8611ead82ff5ad9f351
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34709087"
---
# <a name="installing-sql-server-migration-assistant-for-access-accesstosql"></a>L’installation de SQL Server Migration Assistant pour Access (AccessToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour l’accès est installé à l’aide d’un Assistant basé sur le programme d’installation de Windows. Cette rubrique fournit des informations sur les conditions préalables d’installation, un lien vers la version la plus récente de SSMA et des instructions pour l’installation, la gestion de licences, la désinstallation et la mise à niveau de SSMA.  
  
## <a name="prerequisites"></a>Configuration requise  
Avant d’installer SSMA, assurez-vous que votre système répond aux exigences suivantes :  
  
-   Windows 7 ou une version ultérieure, ou Windows Server 2008 ou une version ultérieure.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 ou une version ultérieure.  
  
-   Le [!INCLUDE[msCoName](../../includes/msconame_md.md)] .NET Framework version 4.0 ou version ultérieure. Le .NET Framework version 4.0 est disponible sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] du produit et à l’aide des informations contenues dans le [Guide de Microsoft .NET](https://docs.microsoft.com/dotnet/framework/).
  
-   Accès à et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]base de données SQL Azure à laquelle vous migrerez des objets de base de données et les données.  
  
-   Microsoft objet DAO (Data Access) version 12.0 ou 14.0 du fournisseur. Vous pouvez installer le fournisseur DAO à partir du produit Microsoft Office 2010/2007, ou téléchargez-le à partir du site web de Microsoft.  
  
-   Le SQL Server Native Access Client (SNAC) version 10.5 et versions ultérieures pour la migration vers SQL Azure. Vous pouvez obtenir la dernière version de SNAC de [Microsoft® SQL Server® 2008 R2 Feature Pack](http://go.microsoft.com/fwlink/?LinkId=196940)  
  
-   4 Go de RAM (recommandé).  
  
## <a name="installing-ssma"></a>L’installation de SSMA  
SSMA est téléchargeable sur le Web. Pour télécharger la version la plus récente, consultez le [page de téléchargement de l’Assistant Migration SQL Server](http://aka.ms/ssmaforaccess).  
  
Après avoir téléchargé la dernière version, vous devez extraire les fichiers d’installation avant de pouvoir installer SSMA.

> [!IMPORTANT]  
> -   Désinstallez toutes les versions précédentes de SSMA pour Access avant d’installer la nouvelle version.  
  
**Pour installer SSMA**  
  
1.  Double-cliquez sur SSMA pour Access *n*.msi, où *n* est le numéro de build.  
  
2.  Dans la page d’accueil, cliquez sur **suivant**.  
  
    Si vous n’avez pas les composants requis installés, un message apparaît indiquant que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis et puis exécutez à nouveau le programme d’installation.  
  
3.  Lisez le contrat de licence utilisateur final ; Si vous acceptez, cochez **J’accepte le contrat**, puis cliquez sur **suivant**.  
  
4.  Dans la page Choisir un Type d’installation, cliquez sur **standard**.  
  
5.  Cliquez sur **Installer**.  
  
L’emplacement d’installation par défaut est C:\Program Files\Microsoft SQL Server Migration Assistant pour l’accès.  
  
## <a name="uninstalling-ssma-for-access"></a>Désinstallation de SSMA pour Access  
Désinstaller SSMA à l’aide de **Ajout / Suppression de programmes** dans le panneau de configuration. N’oubliez pas que la désinstallation du programme ne supprime pas les fichiers de projet SSMA ou des fichiers journaux.  
  
**Pour désinstaller SSMA**  
  
1.  Cliquez sur **Démarrer**, cliquez sur **le panneau de configuration**, puis cliquez sur **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez **Microsoft SQL Server Migration Assistant pour Access**, puis cliquez sur **supprimer**.  
  
## <a name="upgrading-to-a-later-version"></a>La mise à niveau vers une version ultérieure  
Si vous souhaitez mettre à niveau vers une version ultérieure de SSMA pour Access, vous devez d’abord désinstaller SSMA pour Access et puis installez la version la plus récente. Suivez les instructions dans l’Assistant de désinstallation de SSMA pour la section d’accès exécuter ce processus.  
  
Si vous ouvrez un projet créé dans une version antérieure de SSMA pour Access, SSMA vous demande si vous souhaitez convertir le projet vers la version la plus récente. Cliquez sur **Oui** pour travailler avec le projet dans la version la plus récente de SSMA.  
  
## <a name="see-also"></a>Voir aussi  
[Préparation pour la Migration des bases de données Access](http://msdn.microsoft.com/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/76a3abcf-2998-4712-9490-fe8d872c89ca)  
[Liaison des Applications d’accès à SQL Server](http://msdn.microsoft.com/82374ad2-7737-4164-a489-13261ba393d4)  
  
