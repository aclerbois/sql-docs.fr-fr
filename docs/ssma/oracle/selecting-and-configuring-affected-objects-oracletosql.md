---
title: Sélectionner et configurer les objets (OracleToSQL) affectés | Documents Microsoft
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
- Columns Comparison Settings
- Selection of Affected Objects
ms.assetid: 545eeda2-9829-4187-a858-619a96b4b71d
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 4593830a2cc22b04977f117502bb14118b1b31a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="selecting-and-configuring-affected-objects-oracletosql"></a>Sélectionner et configurer les objets (OracleToSQL) affectés
Sur cette page, vous pouvez sélectionner les tables et les clés étrangères, les modifications dans lequel doivent être comparés quand SSMA vérifie les résultats de l’exécution pour les objets sélectionnés dans l’étape précédente. En outre, vous pouvez personnaliser les paramètres de vérification.  
  
## <a name="selection-of-affected-objects"></a>Sélection des objets affectés  
Dans l’arborescence des objets Oracle situé sur le côté gauche de la fenêtre, vérifiez les tables et les clés étrangères, les modifications dans lequel doivent être comparées pour étant identiques.  
  
Si le testeur de SSMA ne peut pas vérifier la valeur d’un de ces objets, vous verrez le lien **certains des objets sélectionnés contiennent des erreurs** sous l’arborescence d’objets. Cliquez sur ce lien pour afficher les raisons pour lesquelles ces objets ne peuvent pas être comparées et pour effacer la sélection d’objets incorrectes.  
  
## <a name="table"></a>Table  
L’onglet de la Table contient l’affichage de grille de la table sélectionnée. La grille contient les informations suivantes sur la table sélectionnée :  
  
-   Nom de la colonne  
  
-   Type de données  
  
-   Précision  
  
-   Échelle  
  
-   Règle  
  
-   Par défaut  
  
-   Identité  
  
-   Nullable  
  
## <a name="sql"></a>SQL  
Onglet SQL contient la table « créer » SQL de la table sélectionnée.  
  
## <a name="data"></a>data  
Onglet données affiche les données présentes dans la table sélectionnée.  
  
## <a name="properties"></a>Propriétés  
Onglet Propriétés affiche les propriétés de la table sélectionnée. Les champs suivants ne figurent pas sous l’onglet Propriétés :  
  
-   Création ou dernière modification  
  
-   Nom de l’objet  
  
## <a name="columns-comparison-settings"></a>Paramètres de comparaison des colonnes  
Établissez les règles de comparaison pour les colonnes de table sur **comparaison des colonnes** page. Vous pouvez rendre les paramètres suivants.  
  
### <a name="use-during-test-comparisons"></a>Utilisez des comparaisons de tests  
Déterminer si cette colonne fera partie de vérification des résultats de test.  
  
-   Si vous choisissez **True**, SSMA compare le contenu de cette colonne après l’exécution du test sur Oracle avec le contenu de la colonne dans SQL Server. 
  
-   Si vous choisissez**False**, la colonne est exclue de vérification des résultats.  
  
### <a name="use-custom-scale"></a>Utiliser l’échelle personnalisée  
Pour les colonnes de type de données numérique, vous pouvez définir une échelle personnalisée pour la comparaison.  
  
-   Si vous choisissez **True**, valeurs numériques seront arrondies en fonction de la **comparaison de la montée en puissance** valeur avant d’être comparées.  
  
-   Si vous choisissez**False**, la comparaison numérique seront exacte.  
  
### <a name="comparing-scale"></a>Comparaison de l’échelle  
  
-   Disponible uniquement si le **Use Custom Scale** option est définie sur **True**. Il s’agit de la précision d’une comparaison numérique.  
  
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
  
-   Si vous choisissez **False**, la comparaison en compte de la casse.  
  
## <a name="comparing-sql"></a>Comparaison de SQL  
Vous pouvez afficher les instructions SELECT, générées par le testeur de SSMA sur le **SQL de comparaison** page. Le testeur comparera les jeux de résultats de ces instructions sur une ligne par ligne de base. Chaque ligne suivante d’un jeu de résultats Oracle doit être égale à la ligne suivante du jeu de résultats produit dans SQL Server.
  
Vous pouvez modifier les instructions SELECT pour fournir une vérification personnalisée. Pour enregistrer les modifications dans les instructions de SQL Server et Oracle, utilisez le **appliquer** boutons sous la source et la cible SQL, en conséquence.  
  
## <a name="next-step"></a>Étape suivante  
[Personnalisation des appels ordre &#40;OracleToSQL&#41;](../../ssma/oracle/customizing-calls-order-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Préparation du cas de Test de fin &#40;OracleToSQL&#41;](../../ssma/oracle/finishing-test-case-preparation-oracletosql.md)  
[Exécuter des cas de Test &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test de migration des objets de base de données &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
