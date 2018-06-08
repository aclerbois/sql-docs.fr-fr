---
title: Administrer une topologie d’égal à égal (programmation Transact-SQL de la réplication) | Microsoft Docs
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
- TSQL
helpviewer_keywords:
- transactional replication, peer-to-peer replication
ms.assetid: 4d0fa941-f9ea-4a14-aed9-34df593fc6f2
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: a6555cb74774b3c883580da305f66552a1ca7bf9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="administer-a-peer-to-peer-topology-replication-transact-sql-programming"></a>Administrer une topologie d'égal à égal (programmation Transact-SQL de la réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'administration d'une topologie d'égal à égal est semblable à celle d'une topologie de réplication transactionnelle typique, mais plusieurs zones méritent une attention particulière. La principale différence dans l'administration d'une topologie d'égal à égal est que certaines modifications requièrent que le système soit *suspendu*. Suspendre un système revient à interrompre toute activité sur les tables publiées de tous les nœuds et à vérifier que chaque nœud a reçu toutes les modifications des autres nœuds. Pour plus d’informations, consultez [Suspendre une topologie de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).  
  
> [!NOTE]  
>  Dans une topologie d'égal à égal, le serveur de distribution ne peut pas utiliser une version antérieure de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] par rapport à celle d'un abonné à un abonnement par extraction.  
  
### <a name="to-add-an-article-to-an-existing-configuration"></a>Pour ajouter un article à une configuration existante  
  
1.  Suspendez le système.  
  
2.  Arrêtez l'Agent de distribution à chaque nœud de la topologie. Pour plus d’informations, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) ou [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
3.  Exécutez l'instruction CREATE TABLE pour ajouter la nouvelle table à chaque nœud de la topologie.  
  
4.  Copiez manuellement en bloc les données de la nouvelle table sur tous les nœuds à l'aide de l' [utilitaire bcp](../../../tools/bcp-utility.md).  
  
5.  Exécutez [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) pour créer l’article sur chaque nœud de la topologie. Pour plus d'informations, voir [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Après l’exécution de [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), la réplication ajoute automatiquement l’article aux abonnements de la topologie.  
  
6.  Redémarrez les Agents de distribution sur chaque nœud de la topologie.  
  
### <a name="to-make-schema-changes-to-a-publication-database"></a>Pour apporter des modifications de schéma à une base de données de publication  
  
1.  Suspendez le système.  
  
2.  Exécutez les instructions de langage de définition de données (DDL) pour modifier le schéma des tables publiées. Pour plus d’informations sur les modifications de schéma prises en charge, consultez [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
3.  Avant de reprendre l'activité sur les tables publiées, suspendez à nouveau le système. Vous garantissez ainsi que les modifications de schéma ont été reçues par tous les nœuds avant que de nouvelles modifications de données ne soient répliquées.  
  
## <a name="example"></a> Exemple  
 L'exemple suivant montre comment ajouter un nouvel article de table à une topologie existante de réplication d'égal à égal et composée de deux nœuds.  
  
 [!code-sql[HowTo#sp_addp2particle_createtables](../../../relational-databases/replication/codesnippet/tsql/administer-a-peer-to-pee_1.sql)]  
  
 [!code-sql[HowTo#sp_addp2particle_cmdline](../../../relational-databases/replication/codesnippet/tsql/administer-a-peer-to-pee_2.sql)]  
  
 [!code-sql[HowTo#sp_addp2particle_createarticle](../../../relational-databases/replication/codesnippet/tsql/administer-a-peer-to-pee_3.sql)]  
  
## <a name="see-also"></a> Voir aussi  
 [Administration &#40;réplication&#41;](../../../relational-databases/replication/administration/administration-replication.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Réplication transactionnelle d’égal à égal](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)  
  
  
