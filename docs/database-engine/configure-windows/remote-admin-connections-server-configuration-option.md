---
title: remote admin connections (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- administrator connections [SQL Server]
- DAC
- connections [SQL Server], dedicated administrator
- remote admin connections option
- dedicated administrator connections [SQL Server]
ms.assetid: bf32b60a-7a48-401f-b6be-b5e2e46c413f
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 5a2f4381d0a72c582a4a8c592328539b92afbf27
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="remote-admin-connections-server-configuration-option"></a>remote admin connections (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une connexion administrateur dédiée. Celle-ci permet à un administrateur d’accéder à un serveur actif pour exécuter des fonctions de diagnostic ou des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou pour résoudre des problèmes sur ce serveur, même s’il est verrouillé, que son état est anormal ou qu’il ne répond pas à une connexion du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Par défaut, cette connexion n'est disponible qu'à partir d'un client sur le serveur. Pour autoriser des applications clientes sur des ordinateurs distants à utiliser la connexion administrateur dédiée, utilisez l'option remote admin connections de sp_configure.  
  
 Par défaut, la connexion administrateur dédiée n'écoute que sur l'adresse IP (127.0.0.1), port 1434. Si le port TCP 1434 n’est pas disponible, un port TCP est affecté dynamiquement lors du démarrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Lorsque plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées sur un ordinateur, consultez le journal des erreurs afin de connaître le numéro de port TCP.  
  
 Le tableau suivant répertorie les valeurs possibles de l'option remote admin connections.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|Indique que seules les connexions locales sont autorisées à utiliser la connexion administrateur dédiée.|  
| 1|Indique que les connexions à distance sont autorisées à utiliser la connexion administrateur dédiée.|  
  
## <a name="example"></a> Exemple  
 L'exemple suivant active la connexion administrateur dédiée à partir d'un ordinateur distant.  
  
```  
sp_configure 'remote admin connections', 1;  
GO  
RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)  
  
  
