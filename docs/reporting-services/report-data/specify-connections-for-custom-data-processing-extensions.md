---
title: Spécifier des connexions pour des extensions de traitement de données personnalisées | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- custom data processing extensions [Reporting Services]
- IDbConnection interface, connection strings
- impersonation [Reporting Services]
- IDbConnectionExtension interface, connection strings
- data sources [Reporting Services], security
- connection strings [Reporting Services]
- credentials [Reporting Services]
- authentication [Reporting Services]
- external data sources [Reporting Services]
- data processing extensions [Reporting Services], connections
ms.assetid: 2cddc9ea-0e28-4350-80ae-332412908e47
caps.latest.revision: 20
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 7c0b6fa4f93320e0648ffb1923aaf593ff708b53
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34550500"
---
# <a name="specify-connections-for-custom-data-processing-extensions"></a>Spécifier des connexions pour des extensions de traitement de données personnalisées
  Vous pouvez créer des extensions pour le traitement des données personnalisées ou utiliser des extensions tierces sur un serveur de rapports, soit pour améliorer la capacité de traitement des sources de données prises en charge, soit pour prendre en charge des types de données supplémentaires qui ne sont pas disponibles dans une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] par défaut. Les connexions sont traitées différemment en fonction de l'implémentation. Les implémentations suivantes sont disponibles pour les extensions de traitement de données :  
  
-   Fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] personnalisés (si vous accédez à des données provenant de sources de données DB2.NET, Oracle, ODP.NET ou Teradata, vous pouvez utiliser un fournisseur de données .NET personnalisé)  
  
-   Les extensions de traitement de données personnalisées qui prennent en charge <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>  
  
-   Les extensions de traitement de données personnalisées qui prennent en charge <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>  
  
> [!NOTE]  
>  Demandez à votre fournisseur tiers comment votre extension de traitement de données personnalisée est implémentée.  
  
## <a name="impersonation-and-custom-data-processing-extensions"></a>Emprunt d'identité et extensions de traitement de données personnalisées  
 Si votre extension de traitement de données personnalisée se connecte à des sources de données utilisant l’emprunt d’identité, vous devez utiliser la méthode Open sur les interfaces <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> ou <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> pour effectuer la demande. Vous pouvez également stocker l’objet d’identité utilisateur (System.Security.Principal.WindowsIdentity), puis le réutiliser dans d’autres API d’extension de traitement de données.  
  
 Dans les versions antérieures de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], toutes les extensions pour le traitement des données personnalisées étaient appelées via un emprunt d'identité utilisateur. Dans cette version, seule la méthode Open est appelée lors de l’emprunt de l’identité utilisateur. Si vous avez une extension de traitement de données existante nécessitant une sécurité intégrée, vous devez modifier votre code de manière à utiliser la méthode Open ou stocker l’objet d’identité utilisateur.  
  
## <a name="connections-for-custom-net-framework-data-providers"></a>Connexions pour les fournisseurs de données .NET Framework personnalisés  
 Lorsque vous configurez une source de données spécifique, vous définissez des propriétés qui déterminent le type de source de données, la chaîne de connexion et les informations d'identification qui sont utilisées pour accéder à la source de données. Le tableau suivant décrit les types d'informations d'identification qui sont pris en charge pour les fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Pour plus d’informations sur la définition des propriétés de la source de données de rapport, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
|Informations d'identification|Connexions|  
|-----------------|-----------------|  
|Sécurité intégrée|Si votre fournisseur de données la prend en charge, vous pouvez utiliser la sécurité intégrée Windows. La demande est envoyée en utilisant les informations d'identification de l'utilisateur actuel.<br /><br /> Lors de la définition de la chaîne de connexion, n’oubliez pas d’inclure les arguments qui spécifient une sécurité intégrée (par exemple, une connexion à une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut inclure **Integrated Security=SSPI** dans la chaîne de connexion).|  
|Authentification Windows|Si votre fournisseur de données le prend en charge, vous pouvez utiliser un compte d'utilisateur de domaine Windows. Le serveur de rapports emprunte l'identité du compte d'utilisateur avant l'appel de l'extension de traitement de données.<br /><br /> Lors de la définition de la chaîne de connexion, n’oubliez pas d’inclure les arguments qui spécifient une sécurité intégrée (par exemple, une connexion à une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut inclure **Integrated Security=SSPI** dans la chaîne de connexion).|  
|Informations d'identification de la base de données|L'authentification de la base de données n'est pas prise en charge pour les connexions établies au moyen d'un fournisseur de données .NET personnalisé. Dans tous les cas, le serveur de rapports ne pourra pas établir la connexion.|  
|Ne pas demander les informations d'identification|Vous pouvez utiliser l'option Ne pas demander les informations d'identification avec les fournisseurs de données .NET personnalisés. Si le compte d'exécution sans assistance est spécifié, la chaîne de connexion détermine les informations d'identification qui sont utilisées. Le serveur de rapports emprunte l'identité du compte d'exécution sans assistance pour établir la connexion.<br /><br /> Si le compte d'exécution sans assistance n'est pas défini, le serveur de rapports ne peut pas établir la connexion. Pour plus d’informations sur la définition du compte, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).|  
  
## <a name="connections-for-idbconnection"></a>Connexions pour IDbConnection  
 Si vous utilisez une extension de traitement de données personnalisée qui prend uniquement en charge <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>, vous devez spécifier la connexion de cette façon :  
  
1.  Configurer le compte d'exécution sans assistance. La configuration de ce compte est requise pour les connexions établies à l'aide de **IDbConnection**. Le serveur de rapports emprunte l'identité du compte lors de l'établissement de la connexion.  
  
2.  Configurez les propriétés de la source de données sur le rapport de façon à utiliser **Ne pas demander les informations d'identification**.  
  
3.  Incluez dans la chaîne de connexion les informations d'identification utilisées pour établir une connexion à la source de données.  
  
 Lors de l'utilisation d' **IDbConnection**, les types d'informations d'identification suivants ne sont pas pris en charge : sécurité intégrée, comptes d'utilisateur Windows et informations d'identification de base de données. Si la connexion à la source de données utilise ces options, la connexion échoue sur le serveur de rapports.  
  
## <a name="connections-for-idbconnectionextension"></a>Connexions pour IDbConnectionExtension  
 Si vous utilisez une extension de traitement de données personnalisée qui prend en charge <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>, vous pouvez spécifier la connexion de différentes façons :  
  
|Informations d'identification|Connexions|  
|-----------------|-----------------|  
|Sécurité intégrée|Si votre fournisseur de données la prend en charge, vous pouvez utiliser la sécurité intégrée Windows avec des extensions de traitement de données personnalisées qui utilisent **IDbConnectionExtension**.<br /><br /> Lors de la définition de la chaîne de connexion, n’oubliez pas d’inclure les arguments qui spécifient une sécurité intégrée (par exemple, une connexion à une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut inclure **Integrated Security=SSPI** dans la chaîne de connexion).|  
|Authentification Windows|Si votre fournisseur de données le prend en charge, vous pouvez utiliser un compte d'utilisateur de domaine Windows avec des extensions de traitement de données personnalisées qui utilisent **IDbConnectionExtension**.<br /><br /> Le serveur de rapports emprunte l'identité du compte d'utilisateur avant l'appel de l'extension de traitement de données. Lors de la définition de la chaîne de connexion, n’oubliez pas d’inclure les arguments qui spécifient une sécurité intégrée (par exemple, une connexion à une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut inclure **Integrated Security=SSPI** dans la chaîne de connexion).|  
|Informations d'identification de la base de données|Vous pouvez utiliser l'authentification de base de données pour configurer des connexions pour des extensions de traitement de données personnalisées qui utilisent **IDbConnectionExtension**.|  
|Ne pas demander les informations d'identification|Si le compte d'exécution sans assistance est spécifié, la chaîne de connexion détermine les informations d'identification qui sont utilisées.<br /><br /> Si le compte d'exécution sans assistance n'est pas défini, le serveur de rapports ne peut pas établir la connexion.|  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)   
 [Spécifier des informations d'identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Implémentation d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Configurer des propriétés de la source de données d'un rapport](../../reporting-services/report-data/configure-data-source-properties-for-a-report-report-manager.md)  
  
  
