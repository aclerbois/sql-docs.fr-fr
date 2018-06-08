---
title: Installer Reporting Services 2016 à partir de l’invite de commandes - SSRS | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- command line
ms.assetid: 048169b3-512c-41e4-895a-0416eff41268
caps.latest.revision: 11
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d81e8c8d8b75fd9557afdb99897b543eb8f9a6ff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="install-reporting-services-2016-at-the-command-prompt"></a>Installer Reporting Services 2016 à partir de l’invite de commandes

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge l’installation à partir de la ligne de commande du programme d’installation de SQL Server. Cette rubrique contient plusieurs exemples d'installations à partir de la ligne de commande spécifiques à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour obtenir une description complète des options de ligne de commande disponibles pour tous les composants SQL Server, consultez [Installer SQ  Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md). Cette rubrique ne décrit pas les options de ligne de commande du complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint. Pour plus d’informations sur l’installation du complément à partir de la ligne de commande, consultez [Installer le complément à l’aide du fichier d’installation rsSharePoint.msi](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md#bkmk_install_rssharepoint).

##  <a name="bkmk_native_mode"></a> Reporting Services - Mode natif

### <a name="rsinstallmode-native-mode"></a>RSINSTALLMODE (mode natif)
 Le paramètre d’entrée principal pour installer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est le paramètre d’entrée **/RSINSTALLMODE** . Le paramètre présente deux options : **DefaultNativeMode** et **FilesOnlyMode**.  
  
 Si le programme d'installation inclut le moteur de base de données SQL Server, la valeur par défaut RSINSTALLMODE est DefaultNativeMode. Si l'installation n'inclut pas le moteur de base de données SQL Server, la valeur par défaut RSINSTALLMODE est FilesOnlyMode. Si vous choisissez DefaultNativeMode mais que l'installation n'inclut pas le moteur de base de données SQL Server, le programme d'installation modifie automatiquement RSINSTALLMODE en FilesOnlyMode. Pour plus d’informations sur le paramètre d’entrée, consultez [Installer SQL Server 2016 à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).

### <a name="examples-of-native-mode-installation"></a>Exemples d'installation en mode natif

 L’exemple suivant porte sur l’installation des éléments ci-après et configure les comptes pour :  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif.  
  
-   Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
-   SQL Server Agent, qui est requis pour les fonctionnalités d’abonnement associées à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
```  
Setup.exe /q /IACCEPTSQLSERVERLICENSETERMS /ACTION="install" /ERRORREPORTING=1 /UPDATEENABLED="False" /INSTANCENAME="MSSQLSERVER" /FEATURES="SQLEngine,Adv_SSMS,RS" /RSINSTALLMODE="DefaultNativeMode" /SQLSVCACCOUNT="[DOMAIN\ACCOUNT]" /SQLSVCPASSWORD="[PASSWORD]" /AGTSVCACCOUNT="[DOMAIN\ACCOUNT]" /AGTSVCPASSWORD="[PASSWORD]" /SQLSYSADMINACCOUNTS="[DOMAIN\ACCOUNT]"  
```  
  
##  <a name="bkmk_sharepoint_mode"></a> Mode SharePoint [Reporting Services]  
  
### <a name="rsshpinstallmode-sharepoint-mode"></a>RSSHPINSTALLMODE (mode SharePoint)  
 Le paramètre d’entrée pour installer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint est **/RSSHPINSTALLMODE**. Le paramètre d'entrée a une option : SharePointFilesOnlyMode. L'option installe tous les fichiers nécessaires pour le mode SharePoint mais une configuration est requise après l'installation. Les étapes de configuration supplémentaires sont complétées à l'aide de l'Administration centrale de SharePoint. Pour plus d’informations, consultez [Installer le premier serveur de rapports en mode SharePoint](http://msdn.microsoft.com/en-us/b29d0f45-0068-4c84-bd7e-5b8a9cd1b538).  
  
### <a name="examples-of-sharepoint-mode-installation"></a>Exemples d'installation en mode SharePoint  
 L'exemple suivant installe SQL Server, le service moteur de base de données et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint ainsi que le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour SharePoint (RS_SHPWFE).  
  
```  
setup /q /ACTION=install /FEATURES=SQL, RS_SHP, RS_SHPWFE,TOOLS /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="BUILTIN\ADMINISTRATORS" /RSSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE" /SQLSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE" /AGTSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE"  
```  
  
 L'exemple suivant installe uniquement le mode SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
```  
Setup.exe /q /ACTION="Install" /IACCEPTSQLSERVERLICENSETERMS /FEATURES="RS_SHP" /INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /INSTALLSHAREDDIR="C:\Program Files\Microsoft SQL Server" /INSTALLSHAREDWOWDIR="C:\Program Files (x86)\Microsoft SQL Server" /INSTALLSQLDATADIR="C:\Program Files\Microsoft SQL Server" /SECURITYMODE="SQL" /SAPWD="[PASSWORD]" /PID="[Your PID Value]" /SQLSYSADMINACCOUNTS="[Account Name]" "AutoSql Admin Group" /ASSYSADMINACCOUNTS="[Account Name]" /UPDATEENABLED="False"  
  
```  
  
### <a name="examples-of-sharepoint-mode-upgrade"></a>Exemples de mise à niveau en mode SharePoint  
 L'exemple suivant met à niveau le mode SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . **RSUPGRADEPASSWORD** est le mot de passe du compte de service Report Server existant. RSUPGRADEPASSWORD est un champ obligatoire dans un scénario de mise à niveau, à moins que le compte de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne soit un compte intégré.  
  
```  
Setup.exe /q /ACTION="Upgrade" /INSTANCENAME="MSSQLSERVER" /PID="[PID value]" /FTSVCACCOUNT="[DOMAIN\ACCOUNT]" /FTSVCPASSWORD="[PASSWORD]" /UPDATEENABLED="False" /IACCEPTSQLSERVERLICENSETERMS /RSUPGRADEPASSWORD="[PASSWORD]"  
```  
  
 L'exemple suivant peut être utilisé pour mettre à niveau une installation en mode SharePoint basée sur l'architecture de service partagé SharePoint. L'exemple suivant utilise le commutateur ALLOWUPGRADEFORSSRSSHAREPOINTMODE. Le commutateur n'est pas nécessaire pour mettre à niveau des versions antérieures qui ne sont pas basées sur l'architecture de service partagé :  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
```  
Setup.exe /q /ACTION="Upgrade" /INSTANCENAME="MSSQLSERVER" /PID="[Your PID Value]" /FTSVCACCOUNT="[ACCOUNT Name]" /FTSVCPASSWORD="[PASSWORD]" /UPDATEENABLED="False" /IACCEPTSQLSERVERLICENSETERMS /ALLOWUPGRADEFORSSRSSHAREPOINTMODE="True"  
```

## <a name="next-steps"></a>Étapes suivantes

[Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
[Paramètres SysPrep](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#SysPrep)   
[Installer Power Pivot à partir de l’invite de commandes](http://msdn.microsoft.com/en-us/7f1f2b28-c9f5-49ad-934b-02f2fa6b9328)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
