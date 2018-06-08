---
title: Affichage des rapports de cas de Test (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Tester Component,Test Case Reports
ms.assetid: cb75d281-43ef-4f4a-b754-2c4ee3b62ae7
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 69321fdc62eb87b35f183e87ab49a46c3fbb6b21
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="viewing-test-case-reports-sybasetosql"></a>Affichage des rapports de cas de Test (SybaseToSQL)
Le rapport de cas de Test affiche les résultats de test de vérification et les informations de test général. En cas d’échec d’un test, les informations sur toutes les données ne correspondent pas dans les objets vérifiés s’affiche également.  
  
## <a name="report-structure"></a>Structure de rapport  
La partie supérieure du rapport montre ces statistiques :  
  
-   Nombre total d’objets testées et le nombre d’objets pour lesquels le test a réussi.  
  
-   Le nombre total de tables vérifiées et les clés étrangères et le nombre de tables et des clés étrangères correctement mis en correspondance.  
  
-   L’heure de début, heure de fin du cas de test et le temps total nécessaire pour l’exécution.  
  
Le reste du rapport affiche des informations dans quatre catégories :  
  
**Erreurs de configuration requise**  
Affiche toutes les erreurs qui se sont produites à le **conditions préalables** étape. En règle générale, il est ignoré.  
  
**Initialisation**  
Indique l’état de l’exécution en tant que **réussite** ou **échec**.  
  
**Objets résultat de test**  
Une comparaison des résultats (réussite ou échec) et les incompatibilités SSMA testeur détecté en cas d’échec.  
  
**Finalisation**  
Indique l’état de l’exécution en tant que **réussite** ou **échec**.  
  
## <a name="see-also"></a>Voir aussi  
[Exécuter des cas de Test &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Test de migration des objets de base de données &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
