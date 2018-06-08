---
title: Exécuter des cas de Test (SybaseToSQL) | Documents Microsoft
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
- Tester Component,Execution Steps
ms.assetid: 195ffdef-cfde-4bf4-a3ae-e7402bb07972
caps.latest.revision: 6
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 5a6f403bd7f4d3168ceeea447c541f311e5eab6f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="running-test-cases-sybasetosql"></a>Cas de Test en cours d’exécution (SybaseToSQL)
Lorsque le testeur de SSMA s’exécute à un cas de Test, il exécute les objets sélectionnés pour le test et crée un rapport sur les résultats de la vérification. Si les résultats sont identiques sur les deux plateformes, le test a réussi. La correspondance des objets entre Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est déterminé en fonction des paramètres de mappage de schéma pour le projet SSMA actuel.  
  
Une condition requise pour un test réussi est que tous les objets Sybase sont convertis et chargées dans la base de données cible. En outre, les données de table doivent être migrées et que le contenu des tables sur les deux plateformes est synchronisé.  
  
## <a name="run-test-case"></a>Exécuter des cas de Test  
Pour exécuter le cas de Test préparée :  
  
1.  Cliquez sur le **exécuter** bouton.  
  
2.  Dans le **se connecter à Sybase** boîte de dialogue, entrez les informations de connexion, puis cliquez sur **connexion**.  
  
Lorsque le test est terminé, le rapport de cas de Test est créé. Cliquez sur le **rapport** bouton pour afficher la [afficher des rapports de cas de Test &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md). Le résultat du test (rapport de cas de Test) est automatiquement stocké dans le [référentiels de Test à l’aide de &#40;SybaseToSQL&#41; ](../../ssma/sybase/using-test-repositories-sybasetosql.md) pour une utilisation ultérieure.  
  
## <a name="test-case-execution-steps"></a>Étapes d’exécution des cas de test  
  
### <a name="prerequisites"></a>Configuration requise  
Testeur de SSMA vérifie si toutes les conditions préalables sont remplies pour que l’exécution du test avant le début du test. Si certaines conditions ne sont pas satisfaites, un message d’erreur s’affiche.  
  
### <a name="initialization"></a>Initialisation  
À ce stade, SSMA testeur crée auxiliaires objets (tables, déclencheurs et vues) à la fois au niveau de la Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Ils permettent aux modifications de suivi dans les tables affectées choisis pour la vérification si le mode de comparaisons de table est **modifie seulement**.  
  
Supposons que la table vérifiée est nommée USER_TABLE. Pour une telle table, les objets auxiliaires suivants sont créés dans Sybase.  
  
Les objets suivants sont créés au Sybase, dans la base de données SSMATESTER2005db ou SSMATESTER2008db et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans la base de données ssmatesterdb_syb.  
  
|Nom|Type| Description|  
|--------|--------|---------------|  
|USER_TABLE$ Trg|Déclencheur|Déclencheur d’audit les modifications dans la table vérifiée.|  
|USER_TABLE$ Aud|Table|Tableau dans lequel sont enregistrés les lignes supprimées et remplacées.|  
|USER_TABLE$ AudID|Table|Tableau dans lequel les lignes nouvelles et modifiées sont enregistrés.|  
|USER_TABLE|Affichage|Représentation simplifiée des modifications de la table.|  
|$ USER_TABLE nouveau|Affichage|Simplifiée de la représentation sous forme de lignes insérées et remplacés.|  
|USER_TABLE$ new_id|Affichage|Identification des lignes insérées et modifiés.|  
|$ USER_TABLE ancien|Affichage|Simplifiée de la représentation sous forme de lignes supprimés et remplacés.|  
  
L’objet suivant est créé dans la base de données de table vérifié à Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
|Nom|Type| Description|  
|--------|--------|---------------|  
|USER_TABLE$ Trg|Déclencheur|Déclencheur d’audit les modifications dans la table vérifiée.|  
  
### <a name="test-object-calls"></a>Appels de l’objet de test  
À ce stade, SSMA testeur appelle chaque objet sélectionné pour le test, compare les résultats et affiche le rapport.  
  
### <a name="finalization"></a>Finalisation  
Lors de la finalisation SSMA testeur nettoie les objets auxiliaires créés lors de la **initialisation** étape.  
  
## <a name="next-step"></a>Étape suivante  
[Affichage des rapports de cas de Test &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Sélection et configuration d’objets pour le Test &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md)  
[Sélectionner et configurer les objets affectés &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
[Test de migration des objets de base de données &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
