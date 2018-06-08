---
title: Installer la réplication SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server replication]
- command line installations [SQL Server replication]
- installing replication
- replication [SQL Server], installing
- command prompt [SQL Server replication]
ms.assetid: c50ad078-060b-4a8d-ad45-9e31a8d85729
caps.latest.revision: 41
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: c92777754814f0c0dc9e49919bbffe068eed0899
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770445"
---
# <a name="install-sql-server-replication"></a>Installer la réplication SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Les composants de réplication peuvent être installés à l'aide de l'Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à l'invite de commandes. Installez la réplication lors de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou lors de la modification d'une instance existante.  
  
Une fois que les composants de réplication sont installés, vous devez configurer le serveur avant de pouvoir utiliser la réplication. Pour plus d’informations, consultez [Configurer la distribution](../../relational-databases/replication/configure-distribution.md) dans la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
>[!IMPORTANT]  
>Si vous installez des composants de réplication lorsque vous modifiez une instance existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez arrêter et redémarrer l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] une fois l'installation terminée. Cette action garantit que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconnaît les sous-systèmes de l'agent de réplication et peut appeler les agents de réplication à partir des étapes de travail.  
  
## <a name="installing-replication-by-using-setup"></a>Installation de la réplication à l’aide du programme d’installation  
**Pour installer la réplication en même temps qu’une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Pour installer les composants de réplication, notamment des objets RMO (Replication Management Objects), sélectionnez **Réplication SQL Server** dans la page **Sélection de composant** de l’Assistant Installation.  
  
## <a name="installing-replication-from-the-command-prompt"></a>Installation de la réplication à partir de l'invite de commandes  
 **Pour installer la réplication en même temps qu’une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Voir [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server](../../database-engine/install-windows/install-sql-server.md)   
 [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)   
 [Fonctionnalités prises en charge par les éditions de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
  
