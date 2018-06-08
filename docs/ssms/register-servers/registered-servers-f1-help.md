---
title: Serveurs inscrits – Aide (F1) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-registration
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], Help
- Registered Servers [SQL Server], help
- SQL Server Management Studio Help [SQL Server], registered servers
ms.assetid: 59f76b28-ba78-4a1a-b5d5-8b581f30114d
caps.latest.revision: 23
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 00fe3c62180ada724cf202ae1dc2c3e6e9c6df8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="registered-servers-f1-help"></a>Serveurs inscrits – Aide (F1)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Cette section contient l’aide, accessible via la touche F1, relative au composant Serveurs inscrits de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Elle décrit les différentes options.
  
 Pour en savoir plus sur les serveurs inscrits et obtenir des liens vers les options qu’ils offrent, accédez à la rubrique [Inscrire les serveurs](../../tools/sql-server-management-studio/register-servers.md) . 
 

 Cliquez sur ce bouton pour enregistrer les paramètres des serveurs inscrits. 
 
 ## <a name="reporting-services-new-or-edit-server-registration-general-tab"></a>Reporting Services - Nouvelle inscription de serveur ou Modifier les propriétés d’inscription du serveur (onglet Général) 
  Cet onglet vous permet de spécifier des options lors de l'inscription d'une instance de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Pour accéder à cette page, dans Serveurs inscrits, cliquez sur **Reporting Services** dans la barre d’outils **Serveurs inscrits** , cliquez avec le bouton droit sur un groupe de serveurs inscrits quelconque, tel que **Reporting Services**, pointez sur **Nouveau**, puis cliquez sur **Inscription du serveur**.  
  
### <a name="options"></a>Options  
 **Type de serveur**  
 Quand un serveur est inscrit à partir de Serveurs inscrits, la zone **Type de serveur** est en lecture seule et correspond au type de serveur affiché dans le volet **Serveurs inscrits** . Pour inscrire un autre type de serveur, cliquez sur le serveur souhaité dans la barre d'outils **Serveurs inscrits** avant de commencer l'inscription du nouveau serveur.  
  
 **Nom du serveur**  
 Spécifiez l'instance de serveur de rapports à laquelle se connecter. Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vous pouvez accéder à un serveur de rapports par son nom d'instance. Vous pouvez avoir une instance de serveur de rapports pour chaque instance SQL Server que vous installez. Si vous utilisez l'instance par défaut, tapez le nom de l'instance SQL Server. Si vous utilisez une instance nommée, spécifiez cette instance nommée pour vous connecter au serveur de rapports, sous le format MSSQL$InstanceName.  
  
 **Authentification**  
 L'authentification sur un serveur de rapports s'effectue par l'intermédiaire d'IIS (Internet Information Services). Sélectionnez l'un des modes d'authentification ci-dessous pour vous connecter à Reporting Services :  
  
 **Mode d'authentification Windows (authentification Windows)**  
 La connexion à l'instance de serveur de rapports s'effectue à l'aide de l'authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Authentification de base**  
 Connectez-vous au moyen de l’ **Authentification de base** si votre installation Reporting Services est configurée pour utiliser l’authentification de base.  
  
 **Authentification par formulaire**  
 Connectez-vous au moyen de l’ **Authentification par formulaire** si votre installation Reporting Services est configurée pour utiliser une extension d’authentification personnalisée.  
  
 **Nom d'utilisateur**  
 Entrez le nom d'utilisateur à utiliser pour se connecter. Cette option n'est disponible que si vous avez choisi **Authentification de base** ou **Authentification par formulaire**.  
  
 **Mot de passe**  
 Entrez le mot de passe correspondant au nom d'utilisateur indiqué. Cette option n'est modifiable que si vous avez choisi **Authentification de base** ou **Authentification par formulaire**.  
  
 **Mémoriser le mot de passe**  
 Permet d'enregistrer le mot de passe que vous avez entré. Cette option n'est disponible que si vous avez cliqué sur **Authentification de base** ou **Authentification par formulaire**.  
  
> **REMARQUE :** Si vous avez stocké le mot de passe et ne voulez plus le conserver en mémoire, décochez la case, puis cliquez sur **Enregistrer**.  
  
 **Nom du serveur inscrit**  
 Le nom qui doit apparaître dans Serveurs inscrits. Il n'est pas nécessaire que ce nom corresponde à celui de la zone **Nom du serveur** .  
  
 **Description du serveur inscrit**  
 Entrez une description facultative du serveur.  
  
 **Test**  
 Cliquez sur cette option pour tester la connexion au serveur sélectionné dans la zone **Nom du serveur**.  
  
 
 ## <a name="analysis-services---multidimensional-data-new-or-edit-server-registration-general-tab"></a>Analysis Services - Données multidimensionnelles - Nouvelle inscription de serveur ou Modifier les propriétés d’inscription du serveur (onglet Général)
 
  Cet onglet vous permet de spécifier des options lors de l'inscription d'une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Pour accéder à cette page, dans Serveurs inscrits, cliquez sur **Analysis Services** dans la barre d’outils Serveurs inscrits, cliquez avec le bouton droit sur n’importe quel groupe de serveurs inscrits (par exemple, **Analysis Services**), pointez sur **Nouveau**, puis cliquez sur **Inscription du serveur**.  
  
### <a name="options"></a>Options  
 **Type de serveur**  
 Quand un serveur est inscrit à partir de Serveurs inscrits, la zone **Type de serveur** est en lecture seule et correspond au type de serveur affiché dans le volet Serveurs inscrits. Pour inscrire un autre type de serveur, cliquez sur le serveur souhaité dans la barre d'outils **Serveurs inscrits** avant de commencer l'inscription du nouveau serveur.  
  
 **Nom du serveur**  
 Sélectionnez l'instance de serveur à laquelle se connecter. La dernière instance de serveur à laquelle une connexion a été établie est affichée par défaut.  
  
 **Authentification**  
 L'authentification Windows permet à l'utilisateur de se connecter au moyen de ses informations d'identification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, en tant qu'utilisateur Windows ou en tant que membre d'un groupe Windows.  
  
 **User name**  
 Cette option n'est pas disponible dans cette version.  
  
 **Mot de passe**  
 Cette option n'est pas disponible dans cette version.  
  
 **Mémoriser le mot de passe**  
 Cette option n'est pas disponible dans cette version.  
  
 **Nom du serveur inscrit**  
 Le nom qui doit apparaître dans Serveurs inscrits. Ce nom ne doit pas correspondre à celui de la zone **Nom du serveur** .  
  
 **Description du serveur inscrit**  
 Entrez une description facultative du serveur.  
  
 **Test**  
 Cliquez sur cette option pour tester la connexion au serveur sélectionné dans la zone **Nom du serveur**. 
 
 ## <a name="ssis-new-or-edit-server-registration-general-tab"></a>SSIS - Nouvelle inscription de serveur ou Modifier les propriétés d’inscription du serveur (onglet Général) 
 
 Cet onglet vous permet de spécifier des options lors de l'inscription de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Pour accéder à cette page, dans Serveurs inscrits, cliquez sur **Integration Services** dans la barre d’outils **Serveurs inscrits** , cliquez avec le bouton droit sur un groupe de serveurs inscrits quelconque, pointez sur **Nouveau**, puis cliquez sur **Inscription du serveur**.  
  
### <a name="options"></a>Options  
 **Type de serveur**  
 Quand un serveur est inscrit dans Serveurs inscrits, la zone **Type de serveur** est en lecture seule et concorde avec le type de serveur affiché dans Serveurs inscrits. Pour inscrire un autre type de serveur, cliquez sur **Moteur de base de données**, **Serveur d'analyse**, **Reporting Services**, **SQL Server Compact** **Edition**ou **Integration Services** dans la barre d'outils **Serveurs inscrits** avant de commencer à inscrire un nouveau serveur.  
  
 **Nom du serveur**  
 Sélectionnez le serveur auquel vous souhaitez vous connecter. Le dernier serveur avec lequel une connexion a été établie apparaît par défaut.  
  
 **Authentification**  
 Le mode d'authentification Windows permet à l'utilisateur de se connecter au moyen d'un compte d'utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **User name**  
 Cette option n'est pas disponible dans cette version.  
  
 **Mot de passe**  
 Cette option n'est pas disponible dans cette version.  
  
 **Mémoriser le mot de passe**  
 Cette option n'est pas disponible dans cette version.  
  
 **Nom du serveur inscrit**  
 Nom qui doit apparaître dans **Serveurs inscrits**. Ce nom ne doit pas correspondre à celui de la zone **Nom du serveur** .  
  
 **Description du serveur inscrit**  
 Entrez une description facultative du serveur.  
  
 **Test**  
 Cliquez sur cette option pour tester la connexion au serveur sélectionné dans la zone **Nom du serveur**. 
  

 
 
  
