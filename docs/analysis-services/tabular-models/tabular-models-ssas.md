---
title: Les modèles tabulaires | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8e85a618379b5b3c6da010c8b25782ed6836edb4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tabular-models"></a>Modèles tabulaires
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les modèles tabulaires sont des bases de données Analysis Services qui s’exécutent en mémoire ou en mode DirectQuery, en accédant aux données directement à partir de sources de données relationnelles principales. À l’aide des algorithmes de compression de pointe et processeur de requêtes multithread, le moteur analytique permet un accès rapide aux données et objets de modèle tabulaire en utilisant des applications clientes telles que Power BI et Excel.  
  
 Alors que les modèles en mémoire sont la valeur par défaut, DirectQuery est un mode de requête alternatif pour les modèles qui sont trop volumineux pour tenir en mémoire, ou lorsque la volatilité des données empêche une stratégie de traitement raisonnable. DirectQuery permet d’obtenir la parité avec les modèles en mémoire prise en charge pour un large éventail de sources de données, permet de gérer les tables calculées et les colonnes dans un modèle DirectQuery, la sécurité de niveau ligne via des expressions DAX qui atteignent la base de données et de requête optimisations qui entraînent un débit plus rapide.
  
 Les modèles tabulaires sont créés dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] à l’aide du modèle de projet de modèle tabulaire qui fournit une aire de conception pour la création d’un modèle, les tables, les relations et les expressions DAX. Vous pouvez importer des données depuis plusieurs sources, puis enrichir le modèle en ajoutant des relations, des colonnes et des tables calculées, des mesures, des indicateurs de performance clés, des hiérarchies et des traductions.  
  
 Les modèles peuvent ensuite être déployés sur Azure Analysis Services ou une instance de SQL Server Analysis Services est configuré pour le mode serveur tabulaire. Les modèles tabulaires déployés peuvent être gérés dans SQL Server Management Studio. À mesure que vos modèles augmentent, ils peuvent partitionnés pour un traitement optimisé et sécurisées au niveau de la ligne à l’aide en fonction du rôle de sécurité.  

  
  
