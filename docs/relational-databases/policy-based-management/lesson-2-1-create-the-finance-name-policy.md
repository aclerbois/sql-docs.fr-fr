---
title: Créer la stratégie Nom financier | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
ms.assetid: 56b2c852-fd69-4cd2-9b5d-977467b94fd9
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b77e28d77f4e968874fd4c378d2c548eed8aed4e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-1---create-the-finance-name-policy"></a>Leçon 2-1 : Créer la stratégie Nom financier
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette tâche, vous allez créer une base de données nommée Finance, puis créer une condition qui exige que toutes les tables commencent par les lettres **fintbl**. Ensuite, vous allez créer une stratégie et une catégorie de stratégies afin d'appliquer une norme d'affectation de noms pour les tables dans la base de données Finance.  
  
### <a name="to-create-the-finance-database"></a>Pour créer la base de données Finance  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ouvrez une fenêtre de requête et exécutez l'instruction suivante :  
  
    ```  
    CREATE DATABASE Finance ;  
    GO  
    ```  
  
2.  Dans l’Explorateur d’objets, cliquez sur **Bases de données**, puis appuyez sur la touche F5 pour actualiser la liste de bases de données.  
  
### <a name="to-create-the-finance-tables-condition"></a>Pour créer la condition Tables de finance  
  
1.  Dans l’Explorateur d’objets, développez **Gestion**, **Gestion de la stratégie**, cliquez avec le bouton droit sur **Conditions**, puis cliquez sur **Nouvelle condition**.  
  
2.  Dans la boîte de dialogue **Créer une nouvelle condition** , dans la zone **Nom** , tapez **Tables de finance**.  
  
3.  Dans la liste **Facette** , sélectionnez **Nom en plusieurs parties**.  
  
4.  Dans la boîte de dialogue **Expression** , dans la zone **Champ** , sélectionnez **@Name**, dans la zone **Opérateur** , sélectionnez **Comme**et dans la zone **Valeur** , tapez **'fintbl%'** pour forcer tous les noms de tables à commencer par les lettres **fintbl**.  
  
5.  Dans la page **Description** , tapez **Les noms des tables de finance doivent commencer par fintbl**, puis cliquez sur **OK** pour créer la condition.  
  
### <a name="to-create-the-finance-name-policy"></a>Pour créer la stratégie Nom financier  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur **Stratégies**, puis cliquez sur **Nouvelle stratégie**.  
  
2.  Dans la boîte de dialogue **Créer une nouvelle stratégie** , dans la zone **Nom** , tapez **Nom financier**.  
  
3.  Dans la liste **Vérifier la condition** , sélectionnez **Tables de finance**. Cette zone se trouve dans la zone **Nom en plusieurs parties** .  
  
4.  Dans la zone **Par rapport à** est affichée une liste des objets de base de données qui pourraient appliquer cette stratégie. Cochez la case **Toutes les tables**.  
  
5.  Dans la zone **Toutes les bases de données** , développez **Toutes les**, puis cliquez sur **Nouvelle condition**.  
  
6.  Dans la boîte de dialogue **Créer une nouvelle condition** , dans la zone **Nom** , tapez **Base de données de finance**.  
  
7.  Dans la zone **Expression**, complétez l’expression de façon à inclure **@Name = 'Finance'**, puis cliquez sur **OK** pour fermer la page de condition.  
  
    > [!NOTE]  
    > Vous devrez peut-être quitter la zone **Valeur** pour activer le bouton **OK** .  
  
8.  Dans la liste **Mode d’évaluation** , sélectionnez **Sur modification : empêcher**. Cela applique la stratégie en créant un déclencheur de base de données sur la base de données Finance.  
  
9. Sélectionnez la liste **Activé** . (La case **Activé** ne s’applique pas aux stratégies **À la demande** .)  
  
10. Dans la liste **Restriction sur le serveur** , sélectionnez **Aucune**.  
  
11. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-create-the-finance-policy-category"></a>Pour créer la catégorie de stratégies Finance  
  
1.  Dans l’Explorateur d’objets, développez **Gestion**, cliquez avec le bouton droit sur **Gestion de la stratégie**, puis cliquez sur **Gérer les catégories**.  
  
2.  Dans la boîte de dialogue **Administration des catégories de stratégie** , sous **Nom**, tapez **Finance** dans la zone vierge, puis décochez **Abonnements à la base de données autorisée**. **Abonnements à la base de données autorisée** force chaque base de données de l’instance à s’abonner aux stratégies appartenant à cette catégorie de stratégie. Dans le cadre de cette leçon, seule la base de données Finance doit s'abonner à la stratégie Nom financier.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Vérifier et s'abonner à la stratégie Nom financier](../../relational-databases/policy-based-management/lesson-2-2-subscribe-to-and-check-the-finance-name-policy.md)  
  
  
  
