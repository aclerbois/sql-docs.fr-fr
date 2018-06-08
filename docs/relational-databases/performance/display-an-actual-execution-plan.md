---
title: Afficher un plan d’exécution réel | Microsoft Docs
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
- displaying execution plans
- actual execution plans
- viewing execution plans
- execution plans [SQL Server], displaying
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
caps.latest.revision: 24
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 35be4c1b54060b65d328de29a7acc13326e6b25b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="display-an-actual-execution-plan"></a>Afficher un plan d'exécution réel
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique explique comment générer des plans d'exécution graphiques réels à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Les plans d’exécution réels sont générés une fois que les requêtes ou les lots [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été exécutés. Pour cette raison, un plan d’exécution réel contient des informations d’exécution, comme des avertissements d’exécution (s’il y en a) et des métriques d’utilisation des ressources réelles. Le plan d’exécution généré affiche le plan d’exécution réel que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] a utilisé pour exécuter les requêtes.  
  
 Pour être en mesure d'utiliser cette fonction, les utilisateurs doivent bénéficier des autorisations permettant d'exécuter les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] pour lesquelles un plan d'exécution graphique est actuellement généré. Qui plus est, les utilisateurs doivent se voir accorder l'autorisation SHOWPLAN pour toutes les bases de données référencées par la requête.  
  
### <a name="to-include-an-execution-plan-for-a-query-during-execution"></a>Pour inclure un plan d'exécution pour une requête durant l'exécution  
  
1.  Dans la barre d’outils [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , cliquez sur **Requête de moteur de base de données**. Vous pouvez également ouvrir une requête existante et afficher le plan d’exécution estimé en cliquant sur le bouton **Ouvrir un fichier** dans la barre d’outils puis en choisissant la requête existante. 
  
2.  Entrez la requête pour laquelle vous souhaitez afficher le plan d'exécution réel.  
  
3.  Dans le menu **Requête** , cliquez sur **Inclure le plan d’exécution réel** ou cliquez sur le bouton de la barre d’outils **Inclure le plan d’exécution réel** .  
  
4.  Exécutez la requête en cliquant sur le bouton de la barre d’outils **Exécuter** . Le plan utilisé par l’optimiseur de requête est affiché sous l’onglet **Plan d’exécution** dans le volet de résultats. Placez le curseur de la souris sur les opérateurs logiques et physiques pour visualiser la description et les propriétés des opérateurs dans l'info-bulle affichée.  
  
     Il est également possible d'afficher les propriétés des opérateurs dans la fenêtre Propriétés. Si Propriétés n’est pas visible, cliquez avec le bouton droit sur un opérateur et sélectionnez **Propriétés**. Sélectionnez l'opérateur de votre choix.  
  
5.  Si vous voulez modifier l’affichage du plan d’exécution, cliquez avec le bouton droit sur le plan d’exécution et sélectionnez **Zoom avant**, **Zoom arrière**, **Zoom personnalisé**ou **Zoom pour ajuster**. Les options**Zoom avant** et **Zoom arrière** vous permettent d’effectuer un zoom avant ou arrière sur le plan d’exécution. Quant à l’option **Zoom personnalisé** , elle vous permet de définir votre propre zoom, par exemple, un pourcentage de zoom de 80. Enfin, l’option**Zoom pour ajuster** agrandit le plan de sorte que sa taille soit ajustée en fonction de celle du volet de résultats. Vous pouvez également utiliser une combinaison de la touche Ctrl et de la roulette de votre souris pour activer le **zoom dynamique**.  
  
 
 > [!NOTE] 
 > Vous pouvez également utiliser [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) pour retourner les informations du plan d’exécution pour chaque instruction après l’avoir exécuté. En cas d’utilisation dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’onglet *Résultats* contient un lien pour ouvrir le plan d’exécution sous forme de graphique.   
