---
title: Afficher et lire les fichiers journaux d’installation de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing logs
- displaying log files
- Setup [SQL Server], logs
- installation log files [SQL Server]
- installing SQL Server, logs
- errors [SQL Server], Setup
- logs [SQL Server], Setup
ms.assetid: 9d77af64-9084-4375-908a-d90f99535062
caps.latest.revision: 54
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4d3b40219a412b498e891d0550a10a99e23b7adf
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771455"
---
# <a name="view-and-read-sql-server-setup-log-files"></a>Afficher et lire les fichiers journaux d’installation de SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Par défaut, le programme d’installation de SQL Server crée des fichiers journaux dans un dossier de journal horodaté à l’emplacement %programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log. Le format du nom du dossier de journal horodaté est AAAAMMJJ_hhmmss. Quand le programme d’installation est exécuté en mode sans assistance, les journaux sont créés dans %temp%\sqlsetup*.log. Tous les fichiers du dossier de journal sont archivés dans le fichier Log\*.cab dans leur dossier de journal respectif.  

 ![setup-bootstrap-example.png](media/view-and-read-sql-server-setup-log-files/setup-bootstrap-example.png)

 >[!NOTE]
 > Les nombres figurant dans le chemin *nnn* correspondent à la version installée de SQL. Dans l’image ci-dessus, SQL 2017 a été installé. De ce fait, le dossier est 140. Pour SQL 2016, le dossier aurait été 130 et pour SQL 2014, il aurait été 120.
  
 Le programme d’installation de SQL Server effectue trois étapes simples : 
  
1.  Vérification des règles globales : valide la configuration système requise de base
2.  Mise à jour des composants : vérifie si des mises à jour sont disponibles pour le média installé
3.  Action demandée par l’utilisateur : autorise l’utilisateur à sélectionner et personnaliser des fonctionnalités
  

Ce flux de travail génère un journal résumé et un journal détaillé dans le cas d’une installation RTM ou deux journaux détaillés quand le média est intégré.
  
Les fichiers de banque de données contiennent un instantané de l’état de tous les objets de configuration qui sont suivis par le processus d’installation et qui sont utiles pour réparer les erreurs de configuration. Des fichiers de vidage XML sont créés à chaque phase d’exécution et sont enregistrés dans le sous-dossier de journal de banque de données sous le dossier de journal horodaté. 

Les sections suivantes décrivent les fichiers journaux d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="summarytxt-file"></a>Fichier Summary.txt 
  
### <a name="overview"></a>Vue d'ensemble  
 Ce fichier montre les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détectés au cours de l'installation, l'environnement du système d'exploitation, les valeurs des paramètres de ligne de commande (si elles sont spécifiées) et l'état d'ensemble de chaque fichier MSI/MSP exécuté.
  
 Le journal comprend les sections suivantes :
  
-   un résumé d'ensemble de l'exécution ;  
-   les propriétés et la configuration de l'ordinateur sur lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été exécuté ;  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] précédemment installées sur l'ordinateur ;  
-   la description des propriétés de la version d'installation et du package d'installation ;
-   les paramètres d'entrée d'exécution fournis au cours de l'installation ;  
-   l'emplacement du fichier de configuration ;  
-   les détails des résultats d'exécution ;  
-   les règles globales ;  
-   les règles spécifiques au scénario d'installation ;  
-   les règles ayant échoué ;  
-   l'emplacement du fichier du rapport de règles.


  >[!NOTE]
  > Notez qu’à l’occasion de l’application de correctifs, plusieurs sous-dossiers (un pour chaque instance à laquelle est appliquée le correctif et un pour les fonctionnalités partagées) peuvent contenir un ensemble de fichiers similaire (c’est-à-dire, %programfiles%\MicrosoftSQL Server\130\Setup Bootstrap\Log\<AAAAMMJJ_HHMM>\MSSQLSERVER). 
  
### <a name="location"></a>Emplacement  
 Le fichier summary.txt se trouve à l’emplacement %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\.
  
 Pour trouver les erreurs dans le fichier texte résumé, recherchez les mots clés « error » ou « failed » dans le fichier.
  
## <a name="summarymachinenameyyyymmddhhmmsstxt-file"></a>Fichier Summary_\<NomOrdinateur>_AAAAMMJJ_HHMMss.txt
  
### <a name="overview"></a>Vue d'ensemble  
 Le fichier de base summary_engine est semblable au fichier résumé et est généré au cours du flux de travail principal.
  
### <a name="location"></a>Emplacement  
 Le fichier Summary_\<NomOrdinateur>_AAAAMMJJ_HHMMss.txt se trouve à l’emplacement % programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<AAAAMMJJ_HHMM>\\.
  
  
## <a name="detailtxt-file"></a>Fichier Detail.txt
  
### <a name="overview"></a>Vue d'ensemble
 Detail.txt est généré pour le flux de travail principal, par exemple l'installation ou la mise à niveau, et fournit les détails de l'exécution. Les journaux contenus dans le fichier sont générés en fonction de l’heure à laquelle chaque action d’installation a été appelée. Le fichier texte indique l’ordre d’exécution des actions, ainsi que leurs dépendances.  
  
### <a name="location"></a>Emplacement  
 Le fichier detail.txt se trouve à l’emplacement %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<AAAAMMJJ_HHMM>\Detail.txt.  
  
 Si une erreur se produit pendant l’installation, l’exception ou l’erreur est consignée à la fin de ce fichier. Pour trouver les erreurs dans ce fichier, examinez d’abord la fin du fichier, puis lancez une recherche sur les mots clés « error » ou « exception » dans le fichier.
    
## <a name="msi-log-files"></a>Fichiers journaux MSI
  
### <a name="overview"></a>Vue d'ensemble  
 Les fichiers journaux MSI fournissent des détails sur le processus du package d'installation. Ils sont générés par le programme MSIEXEC lors de l'installation du package spécifié.  
  
 Types de fichiers journaux MSI :
  
-   \<Fonctionnalité>_\<Architecture>\_\<Interaction>.log   
-   \<Fonctionnalité>_\<Architecture>\_\<Langue\_\<Interaction>.log   
-   \<Fonctionnalité>_\<Architecture>\_\<Interaction>\_\<FLux_de_travail>.log  
  
### <a name="location"></a>Emplacement  
 Les fichiers journaux MSI se trouvent à cet emplacement : %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<AAAAMMJJ_HHMM>\\<Nom\>.log.  
  
 À la fin du fichier se trouve un résumé de l’exécution, qui comprend l’état de réussite ou d’échec, ainsi que les propriétés. Pour trouver l’erreur dans le fichier MSI, recherchez « value 3 » et examinez le texte situé avant et après.  
  
## <a name="configurationfileini-file"></a>Fichier ConfigurationFile.ini
  
### <a name="overview"></a>Vue d'ensemble  
 Le fichier de configuration contient les paramètres d'entrée fournis au cours de l'installation. Vous pouvez l'utiliser pour redémarrer l'installation sans entrer les paramètres manuellement. Toutefois, les mots de passe pour les comptes, PID et certains paramètres ne sont pas enregistrés dans le fichier de configuration. Les paramètres peuvent être soit ajoutés au fichier, soit fournis à l'aide de la ligne de commande ou de l'interface utilisateur du programme d'installation. Pour plus d’informations, consultez [Installer SQL Server 2016 à l’aide d’un fichier de configuration](../../database-engine/install-windows/install-sql-server-2016-using-a-configuration-file.md).  
  
### <a name="location"></a>Emplacement  
 Le fichier Configuration File.ini se trouve à l’emplacement %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<AAAAMMJJ_HHMM>\\.  
  
## <a name="systemconfigurationcheckreporthtm-file"></a>Fichier SystemConfigurationCheck_Report.htm
  
### <a name="overview"></a>Vue d'ensemble  
 Le rapport de vérification de la configuration du système contient une brève description de chaque rôle exécuté et de l'état d'exécution.
  
### <a name="location"></a>Emplacement  
Le fichier SystemConfigurationCheck_Report.htm se trouve à l’emplacement %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<AAAAMMJJ_HHMM>\\.
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server 2017](../../database-engine/install-windows/install-sql-server.md)
