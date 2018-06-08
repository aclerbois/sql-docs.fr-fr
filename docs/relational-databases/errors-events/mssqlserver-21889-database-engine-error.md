---
title: MSSQLSERVER_21889 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 21889 (Database Engine error)
ms.assetid: ae199540-7986-4cc2-b782-cd22793236d3
caps.latest.revision: 6
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8241192ce757574e301baed8e8a694ab02656514
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver21889"></a>MSSQLSERVER_21889
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|21889|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum21889|  
|Texte du message|L'instance '%s' de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas un serveur de publication de réplication. Exécutez **sp_adddistributor** sur l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] '%s' avec le serveur de distribution '%s' pour permettre à l’instance d’héberger la base de données de publication '%s'. Veillez à spécifier la même connexion et mot de passe que ceux utilisés pour le serveur de publication d'origine.|  
  
## <a name="explanation"></a>Explication  
Afin d'héberger la base de données du serveur de publication, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être un serveur de publication de réplication. **sp_validate_redirected_publisher** appelle **sp_helpdistributor** sur le serveur distant pour déterminer si le serveur est un serveur de publication de réplication. Cette erreur est retournée quand l’exécution de la procédure stockée **sp_helpdistributor** indique que l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas un serveur de publication de réplication.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez **sp_adddistributor** sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge la base de données du serveur de publication. Lors de l’exécution de **sp_adddistributor**, spécifiez le serveur de distribution approprié. Utilisez la même valeur pour le paramètre *@password* que celle utilisée quand **sp_adddistributor** a été initialement exécuté sur le serveur de distribution.  
  
