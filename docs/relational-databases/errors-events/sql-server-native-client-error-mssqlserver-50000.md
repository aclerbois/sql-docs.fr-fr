---
title: MSSQLSERVER_50000 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- 50000 [SQL Server Native Client setup error]
ms.assetid: 5426d87a-d5d9-4984-b211-b07d69e834a2
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a67941c3eb21b0029a84aa3c2abbe79b34aa38d3
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-native-client-error-mssqlserver50000"></a>Erreur de SQL Server Native Client MSSQLSERVER_50000
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

    
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|Version du produit|11.0|  
|ID d'événement|50000|  
|Source de l'événement|SETUP|  
|Composant|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client|  
|Nom symbolique||  
|Texte du message|Erreur réseau lors de la tentative de lecture du fichier : '%. * ls'.|  
  
## <a name="explanation"></a>Explication  
 Tentative d'installation (ou de mise à jour) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sur un ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est déjà installé, et où l'installation existante provenait d'un fichier MSI qui a été renommé de sqlncli.msi.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Pour résoudre cette erreur, désinstallez la version existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Pour empêcher cette erreur, n'installez pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client à partir d'un fichier MSI qui n'est pas nommé sqlncli.msi.  
  
## <a name="internal-only"></a>Interne uniquement  
  
