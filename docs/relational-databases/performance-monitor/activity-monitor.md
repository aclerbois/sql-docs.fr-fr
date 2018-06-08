---
title: Moniteur d’activité | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- Activity Monitor [SQL Server]
ms.assetid: 1e6c430d-3a2a-468e-a3d5-ef5459c36c15
caps.latest.revision: 7
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2e5a6b6fca195140ef2320b0dcb58132f821a134
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="activity-monitor"></a>Moniteur d'activité
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le Moniteur d'activité affiche des informations sur les processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et sur la façon dont ces processus affectent l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le Moniteur d’activité est une fenêtre de document à onglets comportant les volets suivants, qui peuvent être développés ou réduits : **Vue d’ensemble**, **Tâches utilisateur actives**, **Attentes de ressources**, **E/S du fichier de données**et **Requêtes coûteuses récentes**. Lorsqu'un volet est développé, le Moniteur d'activité interroge l'instance pour obtenir des informations. Lorsqu'un volet est réduit, toutes les activités d'interrogation cessent pour ce volet. Vous pouvez développer en même temps un ou plusieurs volets pour afficher différents types d’activité sur l’instance.  
 
 ## <a name="customize-columns"></a>Personnaliser les colonnes 
 Pour les colonnes incluses dans les volets **Tâches utilisateur actives**, **Attentes de ressources**, **E/S du fichier de données**et **Requêtes coûteuses récentes** , personnalisez l’affichage comme suit :  
  
1.  Pour réorganiser l’ordre des colonnes, cliquez sur l’en-tête de colonne et faites-le glisser vers un autre emplacement dans le ruban de titre.  
  
2.  Pour trier une colonne, cliquez sur son nom.  
  
3.  Pour effectuer un filtrage sur une ou plusieurs colonnes, cliquez sur la flèche de déroulement de l'en-tête de colonne, puis sélectionnez une valeur.  
  
## <a name="more-information"></a>Informations complémentaires  
   
|||  
|-|-|  
|Décrit comment ouvrir le Moniteur d'activité et définir son intervalle d'actualisation.|[Ouvrir le Moniteur d’activité &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)|  
|Liens vers des rubriques relatives à la surveillance des performances et de l'activité du serveur.|[Analyse des performances et surveillance de l'activité du serveur](../../relational-databases/performance/server-performance-and-activity-monitoring.md)|  
  
  
