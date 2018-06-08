---
title: Sélection et configuration des objets de Test (SybaseToSQL) | Documents Microsoft
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
- Tester Component,Parameter Comparision Setting
- Tester Component,Selecting Objects
ms.assetid: 89c23aad-bfee-4917-bc16-175288390ac0
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 2f1833ea7a0c2d2be970c5f13c365aa13f504540
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="selecting-and-configuring-objects-to-test-sybasetosql"></a>Sélection et configuration des objets de Test (SybaseToSQL)
À ce stade, sélectionner les objets pour tester et de configurer les paramètres pour la comparaison des procédures et fonctions paramètres de sortie, ainsi que les valeurs de retour des fonctions.  
  
## <a name="selection-of-objects-to-test"></a>Sélection d’objets de Test  
Dans l’arborescence d’objets Sybase situé sur le côté gauche de la fenêtre, vérifiez les objets que vous souhaitez appeler pendant le processus de test. Consultez la liste complète des objets testables dans le [test des objets de base de données migrés &#40;SybaseToSQL&#41; ](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md) rubrique.  
  
Si le testeur de SSMA ne prend pas en charge les objets sélectionnés pour le test, vous verrez le lien **certains des objets sélectionnés contiennent des erreurs** sous l’arborescence d’objets. Cliquez sur ce lien pour afficher les raisons pour lesquelles ces objets ne peuvent pas être testées et pour effacer la sélection d’objets incorrectes.  
  
Sur le côté droit, vous pouvez afficher plusieurs pages le **SQL** page affiche la définition de l’objet actif. Dans le **Pre SQL** et **Post SQL** pages peuvent spécifier des scripts qui s’exécutent avant et après l’appel du test objet démarre. Il s’agit de peut être utile lorsque l’objet requiert des objets supplémentaires ces tables temporaires ou les curseurs. Le **paramètres** page répertorie les paramètres si l’objet est une procédure stockée ou une fonction. Le **propriétés** page montre d’autres caractéristiques de l’objet. Consultez la description de **paramètres Comparsions** et **appeler les valeurs** pages ci-dessous.  
  
## <a name="parameter-comparison-settings"></a>Paramètres de comparaison  
Établissez les règles de comparaison des paramètres de sortie et de retourner des valeurs dans le **comparaison des paramètres** page. Vous pouvez rendre les paramètres suivants.  
  
### <a name="use-during-comparisons"></a>Utilisation des comparaisons  
Activer à l’aide du paramètre sélectionné dans la comparaison des résultats de test.  
  
-   Si vous choisissez **True**, SSMA compare la valeur de ce paramètre de sortie après l’exécution de la procédure sur Sybase avec la valeur correspondante sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]  
  
-   Si vous choisissez**False**, le paramètre sera exclu de la vérification des résultats.  
  
### <a name="use-custom-scale"></a>Utiliser l’échelle personnalisée  
Pour les paramètres de type de données numérique approximatif et fixe à la longueur, vous pouvez définir une échelle personnalisée pour la comparaison.  
  
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
Vous pouvez spécifier des valeurs de paramètre d’entrée sur le **appeler les valeurs** page. Le **ajouter un appel** bouton ajoute un nouvel appel avec des valeurs de paramètres vide. Le **supprimer appeler** bouton supprime l’appel en cours.  
  
## <a name="next-step"></a>Étape suivante  
[Sélectionner et configurer les objets affectés &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Test de migration des objets de base de données &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
