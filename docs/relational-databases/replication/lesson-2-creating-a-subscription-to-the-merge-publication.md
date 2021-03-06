---
title: "Leçon 2 : Création d’un abonnement à la publication de fusion | Microsoft Docs"
ms.custom: 
ms.date: 03/01/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: replication
ms.reviewer: 
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: 
ms.topic: article
applies_to:
- SQL Server 2016
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: 06722baa-9065-443e-b1d5-99036cf89074
caps.latest.revision: 
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: ff899aa93ddedd959cdbe70a0a292752d851c92b
ms.sourcegitcommit: ab25b08a312d35489a2c4a6a0d29a04bbd90f64d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
# <a name="lesson-2-creating-a-subscription-to-the-merge-publication"></a>Leçon 2 : Création d'un abonnement à la publication de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous allez créer l’abonnement à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Puis, vous définirez les autorisations sur la base de données d'abonnement et génèrerez manuellement l'instantané filtré des données du nouvel abonnement. Pour effectuer cette leçon, vous devez avoir terminé la leçon précédente, [Leçon 1 : Publication de données à l’aide de la réplication de fusion](../../relational-databases/replication/lesson-1-publishing-data-using-merge-replication.md).  
  
### <a name="to-create-the-subscription"></a>Pour créer l'abonnement  
  
1.  Connectez-vous à l’abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, développez le dossier **Réplication** , cliquez avec le bouton droit sur le dossier **Abonnements locaux** , puis choisissez **Nouvel abonnement**.  
  
    L'Assistant Nouvel abonnement démarre.  
  
2.  Dans la page **Publication** , choisissez **Rechercher un serveur de publication SQL** dans la liste **Serveur de publication** .  
  
3.  Dans la boîte de dialogue **Connect Se connecter au serveur** , entrez le nom de l’instance du serveur de publication dans la zone **Nom du serveur** , puis cliquez sur **Se connecter**.  
  
4.  Cliquez sur **AdvWorksSalesOrdersMerge**, puis sur **Suivant**.  
  
5.  Dans la page Emplacement de l’Agent de fusion, cliquez sur **Exécuter chaque agent sur son Abonné**, puis cliquez sur **Suivant**.  
  
6.  Dans la page Abonnés, sélectionnez le nom d’instance du serveur de l’Abonné, et sous **Base de données d’abonnement**sélectionnez **<New Database>** dans la liste.  
  
7.  Dans la boîte de dialogue **Nouvelle base de données** , entrez **SalesOrdersReplica** dans la zone **Nom de la base de données** , cliquez sur **OK**, puis cliquez sur **Suivant**.  
  
8.  Dans la page Sécurité de l’Agent de fusion, cliquez sur le bouton de sélection (**…**), entrez \<*nom_ordinateur>\repl_merge** dans la zone **Compte de processus**, fournissez le mot de passe de ce compte, cliquez sur **OK**, sur **Suivant**, et une nouvelle fois sur **Suivant**.  
  
9. Dans la page Initialiser les abonnements, sélectionnez **Lors de la première synchronisation** dans la liste **À quel moment** , cliquez sur **Suivant**, puis une nouvelle fois sur **Suivant** .  
  
10. Dans la page Valeurs de HOST_NAME, entrez **adventure-works\pamela0** dans la zone **Valeur de HOST_NAME** , puis cliquez sur **Terminer**.  
  
11. Cliquez à nouveau sur **Terminer** et, une fois l’abonnement créé, cliquez sur **Fermer**.  
  
### <a name="setting-database-permissions-at-the-subscriber"></a>Définition des autorisations de base de données sur l'Abonné  
  
1.  Connectez-vous à l’Abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez **Bases de données**, **SalesOrdersReplica**et **Sécurité**, cliquez avec le bouton droit sur **Utilisateurs**, puis choisissez **Nouvel utilisateur**.  
  
2.  Dans la page **Général**, entrez \<*nom_ordinateur>***\repl_merge** dans la zone **Nom d’utilisateur**, cliquez sur le bouton de sélection (**…**), cliquez sur **Parcourir**, sélectionnez \<*nom_ordinateur>***\repl_merge**, cliquez sur **OK**, sur **Vérifier les noms**, et à nouveau sur **OK**.  
  
3.  Dans **Appartenance au rôle de base de données**, sélectionnez **db_owner**, puis cliquez sur **OK** pour créer l’utilisateur.  
  
### <a name="to-create-the-filtered-data-snapshot-for-the-subscription"></a>Pour créer l'instantané filtré des données de l'abonnement  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication** .  
  
2.  Dans le dossier **Publications locales** , cliquez avec le bouton droit sur la publication **AdvWorksSalesOrdersMerge** , puis cliquez sur **Propriétés**.  
  
    La boîte de dialogue **Propriétés de la publication** s’affiche.  
  
3.  Sélectionnez la page **Partitions de données** , puis cliquez sur **Ajouter**.  
  
4.  Dans la boîte de dialogue **Ajouter une partition de données** , entrez **adventure-works\pamela0** dans la zone **Valeur de HOST_NAME** , puis cliquez sur **OK**.  
  
5.  Sélectionnez la partition nouvellement ajoutée, cliquez sur **Générer les instantanés sélectionnés maintenant**, puis sur **OK**.  
  
## <a name="next-steps"></a>Next Steps  
Vous avez créé avec succès un abonnement à la publication de fusion et généré l'instantané filtré pour la partition de données du nouvel abonnement de telle sorte qu'il soit disponible lors de l'initialisation de l'abonnement. Ensuite, vous allez accorder des droits à l'Agent de fusion sur la base de données d'abonnement et exécuter l'Agent de fusion pour démarrer la synchronisation et initialiser l'abonnement. Consultez [Leçon 3 : Synchronisation de l’abonnement et de la publication de fusion](../../relational-databases/replication/lesson-3-synchronizing-the-subscription-to-the-merge-publication.md).  
  
## <a name="see-also"></a> Voir aussi  
[S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
[Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)  
[Instantanés des publications de fusion avec des filtres paramétrés](../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md)  
  
