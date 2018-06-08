---
title: Utiliser plusieurs versions et instances de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/25/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- concurrent installations [SQL Server]
- versions [SQL Server], multiple
- side-by-side installations [SQL Server]
- multiple SQL Server component versions
- installing SQL Server, side-by-side installations
- Setup [SQL Server], side-by-side installations
- 64-bit edition [SQL Server]
- 32-bit edition [SQL Server]
- editions [SQL Server], side-by-side installations
ms.assetid: 93acefa8-bb41-4ccc-b763-7801f51134e0
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ff1d58e3a860083b764c142dbb0d437e8b449de5
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772715"
---
# <a name="work-with-multiple-versions-and-instances-of-sql-server"></a>Utiliser plusieurs versions et instances de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge plusieurs instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)], d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]et de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur le même ordinateur. Vous pouvez également mettre à niveau des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un ordinateur où des versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont déjà installées. Pour connaître les scénarios de mise à niveau pris en charge, consultez [Mises à niveau de la version et de l’édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
## <a name="version-components-and-numbering"></a>Composants et numérotation de version  
 Les concepts suivants sont utiles pour comprendre le comportement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les instances côte à côte de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le format de version de produit standard pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est MM.nn.bbbb.rr, où chaque segment est défini comme suit :  
  
 MM - Version principale  
  
 nn - Version secondaire  
  
 bbbb - Numéro de build  
  
 rr - Numéro de révision de build  
  
 Dans chaque version finale principale ou secondaire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le numéro de version est incrémenté afin de la différencier des versions antérieures. Cette modification de la version est utilisée à de nombreuses fins. Cela comprend l'affichage des informations sur la version dans l'interface utilisateur, le contrôle de la manière dont les fichiers sont remplacés pendant la mise à niveau, l'application des Service Packs, et cela joue également le rôle de mécanisme pour la différentiation fonctionnelle entre les versions consécutives.  
  
### <a name="components-shared-by-all-versions-of-includessnoversionincludesssnoversion-mdmd"></a>Composants partagés par toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Certains composants sont partagés par toutes les instances de toutes les versions installées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Lorsque vous installez des versions différentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] côte à côte sur le même ordinateur, ces composants sont mis à niveau automatiquement vers la version la plus récente. Ces composants sont habituellement désinstallés automatiquement lorsque la dernière instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est désinstallée.  
  
 Exemples : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser et Enregistreur Microsoft VSS [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="components-shared-across-all-instances-of-the-same-major-version-of-includessnoversionincludesssnoversion-mdmd"></a>Composants partagés par toutes les instances de la même version principale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les versions qui ont le même numéro de version principale partagent certains composants d’une instance à l’autre. Si les composants partagés sont sélectionnés pendant la mise à niveau, les composants existants sont mis à niveau vers la version la plus récente.  
  
 Exemples : [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]et documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="components-shared-across-minor-versions"></a>Composants partagés par les versions secondaires  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les versions qui ont le même numéro de version principale.secondaire partagent des composants.  
  
 Exemple : fichiers de support d'installation.  
  
### <a name="components-specific-to-an-instance-of-includessnoversionincludesssnoversion-mdmd"></a>Composants spécifiques à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Certains composants ou services de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont spécifiques à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ils sont également définis comme étant dépendants de l'instance. Ils partagent la même version que l'instance qui les héberge et sont utilisés exclusivement pour cette instance.  
  
 Exemples : [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
### <a name="components-that-are-independent-of-the-includessnoversionincludesssnoversion-mdmd-versions"></a>Composants qui sont indépendants des versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Certains composants sont installés pendant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mais sont indépendant des versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ils peuvent être partagés par les versions principales ou par toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Exemples : Microsoft Sync Framework, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, consultez [Installer SQL Server 2016 avec l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md). Pour plus d’informations sur la désinstallation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, consultez [Désinstaller une instance existante de SQL Server &#40;programme d’installation&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md).  
  
## <a name="using-includessnoversionincludesssnoversion-mdmd-side-by-side-with-previous-versions-of-includessnoversionincludesssnoversion-mdmd"></a>Utilisation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] côte à côte avec les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Vous pouvez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un ordinateur exécutant déjà les instances d'une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si une instance par défaut existe sur l'ordinateur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être installé comme instance nommée.  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep ne prend pas en charge une installation côte à côte d’instances préparées de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] avec des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le même ordinateur. Par exemple, vous ne pouvez pas préparer d'instance [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] côte à côte avec une instance préparée de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Toutefois, vous pouvez installer plusieurs instances préparées de la même version principale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] côte à côte sur le même ordinateur. Pour plus d'informations, consultez [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
>   
>  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] ne peut pas être installé côte à côte avec des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un ordinateur qui exécute Windows Server 2008 R2 Server Core SP1. Pour plus d’informations sur les installations Server Core, consultez [Installer SQL Server 2016 sur Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md).  
  
Le tableau suivant répertorie la prise en charge côte à côte de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]:  
  
|Instance existante de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]|Prise en charge côte à côte|  
|--------------------------------------------------|----------------------------|  
|[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)] <br /><br /> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|  

Le tableau suivant montre la prise en charge côte à côte de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] avec les versions précédentes :  
  
|Instance existante de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|Prise en charge côte à côte avec les versions antérieures|  
|--------------------------------------------------|----------------------------|  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (32 bits)<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (64 bits) [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]|  

  
## <a name="preventing-ip-address-conflicts"></a>Éviter les conflits d'adresse IP  
 Lorsqu'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installée côte à côte avec une instance autonome de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], prenez soin d'éviter les conflits de numéro de port TCP sur les adresses IP. Les conflits se produisent généralement lorsque deux instances de [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont configurées pour utiliser le port TCP par défaut (1433). Pour éviter des conflits, configurez une instance pour utiliser un port fixe non défini par défaut. La configuration d'un port fixe est généralement plus simple sur l'instance autonome. La configuration de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour utiliser des ports différents empêche un conflit inattendu adresse IP/port TCP qui bloque un démarrage de l'instance lorsqu'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] échoue au nœud en attente  
  
## <a name="see-also"></a> Voir aussi  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Installer SQL Server à partir de l'Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)   
 [Mises à niveau de la version et de l'édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Mettre à niveau SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Compatibilité descendante_supprimé](http://msdn.microsoft.com/library/15d9117e-e2fa-4985-99ea-66a117c1e9fd)  
  
  
