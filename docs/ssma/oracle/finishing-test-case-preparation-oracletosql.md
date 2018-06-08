---
title: Terminer la préparation du cas de Test (OracleToSQL) | Documents Microsoft
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
ms.assetid: 32f38713-7ae4-48d3-980d-74cadc8545a0
caps.latest.revision: 6
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 5103b990f98246b35de80767cabdc7b94006f23b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="finishing-test-case-preparation-oracletosql"></a>Terminer la préparation du cas de Test (OracleToSQL)
Dernière page de l’Assistant affiche la description du cas de Test et des informations sur les objets impliqués dans le test. En outre, sur cette page vous pouvez définir le test des options d’exécution.  
  
Le **les informations de cas de Test** section affiche le nom de cas de Test et la description.  
  
Le **objets sélectionnés pour être testé** section contient la liste nommée des objets testées regroupés par type d’objet.  
  
Le **objets affectés par Test que vous analyserez** section affiche la liste nommée d’objets, les modifications de données doivent être comparées après l’exécution des objets testée.  
  
## <a name="test-case-settings"></a>Paramètres de cas de test  
Dans le **les paramètres de cas de Test** section, vous pouvez définir l’exécution en suivante les options de test :  
  
### <a name="stop-test-execution-after-first-failure"></a>Arrêter l’exécution de tests après la première défaillance  
Spécifie pour interrompre le test si une erreur se produit pendant l’exécution du test.  
  
-   Si vous choisissez **Oui**, tester l’exécution s’arrête si une erreur se produit.  
  
-   Si vous choisissez **non**, l’exécution de tests se poursuit après une erreur.  
  
### <a name="perform-data-rollback"></a>Effectuer la restauration de données  
Permet la restauration automatique des données après l’exécution du test.  
  
-   Si vous choisissez **Oui**, les modifications de données seront perdues après l’exécution du test.  
  
-   Si vous choisissez **non**, tous les tests de l’exécution de modifications de données seront enregistrées.  
  
### <a name="auxiliary-tables-saving-mode"></a>Mode d’économie de tables auxiliaires  
Définit le mode d’enregistrement pour les tables auxiliaires créés pendant l’exécution du test. Consultez la description de tables auxiliaires dans le [cas de Test en cours d’exécution &#40;OracleToSQL&#41; ](../../ssma/oracle/running-test-cases-oracletosql.md) rubrique.  
  
-   Si vous sélectionnez **toujours enregistrer**, les données de table auxiliaire sont toujours stockées pour une utilisation ultérieure.  
  
-   Si vous sélectionnez **enregistrer en cas d’échec de la comparaison des tables**, les données de table auxiliaire doivent être stockées uniquement si une erreur se produit.  
  
-   Si vous sélectionnez **toujours supprimer**, tables auxiliaires toujours être supprimées après l’exécution du test.  
  
-   Si vous sélectionnez **demandez à un utilisateur en cas d’échec de la comparaison des tables**, l’utilisateur peut sélectionner l’action requise si une erreur se produit.  
  
Cliquez sur le **Terminer** bouton pour enregistrer le cas de Test prêt dans [référentiels de Test à l’aide (OracleToSQL)](http://msdn.microsoft.com/en-us/f941cce4-d3e3-4aeb-a88a-4f101a97a9f4).  
  
## <a name="see-also"></a>Voir aussi  
[À l’aide de Test référentiels &#40;OracleToSQL&#41;](../../ssma/oracle/using-test-repositories-oracletosql.md)  
[Exécuter des cas de Test &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test de migration des objets de base de données &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
