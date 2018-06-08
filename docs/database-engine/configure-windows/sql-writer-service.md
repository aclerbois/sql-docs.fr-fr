---
title: Service SQL Writer | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- VDI [SQL Server]
- restoring [SQL Server], SQL Writer Service
- backups [SQL Server], while SQL Server running
- Volume Shadow Copy Service
- volume backups while running [SQL Server]
- Virtual Backup Device Interface [SQL Server]
- SQL Writer Service
- services [SQL Server], SQL Writer
- MSDE Writer
- VSS
ms.assetid: 0f299867-f499-4c2a-ad6f-b2ef1869381d
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b6fab60401596743dc1cc38dd0c115e42ec89c71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-writer-service"></a>Service SQL Writer
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le service SQL Writer complète les fonctionnalités de sauvegarde et de restauration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le biais du service VSS.  
  
 Le service SQL Writer est installé automatiquement. Il doit être en cours d'exécution lorsque l'application VSS (Volume Shadow Copy Service) demande une sauvegarde ou une restauration. Pour configurer le service, utilisez l'applet des services [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Le service SQL Writer s'installe sur tous les systèmes d'exploitation.  
  
## <a name="purpose"></a>Fonction  
 Lors de son exécution, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] verrouille les fichiers de données pour être seul à pouvoir y accéder. Lorsque le service SQL Writer n'est pas exécuté, les programmes de sauvegarde exécutés dans Windows n'ont pas accès à ces fichiers de données, et les sauvegardes doivent s'effectuer au moyen de la sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Utilisez le service SQL Writer pour permettre aux programmes de sauvegarde de Windows de copier les fichiers de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] même lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.  
  
## <a name="volume-shadow-copy-service"></a>Service VSS  
 Le service VSS est un ensemble d'API COM qui forment un cadre permettant la sauvegarde de volumes même lorsque des opérations d'écriture sont en cours. Il offre une interface cohérente qui permet la coordination entre les différentes applications utilisateur qui mettent à jour les données sur le disque (les writers) et celles qui assurent la sauvegarde des applications (les demandeurs).  
  
 Ce service capture et copie des images stables pour la sauvegarde sur des systèmes en cours d'utilisation, en particulier des serveurs, sans dégradation superflue des performances et de la stabilité des services qu'ils assurent. Pour plus d'informations sur le service VSS, consultez votre documentation Windows.  
  
## <a name="virtual-backup-device-interface-vdi"></a>Interface d'unité de sauvegarde virtuelle  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une API appelée « Interface d’unité de sauvegarde virtuelle » qui permet aux éditeurs de logiciels indépendants d’intégrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans leurs produits pour la prise en charge des opérations de sauvegarde et de restauration. Conçues pour fournir une fiabilité et des performances optimales, ces API prennent en charge l'éventail complet de fonctions de sauvegarde et de restauration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , y compris la gamme totale des sauvegardes à chaud et instantanées.  
  
## <a name="permissions"></a>Autorisations  
 Le service SQL Writer doit s'exécuter sous le compte **système local** . Le service SQL Writer utilise la connexion **NT Service\SQLWriter** pour la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le fait d’utiliser la connexion **NT Service\SQLWriter** permet au processus SQL Writer de s’exécuter à un niveau de droits inférieur dans un compte indiqué comme étant **sans connexion**, ce qui limite la vulnérabilité. Si le service SQL Writer est désactivé, les utilitaires qui s'appuient sur les instantanés VSS, tels que System Center Data Protection Manager, ainsi que certains autres produits tiers, seront rompus ou pire, risquent d'effectuer des sauvegardes de bases de données qui ne sont pas cohérentes. Si ni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le système sur lequel il s'exécute, ni le système hôte (dans le cas d'une machine virtuelle), ne doit utiliser un élément autre que la sauvegarde [!INCLUDE[tsql](../../includes/tsql-md.md)] , le service SQL Writer peut être désactivé en toute sécurité et la connexion supprimée.  Notez que le service SQL Writer peut être appelé par une sauvegarde au niveau du système ou du volume, que la sauvegarde repose directement sur des instantanés ou non. Certains logiciels de sauvegarde système utilisent VSS pour éviter d’être bloqués par des fichiers ouverts ou verrouillés. Le service SQL Writer nécessite des autorisations élevées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En effet, au cours de ses activités, il fige brièvement toutes les E/S pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="features"></a>Fonctionnalités  
 SQL Writer prend en charge les possibilités suivantes :  
  
-   Sauvegarde et restauration complètes de bases de données, y compris des catalogues de texte intégral  
  
-   Sauvegarde et restauration différentielle  
  
-   Restauration avec déplacement  
  
-   Modification du nom d'une base de données  
  
-   Sauvegarde de copie seule  
  
-   Récupération automatique d'un instantané de base de données  
  
 SQL Writer ne prend pas en charge les fonctions suivantes :  
  
-   Sauvegarde de journaux  
  
-   Sauvegarde de fichiers et de groupes de fichiers  
  
-   Restauration de pages  
  
  
