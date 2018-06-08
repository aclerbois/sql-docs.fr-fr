---
title: Outil de résolution des conflits de réplication Microsoft interactif | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.replconflictviewer.interactiveresolver.f1
ms.assetid: d3d4a480-782b-4b1d-b839-565c8cf6cb24
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 095bfed29b4282f8808967b6e81b413ae4bc7bee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-replication-interactive-conflict-resolver"></a>Outil de résolution des conflits de réplication Microsoft interactif
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  L'Outil de résolution des conflits de réplication Microsoft interactif peut s'utiliser pour les abonnements de fusion synchronisés au moyen du Gestionnaire de synchronisation Windows. Il permet d'afficher, comparer, modifier et sélectionner les conflits dans les données des résultats. La réplication comporte également l'Outil de résolution des conflits qui permet d'afficher et de modifier les résultats des conflits après leur validation. L'Outil de résolution des conflits interactif permet de sélectionner le résultat pendant la synchronisation.  
  
> [!NOTE]  
>  Les conflits qui concernent des enregistrements logiques ne sont pas affichés dans le résolveur interactif. Pour afficher des informations sur ces conflits, utilisez des procédures stockées de réplication. Pour plus d’informations, consultez [Afficher les informations relatives aux conflits pour les publications de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/view-conflict-information-for-merge-publications.md).  
  
## <a name="options"></a>Options  
 **Nom de colonne**  
 Noms de toutes les colonnes de la table. Une ou plusieurs colonnes peuvent comporter des données en conflit. Indépendamment des colonnes en conflit, la ligne gagnante complète remplace la totalité de la ligne perdante.  
  
 **Résolution suggérée**  
 Résolution du conflit suggérée par l'outil de résolution des conflits pour l'article.  
  
 **Serveur de publication**  
 Valeur des données du serveur de publication.  
  
 **Abonné**  
 Valeur des données de l'abonné.  
  
 **Accepter la suggestion**, **Accepter le serveur de publication**et **Accepter l'Abonné**  
 Cliquez sur l'option correspondante pour accepter la ligne qui sera appliquée par le serveur de publication ou l'abonné, en fonction du perdant du conflit. Si le serveur de publication perd le conflit, tous les autres abonnés reçoivent la ligne gagnante lors de leur prochaine synchronisation avec le serveur de publication.  
  
 **Résoudre automatiquement tous les conflits restants**  
 Résout tous les conflits restants en utilisant la résolution suggérée par l'outil de résolution des conflits pour l'article.  
  
 **Journaliser les détails de ce conflit pour future référence**  
 Enregistre les détails du conflit dans des tables système.  
  
## <a name="see-also"></a> Voir aussi  
 [Résolution interactive des conflits](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [Afficher et résoudre les conflits de données pour les publications de fusion &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Synchroniser un abonnement à l’aide du Gestionnaire de synchronisation Windows &#40;Gestionnaire de synchronisation Windows&#41;](../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md)   
 [Détection et résolution des conflits de réplication de fusion avancée](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
