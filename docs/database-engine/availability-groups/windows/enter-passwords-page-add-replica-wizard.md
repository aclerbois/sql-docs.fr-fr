---
title: Page Entrer les mots de passe (Assistant Ajouter un réplica) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.enterpasswords.f1
ms.assetid: e69207a0-c5c4-44e4-ae9a-4afbb67251d1
caps.latest.revision: 7
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 62845db1fc14d357a414b9f3ea8ecf65f9b0a364
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771335"
---
# <a name="enter-passwords-page-add-replica-wizard"></a>Page Entrer les mots de passe (Assistant Ajouter un réplica)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique d’aide décrit les options de la page **Entrer les mots de passe** . Cette rubrique s’applique à [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
 Si les réplicas que vous avez sélectionnés sur la page **Spécifier les réplicas** contiennent des bases de données comportant une clé principale de base de données, la page Entrer les mots de passe s’affiche.  
  
## <a name="enter-passwords-options"></a>Options de la page Entrer les mots de passe  
 La grille **Bases de données utilisateur sur cette instance de SQL Server** répertorie chaque base de données utilisateur locale. Les colonnes sont les suivantes :  
  
 **Nom**  
 Affiche le nom d'une base de données utilisateur locale.  
  
 **Taille**  
 Affiche la taille de la base de données, si cette information est connue de l'Assistant.  
  
 **État**  
 Indique **Mot de passe requis** pour les bases de données comportant une clé principale de base de données. Après avoir entré les mots de passe correspondant aux clés principales de base de données dans la colonne **Mots de passe** , cliquez sur **Actualiser**. Si les mots de passe entrés sont corrects, la colonne **État** indique **Mot de passe entré**.  
  
 Si la base de données n’a pas de clé principale de base de données, la colonne **État** indique **Mot de passe non nécessaire**.  
  
 **Mot de passe**  
 Si la colonne **État** indique **Mot de passe requis**, entrez le mot de passe correspondant à la clé principale de base de données.  
  
 **Actualiser**  
 Cliquez pour actualiser la grille. Cette opération est utile après la saisie des mots de passe requis.  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Conditions préalables, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
