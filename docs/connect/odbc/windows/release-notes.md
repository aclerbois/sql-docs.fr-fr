---
title: Notes de publication (le pilote ODBC pour SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b8459ed8-625e-4d8b-891c-e7e78c9977cc
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 21380decd228d82695c4ca9972852585a4fc3dbc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="release-notes"></a>Notes de publication
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

  Notes de publication pour Microsoft ODBC Driver for SQL Server sur Windows.  

## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-171-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] le pilote ODBC 17.1 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows

**Fonctionnalités ajoutées**:

Prise en charge de `SQL_COPT_SS_CEKCACHETTL` et `SQL_COPT_SS_TRUSTEDCMKPATHS` attributs de connexion (pour plus d’informations, consultez [à l’aide d’Always Encrypted avec le pilote ODBC pour SQL Server](../using-always-encrypted-with-the-odbc-driver.md))
- `SQL_COPT_SS_CEKCACHETTL` Permet de contrôler l’heure à laquelle le cache local des clés de chiffrement de colonne existe, ainsi que le vidage il
- `SQL_COPT_SS_TRUSTEDCMKPATHS` Permet à l’application limiter les opérations AE pour utiliser uniquement la liste spécifiée de clés principales de colonne


Prise en charge de l’authentification Interactive Azure Active Directory

[Correctifs de bogues](../bug-fixes.md)


## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-17-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] le pilote ODBC 17 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows

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
  

## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-131-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows  
 ODBC Driver 13.1 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] ajoute la prise en charge de [Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) et [Azure Active Directory](../../../connect/odbc/using-azure-active-directory.md) lorsqu’il est utilisé conjointement avec Microsoft SQL Server 2016.  Connexion de mise en pool des mots clés et attributs sont décrits dans [pilote prenant en charge le regroupement de connexions dans le pilote ODBC pour SQL Server](../../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md).

 ## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-13-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows  
 ODBC Driver 13 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] inclut une fonctionnalité précédente à partir d’ODBC Driver 11 pour SQL Server et ajoute la prise en charge de Microsoft SQL Server 2016.

## <a name="whats-new-in-the-includemsconameincludesmsconamemdmd-odbc-driver-11-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Quelles sont les nouveautés le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows  
 ODBC Driver 11 pour SQL Server contient de nouveaux [fonctionnalités](./features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md) , ainsi que toutes les fonctionnalités fournies avec ODBC dans SQL Server 2012 Native Client.  
