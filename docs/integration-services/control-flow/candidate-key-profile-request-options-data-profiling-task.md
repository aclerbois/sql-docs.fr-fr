---
title: Options Demande de profil de clé candidate (tâche de profilage des données) | Microsoft Docs
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
ms.assetid: 8632dbc4-4394-4dc7-b19c-f9adeb21ba52
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2cff19de2067c9a0822784439d952622387bdea0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="candidate-key-profile-request-options-data-profiling-task"></a>Options Demande de profil de clé candidate (tâche de profilage des données)
  Utilisez le volet **Propriétés de la demande** de la page **Demandes de profil** pour définir les options de la **Demande de profil de clé candidate** sélectionnée dans le volet Demandes. Un profil de clé candidate signale si une colonne ou un ensemble de colonnes est une clé, ou une clé approximative, pour la table sélectionnée. Ce profil peut également vous aider à identifier des problèmes dans vos données tels que des valeurs en double dans une colonne clé potentielle.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="understanding-the-selection-of-columns-for-the-keycolumns-property"></a>Fonctionnement de la sélection des colonnes pour la propriété KeyColumns  
 Chaque **Demande de profil de clé candidate** calcule la puissance de clé d’un candidat de clé unique composé d’une colonne unique ou de plusieurs colonnes :  
  
-   Quand vous sélectionnez une seule colonne dans **KeyColumns**, la tâche calcule la puissance de clé de cette colonne.  
  
-   Quand vous sélectionnez plusieurs colonnes dans **KeyColumns**, la tâche calcule la puissance de clé de la clé composite composée de toutes les colonnes sélectionnées.  
  
-   Quand vous sélectionnez le caractère générique **(\*)** dans **KeyColumns**, la tâche calcule la puissance de clé de chaque colonne dans la table ou la vue.  
  
 Par exemple, imaginez une table qui contient des colonnes A, B et C. Vous effectuez les sélections suivantes pour **KeyColumns** :  
  
-   Vous sélectionnez (\*) et la colonne C dans **KeyColumns**. La tâche calcule la puissance de clé de la colonne C, puis celle des candidats de clé composite (A, C) et (B, C).  
  
-   Vous sélectionnez (\*) et (\*) dans **KeyColumns**. La tâche calcule la puissance de clé des colonnes individuelles A, B, et C, puis des candidats de clé composite (A, B), (A, C) et (B, C).  
  
> [!NOTE]  
>  Si vous sélectionnez le caractère générique (*), cette option risque d'aboutir à un grand nombre de calculs et de diminuer les performances de la tâche. En revanche, si la tâche détecte un sous-ensemble qui respecte le seuil défini pour une clé, la tâche ne procède pas à l'analyse des combinaisons supplémentaires. Par exemple, dans l'exemple de table décrit ci-dessus, si la tâche détermine que la colonne C est une clé, la tâche ne poursuit pas l'analyse des candidats de clé composite.  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **demande de profil de clé candidate**, le volet **Propriétés de la demande** affiche les groupes d’options suivants :  
  
-   **Données**, qui incluent les options **TableOrView** et **KeyColumns** .  
  
-   **Général**  
  
-   **Options**  
  
### <a name="data-options"></a>Options de données  
 **ConnectionManager**  
 Sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) pour établir la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table ou la vue à profiler.  
  
 **TableOrView**  
 Sélectionnez la table ou la vue existante à profiler.  
  
 Pour plus d'informations, consultez la section « Options TableorView » dans cette rubrique.  
  
 **KeyColumns**  
 Sélectionnez la colonne ou les colonnes existantes à profiler. Sélectionnez **(\*)** pour profiler toutes les colonnes.  
  
 Pour plus d'informations, consultez les sections « Fonctionnement de la sélection des colonnes pour la propriété KeyColumns » et « Options KeyColumns », plus haut dans cette rubrique.  
  
#### <a name="tableorview-options"></a>Options TableOrView  
 **Schéma**  
 Spécifiez le schéma auquel la table sélectionnée appartient. Cette option est en lecture seule.  
  
 **Table**  
 Affiche le nom de la table sélectionnée. Cette option est en lecture seule.  
  
#### <a name="keycolumns-options"></a>Options KeyColumns  
 Les options suivantes sont proposées pour chaque colonne sélectionnée à des fins de profilage dans **KeyColumns** ou pour l’option **(\*)**.  
  
 Pour plus d'informations, consultez la section « Fonctionnement de la sélection des colonnes pour la propriété KeyColumns » plus haut dans cette rubrique.  
  
 **IsWildcard**  
 Indique si le caractère générique **(\*)** a été sélectionné. Cette option a la valeur **True** si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Sa valeur est **False** si vous avez sélectionné une colonne spécifique dont le profil doit être généré. Cette option est en lecture seule.  
  
 **ColumnName**  
 Affiche le nom de la colonne sélectionnée. Cette option est vide si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Cette option est en lecture seule.  
  
 **StringCompareOptions**  
 Sélectionnez les options de comparaison des valeurs de chaîne. Cette propriété dispose des options répertoriées dans le tableau suivant. La valeur par défaut de cette option est **Default**.  
  
> [!NOTE]  
>  Quand vous utilisez un caractère générique **(\*)** pour **ColumnName**, l’option **CompareOptions** est en lecture seule et est définie sur le paramètre **Par défaut**.  
  
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
 Cette propriété dispose des options répertoriées dans le tableau suivant. La valeur par défaut de cette propriété est **Spécifié**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Aucun seuil n’est spécifié. La puissance de la clé est signalée, quelle que soit sa valeur.|  
|**Spécifié**|Un seuil est spécifié dans **KeyStrengthThreshold**. La puissance de clé est précisée uniquement si elle est supérieure au seuil.|  
|**Exact**|Aucun seuil n’est spécifié. La puissance de clé est précisée uniquement si les colonnes sélectionnées sont une clé exacte.|  
  
 **KeyStrengthThreshold**  
 Spécifiez le seuil (au moyen d'une valeur comprise entre 0 et 1) au-dessus duquel la puissance de clé doit être précisée. La valeur par défaut de cette propriété est 0,95. Cette option est activée uniquement quand **Spécifié** est sélectionné comme valeur **KeyStrengthThresholdSetting**.  
  
 **MaxNumberOfViolations**  
 Spécifiez le nombre maximal de violations de clé candidate à signaler dans la sortie. La valeur par défaut de cette propriété est 100. Cette option est désactivée quand **Exact** est sélectionné en tant que valeur **KeyStrengthThresholdSetting**.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
