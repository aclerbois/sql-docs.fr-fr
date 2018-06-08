---
title: Page Sélectionner les bases de données (Assistant Nouveau groupe de disponibilité et Assistant Ajouter une base de données) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectdatabases.f1
- sql13.swb.newagwizard.selectdatabases.f1
ms.assetid: 929c5e15-d087-438d-b1f2-aa97c5f8bff8
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7235f453c1476cea469aedaeca2cafa794dc483a
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769055"
---
# <a name="select-databases-page-new-availability-group-wizard-and-add-database-wizard"></a>Page Sélectionner les bases de données (Assistant Nouveau groupe de disponibilité et Assistant Ajouter une base de données)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique d'aide décrit les options de la page **Spécifier les bases de données** . Cette rubrique s'applique à l' [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] et à l' [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
##  <a name="PageOptions"></a> Options Sélectionner les bases de données  
 La grille **Bases de données utilisateur sur cette instance de SQL Server** répertorie chaque base de données utilisateur locale. Les colonnes sont les suivantes :  
  
 **Nom**  
 Affiche le nom d'une base de données utilisateur locale.  

 **Taille**  
 Affiche la taille de la base de données, si cette information est connue de l'Assistant.  
  
 **État**  
 Affiche un lien hypertexte dont le texte indique si une base de données particulière réunit les conditions préalables requises en vue de l'ajout à un groupe de disponibilité. Si l'état est «**Répond aux conditions requises**», vous pouvez ajouter la base de données au groupe de disponibilité. Si une base de données ne réunit pas l'ensemble des conditions préalables requises, le lien hypertexte **État** fournit une brève explication de la raison pour laquelle la base de données n'est pas éligible. Pour plus d'informations, cliquez sur le lien hypertexte.  
  
 Vous pouvez laisser l'Assistant sur la page **Sélectionner une base de données** lorsque vous agissez sur une base de données afin qu'elle remplisse une condition préalable. Lorsque vous revenez à la page **Sélectionner les bases de données** , cliquez sur **Actualiser** pour mettre à jour la grille.  
  
 **Mot de passe**  
 Si la base de données contient une clé principale de base de données, entrez le mot de passe de la clé principale de base de données.  
  
 **Actualiser**  
 Cliquez pour actualiser la grille. Cette action est utile lorsque vous agissez sur une base de données afin qu'elle remplisse une condition préalable.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
