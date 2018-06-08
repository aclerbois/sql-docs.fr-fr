---
title: Options Demande de profil d’inclusion de valeur (tâche de profilage des données) | Microsoft Docs
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
ms.assetid: ca94da82-a4c9-4e87-9cba-c2d85bd31f01
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 27c0aa5259be671931cbbe9766302123ad5282d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="value-inclusion-profile-request-options-data-profiling-task"></a>Options Demande de profil d'inclusion de valeur (tâche de profilage des données)
  Utilisez le volet **Propriétés de la demande** de la page **Demandes de profil** pour définir les options de la **Demande de profil d’inclusion de valeur** sélectionnée dans le volet Demandes. Un profil d'inclusion de valeur calcule le chevauchement des valeurs entre deux colonnes ou des ensembles de colonnes. De cette manière, ce profil permet également de déterminer si une colonne ou un ensemble de colonnes peut servir de clé étrangère entre les tables sélectionnées. Ce profil peut également vous aider à identifier les problèmes dans vos données, tels que les valeurs non valides. Par exemple, vous utilisez un profil d'inclusion de valeur pour profiler la colonne ProductID d'une table Sales. et découvrez que la colonne contient des valeurs qui sont introuvables dans la colonne ProductID de la table Products.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="understanding-the-selection-of-columns-for-the-inclusioncolumns-property"></a>Fonctionnement de la sélection des colonnes pour la propriété InclusionColumns  
 Une **demande de profil d’inclusion de valeur** calcule si toutes les valeurs d’un sous-ensemble sont présentes dans le sur-ensemble. Le sur-ensemble est souvent une table de recherche ou de référence. Par exemple, la colonne des états dans une table d'adresses est la table de sous-ensemble. Chaque code d'état à deux caractères dans cette colonne doit également être disponible dans la table des codes postaux des états des États-Unis, ce qui équivaut à la table de sur-ensemble.  
  
 Lorsque vous utilisez le caractère générique (*) en tant que valeur de la colonne côté sous-ensemble ou de la colonne côté sur-ensemble, la tâche de profilage des données compare chaque colonne d'un côté avec la colonne spécifiée de l'autre côté.  
  
> [!NOTE]  
>  Si vous sélectionnez le caractère générique (*), cette option risque d'aboutir à un grand nombre de calculs et de diminuer les performances de la tâche.  
  
## <a name="understanding-the-threshold-settings"></a>Fonctionnement des paramètres de seuil  
 Vous pouvez utiliser deux paramètres de seuil différents pour améliorer la sortie d'une demande de profil d'inclusion de valeur.  
  
 Quand vous spécifiez une valeur autre que **Aucun** pour **InclusionThresholdSetting**, le profil signale la puissance d’inclusion du sous-ensemble figurant dans le sur-ensemble uniquement si l’une des conditions suivantes est satisfaite :  
  
-   quand la puissance d’inclusion dépasse le seuil spécifié dans **InclusionStrengthThreshold**;  
  
-   quand la puissance d’inclusion affiche une valeur égale à 1,0 et si **InclusionStrengthThreshold** a la valeur **Exact**.  
  
 Vous pouvez améliorer davantage la sortie en éliminant les combinaisons pour lesquelles la colonne côté sur-ensemble ne constitue pas une clé adéquate pour la table côté sur-ensemble en raison de valeurs non uniques. Quand vous spécifiez une valeur autre que **Aucun** pour **SupersetColumnsKeyThresholdSetting**, le profil signale la puissance d’inclusion du sous-ensemble figurant dans le sur-ensemble uniquement si l’une des conditions suivantes est satisfaite :  
  
-   quand l’adéquation des colonnes côté sur-ensemble en qualité de clé dans la table du sur-ensemble dépasse le seuil spécifié dans **SupersetColumnsKeyThreshold**;  
  
-   quand la puissance d’inclusion affiche une valeur ou 1.0 et si **SupersetColumnsKeyThreshold** a la valeur **Exact**.  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **Demande de profil d’inclusion de valeur**, le volet **Propriétés de la demande** affiche les groupes d’options suivants :  
  
-   **Données**, qui incluent les options **SubsetTableOrView**, **SupersetTableOrView**et **InclusionColumns**  
  
-   **Général**  
  
-   **Options**  
  
### <a name="data-options"></a>Options de données  
 **ConnectionManager**  
 Sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) pour se connecter à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table ou la vue à profiler.  
  
 **SubsetTableOrView**  
 Sélectionnez la table ou la vue existante à profiler.  
  
 Pour plus d'informations, consultez la section « Options SubsetTableOrView et SupersetTableOrView » dans cette rubrique.  
  
 **SupersetTableOrView**  
 Sélectionnez la table ou la vue existante à profiler.  
  
 Pour plus d'informations, consultez la section « Options SubsetTableOrView et SupersetTableOrView » dans cette rubrique.  
  
 **InclusionColumns**  
 Sélectionnez les colonnes ou les ensembles de colonnes à partir des tables côté sous-ensemble et sur-ensemble.  
  
 Pour plus d'informations, consultez les sections « Fonctionnement de la sélection des colonnes pour la propriété InclusionColumns » et « Options KeyColumns » dans cette rubrique.  
  
#### <a name="subsettableorview-and-supersettableorview-options"></a>Options SubsetTableOrView et SupersetTableOrView  
 **Schéma**  
 Spécifie le schéma auquel la table sélectionnée appartient. Cette option est en lecture seule.  
  
 **TableOrView**  
 Affiche le nom de la table sélectionnée. Cette option est en lecture seule.  
  
#### <a name="inclusioncolumns-options"></a>Options InclusionColumns  
 Les options suivantes sont proposées pour chaque ensemble de colonnes sélectionné à des fins de profilage dans **InclusionColumns**.  
  
 Pour plus d'informations, consultez la section « Fonctionnement de la sélection des colonnes pour la propriété InclusionColumns », plus haut dans cette rubrique.  
  
 **IsWildcard**  
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
  
### <a name="options"></a>Options  
 **InclusionThresholdSetting**  
 Sélectionnez le paramètre de seuil pour améliorer la sortie du profil. La valeur par défaut de cette propriété est **Specified**. Pour plus d'informations, consultez la section « Fonctionnement des paramètres de seuil », plus haut dans cette rubrique.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Aucun seuil n'est spécifié. La puissance de la clé est signalée, quelle que soit sa valeur.|  
|**Specified**|Utilisez le seuil spécifié dans **InclusionStrengthThreshold**. La puissance d'inclusion est précisée uniquement si elle est supérieure au seuil.|  
|**Exact**|Aucun seuil n'est spécifié. La puissance d'inclusion est signalée uniquement si les valeurs de sous-ensemble sont incluses entièrement dans les valeurs de sur-ensemble.|  
  
 **InclusionStrengthThreshold**  
 Spécifiez le seuil (au moyen d'une valeur comprise entre 0 et 1) au-dessus duquel la puissance d'inclusion doit être précisée. La valeur par défaut de cette propriété est 0,95. Cette option est activée uniquement quand **Specified** est sélectionné comme valeur **InclusionThresholdSetting**.  
  
 Pour plus d'informations, consultez la section « Fonctionnement des paramètres de seuil », plus haut dans cette rubrique.  
  
 **SupersetColumnsKeyThresholdSetting**  
 Spécifiez le seuil de sur-ensemble. La valeur par défaut de cette propriété est **Specified**. Pour plus d'informations, consultez la section « Fonctionnement des paramètres de seuil », plus haut dans cette rubrique.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Aucun seuil n'est spécifié. La puissance d'inclusion est signalée, quelle que soit la puissance de clé de la colonne du sur-ensemble.|  
|**Specified**|Utilisez le seuil spécifié dans **SupersetColumnsKeyThreshold**. La puissance d'inclusion est signalée uniquement si la puissance de clé de la colonne du sur-ensemble est supérieure au seuil.|  
|**Exact**|Aucun seuil n'est spécifié. La puissance d'inclusion est signalée uniquement si les colonnes du sur-ensemble sont une clé exacte dans la table côté sur-ensemble.|  
  
 **SupersetColumnsKeyThreshold**  
 Spécifiez le seuil (au moyen d'une valeur comprise entre 0 et 1) au-dessus duquel la puissance d'inclusion doit être précisée. La valeur par défaut de cette propriété est 0,95. Cette option est activée uniquement quand vous sélectionnez **Specified** en tant que valeur **SupersetColumnsKeyThresholdSetting**.  
  
 Pour plus d'informations, consultez la section « Fonctionnement des paramètres de seuil », plus haut dans cette rubrique.  
  
 **MaxNumberOfViolations**  
 Spécifiez le nombre maximal de violations d'inclusion à signaler dans la sortie. La valeur par défaut de cette propriété est 100. Cette option est désactivée quand vous sélectionnez **Exact** comme valeur **InclusionThresholdSetting**.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
