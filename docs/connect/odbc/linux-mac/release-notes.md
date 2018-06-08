---
title: Notes de mise à jour-pilote Microsoft ODBC pour SQL Server sur Linux et macOS | Documents Microsoft
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bddb826772d1c6ad7e33dce92b1e2615779b8931
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="release-notes-for-the-microsoft-odbc-driver-for-sql-server-on-linux-and-macos"></a>Notes de publication pour le pilote Microsoft ODBC pour SQL Server sur Linux et Mac OS
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-171-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] le pilote ODBC 17.1 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows

**Fonctionnalités ajoutées**:

Prise en charge de `SQL_COPT_SS_CEKCACHETTL` et `SQL_COPT_SS_TRUSTEDCMKPATHS` attributs de connexion (pour plus d’informations, consultez [à l’aide d’Always Encrypted avec le pilote ODBC pour SQL Server](../using-always-encrypted-with-the-odbc-driver.md))
- `SQL_COPT_SS_CEKCACHETTL` Permet de contrôler l’heure à laquelle le cache local des clés de chiffrement de colonne existe, ainsi que le vidage il
- `SQL_COPT_SS_TRUSTEDCMKPATHS` Permet à l’application limiter les opérations AE pour utiliser uniquement la liste spécifiée de clés principales de colonne



Prise en charge pour le chargement du `.rll` à partir de l’emplacement par défaut (pour plus d’informations, consultez [« Chargement de fichier ressource » une section dans le document Installation](installing-the-microsoft-odbc-driver-for-sql-server.md#resource-file-loading))

[Correctifs de bogues](../bug-fixes.md)



## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-17-for-includessnoversionincludesssnoversionmdmd-on-linux-and-macos"></a>Nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] 17 du pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux et Mac OS

**Nouvelle prise en charge les distributions**: macOS High Sierra et Ubuntu 17.10 

**Améliorations des performances**: supérieur à 10 x amélioration des performances lorsque le pilote convertit vers/depuis le format UTF-8/16.

**Fonctionnalités ajoutées**:

Always Encrypted prise en charge des API BCP

Nouvel attribut de chaîne de connexion UseFMTOnly conduit le pilote à utiliser les métadonnées héritées dans les cas spéciaux exigeant des tables temporaires.

Prise en charge pour l’Instance gérée de SQL Azure (version préliminaire privée étendu). 
> [!NOTE]
> Il existe plusieurs différences lors de l’utilisation d’une Instance gérée :
> -   FILESTREAM n’est pas pris en charge. 
> -   Accès de système de fichiers local n’est pas pris en charge, mais nécessaires pour les opérations que tracefiles 
> -   Créer des UDT local chemin d’accès n’est pas pris en charge. 
> -   L’authentification Windows intégrée n’est pas pris en charge. 
> -   DTC n’est pas pris en charge. 
> -   compte 'sa' n’est pas présent (compte par défaut est appelé « cloudSA »)
> -   Erreur de jeton TDS (0xAA) renvoie le nom de serveur incorrect
> -   Caractères spéciaux dans le nom de la base de données ne sont pas pris en charge. 
> -   MODIFIER le nom de ALTER DATABASE [dbname1] = [dbname2] n’est pas pris en charge.
> -   Les messages d’erreur sont toujours affichées en anglais, quel que soit le langage de paramètres (identique à Azure) 

## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-131-for-includessnoversionincludesssnoversionmdmd-on-linux-and-macos"></a>Nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13.1 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux et Mac OS  

ODBC Driver 13.1 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] ajoute la prise en charge pour Always Encrypted et Azure Active Directory lorsqu’il est utilisé conjointement avec Microsoft SQL Server 2016.

**Nouvelle prise en charge les distributions**: OS X 10.11 et macOS 10.12 sont pris en charge dans la première version du pilote ODBC sur macOS. 16.10 d’Ubuntu est maintenant également pris en charge, ainsi que Red Hat 6, 7 et SUSE 12. Chaque plateforme possède un package relatifs à la plate-forme (RPM ou DEB) pour faciliter l’installation et la configuration.  Consultez [l’installation du pilote](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) pour obtenir des instructions d’installation.

**unixODBC 2.3.1 du Gestionnaire de pilotes en charge les modifications**: le pilote ODBC ne dépend plus emballage personnalisé pour le Gestionnaire de pilotes unixODBC (sauf sur RedHat 6) et s’appuie sur le Gestionnaire de package de distribution pour résoudre la dépendance UnixODBC à partir des référentiels de la distribution.

**Prise en charge des API BCP**: Linux et macOS le pilote ODBC prend désormais en charge l’utilisation de la [fonctions de l’API BCP (**bcp_init**, etc..)](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)

## <a name="whats-new-in-the-microsoft-odbc-driver-130-for-includessnoversionincludesssnoversionmdmd-on-linux"></a>Nouveautés de Microsoft ODBC Driver 13.0 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux  
Avec Microsoft ODBC Driver 13.0 pour SQL Server, SQL Server 2014 et SQL Server 2016 sont maintenant également pris en charge.  

**Nouvelle prise en charge les distributions**:

Ubuntu est maintenant pris en charge, ainsi que Red Hat et SUSE. Chaque plateforme possède un package relatifs à la plate-forme (RPM ou DEB) pour faciliter l’installation et la configuration.  Consultez [l’installation du pilote](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) pour obtenir des instructions d’installation.

**prise en charge 2.3.1 du Gestionnaire de pilotes unixODBC**: en plus d’un gestionnaire de pilotes plus récent, il existe également un package d’installation de cette dépendance qui facilite l’installation et la configuration.  

**La résolution IP réseau transparent**: la résolution IP réseau Transparent est une révision de la fonctionnalité de basculement de sous-réseaux multiples existante qui affecte la séquence de connexion du pilote dans le cas où le premier résolu n’est pas le cas de l’adresse IP du nom d’hôte répondre et qu’il existe plusieurs adresses IP associée avec le nom d’hôte.

**Prise en charge de TLS 1.2**: le 13.0 de pilote Microsoft ODBC pour SQL Server sur Linux prend désormais en charge TLS 1.2 lorsque sécuriser les communications avec SQL Server sont utilisées.

## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-11-for-includessnoversionincludesssnoversionmdmd-on-linux"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux  
Le pilote ODBC sur SUSE Linux (Preview) prend en charge 64 bits SUSE Linux Enterprise 11 Service Pack 2. Pour plus d’informations, consultez [requise](../../../connect/odbc/linux-mac/system-requirements.md).  

Le pilote ODBC sur Linux prend en charge [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]. Pour plus d’informations, consultez [du pilote ODBC pour la prise en charge Linux pour une haute disponibilité, la récupération d’urgence](../../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  

Le pilote ODBC sur Linux prend en charge les connexions à Microsoft Azure SQL Database. Pour plus d’informations, consultez [How to: Connect to Windows Azure SQL Database Using ODBC](http://msdn.microsoft.com/library/hh974312.aspx).  

Le `-l` option (délai d’expiration de connexion) a été ajoutée à `bcp`. Pour plus d’informations, consultez [connexion avec **bcp**](../../../connect/odbc/linux-mac/connecting-with-bcp.md).
