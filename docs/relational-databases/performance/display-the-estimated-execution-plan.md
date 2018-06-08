---
title: Afficher le plan d’exécution estimé | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- zoom [SQL Server]
- estimated execution plan [SQL Server]
- displaying execution plans
- viewing execution plans
- execution plans [SQL Server], displaying
- customizing execution plan display [SQL Server]
- modifying execution plan display
- custom zoom [SQL Server]
ms.assetid: e94aa576-4c0c-4c54-ad05-6c3432cc615b
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ee1716d5549f8c9e7023053751904dedadf871a7
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="display-the-estimated-execution-plan"></a>Affichage du plan d'exécution estimé
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique décrit la génération d'un plan d'exécution estimé au format graphique à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Lors de la génération de ces plans, les traitements ou requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] ne sont pas exécutés, Pour cette raison, un plan d’exécution estimé ne contient aucune information d’exécution, comme des avertissements d’exécution ou des métriques d’utilisation des ressources réelles. Au lieu de cela, le plan d’exécution généré affiche le plan d’exécution des requêtes que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utiliserait probablement si celles-ci étaient réellement exécutées, ainsi qu’une estimation des lignes parcourant les divers opérateurs du plan.  
  
 Pour utiliser cette fonctionnalité, les utilisateurs doivent disposer des autorisations adéquates pour exécuter la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant au plan d'exécution graphique à générer, et ils doivent posséder l'autorisation SHOWPLAN pour toutes les bases de données référencées par la requête.  
  
### <a name="to-display-the-estimated-execution-plan-for-a-query"></a>Pour afficher le plan d'exécution estimé pour une requête  
  
1.  Dans la barre d’outils, cliquez sur **Requête de moteur de base de données**. Vous pouvez également ouvrir une requête existante et afficher le plan d’exécution estimé en cliquant sur le bouton **Ouvrir un fichier** dans la barre d’outils puis en choisissant la requête existante.  
  
2.  Entrez la requête pour laquelle afficher le plan d'exécution estimé.  
  
3.  Dans le menu **Requête** , cliquez sur **Afficher le plan d’exécution estimé** ou cliquez sur le bouton **Afficher le plan d’exécution estimé** dans la barre d’outils. Le plan d’exécution estimé s’affiche sous l’onglet **Plan d’exécution** du volet de résultats. Pour afficher des informations supplémentaires, placez le curseur sur les icônes des opérateurs logiques et physiques, et consultez la description et les propriétés de l'opérateur indiquées dans l'infobulle. Il est également possible d'afficher les propriétés des opérateurs dans la fenêtre Propriétés. Si les propriétés ne sont pas visibles, cliquez avec le bouton droit sur un opérateur, puis cliquez sur **Propriétés**. Sélectionnez l'opérateur de votre choix.  
  
4.  Pour modifier l’affichage du plan d’exécution, cliquez avec le bouton droit sur le plan d’exécution, puis sélectionnez **Zoom avant**, **Zoom arrière**, **Zoom personnalisé**ou **Zoom pour ajuster**. **Zoom avant** et **Zoom arrière** permettent respectivement d’agrandir et de réduire l’affichage du plan d’exécution suivant des pourcentages fixes. **Zoom personnalisé** vous permet de définir votre propre facteur de zoom, par exemple 80 %. Enfin, l’option**Zoom pour ajuster** agrandit le plan de sorte que sa taille soit ajustée en fonction de celle du volet de résultats. Vous pouvez également utiliser une combinaison de la touche Ctrl et de la roulette de votre souris pour activer le **zoom dynamique**.  
 
 > [!NOTE] 
 > Vous pouvez également utiliser [SET SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md) pour retourner les informations du plan d’exécution pour chaque instruction sans l’exécuter. En cas d’utilisation dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’onglet *Résultats* contient un lien pour ouvrir le plan d’exécution sous forme de graphique.   
