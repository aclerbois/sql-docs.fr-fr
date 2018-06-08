---
title: Créer une stratégie de gestion basée sur des stratégies | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policies
ms.assetid: b28bf963-89f9-4941-b6c1-6004fec347f1
caps.latest.revision: 9
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 43132cbc9dc61ec0782abb01121deff7020d7eeb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-policy-based-management-policy"></a>Créer une stratégie de gestion basée sur des stratégies
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment créer une stratégie de gestion basée sur des stratégies dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour créer une stratégie à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-policy"></a>Pour créer une stratégie  
  
1.  Dans l’ **Explorateur d’objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez créer une nouvelle stratégie de gestion basées sur des stratégies.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez sur le signe plus (+) pour développer **Gestion de la stratégie**.  
  
4.  Cliquez avec le bouton droit sur le dossier **Stratégies** et sélectionnez **Nouvelle stratégie**.  
  
5.  Dans la boîte de dialogue **Créer une nouvelle stratégie** , dans la zone **Nom** , tapez le nom de la nouvelle stratégie.  
  
6.  Si vous souhaitez que la stratégie soit activée dès sa création, activez la case à cocher **Activé** . Si le mode d'évaluation est **À la demande**, la case à cocher **Activé** n'est pas disponible.  
  
7.  Dans la liste **Vérifier la condition** , sélectionnez l'une des conditions existantes ou sélectionnez **Nouvelle condition**. Pour modifier une condition, sélectionnez-la, puis cliquez sur le bouton de sélection (**...**). Pour plus d’informations, consultez [Créer une condition de gestion basée sur des stratégies.](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) ou [Afficher ou modifier les propriétés d’une condition de gestion basée sur des stratégies](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md).  
  
8.  Dans la zone **Par rapport aux cibles** , sélectionnez un ou plusieurs types de cibles pour cette stratégie. Certaines conditions et facettes peuvent être appliquées uniquement à certains types de cibles. Les jeux de cibles disponibles apparaissent dans la zone associée. Développez **Toutes les** pour sélectionner une condition de filtrage pour certains types des cibles. Si aucune cible n'apparaît dans cette zone, la portée de la condition de vérification est le niveau serveur.  
  
9. Dans la zone **Mode d'évaluation** , sélectionnez le comportement de cette stratégie. Différentes conditions peuvent avoir différents modes d'évaluation valides. Pour plus d’informations sur les modes d’évaluation valides, consultez [Administrer des serveurs à l’aide de la gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).  
  
10. Si la stratégie doit être évaluée selon une planification, définissez le mode d'évaluation sur la valeur **Selon la planification**, puis cliquez sur **Choisir** pour sélectionner une planification, ou cliquez sur **Nouvelle** pour créer une planification.  
  
11. Pour limiter la stratégie au sous-ensemble des types cibles, dans la zone **Restriction sur le serveur** , sélectionnez une condition de limitation ou créez-en une.  
  
     Pour plus d'informations sur les options disponibles de la boîte de dialogue **Créer une nouvelle stratégie** , consultez [Create New Policy or Open Policy Dialog Box, General Page](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) ou [Create New Policy or Open Policy Dialog Box, Description Page](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md).  
  
12. Lorsque vous avez terminé, cliquez sur **OK**.  
  
  
