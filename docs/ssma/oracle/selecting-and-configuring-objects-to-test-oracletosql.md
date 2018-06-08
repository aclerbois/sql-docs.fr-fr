---
title: Sélection et configuration des objets de Test (OracleToSQL) | Documents Microsoft
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
- Selection of Objects to Test,Parameter Comparison Settings
ms.assetid: 29fb6542-5c1f-4b14-a822-87700beb7623
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 8f4e53df32355dbe26efee5f680eb19e529d9670
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="selecting-and-configuring-objects-to-test-oracletosql"></a>Sélection et configuration des objets de Test (OracleToSQL)
À ce stade, sélectionner les objets pour tester et de configurer les paramètres pour la comparaison des procédures et fonctions paramètres de sortie, ainsi que les valeurs de retour des fonctions.  
  
## <a name="selection-of-objects-to-test"></a>Sélection d’objets de Test  
Dans l’arborescence des objets Oracle situé sur le côté gauche de la fenêtre, vérifiez les objets que vous souhaitez appeler pendant le processus de test. Consultez la liste complète des objets testables dans le [test des objets de base de données migrés &#40;OracleToSQL&#41; ](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md) rubrique.  
  
Si le testeur de SSMA ne prend pas en charge les objets sélectionnés pour le test, vous verrez le lien **certains des objets sélectionnés contiennent des erreurs** sous l’arborescence d’objets. Cliquez sur ce lien pour afficher les raisons pour lesquelles ces objets ne peuvent pas être testées et pour effacer la sélection d’objets incorrectes.  
  
Sur le côté droit, vous pouvez afficher plusieurs pages le **SQL** page affiche la définition de l’objet actif. Le **paramètres** page répertorie les paramètres si l’objet est une procédure stockée ou une fonction. Le **propriétés** page montre d’autres caractéristiques de l’objet. Consultez la description de **paramètre comparaisons** et **appeler les valeurs** pages ci-dessous.  
  
## <a name="parameter-comparison-settings"></a>Paramètres de comparaison  
Établissez les règles de comparaison des paramètres de sortie et de retourner des valeurs dans le **paramètre comparaisons** page. Vous pouvez rendre les paramètres suivants.  
  
### <a name="use-during-test-comparisons"></a>Utilisez des comparaisons de tests  
Activer à l’aide du paramètre sélectionné dans la comparaison des résultats de test.  
  
-   Si vous choisissez **True**, SSMA compare la valeur de ce paramètre de sortie après l’exécution de la procédure sur Oracle avec la valeur correspondante sur SQL Server.
  
-   Si vous choisissez**False**, le paramètre sera exclu de la vérification des résultats.  
  
### <a name="use-custom-scale"></a>Utiliser l’échelle personnalisée  
Pour les paramètres de type de données numérique, vous pouvez définir une échelle personnalisée pour la comparaison.  
  
-   Si vous choisissez **True**, valeurs numériques seront arrondies en fonction de la **comparaison de la montée en puissance** valeur avant d’être comparées.  
  
-   Si vous choisissez**False**, la comparaison numérique seront exacte.  
  
### <a name="comparing-scale"></a>Comparaison de l’échelle  
Disponible uniquement si le **Use Custom Scale** option est définie sur **True**. Il s’agit de la précision d’une comparaison numérique.  
  
### <a name="date-time-comparing"></a>Comparaison de date heure  
Définit la date/heure sont comparés.  
  
-   Si vous sélectionnez **comparer un ensemble Date**, une comparaison complète des valeurs de ces deux plateformes sera effectuée.  
  
-   Si vous sélectionnez **comparer uniquement Date**, l’heure partie est ignorée.  
  
-   Si vous sélectionnez **comparer l’heure uniquement**, la date partie est ignorée.  
  
-   Si vous sélectionnez **ignorer les millisecondes**, les résultats sont comparés jusqu'à secondes.  
  
-   Si vous sélectionnez **ignorer la Date et les millisecondes**, le résultat sera uniquement par rapport à la partie heure et ignorer des parties fractionnaires d’une seconde.  
  
### <a name="ignore-strings-case"></a>Ignorer la casse des chaînes  
Contrôle le respect de la casse de la comparaison.  
  
-   Si vous choisissez **True**, la comparaison sera insensible à la casse.  
  
-   Si vous choisissez **False**, la comparaison de respecter la casse.  
  
### <a name="ignore-trailing-spaces"></a>Ignorez les espaces de fin  
Contrôle la façon dont les espaces à droite sont traités pendant la comparaison.  
  
-   Si vous choisissez **True**, les chaînes comparées seront supprimés de droite avant la comparaison.  
  
-   Si vous choisissez **False**, les chaînes comparées conserver les espaces de fin.  
  
## <a name="specify-input-values-for-procedures-and-functions-call-values"></a>Spécifiez les valeurs d’entrée pour les procédures et fonctions (valeurs appeler)  
Vous pouvez spécifier des valeurs de paramètre d’entrée sur le **appeler les valeurs** page. Le **ajouter un appel** bouton ajoute un nouvel appel avec des valeurs de paramètres vide. Le **supprimer les appels** bouton supprime l’appel en cours.  
  
## <a name="next-step"></a>Étape suivante  
[Sélectionner et configurer les objets affectés &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Test de migration des objets de base de données &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
