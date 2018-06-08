---
title: Options Demande de profil de distribution de longueurs de colonne (tâche de profilage des données) | Microsoft Docs
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
ms.assetid: 1a4de41f-f38d-40ea-ba1b-6c0ef67ea52a
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d03b489b0d5b94fe275df2abf6aa4c06a4098753
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-length-distribution-profile-request-options-data-profiling-task"></a>Options Demande de profil de distribution de longueurs de colonne (tâche de profilage des données)
  Utilisez le volet **Propriétés de demandes** de la page **Demandes de profil** pour définir les options de la **Demande de profil de distribution de longueurs de colonne** sélectionnée dans le volet Demandes. Un profil de distribution de longueurs de colonne signale toutes les longueurs distinctes des valeurs de chaîne dans la colonne sélectionnée, et le pourcentage de lignes dans la table que chaque longueur représente. Ce profil peut vous aider à identifier les problèmes dans vos données, tels que les valeurs non valides. Par exemple, vous profilez une colonne de codes d'états des États-Unis à deux caractères et découvrez des valeurs excédant deux caractères.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **Demande de profil de distribution de longueurs de colonne**, le volet **Propriétés de la demande** affiche les groupes d’options suivants :  
  
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
 Spécifiez le schéma auquel la table sélectionnée appartient. Cette option est en lecture seule.  
  
 **Table**  
 Affiche le nom de la table sélectionnée. Cette option est en lecture seule.  
  
#### <a name="column-options"></a>Options de colonne  
 **IsWildCard**  
 Indique si le caractère générique **(\*)** a été sélectionné. Cette option a la valeur **True** si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Sa valeur est **False** si vous avez sélectionné une colonne spécifique dont le profil doit être généré. Cette option est en lecture seule.  
  
 **ColumnName**  
 Affiche le nom de la colonne sélectionnée. Cette option est vide si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Cette option est en lecture seule.  
  
 **StringCompareOptions**  
 Cette option ne s'applique pas au profil de distribution de longueurs de colonne.  
  
### <a name="general-options"></a>Options générales  
 **RequestID**  
 Tapez un nom descriptif pour identifier cette demande de profil. En règle générale, il n'est pas nécessaire de modifier la valeur générée automatiquement.  
  
### <a name="options"></a>Options  
 **IgnoreLeadingSpaces**  
 Indiquez si les espaces de début doivent être ignorés lorsque le profil compare des valeurs de chaîne. La valeur par défaut de cette option est **False**.  
  
 **IgnoreTrailingSpaces**  
 Indiquez si les espaces de fin doivent être ignorés lorsque le profil compare des valeurs de chaîne. La valeur par défaut de cette option est **True**.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
