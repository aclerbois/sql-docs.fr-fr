---
title: Exécuter des scripts avant et après l’application de l’instantané | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], scripts
- scripts [SQL Server replication], snapshots
- snapshot replication [SQL Server], scripts
- scripts [SQL Server replication]
ms.assetid: 9a6872c2-9bed-477f-9d2f-332d640edcf2
caps.latest.revision: 35
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 12d31432f0997639d2c393838d6b4537609db924
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-scripts-before-and-after-the-snapshot-is-applied"></a>Exécuter des scripts avant et après l'application de l'instantané
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous pouvez spécifier les scripts à exécuter sur l'Abonné avant ou après l'application de l'instantané. Les scripts peuvent être utilisés à diverses fins, par exemple pour créer des connexions et des schémas (propriétaires d'objets) sur chaque Abonné.  
  
 Vous spécifiez un emplacement de fichier pour chaque script et l'Agent d'instantané copie les fichiers de script dans le dossier d'instantanés actif à chaque traitement d'instantané. L'Agent de distribution ou l'Agent de fusion exécute le script antérieur à l'instantané avant tout autre script d'objet répliqué lors de l'application d'un instantané. Il exécute le script postérieur à l'instantané après l'application de tous les autres scripts et données d'objets répliqués. Au terme de l'application de l'instantané et de l'exécution correcte des fichiers de script, ces derniers sont supprimés du répertoire de travail sur l'Abonné.  
  
 Le script est exécuté par le démarrage de l'utilitaire **sqlcmd** . Avant de déployer un script, exécutez-le avec **sqlcmd** pour vérifier qu'il s'exécute comme prévu. Le contenu des scripts exécutés avant et après l'application de l'instantané doit être renouvelable. Si, par exemple, vous créez une table dans le script, commencez par vérifier qu'elle existe et, dans l'affirmative, procédez de la façon appropriée. Le script doit être renouvelable car, s'il est nécessaire de réinitialiser un abonnement dont le script a déjà été appliqué, ce dernier sera réexécuté lors de l'application du nouvel instantané au cours de la réinitialisation.  
  
 Si vous compressez le fichier d'instantanés (en le convertissant au format de fichier [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB), les scripts sont également compressés et placés dans le fichier CAB. Après le transfert du fichier d'instantanés compressé vers l'Abonné et sa décompression dans un répertoire de travail sur l'Abonné, tout script indiqué comme script antérieur à l'instantané est exécuté. De même, tous les scripts postérieurs à l'instantané sont décompressés et exécutés sur l'Abonné lors de l'étape finale de l'application de l'instantané.  
  
 **Pour exécuter des scripts avant et après l'application de l'instantané**  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Guide pratique pour exécuter des scripts avant et après l’application d’un instantané \(SQL Server Management Studio\)](../../relational-databases/replication/execute-scripts-before-and-after-a-snapshot-is-applied.md)  
  
-   Programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] de la réplication : [Configurer les propriétés d’instantané &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Options d’instantané](../../relational-databases/replication/snapshot-options.md)  
  
  
