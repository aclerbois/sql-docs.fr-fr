---
title: Options Demande de profil de distribution de valeurs de colonne (tâche de profilage des données) | Microsoft Docs
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
ms.assetid: c1e5f5de-04f5-4d00-a9f0-55817186bdf9
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d0fb3daaeaac3afc27abfacee4c9f596baaa01e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-value-distribution-profile-request-options-data-profiling-task"></a>Options Demande de profil de distribution de valeurs de colonne (tâche de profilage des données)
  Utilisez le volet **Propriétés de la demande** de la page **Demandes de profil** pour définir les options de la **Demande de profil de distribution de valeurs de colonne** sélectionnée dans le volet Demandes. Un profil de distribution de valeurs de colonne permet de préciser toutes les valeurs distinctes dans la colonne sélectionnée, ainsi que le pourcentage de lignes dans la table que représente chaque valeur. Le profil peut également signaler des valeurs qui représentent beaucoup plus qu'un pourcentage de lignes spécifié dans la table. Il peut vous aider à identifier des problèmes dans vos données, tels qu'un nombre incorrect de valeurs distinctes dans une colonne. Par exemple, vous établissez le profil d'une colonne des états des États-Unis et découvrez plus de 50 valeurs distinctes.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **Demande de profil de distribution de valeurs de colonne**, le volet **Propriétés de la demande** affiche les groupes d’options suivants :  
  
-   **Données**, qui incluent les options **TableOrView** et **Column**  
  
-   **Général**  
  
-   **Options**  
  
### <a name="data-options"></a>Options de données  
 **ConnectionManager**  
 Sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) pour établir la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table ou la vue à profiler.  
  
 **TableOrView**  
 Sélectionnez la table ou la vue existante qui contient la colonne à profiler.  
  
 Pour plus d'informations, consultez la section « Options TableorView » dans cette rubrique.  
  
 **Column**  
 Sélectionnez la colonne existante à profiler. Sélectionnez **(\*)** pour profiler toutes les colonnes.  
  
 Pour plus d'informations, consultez la section « Options de colonne » dans cette rubrique.  
  
#### <a name="tableorview-options"></a>Options TableOrView  
 **Schéma**  
 Spécifie le schéma auquel la table sélectionnée appartient. Cette option est en lecture seule.  
  
 **Table**  
 Affiche le nom de la table sélectionnée. Cette option est en lecture seule.  
  
#### <a name="column-options"></a>Options de colonne  
 **IsWildCard**  
 Indique si le caractère générique **(\*)** a été sélectionné. Cette option a la valeur **True** si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Sa valeur est **False** si vous avez sélectionné une colonne spécifique dont le profil doit être généré. Cette option est en lecture seule.  
  
 **ColumnName**  
 Affiche le nom de la colonne sélectionnée. Cette option est vide si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Cette option est en lecture seule.  
  
 **StringCompareOptions**  
 Sélectionnez les options de comparaison des valeurs de chaîne. Cette propriété dispose des options répertoriées dans le tableau suivant. La valeur par défaut de cette option est **Default**.  
  
> [!NOTE]  
>  Si vous utilisez le caractère générique **(\*)** pour **ColumnName**, **CompareOptions** est en lecture seule et est définie avec le paramètre **Default**.  
  
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
 **ValueDistributionOption**  
 Spécifiez si la distribution est à calculer pour toutes les valeurs de colonne. La valeur par défaut de cette option est **FrequentValues**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**AllValues**|La distribution est calculée pour toutes les valeurs de colonne.|  
|**FrequentValues**|La distribution est calculée uniquement pour les valeurs dont la fréquence dépasse la valeur minimale spécifiée dans **FrequentValueThreshold**. Les valeurs qui ne correspondent pas à **FrequentValueThreshold** sont exclues du rapport de sortie.|  
  
 **FrequentValueThreshold**  
 Spécifiez le seuil (au moyen d'une valeur comprise entre 0 et 1) au-dessus duquel la valeur de colonne doit être précisée. Cette option est désactivée quand vous sélectionnez **AllValues** comme **ValueDistributionOption**. La valeur par défaut de cette option est 0,001.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
