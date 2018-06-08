---
title: Paramètres (objets de système de chargement) du projet (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 9a545233-1b0a-488a-a1ec-c33aa608dcc1
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: dff1d86aa3aebf59791433ec2631b5b46bb831ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settingsloading-system-objects-db2tosql"></a>Paramètres (objets de système de chargement) du projet (DB2ToSQL)
La page de chargement des objets de système de la **les paramètres de projet** boîte de dialogue vous permet de spécifier les objets de système DB2 SSMA convertit et les charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Le volet du chargement des objets système n’est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue :  
  
-   Pour spécifier les paramètres pour tous les projets SSMA, sur le **outils** menu, sélectionnez **par défaut des paramètres de projet**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** déroulante cliquez sur **général** au bas de la gauche, puis cliquez sur **du chargement des objets système**.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, sélectionnez **les paramètres de projet**, cliquez sur **général** au bas de la gauche, puis cliquez sur **du chargement des objets système**.  
  
## <a name="default-settings"></a>Paramètres par défaut  
Conversion d’objets système consomme des ressources système et du temps. Pour améliorer les performances, SSMA sélectionne uniquement les objets système plus fréquemment utilisées, comme indiqué dans la liste suivante :  
  
-   SYS.DBMS_OUTPUT  
  
-   SYS.DBMS_PIPE  
  
-   SYS.DBMS_UTILITY  
  
-   SYS. STANDARD  
  
-   SYS. UTL_FILE  
  
-   SYS.DBMS_LOB  
  
-   SYS.DBMS_SQL  
  
-   SYS.DBMS_SESSION  
  
Si vos objets DB2 font référence aux objets système supplémentaires, vous devez sélectionner ces objets. Si vous ne sélectionnez pas les objets système qui sont référencés par vos objets de base de données DB2, SSMA signale les erreurs de conversion. Si vous recevez des erreurs de conversion dus à l’absence d’objets système, sélectionnez les objets manquants dans cette boîte de dialogue. Vous pouvez ensuite répéter la conversion si nécessaire.  
  
