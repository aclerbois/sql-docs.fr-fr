---
title: Enregistrer séparément les événements Showplan XML (SQL Server Profiler) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Showplan XML events
- saving Showplan XML events
- events [SQL Server], Showplan XML
ms.assetid: 33320a7a-36e8-401c-876d-5b82c49abd85
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 583873ce1c340f071175d2b8ff909304ca632db0
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="save-showplan-xml-events-separately-sql-server-profiler"></a>Enregistrer séparément les événements Showplan XML (SQL Server Profiler)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment enregistrer des événements **Showplan XML** qui sont capturés dans des traces dans des fichiers .SQLPlan différents avec le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Vous pouvez ouvrir les fichiers d’événements **Showplan XML** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], pour voir le plan d’exécution graphique de chaque événement.  
  
## <a name="save-showplan-xml-events-separately"></a>Enregistrer séparément des événements Showplan XML  
  
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
  
7. Dans la colonne de données **Événements**, développez la catégorie d’événements **Performance**, puis cochez la case **Showplan XML**. Si la catégorie d’événements **Performance** n’est pas visible, cochez la case **Afficher tous les événements** pour l’afficher.  
  
     L’onglet **Paramètres d’extraction des événements** est ajouté à la boîte de dialogue **Propriétés de la trace**.  
  
8. Sous l’onglet **Paramètres d’extraction des événements**, sélectionnez **Enregistrer les événements Showplan XML séparément**.  
  
9. Dans la boîte de dialogue **Enregistrer sous** , entrez le nom du fichier dans lequel vous souhaitez stocker les événements **Showplan XML** .  
  
10. Sélectionnez **Tous les lots Showplan XML dans un seul fichier** pour enregistrer tous les événements **Showplan XML** dans un même fichier XML. Sinon, vous pouvez sélectionner l’option **Chaque lot Showplan XML dans un fichier différent** afin de créer un fichier XML pour chaque événement **Showplan XML**.  
  
11. Pour afficher le fichier d’événements **Showplan XML** dans SQL Server Management Studio, ouvrez le menu **Fichier**, pointez sur **Ouvrir**, puis sélectionnez **Fichier**. Parcourez le répertoire où vous avez enregistré le ou les fichiers d’événements **Showplan XML** pour en sélectionner un et l’ouvrir. Les fichiers d'événements**Showplan XML** ont l'extension .SQLPlan.  
  
## <a name="see-also"></a>Voir aussi  
 [Analyser des requêtes avec des résultats Showplan dans SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)  
  
  
