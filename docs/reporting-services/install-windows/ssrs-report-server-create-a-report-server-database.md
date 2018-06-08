---
title: Créer une base de données du serveur de rapports (Gestionnaire de configuration de SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 09/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 6b45d3d8ec6b7268ff1f1b9069fc9b4fa6780c87
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-report-server-database"></a>Créer une base de données du serveur de rapports

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] **mode natif** utilise deux bases de données relationnelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le stockage des objets et des métadonnées du serveur de rapports. Une base de données est utilisée pour le stockage principal et l'autre pour le stockage des données temporaires. Les bases de données sont créées ensemble et liées par le nom. Avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par défaut, les bases de données ont pour nom respectif **reportserver** et **reportservertempdb**. Les deux bases de données sont collectivement appelées « base de données de serveur de rapports » ou « catalogue du serveur de rapports ».

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] **mode SharePoint** inclut une troisième base de données utilisée pour les métadonnées d’alerte des données. Les trois bases de données sont créées pour chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Par ailleurs, les noms de bases de données incluent par défaut un GUID qui représente l'application de service. Voici des exemples de noms des trois bases de données en mode SharePoint :

-   ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
-   ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
-   ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  
  
> [!IMPORTANT]  
>  N'écrivez pas d'applications qui exécutent des requêtes sur la base de données du serveur de rapports. La base de données du serveur de rapports n'est pas un schéma public. La structure des tables peut changer d'une version à la suivante. Si vous écrivez une application qui nécessite un accès à la base de données du serveur de rapports, utilisez toujours les API [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour accéder à la base de données du serveur de rapports.  
>   
>  Les vues du journal des exécutions constituent une exception à cette règle. Pour plus d’informations, consultez [Journal des exécutions du serveur de rapports et vue ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
## <a name="ways-to-create-the-report-server-database"></a>Méthodes pour créer la base de données de serveur de rapports  
 **Mode natif :** vous pouvez créer la base de données du serveur de rapports en mode natif en procédant comme suit :  
  
-   Automatiquement : utilisez l'Assistant Installation de SQL Server, si vous choisissez l'option d'installation de la configuration par défaut. Dans l’Assistant Installation de SQL Server, localisez **Installer et configurer** dans la page d’options d’installation du serveur de rapports. Si vous avez choisi l’option **Installer uniquement** , vous devez utiliser le Gestionnaire de configuration Reporting Services pour créer la base de données.  
  
-   Manuellement : utilisez le Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vous devez créer la base de données du serveur de rapports manuellement si vous utilisez un [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] distant pour héberger la base de données. Pour plus d’informations, consultez [Créer une base de données du serveur de rapports en mode natif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  
  
 **Mode SharePoint :** la page d’options d’installation du serveur de rapports n’offre que l’option **Installer uniquement** pour le mode SharePoint. Cette option installe tous les fichiers [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ainsi que le service partagé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . L'étape suivante consiste à créer au moins une application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de l'une des manières suivantes :  
  
-   Utilisez l'Administration centrale de SharePoint pour créer une application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez la section « Application de service » dans [Étape 3 : créer une application de service Reporting Services](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication).  
  
-   Utilisez les applets de commande PowerShell [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour créer une application de service et les bases de données du serveur de rapports. Pour plus d’informations, consultez l’exemple pour la création d’applications de service dans la rubrique [Applets de commande PowerShell pour le mode SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  
  
## <a name="database-server-version-requirements"></a>Conditions requises pour une version du serveur de bases de données  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisé pour héberger les bases de données du serveur de rapports. L'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] peut être soit locale, soit distante. Voici les versions prises en charge du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui permettent d'héberger les bases de données du serveur de rapports :  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 Pour créer la base de données du serveur de rapports sur un ordinateur distant, vous devez configurer la connexion de manière à employer un compte d'utilisateur de domaine ou un compte de service pouvant accéder au réseau. Si vous décidez d'utiliser une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante, réfléchissez avec soin aux informations d'identification que le serveur de rapports devra utiliser pour se connecter à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
> [!IMPORTANT]  
>  Le serveur de rapports et l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hébergeant la base de données du serveur de rapports peuvent appartenir à des domaines différents. Dans le cadre d'un déploiement Internet, il est courant d'utiliser un serveur situé derrière un pare-feu. Si vous configurez un serveur de rapports de manière à accéder à Internet, utilisez les informations d'identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vous connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] située derrière le pare-feu et recourez à IPSEC pour sécuriser la connexion.  
  
## <a name="database-server-edition-requirements"></a>Conditions requises pour une édition du serveur de bases de données  
 Lors de la création d’une base de données de serveur de rapports, soyez conscient que certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être utilisées pour héberger la base de données. Pour plus d’informations, consultez la section « Conditions requises pour l’édition SQL Server de la base de données du serveur de rapports » dans [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  

## <a name="next-steps"></a>Étapes suivantes

[Gestionnaire de configuration de Reporting Services](http://msdn.microsoft.com/en-us/63519ef4-e68a-42fb-9cf7-31228ea4e434)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
