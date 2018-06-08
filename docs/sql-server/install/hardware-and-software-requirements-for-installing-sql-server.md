---
title: Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Setup [SQL Server], software
- software [SQL Server]
- installing SQL Server, software
- operating systems [SQL Server], SQL Server requirements
- Setup [SQL Server], cross-language support
- operating systems [SQL Server], cross-language support
- network connections [SQL Server], requirements
- disk space [SQL Server], SQL Server installations
- WOW [SQL Server]
- Setup [SQL Server], hardware
- dependencies [SQL Server], SQL Server installations
- cluster hardware requirements [SQL Server]
- endpoints [SQL Server], SQL Server installations
- Internet [SQL Server], SQL Server installations
- hardware [SQL Server]
- Windows on Windows [SQL Server]
- installing SQL Server, hardware
- Setup Configuration Checker
- SCC [SQL Server]
- operating systems [SQL Server]
- space [SQL Server], SQL Server installations
- system configuration checker
- installing SQL Server, cross-language support
- Internet [SQL Server]
- space [SQL Server]
- extended system support [SQL Server]
- 64-bit edition [SQL Server]
- failover clustering [SQL Server]
- failover clustering [SQL Server], hardware requirements
- 32-bit edition [SQL Server]
- locales [SQL Server], SQL Server installations
- cross-language support
- disk space [SQL Server]
- localized SQL Server versions
ms.assetid: 09bcf20b-0a40-4131-907f-b61479d5e4d8
caps.latest.revision: 333
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: aef7f10021c179b8ab6a6498bbe251159b8bfe7a
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34773185"
---
# <a name="hardware-and-software-requirements-for-installing-sql-server"></a>Configurations matérielle et logicielle requises pour l'installation de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article liste les configurations matérielle et logicielle minimales pour installer et exécuter [!INCLUDE[ssNoVer](../../includes/ssnoversion-md.md)] sur le système d’exploitation Windows. 

[!INCLUDE[sscurrent](../../includes/sssqlv14-md.md)] introduit la prise en charge de [!INCLUDE[ssNoVer](../../includes/ssnoversion-md.md)] sur Linux. Pour obtenir des informations, consultez [Configurations matérielle et logicielle requises pour [!INCLUDE[ssNoVersion](../../includes/ssNoVersion_md.md)] sur Linux](../../linux/sql-server-linux-setup.md#system). 

> Cet article s’applique à [!INCLUDE[ss2016](../../includes/sssql15-md.md)] et ultérieur. Pour obtenir du contenu relatif aux versions précédentes de SQL Server, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx). 
  
**Essayez :**  
  
-   Téléchargez SQL Server à partir du [**Centre d’évaluation**.](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016) 
  
-    Lancez une machine virtuelle sur laquelle [**SQL Server 2016**](https://azure.microsoft.com/en-us/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) est déjà installé.  
  
 **Les remarques suivantes s’appliquent à toutes les éditions :**  
  
-   Nous recommandons d’exécuter [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur des ordinateurs dotés du format de fichier NTFS ou ReFS. L’installation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur un système de fichiers FAT32 est prise en charge mais n’est pas recommandée car elle est moins sécurisée que le système de fichiers NTFS ou ReFS.  
  
-   Le programme d'installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloquera les installations sur les lecteurs en lecture seule, mappés ou compressés.  
  
-   L’installation échoue si vous lancez le programme d’installation par le biais d’une connexion Bureau à distance en utilisant un support sur une ressource locale du client Connexion Bureau à distance. Pour effectuer une installation à distance, le support doit se trouver sur un partage réseau ou être installé en local sur l’ordinateur physique ou la machine virtuelle. Le support d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut soit se trouver sur un partage réseau, sur un lecteur mappé ou sur un lecteur local, soit être présenté à une machine virtuelle sous la forme d’un fichier ISO.  
  
-   L’installation de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] exige l’installation préalable du .NET 4.6.1. Lorsque vous sélectionnez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , le programme d’installation procède automatiquement à l’installation de .NET 4.6.1.  
  
-   Le programme d'installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les composants logiciels suivants requis par le produit :  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client  
  
    -   Fichiers de support du programme d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]   
  
-   Pour obtenir la configuration minimale requise pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur [!INCLUDE[win8srv](../../includes/win8srv-md.md)] ou [!INCLUDE[win8](../../includes/win8-md.md)], consultez [Installation de SQL Server sur Windows Server 2012 ou Windows Server 8](http://support.microsoft.com/kb/2681562) (http://support.microsoft.com/kb/2681562).  
  
##  <a name="hwswr"></a> Configuration matérielle et logicielle requise  
La configuration requise suivante s’applique à toutes les installations :  
  
|Composant|Condition requise|  
|---------------|-----------------|  
|.NET Framework|[!INCLUDE[sql2016](../../includes/sssql15-md.md)] RC1 et versions ultérieures nécessitent le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6 pour la prise en charge du moteur de base de données, de Master Data Services ou de la réplication. Le programme d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 installe automatiquement [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Vous pouvez également installer manuellement [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] à partir de [Microsoft .NET Framework 4.6 (programme d’installation Web) pour Windows](http://support.microsoft.com/kb/3045560).<br/><br/> Pour obtenir davantage d’informations ainsi que des recommandations et des conseils sur [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6, consultez le [Guide de déploiement du .NET Framework pour les développeurs](http://msdn.microsoft.com/library/ee942965\(v=vs.110\).aspx).<br/><br/>[!INCLUDE[winblue_client_2](../../includes/winblue-client-2-md.md)]et [!INCLUDE[winblue_server_2](../../includes/winblue-server-2-md.md)] nécessitent une mise à niveau ( [KB2919355](http://support.microsoft.com/kb/2919355) ) avant l’installation du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.|  
|Logiciel réseau|Les systèmes d'exploitation pris en charge pour [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] possèdent un logiciel réseau intégré. Les instances par défaut et les instances nommées d'une installation autonome prennent en charge les protocoles réseau suivants : protocoles de mémoire partagée, TCP/IP, Canaux nommés et VIA.<br/><br/> Remarque : le protocole de mémoire partagée et le protocole VIA ne sont pas pris en charge sur les clusters de basculement.<br/><br/> Notez également que le protocole VIA est déconseillé. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]<br/><br/> <br/><br/> Pour plus d'informations sur les protocoles et les bibliothèques réseau, consultez [Network Protocols and Network Libraries](../../sql-server/install/network-protocols-and-network-libraries.md).|  
|Disque dur|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] requiert un minimum de 6 GO d'espace disque disponible.<br/><br/> L'espace disque nécessaire varie selon les composants [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] que vous installez. Pour plus d’informations, consultez [Espace disque nécessaire](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#HardDiskSpace) plus loin dans cet article. Pour plus d'informations sur les types de stockage pris en charge pour les fichiers de données, consultez [Storage Types for Data Files](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).|  
|Lecteur|Un lecteur de DVD, selon le cas, est requis pour l'installation à partir d'un disque.|  
|Moniteur|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] requiert un moniteur avec une résolution Super-VGA (800 x 600) ou supérieure.|  
|Internet|Les fonctionnalités Internet requièrent un accès à Internet (des frais peuvent s'appliquer).|  
  
 Remarque : l’exécution de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur une machine virtuelle est plus lente qu’un fonctionnement en mode natif en raison de la charge de la virtualisation.  
  
 La fonctionnalité PolyBase comporte d’autres exigences matérielles et logicielles. Pour plus d’informations, consultez [Prise en main de PolyBase](../../relational-databases/polybase/get-started-with-polybase.md).  
  
##  <a name="pmosr"></a> Processeur, mémoire et système d’exploitation requis  
 Les spécifications de mémoire et de processeur suivantes s'appliquent à toutes les éditions de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]:  
  
|Composant|Condition requise|  
|---------------|-----------------|  
|Mémoire *|**Minimum :**<br/><br/> Éditions Express : 512 MO<br/><br/> Toutes les autres éditions : 1 GO<br/><br/> **Recommandé :**<br/><br/> Éditions Express : 1 GO<br/><br/> Toutes les autres éditions : au moins 4 GO, qui doivent être augmentés au fur et à mesure de l'augmentation de la taille de la base de données pour garantir des performances optimales.|  
|Vitesse du processeur|**Minimum :** processeur x64, 1,4 GHz<br/><br/> **Recommandé** : 2,0 GHz ou plus|  
|Type de processeur|Processeur x64 : AMD Opteron, AMD Athlon 64, Intel Xeon avec prise en charge Intel EM64T Intel Pentium IV avec prise en charge EM64T|  
  
> [!NOTE]  
>  L’installation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] est prise en charge uniquement sur les processeurs x64. Elle n’est plus prise en charge sur les processeurs x86.  
  
 \* La mémoire minimale pour installer le composant [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] dans [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) est de 2 Go de RAM, ce qui est différent de la mémoire minimale exigée pour [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Pour obtenir des informations sur l'installation de DQS, consultez [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  
  
 **Prise en charge de la configuration WOW64 :**  
  
 WOW64 (Windows 32 bits sur Windows 64 bits) est une fonctionnalité des éditions 64 bits de Windows qui permet aux applications 32 bits de s'exécuter de façon native en mode 32 bits. Les applications fonctionnent en mode 32 bits, même si le système d'exploitation sous-jacent est un système 64 bits. WOW64 n’est pas pris en charge pour les installations [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] . Les outils de gestion sont cependant pris en charge dans WOW64.  
  
 **Prise en charge du système d’exploitation :**  
  
 Les éditions de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sont classées comme suit :  
  
-   [Éditions principales](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#TOP_Principal)  
  
-   [Éditions transversales](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#TOP_Breadth)  
  
> [!NOTE]  
>  Les fonctionnalités d’aide à la décision suivantes pour SQL Server 2016 et antérieur ne sont pas prises en charge par le système d’exploitation. Ces fonctionnalités peuvent être installées sur Windows Server 2008 R2 SP1 ou ultérieur :  
>  
>-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] - SharePoint  
> 
>-   Complément[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint  
  
### <a name="features-supported-on-32-bit-client-operating-systems"></a>Fonctionnalités prises en charge sur les systèmes d’exploitation client 32 bits  
 Les systèmes d’exploitation client Windows (par exemple, Windows 10 et Windows 8.1) sont disponibles sous forme d’architectures 32 bits ou 64 bits.   Toutes les fonctionnalités de SQL Server sont prises en charge sur les systèmes d’exploitation client 64 bits. Sur les systèmes d’exploitation client 32 bits, les fonctionnalités suivantes sont prises en charge :  
  
-   Data Quality Client  
  
-   Connectivité des outils clients  
  
-   Integration Services  
  
-   Compatibilité descendante des outils clients  
  
-   Kit de développement logiciel (SDK) des outils clients  
  
-   Composants de documentation  
  
-   Composants Distributed Replay  
  
-   Distributed Replay Controller  
  
-   Distributed Replay Client  
  
-   Kit de développement logiciel (SDK) de l'option Connectivité client de SQL  
  
 Les systèmes d’exploitation serveur[!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] et versions ultérieures ne sont pas disponibles sous forme d’architectures 32 bits. Tous les systèmes d’exploitation serveur pris en charge sont disponibles uniquement sous forme d’architectures 64 bits. Toutes les fonctionnalités sont prises en charge sur les systèmes d’exploitation serveur 64 bits.  
  
###  <a name="TOP_Principal"></a> Principal Editions of [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]  
 Le tableau suivant décrit la configuration requise du système d'exploitation pour les principales éditions de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]:  
  
|Édition de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] |Systèmes d’exploitation pris en charge|  
|---------------------------------------|--------------------------------|  
|[!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)]|[!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[winserver2016_essentials_md](../../includes/winserver2016-essentials-md.md)]* <br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation|  
|[!INCLUDE[ssStandard](../../includes/ssstandard-md.md)]|[!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[winserver2016_essentials_md](../../includes/winserver2016-essentials-md.md)]* <br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation<br/><br/> Windows 10 Famille<br/><br/> Windows 10 Professionnel<br/><br/> Windows 10 Entreprise<br/><br/>Windows 10 IoT Entreprise<br/><br/>[!INCLUDE[winblue_client_2](../../includes/winblue-client-2-md.md)]<br/><br/>[!INCLUDE[winblue_client_pro_2](../../includes/winblue-client-pro-2-md.md)]<br/><br/>[!INCLUDE[winblue_client_ent_2](../../includes/winblue-client-ent-2-md.md)]<br/><br/>[!INCLUDE[win8](../../includes/win8-md.md)]<br/><br/>[!INCLUDE[win8_client_pro_2](../../includes/win8-client-pro-2-md.md)]<br/><br/>[!INCLUDE[win8_client_ent_2](../../includes/win8-client-ent-2-md.md)]|  
|[!INCLUDE[ssWeb](../../includes/ssweb-md.md)]|[!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[winserver2016_essentials_md](../../includes/winserver2016-essentials-md.md)]* <br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation|  
  
\* Non pris en charge pour [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
###  <a name="TOP_Breadth"></a> Éditions transversales de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]  
 Le tableau suivant décrit la configuration requise du système d'exploitation pour les éditions transversales de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]:  
  
|Édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] |Systèmes d’exploitation pris en charge|  
|---------------------------------------|--------------------------------|  
|[!INCLUDE[ssDeveloper](../../includes/ssdeveloper-md.md)]|[!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[winserver2016_essentials_md](../../includes/winserver2016-essentials-md.md)]* <br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation<br/><br/> Windows 10 Famille<br/><br/> Windows 10 Professionnel<br/><br/> Windows 10 Entreprise<br/><br/>Windows 10 IoT Entreprise<br/><br/>[!INCLUDE[winblue_client_2](../../includes/winblue-client-2-md.md)]<br/><br/>[!INCLUDE[winblue_client_pro_2](../../includes/winblue-client-pro-2-md.md)]<br/><br/>[!INCLUDE[winblue_client_ent_2](../../includes/winblue-client-ent-2-md.md)]<br/><br/>[!INCLUDE[win8](../../includes/win8-md.md)]<br/><br/>[!INCLUDE[win8_client_pro_2](../../includes/win8-client-pro-2-md.md)]<br/><br/>[!INCLUDE[win8_client_ent_2](../../includes/win8-client-ent-2-md.md)]|  
|[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]|[!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[winserver2016_essentials_md](../../includes/winserver2016-essentials-md.md)]* <br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation<br/><br/> Windows 10 Famille<br/><br/> Windows 10 Professionnel<br/><br/> Windows 10 Entreprise<br/><br/>Windows 10 IoT Entreprise<br/><br/>[!INCLUDE[winblue_client_2](../../includes/winblue-client-2-md.md)]<br/><br/>[!INCLUDE[winblue_client_pro_2](../../includes/winblue-client-pro-2-md.md)]<br/><br/>[!INCLUDE[winblue_client_ent_2](../../includes/winblue-client-ent-2-md.md)]<br/><br/>[!INCLUDE[win8](../../includes/win8-md.md)]<br/><br/>[!INCLUDE[win8_client_pro_2](../../includes/win8-client-pro-2-md.md)]<br/><br/>[!INCLUDE[win8_client_ent_2](../../includes/win8-client-ent-2-md.md)]|  

\* Non pris en charge pour [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
##  <a name="CrossLanguageSupport"></a> Prise en charge de langues différentes  
 Pour plus d’informations sur la prise en charge de plusieurs langues et sur les considérations relatives à l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans les langues localisées, consultez [Versions linguistiques locales dans SQL Server](../../sql-server/install/local-language-versions-in-sql-server.md)  
  
##  <a name="HardDiskSpace"></a> Espace disque nécessaire  
 Pendant l'installation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)], Windows Installer crée des fichiers temporaires sur le lecteur système. Avant d'exécuter le programme d'installation ou de mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vérifiez que vous disposez d'au moins 6,0 Go d'espace disponible sur le lecteur système pour ces fichiers. Cette configuration requise s'applique même lorsque vous installez tous les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un lecteur différent du lecteur par défaut.  
  
 L'espace disque nécessaire peut varier selon la configuration du système et les fonctionnalités installées. Le tableau suivant présente l'espace disque nécessaire pour les composants [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] .  
  
|**Fonctionnalité**|**Espace disque nécessaire**|  
|-----------------|--------------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] et fichiers de données, réplication, recherche en texte intégral et Data Quality Services|1 480 Mo|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] (dans la configuration ci-dessus) avec R Services (dans la base de données)|2 744 Mo|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] (dans la configuration ci-dessus) avec service de requête PolyBase pour données externes|4 194 Mo|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et fichiers de données|698 Mo|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|967 Mo|  
|[!INCLUDE[rsql_platform](../../includes/rsql-platform-md.md)] (Autonome)|280 Mo|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] - SharePoint|1 203 Mo|  
|Complément[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Add-in for SharePoint Products|325 Mo|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|121 Mo|  
|Connectivité des outils clients|328 Mo|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|306 Mo|  
|Composants clients (autres que la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les outils Integration Services)|445 Mo|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|280 Mo|  
|Composants de la documentation en ligne[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour afficher et gérer le contenu d’aide*|27 Mo|  
|Toutes les fonctionnalités|8 030 Mo|  
  
 *L’espace disque requis pour le contenu de la documentation en ligne téléchargé est de 200 Mo.  
  
##  <a name="StorageTypes"></a> Types de stockage pour les fichiers de données  
 Les types de stockage pris en charge pour les fichiers de données sont :  
  
-   Disque local  
  
-   Stockage partagé  

-   [Espaces de stockage direct \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)
  
-   Partage de fichiers SMB  
  
    > [!NOTE]  
    >  Le stockage de fichiers SMB n'est pas pris en charge pour les fichiers de données d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour les installations autonomes ou en clusters. Utilisez un stockage attaché direct, un réseau de zone de stockage ou des espaces de stockage direct (S2D) à la place.  
  
    > [!IMPORTANT]  
    >  Le stockage SMB peut être hébergé par un serveur de fichiers Windows ou un dispositif de stockage SMB tiers. Si vous utilisez un serveur de fichiers Windows, il doit s'agir de la version 2008 ou ultérieure. Pour plus d'informations sur l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le partage de fichiers SMB comme option de stockage, consultez [Install SQL Server with SMB Fileshare as a Storage Option](../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md).  
  
    > [!WARNING]  
    >  L'installation du cluster de basculement[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge uniquement le disque local pour l'installation des fichiers tempdb. Assurez-vous que le chemin d'accès spécifié pour les données tempdb et les fichiers journaux sont valides sur tous les nœuds du cluster. Pendant le basculement, si les répertoires tempdb ne sont pas disponibles sur le nœud de basculement cible, la ressource [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sera pas en ligne.  
  
##  <a name="DC_support"></a> L’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine  
 Pour des raisons de sécurité, nous recommandons de ne pas installer [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne bloquera pas l'installation sur un ordinateur qui est contrôleur de domaine, mais les limitations suivantes s'appliquent :  
  
-   Vous ne pouvez pas exécuter les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine sous un compte de service local.  
  
-   Après avoir installé sur un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous ne pouvez pas modifier l'ordinateur d'un membre de domaine en un contrôleur de domaine. Vous devez désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de modifier l'ordinateur hôte en un contrôleur de domaine.  
  
-   Après avoir installé sur un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous ne pouvez pas modifier l'ordinateur d'un contrôleur de domaine en un membre de domaine. Vous devez désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de modifier l'ordinateur hôte en un membre de domaine.  
  
-   Les instances de cluster de basculement[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas prises en charge quand les nœuds du cluster sont des contrôleurs de domaine.  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas pris en charge sur un contrôleur de domaine en lecture seule. Le programme d'installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas créer de groupes de sécurité ni configurer de comptes de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine en lecture seule. Dans ce scénario, le programme d'installation échoue.  

- Une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas prise en charge dans un environnement au sein duquel un contrôleur de domaine en lecture seul est accessible. 
  
## <a name="see-also"></a> Voir aussi  
 [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)   
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [Spécifications de produit pour SQL Server 2016](http://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)  
  
  
