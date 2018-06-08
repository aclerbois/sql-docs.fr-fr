---
title: Transférer des instantanés via FTP | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], FTP snapshots
- FTP snapshots [SQL Server replication]
- snapshot replication [SQL Server], FTP
ms.assetid: 55c30791-cd2a-420b-8ba7-5700e005cb45
caps.latest.revision: 40
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 62ffcf664168d92bd7917a64bab3d9a7c689d21d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-snapshots-through-ftp"></a>Transférer des instantanés via FTP
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Par défaut, les instantanés sont stockés dans des dossiers définis sous forme de partages UNC (Universal Naming Convention). La réplication vous permet aussi de spécifier un partage FTP (File Transfer Protocol) plutôt qu'UNC. Pour utiliser FTP, vous devez configurer un serveur FTP puis une publication et un ou plusieurs abonnements qui utiliseront FTP. Pour obtenir des informations sur la configuration d'un serveur SMTP, consultez la documentation IIS (Internet Information Services). Si vous spécifiez des informations FTP pour une publication, les abonnements à cette publication utiliseront par défaut FTP. FTP est uniquement utilisé avec la synchronisation Web lorsque l'ordinateur exécutant IIS est séparé du serveur de distribution par un pare-feu. Dans ce cas, FTP peut être utilisé pour transférer l'instantané entre le serveur de distribution et l'ordinateur qui exécute IIS. (L'instantané est toujours transféré à l'Abonné en utilisant le protocole HTTPS.)  
  
> [!IMPORTANT]  
>  Il est conseillé d'utiliser l'authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows et un partage UNC plutôt qu'un partage FTP puisque les mots de passe FTP doivent être stockés et que le mot de passe est transmis de l'Abonné ou de l'ordinateur exécutant IIS lorsqu'il utilise la synchronisation Web avec le serveur FTP en texte brut. De plus, parce qu'un seul compte contrôle l'accès au partage d'instantané, il n'est pas possible de garantir qu'un Abonné à une publication de fusion filtrée n'aura accès qu'aux fichiers d'instantanés de sa partition de données.  
  
 Pour remettre un instantané via FTP, consultez [Deliver a Snapshot Through FTP](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Synchronisation web pour la réplication de fusion](../../relational-databases/replication/web-synchronization-for-merge-replication.md)   
 [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Options d’instantané](../../relational-databases/replication/snapshot-options.md)  
  
  
