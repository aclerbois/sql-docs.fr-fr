---
title: Fichiers de configuration de Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- deploying [Reporting Services], configuration files
- configuration options [Reporting Services]
- modifying configuration files
- configuration files [Reporting Services]
ms.assetid: 21e5c32f-ad67-4917-b55a-8e21bd64f5a6
caps.latest.revision: 52
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1ce4e8cb5b6bc1e1bf617d1abbd4b5f1a0afcf1a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-configuration-files"></a>Fichiers de configuration de Reporting Services
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke les informations sur les composants dans le Registre et dans des fichiers de configuration qui sont copiés dans le système de fichiers au cours de l'installation. Les fichiers de configuration contiennent une combinaison de valeurs destinées à un usage interne uniquement et de valeurs définies par l'utilisateur. Les valeurs définies par l'utilisateur sont spécifiées par le biais du programme d'installation, des outils de configuration, des utilitaires de ligne de commande ou en modifiant manuellement les fichiers de configuration.  
  
 La modification des fichiers de configuration est uniquement nécessaire si vous ajoutez ou configurez des paramètres avancés. Les paramètres de configuration sont spécifiés soit comme des éléments, soit comme des attributs XML. Si le langage XML et les fichiers de configuration vous sont familiers, vous pouvez modifier les paramètres définissables par l'utilisateur dans un éditeur de texte ou de code. Pour plus d’informations sur la modification d’un fichier de configuration, ou pour en savoir plus sur la façon dont le serveur de rapports lit les paramètres de configuration nouveaux et mis à jour, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
> [!NOTE]  
>  Dans les versions antérieures, le Gestionnaire de rapports avait son propre fichier de configuration nommé RSWebApplication.config. Ce fichier est maintenant obsolète. Si vous avez effectué une mise à niveau à partir d'une installation antérieure, le fichier n'est pas supprimé ; toutefois, le serveur de rapports ne lira pas les paramètres qu'il contient. Si le fichier existe sur votre ordinateur, vous devez le supprimer. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, tous les paramètres de configuration du Gestionnaire de rapports sont stockés et lus dans le fichier RSReportServer.config. Pour obtenir une liste des paramètres qui ont été supprimés ou déplacés, consultez [Modifications importantes de SQL Server Reporting Services dans SQL Server 2016](../../reporting-services/breaking-changes-in-sql-server-reporting-services-in-sql-server-2016.md).  
  
 Dans cette rubrique :  
  
-   [Résumé des fichiers de configuration (mode natif)](#bkmk_config_file_Summary_native_mode)  
  
-   [Résumé des fichiers de configuration (mode SharePoint)](#bkmk_config_file_Summary_sharepoint_mode)  
  
##  <a name="bkmk_config_file_Summary_native_mode"></a> Résumé des fichiers de configuration (mode natif)  
 Le tableau suivant fournit une description de l'emplacement de stockage des paramètres de configuration. La plupart des paramètres de configuration sont stockés dans des fichiers de configuration inclus dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Par défaut, le répertoire d'installation est le suivant :  
  
```  
C:\Program Files\Microsoft SQL Server\MSRS12.MSSQLSERVER  
```  
  
|Stockage dans :|Description|Emplacement|  
|----------------|-----------------|--------------|  
|RSReportServer.config|Stocke les paramètres de configuration des fonctionnalités du service Report Server : Gestionnaire de rapports, service Web Report Server et traitement en arrière-plan. Pour plus d’informations sur chaque paramètre, consultez [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).|\<Répertoire d’installation>\Reporting Services\ReportServer|  
|RSSrvPolicy.config|Stocke les stratégies de sécurité d'accès du code pour les extensions serveur. Pour plus d'informations sur ce fichier, consultez [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|\<Répertoire d’installation>\Reporting Services\ReportServer|  
|RSMgrPolicy.config|Stocke les stratégies de sécurité d'accès du code pour le Gestionnaire de rapports. Pour plus d'informations sur ce fichier, consultez [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|\<Répertoire d’installation>\Reporting Services\ReportManager|  
|Web.config pour le service Web Report Server|Inclut uniquement les paramètres requis pour ASP.NET.|\<Répertoire d’installation>\Reporting Services\ReportServer|  
|Web.config pour le Gestionnaire de rapports|Inclut uniquement les paramètres requis pour ASP.NET.|\<Répertoire d’installation>\Reporting Services\ReportManager|  
|ReportingServicesService.exe.config|Stocke les paramètres de configuration qui spécifient les niveaux de trace et les options de journalisation du service Report Server. Pour plus d'informations sur les éléments de ce fichier, consultez [ReportingServicesService Configuration File](../../reporting-services/report-server/reportingservicesservice-configuration-file.md).|\<Répertoire d’installation>\Reporting Services\ReportServer\Bin|  
|Paramètres du Registre|Stocke l'état de la configuration et d'autres paramètres utilisés pour la désinstallation de Reporting Services. Si vous résolvez un problème d'installation ou de configuration, vous pouvez consulter ces paramètres afin d'obtenir des informations sur le mode de configuration du serveur de rapports.<br /><br /> Ne modifiez pas directement ces paramètres, car cela risque de rendre votre installation non valide.|HKEY_LOCAL_MACHINE \SOFTWARE \Microsoft \Microsoft SQL Server \\<ID_Instance\> \Setup<br /><br /> **- et -**<br /><br /> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\Services\ReportServer|  
|RSReportDesigner.config|Stocke les paramètres de configuration du Concepteur de rapports. Pour plus d’informations, consultez [RSReportDesigner Configuration File](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).|\<lecteur>:\Program Files\Microsoft Visual Studio 10\Common7\IDE\PrivateAssemblies.|  
|RSPreviewPolicy.config|Stocke les stratégies de sécurité d'accès du code pour les extensions serveur utilisées pendant l'aperçu du rapport. Pour plus d'informations sur ce fichier, consultez [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|C:\Program Files\Microsoft Visual Studio 10.0\Common7\IDE\PrivateAssembliesr|  
  
##  <a name="bkmk_config_file_Summary_sharepoint_mode"></a> Résumé des fichiers de configuration (mode SharePoint)  
 Le tableau suivant fournit une description des fichiers de configuration utilisés pour un serveur de rapports en mode SharePoint. La plupart des paramètres de configuration sont stockés dans les bases de données d'application du service SharePoint. Pour plus d’informations, consultez [Services Reporting Services SharePoint et applications de service](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).  
  
 Par défaut, le répertoire d'installation pour le mode SharePoint est le suivant :  
  
```  
C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting  
```  
  
|Stockage dans :|Description|Emplacement|  
|----------------|-----------------|--------------|  
|RSReportServer.config|Stocke les paramètres de configuration des fonctionnalités du service Report Server : Gestionnaire de rapports, service Web Report Server et traitement en arrière-plan. Pour plus d’informations sur chaque paramètre, consultez [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).|\<Répertoire d’installation>\Reporting Services\ReportServer|  
|RSSrvPolicy.config|Stocke les stratégies de sécurité d'accès du code pour les extensions serveur. Pour plus d'informations sur ce fichier, consultez [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|\<Répertoire d’installation>\Reporting Services\ReportServer|  
|Web.config pour le service Web Report Server|Inclut uniquement les paramètres requis pour ASP.NET.|\<Répertoire d’installation>\Reporting Services\ReportServer|  
|Paramètres du Registre|Stocke l'état de la configuration et d'autres paramètres utilisés pour la désinstallation de Reporting Services. Stocke également des informations sur chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .<br /><br /> Ne modifiez pas directement ces paramètres, car cela risque de rendre votre installation non valide.|HKEY_LOCAL_MACHINE \SOFTWARE \Microsoft \Microsoft SQL Server \\<ID_Instance\> \Setup<br /><br /> Exemple d’ID d’instance : MSSQL13.MSSQLSERVER<br /><br /> **- et -**<br /><br /> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\Reporting Services\Service Applications|  
|RSReportDesigner.config|Stocke les paramètres de configuration du Concepteur de rapports. Pour plus d’informations, consultez [RSReportDesigner Configuration File](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).|\<lecteur>:\Program Files\Microsoft Visual Studio 10\Common7\IDE\PrivateAssemblies.|  
  
## <a name="see-also"></a> Voir aussi  
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Extensions Reporting Services](../../reporting-services/extensions/reporting-services-extensions.md)   
 [Utilitaire rsconfig &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md)   
 [Démarrer et arrêter le service du serveur de rapports](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)  
  
  
