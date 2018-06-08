---
title: Enregistrer des événements Deadlock Graph (SQL Server Profiler) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- deadlocks [SQL Server], saving deadlock graphs
- graphs [SQL Server]
- saving deadlock graphs
ms.assetid: bf1fc906-abd6-4a89-842e-da0d66b2defe
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: dc79e1f5434361c899e86b7cbc8beb557a98d3a4
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="save-deadlock-graphs-sql-server-profiler"></a>Enregistrer des événements Deadlock Graph (SQL Server Profiler)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment enregistrer un événement Deadlock Graph à l'aide du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Les événements Deadlock Graph sont enregistrés sous forme de fichiers XML.  
  
## <a name="save-deadlock-graph-events-separately"></a>Enregistrer séparément les événements Deadlock Graph  
  
1. Dans le menu **Fichier**, sélectionnez **Nouvelle trace**, puis connectez-vous à une instance de SQL Server.  
  
     La boîte de dialogue **Propriétés de la trace** s'affiche.  
  
    > [!NOTE]  
    >  Si vous cochez l’option **Démarrer le suivi juste après avoir établi la connexion**, la boîte de dialogue **Propriétés de la trace** ne s’affiche pas et le suivi est lancé. Pour désactiver ce paramètre, dans le menu **Outils**, sélectionnez **Options**, puis décochez la case **Démarrer le suivi juste après avoir établi la connexion**.  
  
2. Dans la zone **Nom de la trace** de la boîte de dialogue **Propriétés de la trace** , entrez le nom de la trace.  
  
3. Dans la liste **Utiliser le modèle**, sélectionnez un modèle de trace sur lequel baser le suivi. Si vous ne souhaitez pas utiliser de modèle, sélectionnez **Vide**.  
  
4. Procédez de l'une des manières suivantes :  
  
    -   Pour capturer la trace dans un fichier, cochez la case **Enregistrer dans le fichier**. Spécifiez une valeur dans **Définir la taille de fichier maximale**.  
  
         Au besoin, activez les cases à cocher **Activer la substitution de fichier** et **Le serveur traite les données de trace** . 
  
    -   Pour capturer la trace dans une table de base de données, cochez la case **Enregistrer dans la table**.  
  
         Au besoin, sélectionnez **Définir le nombre de lignes maximal**, puis spécifiez une valeur.  
  
5. Le cas échéant, activez la case à cocher **Activer l'heure d'arrêt de la trace** et indiquez une date et une heure d'arrêt. 
  
6. Sélectionnez l’onglet **Sélection des événements**.  
  
7. Dans la colonne de données **Événements**, développez la catégorie d’événements **Verrous**, puis cochez la case cocher **Deadlock graph**. Si la catégorie d’événements **Verrous** n’est pas disponible, cochez la case **Afficher tous les événements** pour l’afficher.  
  
     L’onglet **Paramètres d’extraction des événements** est ajouté à la boîte de dialogue **Propriétés de la trace**.  
  
8. Sous l’onglet **Paramètres d’extraction des événements**, sélectionnez **Enregistrer les événements Deadlock XML séparément**.  
  
9. Dans la boîte de dialogue **Enregistrer sous**, entrez le nom du fichier dans lequel les événements Deadlock Graph doivent être stockés.  
  
10. Sélectionnez **Tous les lots Deadlock XML dans un seul fichier** pour enregistrer tous les événements Deadlock Graph dans un même fichier XML. Sinon, vous pouvez sélectionner l’option **Chaque lot Deadlock XML dans un fichier différent** afin de créer un fichier XML pour chaque événement Deadlock Graph.  
  
 Après avoir enregistré le fichier Deadlock, vous pouvez l’ouvrir dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Ouvrir, afficher et imprimer un fichier d’interblocage &#40;SQL Server Management Studio&#41;](../../relational-databases/performance/open-view-and-print-a-deadlock-file-sql-server-management-studio.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Analyser des interblocages à l’aide de SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)  
  
  
