---
title: Vérifier et s’abonner à la stratégie Nom financier | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: 126b4c4c-2a1c-4701-a0ad-8de23fbd7306
caps.latest.revision: 24
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: dbba77023532e1db8f8bda6eb41ebff6cb2ab963
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-2---subscribe-to-and-check-the-finance-name-policy"></a>Leçon 2-2 : Vérifier et s’abonner à la stratégie Nom financier
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette tâche, vous allez configurer la base de données Finance de manière à l'abonner à la catégorie de stratégies Finance. Ensuite, vous allez tester la stratégie Nom financier.  
  
### <a name="to-subscribe-to-the-finance-policy-category"></a>Pour abonner la base de données à la catégorie de stratégies Finance  
  
1.  Dans l’Explorateur d’objets, développez **Bases de données**, cliquez avec le bouton droit sur **Finance**, pointez sur **Stratégies**, puis cliquez sur **Catégories**.  
  
2.  Cochez la case **Abonné** de la catégorie **Finance** .  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-test-the-enforcement-of-the-finance-name-policy"></a>Pour tester l'application de la stratégie Nom financier  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ouvrez une fenêtre de requête. Exécutez les instructions suivantes qui essaient de créer une table qui enfreint la stratégie **Nom financier** . La table enfreint la stratégie car le nom de table ne commence pas par les lettres **fintbl**.  
  
    ```  
    USE Finance ;  
    GO  
    CREATE TABLE NewTable  
    (Col1 int) ;  
    GO  
  
    ```  
  
    Remarquez que la stratégie empêche la création de la table et retourne un message d'information qui indique le nom de la stratégie.  
  
2.  Pour fournir un nom valide, modifiez le code comme suit et réexécutez l'instruction.  
  
    ```  
    USE Finance ;  
    GO  
    CREATE TABLE fintblNewTable  
    (Col1 int) ;  
    GO  
  
    ```  
  
    Cette fois-ci, la table est créée.  
  
### <a name="to-apply-the-policy-to-the-whole-server"></a>Pour appliquer la stratégie à tout le serveur  
  
1.  Actuellement, seule la base de données Finance est abonnée à la catégorie de stratégie Finance. Dans de nombreux cas, il est plus facile d'appliquer la catégorie de stratégie à l'ensemble du serveur. Dans l’Explorateur d’objets, développez **Gestion**, cliquez avec le bouton droit sur **Gestion de la stratégie**, puis cliquez sur **Gérer les catégories**.  
  
2.  Dans la boîte de dialogue **Administration des catégories de stratégie** , recherchez la catégorie Finance et cochez la case **Abonnements à la base de données autorisée** pour la catégorie Finance.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] Désormais, la catégorie Finance s’applique à toutes les bases de données, mais la condition que vous avez créée restreint la stratégie Nom financier à la base de données Finance. Cela démontre la manière dont vous pouvez utiliser des combinaisons de conditions complexes pour cibler des stratégies pour une application correcte sur de nombreux serveurs.  
  
## <a name="summary"></a>Résumé  
Ce didacticiel vous a montré comment créer des conditions, des stratégies et des groupes de stratégies de la Gestion basée sur des stratégies et comment appliquer des filtres et vérifier la conformité des cibles de la Gestion basée sur des stratégies.  
  
## <a name="next"></a>Suivant  
Ce didacticiel est terminé. Pour revenir au début, cliquez sur [Didacticiel : Administration de serveurs à l’aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/tutorial-administering-servers-by-using-policy-based-management.md).  
  
Pour obtenir la liste des didacticiels, consultez [Didacticiels pour SQL Server 2016](../../sql-server/tutorials-for-sql-server-2016.md).  
  
## <a name="see-also"></a> Voir aussi  
[Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
  
  
