---
title: À l’aide de référentiels de Test (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Test Cases Repository
- Test Results Repository
ms.assetid: f941cce4-d3e3-4aeb-a88a-4f101a97a9f4
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: aea9c873a139ed53b64098ef88c9e23aef39bef4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-test-repositories-oracletosql"></a>À l’aide de référentiels de Test (OracleToSQL)
Le référentiel de Test de SSMA magasins SSMA testeur des cas de test et les résultats des tests pour une utilisation ultérieure. Les données de référentiel sont enregistrées dans les tables SQL Server **TestCaseRepository** et **RunTestCaseResultRepository** dans le schéma **ssma_oracle_utilities** de **ssmatesterdb** base de données.  
  
Les boutons suivants sont disponibles dans la boîte de dialogue référentiel de cas de Test :  
  
-   Cliquez sur le **Actualiser** bouton pour actualiser la liste des cas de Test et résultats des tests.  
  
-   Cliquez sur le **fermer** pour fermer la boîte de dialogue référentiel de cas de Test.  
  
## <a name="test-cases-repository"></a>Référentiel de cas de test  
Vous pouvez afficher le référentiel de cas de Test en cliquant sur **des cas de Test...** à partir de la **testeur** menu. SSMA affiche ensuite le **référentiel de cas de Test** boîte de dialogue avec une liste de cas de test enregistrés sur le **des cas de Test** page.  
  
La grille affiche les informations suivantes sur chaque cas de test :  
  
-   Nom : Nom de cas de test.  
  
-   Créée : La cas de test date de création.  
  
-   Modifié : Le cas de test date de dernière modification.  
  
-   Description : Les cas de test descriptions.  
  
Les boutons suivants sont disponibles sur la page de cas de Test :  
  
-   Cliquez sur le **ajouter** bouton pour exécuter l’Assistant de cas de Test et créer un nouveau test.  
  
-   Cliquez sur le **supprimer** bouton pour supprimer le test sélectionné à partir du référentiel. Lorsqu’un cas de Test est supprimé, tous les résultats des tests associés sont également supprimés.  
  
-   Cliquez sur le **modifier** bouton pour exécuter l’Assistant de cas de Test et modifier le test sélectionné.  
  
-   Cliquez sur le **exécuter** bouton pour ouvrir la [cas de Test en cours d’exécution (OracleToSQL)](http://msdn.microsoft.com/en-us/fc208cdb-7373-4f6b-8f6c-cdff9d3dcd02) boîte de dialogue et exécuter le test sélectionné.  
  
## <a name="test-results-repository"></a>Référentiel des résultats des tests  
Vous pouvez afficher le référentiel des résultats des tests sur le **résultats des tests** page de la **référentiel de cas de Test** fenêtre. Ouvrir en cliquant sur **résultats des tests en cours...** à partir de la **testeur** menu.  
  
Vous pouvez utiliser deux filtres sur **résultats des tests** page :  
  
-   Le filtre de nom de cas de Test : permet de choisir les résultats des tests par nom de cas de test. De ce filtre **tous les cas de Test** valeur permet d’afficher les résultats des tests pour tous les cas de test.  
  
-   Le filtre de Date de l’exécution du cas de Test : résultats des tests de filtres à la date de l’enregistrement. De ce filtre **toutes les périodes** valeur permet d’afficher les résultats des tests pour n’importe quelle date de l’enregistrement.  
  
Les informations suivantes sur les résultats des tests s’affiche dans la grille.  
  
-   Nom : nom de cas de Test.  
  
-   Enregistré : Tester la date de dossier de l’enregistrement.  
  
-   Résultats : Un bref résumé de l’exécution du test (info-bulle cette cellule affiche un résumé complet de l’exécution du test).  
  
Les boutons suivants sont disponibles sur la page de résultats de Test :  
  
-   Cliquez sur le **vue** pour ouvrir [afficher des rapports de cas de Test &#40;OracleToSQL&#41; ](../../ssma/oracle/viewing-test-case-reports-oracletosql.md) de résultat de cas de Test actuel.  
  
-   Cliquez sur le **supprimer** bouton pour supprimer le résultat du Test sélectionné  
  
## <a name="see-also"></a>Voir aussi  
[Exécuter des cas de Test &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test de migration des objets de base de données &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
