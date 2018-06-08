---
title: Accéder rapidement aux analyses et tâches courantes dans les opérations de SQL Studio (version préliminaire) | Documents Microsoft
description: En savoir plus sur l’affichage des widgets intéressante dans Studio des opérations SQL (version préliminaire).
ms.custom: tools|sos
ms.date: 11/15/2017
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: yualan
ms.author: alayu
manager: craigg
ms.openlocfilehash: 8ed47935a863a14d68385c540ce68d0e75e99799
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="dashboards-in-includename-sosincludesname-sos-shortmd"></a>Tableaux de bord dans [!INCLUDE[name-sos](../includes/name-sos-short.md)]

Pour afficher un tableau de bord, avec le bouton d’un serveur ou base de données, puis sélectionnez **gérer**.

![Exemple de tableau de bord](media/dashboards/sample-dashboard.png)

**Propriétés du serveur** contient les propriétés du serveur, y compris les versions, Édition, nom de l’ordinateur et du système d’exploitation.

**Tâches** contient commons des tâches telles que la restauration et la nouvelle requête.

**Bases de données de recherche** rechercher facilement des bases de données existantes stockées sur le serveur, y compris les tables.

**État de la sauvegarde** facilement rechercher l’état de sauvegarde pour les bases de données existantes.

## <a name="configuring-insight-widgets"></a>Configuration des Widgets d’un aperçu
Il est vivement recommandé de suivre le didacticiel pour configurer votre tableau de bord, vous pouvez trouver [ici](tutorial-build-custom-insight-sql-server.md).

En outre, veillez à consulter le [procédures sur la configuration des widgets d’insight]().

Une fois suivant ce didacticiel, lisez la suite pour plus d’informations sur les widgets spécifiques qui ne sont pas traitées dans ce didacticiel.

## <a name="insight-detail"></a>Détails de l’analyse
La fenêtre Détails de Insight mobile fournit des informations détaillées pour un widget insight connexes. 
- Un widget Insight restitue un affichage de résumé en un coup de œil avec un nombre, ligne, etc. de graphique. 
- La fenêtre Détails de Insight mobile fournit des détails « descendre dans la hiérarchie » répertoriant des informations plus détaillées données pour chaque élément figurant dans le widget un aperçu de haut niveau. 
  - Le contenu du menu volant des détails est défini avec une requête SQL distincte à la requête de principal du widget. 

Il est inutile de l’ensemble pour une requête de détails de l’analyse, mais la disposition est standard.
- La moitié supérieure de la vue est toujours une vue « résumée » 2 colonnes. Les colonnes à utiliser sont définis par les propriétés « étiquette » et « valeur » de la configuration de JSON
- En cliquant sur une ligne dans la table de résumé, le bas la moitié de la fenêtre mobile répertorie l’ensemble des informations sur les colonnes pour cette ligne.

### <a name="insight-detail-configuration-in-packagejson"></a>Configuration de détail Insight dans package.json

Exemple de configuration de menu volant des détails de l’analyse
```json
"details": {
    "queryFile": "./relative_path_to_sqlfile_from_package_json_file.sql",
    "label": {
        "icon": "database",
        "column": "first_column_name_for_summary_list_view",
        "state": [
            {
                "condition": {
                    "if": "equals",
                    "equals": "0"
                },
                "color": "red"
            },
            {
                "condition": {
                    "if": "equals",
                    "equals": "1"
                },
                "color": "orange"
            },
            {
                "condition": {
                    "if": "equals",
                    "equals": "2"
                },
                "color": "green"
            }
        ]
    },
    "value": "second_column_and_condition_check_value_column_for_summary_list_view",
```
|propriété|Type|valeur|Valeur par défaut|description|comment|
|:---|:---|:---|:---|:---|:---|
|détails|objet JSON|||propriété obligatoire à définir des définitions de détail insight dans leur structure||
|queryFile|chaîne|||le chemin d’accès du fichier de requête sql insight détail et le nom relatif à l’emplacement du package.json||
|Étiquette|objet JSON|||propriété obligatoire pour définir chaque élément de ligne dans la vue liste récapitulative|dans les futures le nom de cette propriété à modifier, comme 'summaryList'|
|Icône|chaîne|||indiquer le nom de l’icône à urgents pour chaque élément de la vue liste récapitulative.|liste (tbd) des icônes pris en charge est documentée|
|column|chaîne|||indiquer le nom de la première colonne dans la vue liste de résumé à partir du jeu de résultats|dans les futures le nom de cette propriété est remplacé par nom plus intuitive.|
|valeur|chaîne|||indiquer le nom de la deuxième colonne de la vue liste de résumé à partir du jeu de résultats. La valeur de cette colonne est utilisée pour vérifier les conditions et définir des couleurs pour le point de couleur de chaque liste récapitulative afficher les éléments|dans les futures le nom de cette propriété change pour un élément plus intuitive|
|condition|objet JSON|||définit la vérification de la condition pour la valeur de la colonne et de déterminer la couleur pour chaque élément de la vue liste récapitulative||
|if|chaîne|toujours, equals, notEquals, greaterThan, lessThan, greaterThanOrEqauls, inférieur ou égal à||opérateur de vérification de condition|dans les futures le nom de propriété change à (opérateur)|
|equals|chaîne|||valeur de vérification de la condition|dans les futures ce nom de propriété passe à 'value'|

## <a name="insight-actions"></a>Actions d’analyse
Un widget d’un aperçu et les détails de l’analyse, vous pouvez facilement s’accompagne d’un plan d’action pour atténuer un problème ou gérer. Par exemple, vous serez considérer l’exécution de DBCC CHECKDB, les journaux d’erreurs de lecture ou restaurer la base de données lors de la base de données est en attente de récupération. Ou bien, il peut être une des actions que vous souhaitez effectuer.

À l’aide de [!INCLUDE[name-sos](../includes/name-sos-short.md)]de configuration des Actions d’analyse, vous pouvez mapper un actions intégrées, telles que les restaurer ou mettre votre propre action définie avec un script sql.

> Configuration des actions personnalisées à l’aide du script sql est en cours de développement, et il n’est pas disponible dans la génération de la version préliminaire privée encore.

## <a name="sample-insight-action-definition"></a>Exemple de définition d’Action d’analyse

```"actions"{}``` définit une action d’analyse. Action peut être définie sur une étendue spécifique tel que ```"server"```, ```"database"``` et ainsi de suite et [!INCLUDE[name-sos](../includes/name-sos-short.md)] transmet les informations de contexte de connexion actuel à l’action. 

Par exemple, action de restauration au lancement de la base de données WideWorldImporters, ```"database": "${Database}"``` définition indique pour passer ```Database``` valeur de colonne dans les résultats de votre requête pour l’action de restauration. Action de restauration démarre ensuite pour la base de données. ```"types"``` est un tableau json et plusieurs actions peuvent être répertoriées dans le tableau. Il est en fait un menu contextuel dans la boîte de dialogue Détails de l’analyse que l’utilisateur pouvez cliquer sur et effectuer l’action. 

> [!INCLUDE[name-sos](../includes/name-sos-short.md)] Aperçu 0.17.1 a activé « backup », « restore », « nouvelle requête » et « nouvelle database » en tant que types d’action.

```json
"details": {
    "queryFile": "./sql/database_state_detail.sql",
    "label": {...},
    "value": "state",
    "actions": {
        "database": "${Database}",
        "types": ["restore"]
    }
}
```
