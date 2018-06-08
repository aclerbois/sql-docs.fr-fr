---
title: Options Demande de profil de modèle de colonne (tâche de profilage des données) | Microsoft Docs
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
ms.assetid: 9ccb8fc5-f65e-41a2-9511-7fa55586eb8b
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 97ebf10bb9ae1a30a7f66ba5b984e0c08ee5ac24
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-pattern-profile-request-options-data-profiling-task"></a>Options Demande de profil de modèle de colonne (tâche de profilage des données)
  Utilisez le volet **Propriétés de la demande** de la page **Demandes de profil** pour définir les options de la **Demande de profil de modèle de colonne** sélectionnée dans le volet Demandes. Un profil de modèle de colonne signale un ensemble d'expressions régulières qui reflètent le pourcentage spécifié des valeurs dans une colonne de chaîne. Ce profil peut vous aider à identifier des problèmes dans vos données, tels que les chaînes non valides, et peut suggérer des expressions régulières susceptibles d'être utilisées à l'avenir pour la validation de nouvelles valeurs. Par exemple, le profil de modèle d'une colonne États-Unis/Codes postaux peut générer les expressions régulières \d{5}-\d{4}, \d{5} et \d{9}. Si vous rencontrez d'autres expressions régulières, il est probable que vos données contiennent des valeurs qui ne sont pas valides ou utilisent un format incorrect.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la Visionneuse du profil des données pour analyser le résultat de la tâche de profilage de données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="understanding-the-use-of-delimiters-and-symbols"></a>Fonctionnement de l'utilisation des séparateurs et des symboles  
 Avant de calculer les modèles d’une **Demande de profil de modèle de colonne**, la tâche de profilage des données marque les données sous forme de jetons. Autrement dit, elle sépare les valeurs de chaîne en unités plus petites appelées « jetons ». Pour séparer les chaînes en jetons, la tâche se base sur les séparateurs et les symboles que vous spécifiez pour les propriétés **Séparateurs** et **Symboles** :  
  
-   **Séparateurs** Par défaut, la liste des séparateurs contient les caractères suivants : espace, tabulation horizontale (\t), nouvelle ligne (\n) et retour chariot (\r). Vous pouvez définir d'autres séparateurs mais vous ne pouvez pas supprimer les séparateurs par défaut.  
  
-   **Symboles** Par défaut, la liste des **symboles** contient les caractères suivants : `,.;:-"'~=&/@!?()<>[]{}|#*^%`, ainsi que la coche. Par exemple, si les symboles sont "`()-`", la valeur "(425) 123-4567" est marquée sous forme de jeton de la manière suivante : ["(", "425", ")", "123", "-", "4567", ")"].  
  
 Un caractère ne peut pas être à la fois un séparateur et un symbole.  
  
 Tous les séparateurs sont normalisés en un espace unique dans le cadre du processus de création de jetons tandis que les symboles sont conservés.  
  
## <a name="understanding-the-use-of-the-tag-table"></a>Fonctionnement de l'utilisation de la table des balises  
 Vous pouvez éventuellement regrouper des jetons associés au moyen d’une balise unique en stockant les balises et les termes associés dans une table spéciale que vous créez dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La table des balises doit être composée de deux colonnes de chaîne, l’une appelée « Balise », l’autre « Terme ». Ces colonnes peuvent être de type **char**, **nchar**, **varchar**, ou **nvarchar**, mais pas **text** ou **ntext**. Vous pouvez fusionner plusieurs balises et leurs termes correspondants dans une seule et unique table. Une demande de profil de modèle de colonne peut utiliser une seule table des balises. Vous pouvez recourir à un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] pour vous connecter à la table des balises. La table des balises peut donc être stockée dans une autre base de données ou sur un autre serveur que les données sources.  
  
 Par exemple, vous pouvez regrouper les valeurs « East », « West », « North » et « South » susceptibles d'apparaître dans des adresses postales en utilisant la balise unique « Direction ». Un exemple de cette table des balises est proposé ci-dessous.  
  
|Balise|Terme|  
|---------|----------|  
|Sens|Est|  
|Sens|Ouest|  
|Sens|Nord|  
|Sens|Sud|  
  
 Vous pouvez éventuellement utiliser une autre balise pour regrouper les différents mots qui expriment la notion de « rue » (Street) dans les adresses postales :  
  
|Balise|Terme|  
|---------|----------|  
|Street|Street|  
|Street|Avenue|  
|Street|Place|  
|Street|Way|  
  
 D'après cette combinaison de balises, le modèle obtenu pour une adresse postale peut se présenter de la manière suivante :  
  
 `\d+\ LookupTag=Direction \d+\p{L}+\ LookupTag=Street`  
  
> [!NOTE]  
>  L'utilisation d'une table des balises diminue les performances de la tâche de profilage des données. N'utilisez pas plus de 10 balises ou plus de 100 termes par balise.  
  
 Le même terme peut appartenir à plusieurs balises.  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **demande de profil de modèle de colonne**, le volet **Propriétés de la demande** affiche les groupes d’options suivants :  
  
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
 Cette option ne s'applique pas au profil de modèle de colonne.  
  
### <a name="general-options"></a>Options générales  
 **RequestID**  
 Tapez un nom descriptif pour identifier cette demande de profil. En règle générale, il n'est pas nécessaire de modifier la valeur générée automatiquement.  
  
### <a name="options"></a>Options  
 **MaxNumberOfPatterns**  
 Spécifiez le nombre maximal de modèles que vous souhaitez calculer à l'aide du profil. La valeur par défaut de cette option est 10. La valeur maximale est 100.  
  
 **PercentageDataCoverageDesired**  
 Spécifiez le pourcentage des données que vous souhaitez refléter avec les modèles calculés. La valeur par défaut de cette option est 95 (pourcent).  
  
 **CaseSensitive**  
 Indiquez si les modèles doivent respecter la casse. La valeur par défaut de cette option est **False**.  
  
 **Séparateurs**  
 Répertoriez les caractères à traiter en tant qu'équivalents des espaces entre les mots lorsque vous marquez du texte sous forme de jetons. Par défaut, la liste des **séparateurs** contient les caractères suivants : espace, tabulation horizontale (\t), nouvelle ligne (\n) et retour chariot (\r). Vous pouvez définir d'autres séparateurs mais vous ne pouvez pas supprimer les séparateurs par défaut.  
  
 Pour plus d'informations, consultez la section « Fonctionnement de l'utilisation des séparateurs et des symboles » plus haut dans cette rubrique.  
  
 **Symboles**  
 Répertoriez les symboles à conserver dans le cadre des modèles. Les exemples peuvent inclure « / » pour les dates, « : » pour les heures et « @ » pour les adresses de messagerie. Par défaut, la liste des **symboles** contient les caractères suivants : `,.;:-"'~=&/@!?()<>[]{}|#*^%`.  
  
 Pour plus d'informations, consultez la section « Fonctionnement de l'utilisation des séparateurs et des symboles » plus haut dans cette rubrique.  
  
 **TagTableConnectionManager**  
 Sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) pour établir la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table des balises.  
  
 Pour plus d'informations, consultez la section « Fonctionnement de l'utilisation de la table des balises » plus haut dans cette rubrique.  
  
 **TagTableName**  
 Sélectionnez la table des balises existante qui doit être composée de deux colonnes de chaîne intitulées Balise et Terme.  
  
 Pour plus d'informations, consultez la section « Fonctionnement de l'utilisation de la table des balises » plus haut dans cette rubrique.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
