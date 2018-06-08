---
title: Autres emplacements du dossier d’instantané | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- snapshots [SQL Server replication], alternate folder locations
- snapshot replication [SQL Server], alternate folder locations
- alternate snapshot folders [SQL Server replication]
ms.assetid: 437553b0-19df-4522-8f27-06b5bc747c69
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 22923e3c7f5cc561e9bd5186bcf3caf2e2a2c9c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alternate-snapshot-folder-locations"></a>Autres emplacements du dossier d'instantané
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les autres emplacements d'instantané vous permettent de stocker les fichiers d'instantanés dans un lieu autre que l'emplacement par défaut (généralement, sur le serveur de distribution), ou s'ajoutent à cet emplacement par défaut. Il peut s'agir d'un autre serveur, d'un lecteur réseau ou d'un support amovible tel qu'un CD-ROM ou un disque.  
  
 L'autre emplacement d'instantané est mémorisé sous la forme d'une propriété de la publication. Dans la mesure où cet emplacement est une propriété de la publication, l'Agent de distribution et l'Agent de fusion sont capables de déterminer l'instantané approprié dans le cadre du processus de synchronisation.  
  
 Si vous souhaitez spécifier un autre emplacement pour le dossier d'instantanés ou compresser des fichiers d'instantanés, créez la publication sans créer immédiatement l'instantané initial, définissez les propriétés de publication de l'emplacement de l'instantané, puis exécutez l'Agent d'instantané pour cette publication. Si vous changez l'autre emplacement après avoir créé l'instantané initial, les instantanés déjà générés pour la publication ne seront pas transférés vers le nouvel emplacement. Dans ce cas, et en fonction des paramètres de publication, l'Agent de fusion ou de distribution ne trouvera peut-être pas les fichiers d'instantanés dans le nouvel emplacement.  
  
> [!NOTE]  
>  Ne spécifiez pas un autre emplacement (à l’aide de la boîte de dialogue **Propriétés de la publication** ou [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)) qui soit identique à l’emplacement par défaut du dossier d’instantané.  
  
> [!CAUTION]  
>  N'utilisez pas WebSync et d'autres emplacements de dossier d'instantanés à la fois.  
  
 **Pour spécifier un autre emplacement d'instantané**  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Spécifier un autre emplacement de dossier d’instantané &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/publish/specify-an-alternate-snapshot-folder-location-sql-server-management-studio.md)  
  
-   Programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] de la réplication : [Configurer les propriétés d’instantané &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Options d’instantané](../../relational-databases/replication/snapshot-options.md)  
  
  
