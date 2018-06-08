---
title: Options Demande de profil de dépendance fonctionnelle (tâche de profilage des données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: 6eb853aa-8016-490c-be4f-06ab8d7f5021
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a1117cc82dee86fa845dc817a7c98eaefe7e4548
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functional-dependency-profile-request-options-data-profiling-task"></a>Options Demande de profil de dépendance fonctionnelle (tâche de profilage des données)
  Utilisez le volet **Propriétés de la demande** de la page **Demandes de profil** pour définir les options de la **Demande de profil de dépendance fonctionnelle** sélectionnée dans le volet Demandes. Un profil de dépendance fonctionnelle indique le degré de dépendance entre les valeurs d'une colonne (colonne dépendante) et celles d'une autre colonne ou d'un ensemble de colonnes (colonne déterminante). Ce profil peut également vous aider à identifier les problèmes dans vos données, tels que les valeurs non valides. Par exemple, vous profilez la dépendance entre une colonne Code postal et une colonne des états des États-Unis. Dans ce profil, la même colonne Code postal doit toujours afficher le même état mais le profil détecte des violations de la dépendance.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="understanding-the-selection-of-determinant-and-dependent-columns"></a>Fonctionnement du processus de sélection des colonnes déterminantes et dépendantes  
 Une **demande de profil de dépendance fonctionnelle** calcule le degré avec lequel la colonne ou l’ensemble de colonnes déterminantes (spécifié dans la propriété **DeterminantColumns** ) détermine la valeur de la colonne dépendante (spécifiée dans la propriété **DependentColumn** ). Par exemple, une colonne des états États-Unis doit fonctionnellement dépendre d'une colonne États-Unis/Code postal. Autrement dit, si la colonne Code postal (colonne déterminante) est 98052, l'état (colonne dépendante) doit toujours être Washington.  
  
 Du côté déterminant, vous pouvez spécifier une colonne ou un ensemble de colonnes dans la propriété **DeterminantColumns** . Par exemple, imaginez une table qui contient les colonnes A, B et C. Vous effectuez les sélections suivantes pour la propriété **DeterminantColumns** :  
  
-   Quand vous sélectionnez le caractère générique **(\*)**, la tâche de profilage des données teste chaque colonne en tant que côté déterminant de la dépendance.  
  
-   Quand vous sélectionnez le caractère générique **(\*)** et une ou plusieurs autres colonnes, la tâche de profilage des données teste chaque combinaison de colonnes en tant que côté déterminant de la dépendance. Par exemple, imaginez une table composée des colonnes A, B et C. Si vous spécifiez **(\*)** et la colonne C comme valeur de la propriété **DeterminantColumns**, la tâche de profilage des données teste les combinaisons (A, C) et (B, C) en tant que côté déterminant de la dépendance.  
  
 Du côté dépendant, vous pouvez spécifier une colonne unique ou le caractère générique **(\*)** dans la propriété **DependentColumn**. Quand vous sélectionnez **(\*)**, la tâche de profilage des données teste la colonne ou l’ensemble de colonnes du côté déterminant avec chaque colonne.  
  
> [!NOTE]  
>  Si vous sélectionnez **(\*)**, cette option risque d’occasionner un grand nombre de calculs et de diminuer les performances de la tâche. En revanche, si la tâche détecte un sous-ensemble qui respecte le seuil défini pour une dépendance fonctionnelle, la tâche ne procède pas à l'analyse des combinaisons supplémentaires. Par exemple, dans l'exemple de table décrit ci-dessus, si la tâche détermine que la colonne C est une colonne déterminante, la tâche ne poursuit pas l'analyse des candidats composites.  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **demande de profil de dépendance fonctionnelle**, le volet **Propriétés de la demande** affiche les groupes d’options suivants :  
  
-   **Données**, qui inclut les options **DeterminantColumns** et **DependentColumn**  
  
-   **Général**  
  
-   **Options**  
  
### <a name="data-options"></a>Options de données  
 **ConnectionManager**  
 Sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) pour établir la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table ou la vue à profiler.  
  
 **TableOrView**  
 Sélectionnez la table ou la vue existante à profiler.  
  
 **DeterminantColumns**  
 Sélectionnez la colonne ou l'ensemble de colonnes déterminantes, à savoir la colonne ou l'ensemble de colonnes dont les valeurs déterminent la valeur de la colonne dépendante.  
  
 Pour plus d'informations, consultez les sections, « Fonctionnement du processus de sélection des colonnes déterminantes et dépendantes » et « Options DeterminantColumns et DependentColumn » dans cette rubrique.  
  
 **DependentColumn**  
 Sélectionnez la colonne dépendante, c'est-à-dire la colonne dont la valeur est déterminée par la valeur de la colonne ou de l'ensemble de colonnes du côté déterminant.  
  
 Pour plus d'informations, consultez les sections, « Fonctionnement du processus de sélection des colonnes déterminantes et dépendantes » et « Options DeterminantColumns et DependentColumn » dans cette rubrique.  
  
#### <a name="determinantcolumns-and-dependentcolumn-options"></a>Options DeterminantColumns et DependentColumn  
 Les options suivantes sont proposées pour chaque colonne sélectionnée à des fins de profilage dans **DeterminantColumns** et **DependentColumn**.  
  
 Pour plus d'informations, consultez la section « Fonctionnement du processus de sélection des colonnes déterminantes et dépendantes » plus haut dans cette rubrique.  
  
 **IsWildCard**  
 Indique si le caractère générique **(\*)** a été sélectionné. Cette option a la valeur **True** si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Sa valeur est **False** si vous avez sélectionné une colonne spécifique dont le profil doit être généré. Cette option est en lecture seule.  
  
 **ColumnName**  
 Affiche le nom de la colonne sélectionnée. Cette option est vide si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Cette option est en lecture seule.  
  
 **StringCompareOptions**  
 Sélectionnez les options de comparaison des valeurs de chaîne. Cette propriété dispose des options répertoriées dans le tableau suivant. La valeur par défaut de cette option est **Default**.  
  
> [!NOTE]  
>  Quand vous utilisez le caractère générique **(\*)** pour **ColumnName**, **CompareOptions** est en lecture seule et est définie avec le paramètre **Default**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Default**|Trie et compare des données d'après le classement de la colonne dans la table source.|  
|**BinarySort**|Trie et compare les données en fonction des modèles binaires définis pour chaque caractère. L'ordre de tri binaire respecte la casse et les accents. Il s'agit aussi de l'ordre de tri le plus rapide.|  
|**DictionarySort**|Trie et compare des données d'après les règles de tri et de comparaison telles que définies dans les dictionnaires de la langue ou de l'alphabet associé.|  
  
 Si vous sélectionnez **DictionarySort**, vous pouvez également sélectionner toutes les combinaisons d’options répertoriées dans le tableau suivant. Par défaut, aucune de ces options supplémentaires n'est sélectionnée.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**IgnoreCase**|Indique si la comparaison fait la distinction entre les lettres majuscules et minuscules. Si cette option est définie, la comparaison de chaînes ignore la casse. Par exemple, « ABC » est alors identique à « abc ».|  
|**IgnoreNonSpace**|Indique si la comparaison fait la distinction entre les caractères avec espace et les caractères diacritiques. Si cette option est définie, la comparaison ignore les caractères diacritiques. Par exemple, « å » est identique à « a ».|  
|**IgnoreKanaType**|Indique si la comparaison fait la distinction entre les deux types de caractères japonais Kana : Hiragana et Katakana. Si cette option est définie, la comparaison de chaînes ignore le type Kana.|  
|**IgnoreWidth**|Indique si la comparaison fait la distinction entre un caractère sur un octet et le même caractère représenté sur deux octets. Si cette option est définie, la comparaison de chaînes traite les représentations sur un octet et sur deux octets du même caractère comme identiques.|  
  
### <a name="general-options"></a>Options générales  
 **RequestID**  
 Tapez un nom descriptif pour identifier cette demande de profil. En règle générale, il n'est pas nécessaire de modifier la valeur générée automatiquement.  
  
### <a name="options"></a>.  
 **ThresholdSetting**  
 Spécifiez le paramètre de seuil. La valeur par défaut de cette propriété est **Specified**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Aucun seuil n'est spécifié. La puissance de la dépendance fonctionnelle est précisée, quelle que soit sa valeur.|  
|**Specified**|Utilisez le seuil spécifié dans **FDStrengthThreshold**. La puissance de la dépendance fonctionnelle est précisée uniquement si elle est supérieure au seuil.|  
|**Exact**|Aucun seuil n'est spécifié. La puissance de la dépendance fonctionnelle est précisée uniquement si la dépendance fonctionnelle entre les colonnes sélectionnées est exacte.|  
  
 **FDStrengthThreshold**  
 Spécifiez le seuil (au moyen d'une valeur comprise entre 0 et 1) au-dessus duquel la puissance de la dépendance fonctionnelle doit être précisée. La valeur par défaut de cette propriété est 0,95. Cette option est activée uniquement quand **Specified** est sélectionné comme valeur de **ThresholdSetting**.  
  
 **MaxNumberOfViolations**  
 Spécifiez le nombre maximal de violations de dépendance fonctionnelle à signaler dans la sortie. La valeur par défaut de cette propriété est 100. Cette option est désactivée quand **Exact** est sélectionné comme valeur de **ThresholdSetting**.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
