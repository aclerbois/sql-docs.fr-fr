---
title: Éditeur de tâche de profilage de données (page Demandes de profil) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.profilerequests.f1
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: c72acb3d-380e-436e-8041-ed364eddfabd
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1d11da7d0a313757de017fdb5c765845650987cf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-profiling-task-editor-profile-requests-page"></a>Éditeur de tâche de profilage de données (page Demandes de profil)
  Utilisez la page **Demandes de profil** de **l’Éditeur de tâche de profilage de données** pour sélectionner et configurer les profils que vous souhaitez calculer. Dans une même tâche de profilage des données, vous pouvez calculer plusieurs profils pour plusieurs colonnes ou des combinaisons de colonnes dans plusieurs tables ou vues.  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
 **Pour ouvrir la page Demandes de profil de la tâche de profilage de données**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] doté de la tâche de profilage de données.  
  
2.  Sous l’onglet **Flux de contrôle** , double-cliquez sur la tâche de profilage des données.  
  
3.  Dans **l’Éditeur de tâche de profilage de données**, cliquez sur **Demandes de profil**.  
  
## <a name="using-the-requests-pane"></a>Utilisation du volet Demandes  
 Le volet Demandes apparaît en haut de la page. Il répertorie tous les profils configurés pour la tâche de profilage de données actuelle. Si aucun profil n'a été configuré, le volet Demandes est vide. Pour ajouter un nouveau profil, cliquez dans une zone vide sous la colonne **Type de profil** , puis sélectionnez un type de profil dans la liste. Pour configurer un profil, sélectionnez-le dans le volet Demandes, puis définissez ses propriétés dans le volet **Propriétés de la demande** .  
  
### <a name="requests-pane-options"></a>Options du volet Demandes  
 Le volet Demandes propose les options suivantes :  
  
 **Afficher**  
 Précisez si vous souhaitez afficher l'ensemble des profils configurés pour la tâche ou simplement l'un d'entre eux.  
  
 Les colonnes dans le volet Demandes changent selon la **vue** que vous sélectionnez. Pour plus d'informations sur chacune de ces colonnes, consultez la section suivante « Colonnes du volet Demandes ».  
  
### <a name="requests-pane-columns"></a>Colonnes du volet Demandes  
 Les colonnes qui s'affichent dans le volet Demandes dépendent de la **vue** que vous avez sélectionnée :  
  
-   Si vous choisissez d'afficher **Toutes les demandes**, le volet Demandes dévoile deux colonnes : **Type de profil** et **ID de demande**.  
  
-   Si vous choisissez d'afficher l'un des cinq profils de colonne, le volet Demandes fait apparaître quatre colonnes : **Type de profil**, **Table ou vue**, **Colonne**et **ID de demande**.  
  
-   Si vous optez pour l’affichage d’un profil de clé candidate, le volet Demandes comprend quatre colonnes : **Type de profil**, **Table ou vue**, **Colonnes clés**et **ID de demande**.  
  
-   Si vous souhaitez afficher un profil de dépendance fonctionnelle, le volet Demandes propose cinq colonnes : **Type de profil**, **Table ou vue**, **Colonnes déterminantes**, **Colonne dépendante**et **ID de demande**.  
  
-   Dans le cas d'un profil d'inclusion de valeur, le volet Demandes affiche six colonnes : **Type de profil**, **Table ou vue côté sous-ensemble**, **Table ou vue côté sur-ensemble**, **Colonnes côté sous-ensemble**, **Colonnes côté sur-ensemble**et **ID de demande**.  
  
 Les sections suivantes décrivent chacune de ces colonnes.  
  
#### <a name="columns-common-to-all-views"></a>Colonnes communes à toutes les vues  
 **Type de profil**  
 Sélectionnez un profil des données à partir des options suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Demande de profil de clé candidate**|Calculez un profil de clé candidate.<br /><br /> Ce profil signale si une colonne ou un ensemble de colonnes est une clé, ou une clé approximative, pour la table sélectionnée. Ce profil peut également vous aider à identifier des problèmes dans vos données, tels que des valeurs dupliquées dans une colonne clé potentielle.|  
|**Demande de profil de distribution de longueurs de colonne**|Calculez un profil de distribution de longueurs de colonne.<br /><br /> Le profil de distribution de longueurs de colonne signale toutes les longueurs distinctes des valeurs de chaîne dans la colonne sélectionnée, ainsi que le pourcentage de lignes dans la table que chaque longueur représente. Ce profil peut vous aider à identifier des problèmes dans vos données, tels que des valeurs non valides. Par exemple, vous profilez une colonne de codes d'états des États-Unis à deux caractères et découvrez des valeurs excédant deux caractères.|  
|**Demande de profil de ratio de colonne Null**|Calculez un profil de ratio de colonne Null.<br /><br /> Le profil de ratio de colonne Null signale le pourcentage de valeurs Null dans la colonne sélectionnée. Ce profil peut vous aider à identifier des problèmes dans vos données, tels qu'un ratio élevé inattendu de valeurs Null dans une colonne. Par exemple, vous profilez une colonne de codes postaux et découvrez un pourcentage élevé et inacceptable de codes manquants.|  
|**Demande de profil de modèle de colonne**|Calculez un profil de modèle de colonne.<br /><br /> Le profil de modèle de colonne signale un ensemble d'expressions régulières qui reflètent le pourcentage spécifié pour les valeurs dans une colonne de chaîne. Ce profil peut vous aider à identifier des problèmes dans vos données, tels que les chaînes non valides. Il peut également suggérer des expressions régulières susceptibles d'être utilisées à l'avenir pour la validation de nouvelles valeurs. Par exemple, le profil de modèle d'une colonne de codes postaux peut générer les expressions régulières suivantes : \d{5}-\d{4}, \d{5} et \d{9}. Si vous rencontrez d'autres expressions régulières, il est probable que vos données contiennent des valeurs qui ne sont pas valides ou utilisent un format incorrect.|  
|**Demande de profil de statistiques de colonnes**|Sélectionnez cette option pour calculer un profil de statistiques de colonnes à l'aide des paramètres par défaut pour toutes les colonnes applicables dans la table ou la vue sélectionnée.<br /><br /> Le profil de statistiques de colonnes répertorie des statistiques, telles que l’écart minimal, maximal, moyen et type pour les colonnes numériques et l’écart minimal et maximal pour les colonnes **datetime** . Ce profil peut vous aider à identifier des problèmes dans vos données, tels que les dates non valides. Par exemple, vous profilez une colonne de dates historiques et découvrez une date maximum dont l'échéance est à venir.|  
|**Demande de profil de distribution de valeurs de colonne**|Calculez un profil de distribution de valeurs de colonne.<br /><br /> Le profil de distribution de valeurs de colonne permet de préciser toutes les valeurs distinctes dans la colonne sélectionnée, ainsi que le pourcentage de lignes dans la table que représente chaque valeur. Ce profil peut également signaler des valeurs qui représentent beaucoup plus qu'un pourcentage spécifié dans la table. Il peut vous aider à identifier des problèmes dans vos données, tels qu'un nombre incorrect de valeurs distinctes dans une colonne. Par exemple, vous profilez une colonne qui contient les états des États-Unis et découvrez plus de 50 valeurs distinctes.|  
|**Demande de profil de dépendance fonctionnelle**|Calculez un profil de dépendance fonctionnelle.<br /><br /> Le profil de dépendance fonctionnelle indique le degré de dépendance entre les valeurs d'une colonne (colonne dépendante) et celles d'une autre colonne ou d'un ensemble de colonnes (colonne déterminante). Ce profil peut également vous aider à identifier des problèmes dans vos données, tels que des valeurs non valides. Par exemple, vous profilez une dépendance entre une colonne États-Unis/Code postal et une colonne des états des États-Unis. Le même code postal doit toujours afficher le même état mais le profil détecte des violations de la dépendance.|  
|**Demande de profil d'inclusion de valeur**|Calculez un profil d'inclusion de valeur.<br /><br /> Le profil d'inclusion de valeur calcule le chevauchement des valeurs entre deux colonnes ou des ensembles de colonnes. Ce profil permet également de déterminer si une colonne ou un ensemble de colonnes peut servir de clé étrangère entre les tables sélectionnées. Ce profil peut également vous aider à identifier des problèmes dans vos données, tels que les valeurs non valides. Par exemple, vous profilez la colonne ProductID d'une table Sales et découvrez que la colonne contient des valeurs qui sont introuvables dans la colonne ProductID de la table Products.|  
  
 **RequestID**  
 Affiche l'identificateur de la demande. En règle générale, il n'est pas nécessaire de modifier la valeur générée automatiquement.  
  
#### <a name="columns-common-to-all-individual-profiles"></a>Colonnes communes à tous les profils individuels  
 **Gestionnaire de connexions**  
 Affiche le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] qui se connecte à la base de données source.  
  
 **ID de demande**  
 Affiche un identificateur pour la demande. En règle générale, il n'est pas nécessaire de modifier la valeur générée automatiquement.  
  
#### <a name="columns-common-to-the-five-individual-column-profiles"></a>Colonnes communes aux cinq profils de colonne individuels  
 **Table ou vue**  
 Affiche la table ou la vue qui contient la colonne sélectionnée.  
  
 **Colonne**  
 Affiche la colonne sélectionnée pour le profilage.  
  
#### <a name="columns-specific-to-the-candidate-key-profile"></a>Colonnes spécifiques au profil de clé candidate  
 **Table ou vue**  
 Affiche la table ou la vue qui contient les colonnes sélectionnées.  
  
 **Colonnes clés**  
 Affiche les colonnes sélectionnées pour le profilage.  
  
#### <a name="columns-specific-to-the-functional-dependency-profile"></a>Colonnes spécifiques au profil de dépendance fonctionnelle  
 **Table ou vue**  
 Affiche la table ou la vue qui contient les colonnes sélectionnées.  
  
 **Colonnes déterminantes**  
 Affiche les colonnes sélectionnées pour le profilage en tant que colonne ou colonnes déterminantes. Dans l'exemple où une colonne États-Unis/Code postal détermine l'état aux États-Unis, la colonne déterminante est la colonne de codes postaux.  
  
 **Dependent column**  
 Affiche les colonnes sélectionnées pour le profilage en tant que colonne dépendante. Dans l'exemple où une colonne États-Unis/Code postal détermine l'état aux États-Unis, la colonne dépendante est celle qui désigne l'état.  
  
#### <a name="columns-specific-to-the-value-inclusion-profile"></a>Colonnes spécifiques au profil d'inclusion de valeur  
 **Table ou vue côté sous-ensemble**  
 Affiche la table ou la vue qui contient la ou les colonnes sélectionnées en tant que colonnes côté sous-ensemble.  
  
 **Table ou vue côté sur-ensemble**  
 Affiche la table ou la vue qui contient la ou les colonnes sélectionnées en tant que colonnes côté sur-ensemble.  
  
 **Colonnes côté sous-ensemble**  
 Affiche la ou les colonnes sélectionnées pour le profilage en tant que colonnes côté sous-ensemble. Dans l'exemple où vous souhaitez vérifier que les valeurs dans une colonne des états américains apparaissent dans une table de référence de codes d'états américains à deux caractères, la colonne de sous-ensemble correspond à la colonne des états dans la table source.  
  
 **Colonnes côté sur-ensemble**  
 Affiche la ou les colonnes sélectionnées pour le profilage en tant que colonnes côté sur-ensemble. Dans l'exemple où vous souhaitez vérifier que les valeurs dans une colonne des états américains apparaissent dans une table de référence de codes d'états américains à deux caractères, la colonne de sur-ensemble correspond à la colonne des codes d'états dans la table de référence.  
  
## <a name="using-the-request-properties-pane"></a>Utilisation du volet Propriétés de la demande  
 Le volet **Propriétés de la demande** apparaît sous le volet Demandes. Ce volet affiche les options qui concernent le profil que vous avez sélectionné dans le volet Demandes.  
  
> [!NOTE]  
>  Après avoir choisi un **Type de profil**, vous devez sélectionner le champ **ID de demande** pour consulter les propriétés de la demande de profil dans le volet **Propriétés de la demande** .  
  
 Ces options varient selon le profil sélectionné. Pour plus d'informations sur les types de profil individuels, consultez les rubriques suivantes :  
  
-   [Options Demande de profil de clé candidate &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/candidate-key-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil de ratio de colonne Null &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-null-ratio-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil de statistiques de colonnes &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-statistics-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil de distribution de valeurs de colonne &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-value-distribution-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil de distribution de longueurs de colonne &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-length-distribution-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil de modèle de colonne &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-pattern-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil de dépendance fonctionnelle &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/functional-dependency-profile-request-options-data-profiling-task.md)  
  
-   [Options Demande de profil d’inclusion de valeur &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/value-inclusion-profile-request-options-data-profiling-task.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
