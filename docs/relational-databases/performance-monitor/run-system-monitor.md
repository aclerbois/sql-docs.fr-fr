---
title: "Exécuter le Moniteur système | Microsoft Docs"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: performance-monitor
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- System Monitor [SQL Server], running
- Windows System Monitor [SQL Server], running
- remote procedure calls [SQL Server]
- starting Windows NT System Monitor
- RPC
ms.assetid: 05297984-bc8d-43df-829c-032387f7ea61
caps.latest.revision: 
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.workload: On Demand
ms.openlocfilehash: 5abae95a55ad92a0d4f24db189a8d5ac4f01d982
ms.sourcegitcommit: dcac30038f2223990cc21775c84cbd4e7bacdc73
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2018
---
# <a name="run-system-monitor"></a>Exécuter le Moniteur système
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)] Le Moniteur système utilise les appels de procédures distantes (RPC) pour collecter les informations de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tout utilisateur qui dispose des autorisations Microsoft Windows pour exécuter le Moniteur système peut l'utiliser pour surveiller le fonctionnement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Lorsque vous utilisez le Moniteur système ou l'Analyseur de performances, vous ne pouvez pas vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnant sous Microsoft Windows 98.  
  
 Comme avec tous les outils d'analyse de performances, attendez-vous à une certaine diminution des performances due à une surcharge du système, lorsque vous utilisez le Moniteur système pour surveiller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La surcharge effective de toute instance particulière dépend de la plateforme matérielle, du nombre de compteurs et de la fréquence de mise à jour choisie. Cependant, l'intégration du Moniteur système à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est conçue pour minimiser toute diminution des performances.  
  
> [!NOTE]  
>  Si vous avez sélectionné les compteurs de performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les surveiller via le composant logiciel enfichable Moniteur système, vous verrez ces compteurs même si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne s'exécute pas.  
  
 Pour plus d’informations sur le démarrage du Moniteur système, consultez [Démarrer le Moniteur système &#40;Windows&#41;](../../relational-databases/performance/start-system-monitor-windows.md).  
  
  
