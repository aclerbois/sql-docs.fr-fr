---
title: Configurer FILESTREAM sur un cluster de basculement | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: blob
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-blob
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], setting up on a failover cluster
ms.assetid: 6721f780-20b7-4109-8ddb-ac327310699e
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 85ae37e539e081d262dfe2bce412d7f349c21e0f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-up-filestream-on-a-failover-cluster"></a>Configurer FILESTREAM sur un cluster de basculement
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment activer FILESTREAM sur un cluster de basculement. Avant d’essayer d’appliquer cette procédure, vous devez comprendre la notion de [clustering de basculement](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) et activer FILESTREAM. Pour plus d’informations sur l’activation de FILESTREAM, consultez [Activer et configurer FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md).  
  
### <a name="to-set-up-filestream-on-a-failover-cluster"></a>Pour configurer FILESTREAM sur un cluster de basculement  
  
1.  Installez le nœud principal pour le cluster de basculement.  
  
     Une fois l’installation terminée, activez FILESTREAM sur le nœud principal à l’aide du **Gestionnaire de configuration SQL Server**. Cela active les paramètres qui requièrent les privilèges d'administrateur Windows. Si l’accès distant est requis, sélectionnez **Autoriser les clients distants à avoir un accès en continu aux données FILESTREAM**. Cela crée une ressource de cluster de fichiers partagés.  
  
2.  Installez un nœud passif.  
  
     Une fois l’installation terminée, activez FILESTREAM sur le nœud passif à l’aide du **Gestionnaire de configuration SQL Server**. Le nom que vous spécifiez pour **Nom de partage Windows** doit être identique sur tous les nœuds du cluster.  
  
3.  Pour ajouter davantage de nœuds passifs, répétez l'étape 2.  
  
4.  Après avoir ajouté tous les nœuds, terminez le processus en exécutant la procédure stockée sp_configure sur chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
5.  Pour ajouter et activer des nœuds supplémentaires au cluster à tout moment, vous pouvez répéter les étapes 2, 3 et 4.  
  
## <a name="see-also"></a> Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)   
 [Supprimer une instance de cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/remove-a-sql-server-failover-cluster-instance-setup.md)   
 [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  
  
  
