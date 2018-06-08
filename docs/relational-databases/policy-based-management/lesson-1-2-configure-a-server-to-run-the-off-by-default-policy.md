---
title: Configurer un serveur pour exécuter la stratégie Désactivé par défaut | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
ms.assetid: 41c3022d-ab13-443e-ac64-ba1d64584f79
caps.latest.revision: 23
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: bd8198d4ab869b38491a58782a03e73d2aaecb5c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-2---configure-a-server-to-run-the-off-by-default-policy"></a>Leçon 1-2 : configurer un serveur pour exécuter la stratégie Désactivé par défaut
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Vous disposez désormais d'une stratégie nommée Désactivé par défaut. Dans cette tâche, vous allez vérifier si votre serveur est conforme aux spécifications de cette stratégie.  
  
### <a name="to-run-the-off-by-default-policy"></a>Pour exécuter la stratégie Désactivé par défaut  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pointez sur **Stratégies**, puis cliquez sur **Évaluer**.  
  
2.  Dans la boîte de dialogue **Évaluer les stratégies** , vous pouvez sélectionner des stratégies à partir d’une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d’un fichier. Pour cette étape, laissez le paramètre **Source** défini à votre instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
3.  Dans la section **Stratégies** , sélectionnez la stratégie **Désactivé par défaut** .  
  
4.  Pour voir si le serveur est conforme à la stratégie, cliquez sur **Évaluer**.  
  
5.  Dans la zone **Résultats** , vous verrez un cercle vert avec une coche si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est conforme à la stratégie. Vous verrez un cercle rouge avec un X si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'est pas conforme à la stratégie.  
  
6.  Dans la zone **Détails sur les cibles** , des informations supplémentaires apparaîtront dans la colonne **Message** si une erreur se produit. Dans la colonne **Message** , cliquez sur **Afficher** pour voir un rapport contenant les résultats de la vérification de chaque propriété de facette.  
  
7.  La description de stratégie est affichée en bas de la page et la section **Aide supplémentaire** affiche le lien hypertexte que vous avez configuré pour la stratégie. Cliquez sur le lien hypertexte pour ouvrir la page Web que vous avez spécifiée lors de la création de la stratégie.  
  
8.  Fermez le navigateur, puis fermez la boîte de dialogue **Vue détaillée des résultats** .  
  
9. Si le serveur n’est pas conforme et que vous souhaitez désactiver la messagerie de base de données, cliquez sur **Appliquer** dans la page **Résultats d’évaluation** .  
  
10. Fermez les boîtes de dialogue **Vue détaillée des résultats** et **Évaluer les stratégies** .  
  
## <a name="next-lesson"></a>Leçon suivante  
[Leçon 2 : Créer et appliquer une stratégie de normes d'affectation de noms](../../relational-databases/policy-based-management/lesson-2-create-and-apply-a-naming-standards-policy.md)  
  
  
  
