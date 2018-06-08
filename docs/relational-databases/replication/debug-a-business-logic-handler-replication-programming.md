---
title: Déboguer un gestionnaire de logique métier (programmation de la réplication) | Microsoft Docs
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
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- merge replication business logic handlers [SQL Server replication]
- business logic handlers [SQL Server replication]
- BusinessLogicModule class
ms.assetid: edd0d17a-0e9c-4c28-8395-a7d47e8ce3d6
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e0cb1eb9c3b39600e486fbe4c70b9deaad580e6b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="debug-a-business-logic-handler-replication-programming"></a>Déboguer un gestionnaire de logique métier (programmation de la réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez un gestionnaire de logique métier pour appeler une logique métier personnalisée lorsqu'un abonnement de fusion est synchronisé. Pour plus d’informations, consultez [Exécuter la logique pendant la synchronisation de fusion](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
 Le réconciliateur de réplication de fusion (replrec.dll) appelle l'assembly de code managé qui contient la logique métier. Dans la plupart des cas, replrec.dll et la logique métier personnalisée sont exécutés sur l'ordinateur où s'exécute l'Agent de fusion (sur l'Abonné pour un abonnement par extraction ou sur le serveur de distribution pour un abonnement par émission de données). Dans le cas de la synchronisation Web ou dans le cas d'un Abonné [!INCLUDE[ssEW](../../includes/ssew-md.md)] , le réconciliateur et la logique métier personnalisée sont exécutés sur le serveur Web.  
  
### <a name="to-debug-a-business-logic-handler-on-a-local-computer"></a>Pour déboguer un gestionnaire de logique métier sur un ordinateur local  
  
1.  Configurez la publication et la distribution, créez une publication et créez un abonnement à la publication. Pour plus d’informations, consultez [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md) et [Créer, modifier et supprimer des publications et des articles &#40;réplication&#41;](../../relational-databases/replication/publish/create-modify-and-delete-publications-and-articles-replication.md).  
  
2.  Créez et inscrivez un gestionnaire de logique métier. Pour plus d'informations, voir [Implémenter un gestionnaire de logique métier pour un article de fusion](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md).  
  
3.  Créez un projet Replication Management Objects dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio qui démarre par programme l'Agent de fusion de façon synchrone. Pour plus d’informations, voir [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
4.  Définissez un point d'arrêt dans le code du gestionnaire de logique métier, soit dans la méthode en cours de débogage, soit dans le constructeur de classe. Pour plus d'informations sur les méthodes qui peuvent être implémentées dans un gestionnaire de logique métier, consultez la rubrique sur les méthodes <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
5.  Construisez le gestionnaire de logique métier en mode débogage et déployez l'assembly et le fichier de symboles de débogage (.pdb) dans l'emplacement inscrit à l'étape 1.  
  
    > [!NOTE]  
    >  Pour simplifier le débogage, créez une solution Visual Studio .NET unique qui contient à la fois le projet de gestionnaire de logique métier et le projet qui synchronise l'abonnement. Dans ce cas, définissez le projet de synchronisation comme projet de démarrage et configurez l'environnement de génération pour déployer l'assembly de logique métier dans l'emplacement inscrit à l'étape 1 au cours du débogage.  
  
6.  Exécutez des commandes d'insertion, de mise à jour ou de suppression sur la base de données d'abonnement ou de publication. L'emplacement de commande et d'exécution dépend de la méthode faisant l'objet du débogage.  
  
7.  Démarrez le projet créé à l'étape 3 en mode débogage pour synchroniser l'abonnement.  
  
8.  En supposant qu'aucun autre point d'arrêt n'est défini et que les commandes correctes sont répliquées, l'exécution s'arrête lorsqu'elle atteint le point d'arrêt dans le gestionnaire de logique métier.  
  
### <a name="to-debug-a-business-logic-handler-on-a-web-server-using-web-synchronization-or-for-a-sql-server-compact-subscriber"></a>Pour déboguer un gestionnaire de logique métier sur un serveur Web à l'aide de la synchronisation Web ou pour un abonné SQL Server Compact  
  
1.  Configurez la publication et la distribution, créez une publication et créez un abonnement par extraction à la publication. La publication doit prendre en charge la synchronisation Web ou les Abonnés [!INCLUDE[ssEW](../../includes/ssew-md.md)] .  
  
2.  Créez et inscrivez un gestionnaire de logique métier. Pour plus d’informations, voir [Implement a Business Logic Handler for a Merge Article](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md).  
  
3.  Définissez un point d'arrêt dans le code du gestionnaire de logique métier, soit dans la méthode en cours de débogage, soit dans le constructeur de classe. Pour plus d'informations sur les méthodes qui peuvent être implémentées dans un gestionnaire de logique métier, consultez la rubrique sur les méthodes <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Construisez le gestionnaire de logique métier en mode débogage et déployez l'assembly et le fichier de symboles de débogage (.pdb) sur le serveur Web, dans l'emplacement inscrit à l'étape 1.  
  
    > [!NOTE]  
    >  Si la création du gestionnaire de logique métier échoue car l'assembly est en cours d'utilisation, tapez la commande `iisreset` sur le serveur Web à l'invite de commandes pour réinitialiser le serveur Web.  
  
5.  Synchronisez l'abonnement avec la synchronisation Web activée. Pendant la synchronisation, le serveur Web charge l'assembly inscrit.  
  
6.  À l'aide du débogueur Visual Studio .NET, attachez à l'un des processus suivants sur le serveur Web :  
  
    -   w3wp.exe – Windows Server 2003.  
  
    -   inetinfo.exe – Windows 2000 et Windows XP.  
  
7.  Dans la fenêtre **Sortie** , examinez la sortie de débogage pour vérifier que les symboles pour l'assembly inscrit ont été chargés correctement. Si les symboles n'ont pas été chargés, assurez-vous que le fichier .pdb correct a été copié à l'étape 4 et répétez l'étape 5.  
  
8.  Exécutez des commandes d'insertion, de mise à jour ou de suppression sur la base de données d'abonnement ou de publication. L'emplacement de commande et d'exécution dépend de la méthode faisant l'objet du débogage.  
  
9. À l'aide du débogueur Visual Studio, attachez au processus w3wp.exe.  
  
10. Synchronisez de nouveau l'abonnement, à l'aide de la synchronisation Web.  
  
11. En supposant qu'aucun autre point d'arrêt n'est défini et que les commandes correctes sont répliquées, l'exécution s'arrête lorsqu'elle atteint le point d'arrêt dans le gestionnaire de logique métier.  
  
## <a name="see-also"></a> Voir aussi  
 [Implement a Business Logic Handler for a Merge Article](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  
  
  
