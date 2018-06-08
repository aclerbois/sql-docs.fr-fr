---
title: Règles pour l’entrée de valeurs de recherche (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- time [SQL Server], searches
- date searches
- dates [SQL Server], searches
- embedding apostrophes [SQL Server]
- logical value searches [SQL Server]
- case-sensitive search matches
- search criteria [SQL Server], rules
- text value searches [SQL Server]
- numeric value searches [SQL Server]
ms.assetid: 3c8134b7-83f4-41b4-99c8-e3949a685ff5
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2308add086753aa3212a5a1860e144b96c9f6ae5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rules-for-entering-search-values-visual-database-tools"></a>Règles pour l'entrée de valeurs de recherche (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Cette rubrique décrit les conventions à utiliser lorsque vous entrez les types de valeurs littérales suivants pour une condition de recherche :  
  
-   Valeurs texte  
  
-   Valeurs numériques  
  
-   Dates  
  
-   Valeurs logiques  
  
> [!NOTE]  
> Les informations contenues dans cette rubrique sont extraites des règles relatives au langage SQL-92 standard. L'implémentation du langage SQL peut cependant être personnalisée dans chaque base de données. Les indications proposées ci-dessous ne sont donc pas forcément applicables dans tous les cas. Si vous avez des questions sur l'entrée des valeurs de recherche pour une base de données particulière, consultez la documentation de la base de données utilisée.  
  
## <a name="searching-on-text-values"></a>Recherche de valeurs texte  
Les indications suivantes s'appliquent lorsque vous entrez des valeurs texte dans les conditions de recherche :  
  
-   **Guillemets** Placez les valeurs texte entre des guillemets simples, à l’instar du nom dans l’exemple suivant :  
  
    ```  
    'Smith'  
    ```  
  
    Si vous entrez une condition de recherche dans le [volet Critères](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md), il vous suffit de taper la valeur texte : le Concepteur de requêtes et de vues insère automatiquement des guillemets simples devant et derrière.  
  
    > [!NOTE]  
    > Dans certaines bases de données, les termes placés entre guillemets simples sont interprétés comme des valeurs littérales et les termes placés entre guillemets doubles comme des objets de base de données, par exemple, des références de colonne ou de table. Par conséquent, même si le Concepteur de requêtes et de vues accepte les termes entre guillemets doubles, il risque de ne pas les interpréter comme vous le souhaitez.  
  
-   **Incorporation d’apostrophes** Si les données recherchées contiennent un guillemet unique (une apostrophe), vous pouvez entrer deux guillemets simples pour indiquer que ce guillemet est une valeur littérale, et non un délimiteur. Par exemple, la condition suivante permet de rechercher la valeur « Aujourd'hui » :  
  
    ```  
    ='Swann''s Way'  
    ```  
  
-   **Longueurs limites** Ne dépassez pas la longueur maximale de l’instruction SQL pour votre base de données quand vous entrez des chaînes longues.  
  
-   **Respect de la casse** Suivez les règles de respect de la casse associées à la base de données utilisée. C'est en effet la base de données utilisée qui détermine si les recherches de texte distinguent les majuscules et les minuscules. Par exemple, certaines bases de données interprètent l'opérateur « = » comme une correspondance avec respect de la casse, tandis que d'autres acceptent les correspondances pour n'importe quelle combinaison de caractères majuscules et minuscules.  
  
    Lorsque vous ignorez si la base de données utilise une recherche avec respect de la casse, vous pouvez utiliser les fonctions UPPER ou LOWER dans la condition de recherche afin de convertir la casse des données recherchées, comme illustré dans l'exemple suivant :  
  
    ```  
    WHERE UPPER(lname) = 'SMITH'  
    ```  
  
## <a name="searching-on-numeric-values"></a>Recherche sur des valeurs numériques  
Les indications suivantes s'appliquent lorsque vous entrez des valeurs numériques dans les conditions de recherche:  
  
-   **Guillemets** Ne mettez pas les nombres entre guillemets.  
  
-   **Caractères non numériques** N’incluez pas de caractères non numériques autres que le délimiteur décimal (défini dans la boîte de dialogue **Paramètres régionaux** du Panneau de configuration Windows) et le signe négatif (-). N'incluez pas de symboles de groupement des chiffres (telle une virgule entre les milliers) ni de symboles monétaires.  
  
-   **Marques décimales** Si vous entrez des nombres entiers, vous pouvez inclure une marque décimale, que la valeur recherchée soit un entier ou un nombre réel.  
  
-   **Notation scientifique** Vous pouvez entrer des nombres très petits ou très grands en utilisant la notation scientifique, comme dans l’exemple suivant :  
  
    ```  
    > 1.23456e-9  
    ```  
  
## <a name="searching-on-dates"></a>Recherche sur des dates  
Le format utilisé pour l'entrée des dates varie selon la base de données employée et le volet du Concepteur de requêtes et de vues dans lequel vous entrez les dates.  
  
> [!NOTE]  
> Si vous ne connaissez pas le format utilisé par votre source de données, tapez une date dans la colonne Filtre du volet Critères dans n'importe quel format qui vous est familier. Le Concepteur convertira la plupart des entrées de ce type au format approprié.  
  
Le Concepteur de requêtes et de vues accepte les formats de date suivants :  
  
-   **Données locales spécifiques** Le format spécifié pour les dates dans la boîte de dialogue **Windows Propriétés de Paramètres régionaux** .  
  
-   **Spécifique à la base de données** N’importe quel format interprété par la base de données.  
  
-   **Date ANSI standard** Format utilisant des accolades, le marqueur 'd' pour désigner la date et une chaîne de date, comme dans l’exemple suivant :  
  
    ```  
    { d '1990-12-31' }  
    ```  
  
-   **DateHeure ANSI standard** Similaire à la date ANSI standard, avec quelques différences : 'ts' est utilisé à la place de 'd' et l’heure (heures, minutes et secondes, dans un format sur 24 heures) est ajoutée à la date. L’exemple suivant indique la notation utilisée pour le 31 décembre 1990 :  
  
    ```  
    { ts '1990-12-31 00:00:00' }  
    ```  
  
    En général, le format de date ANSI standard est employé pour les bases de données qui représentent les dates avec un véritable type de données date. En revanche, le format datetime est employé pour les bases de données qui prennent en charge un type de données datetime.  
  
Le tableau suivant récapitule les formats de date que vous pouvez utiliser dans les différents volets du Concepteur de requêtes et de vues.  
  
|**Volet**|**Format de date**|  
|------------|-------------------|  
|Critères|Données locales spécifiques, spécifiques à la base de données, ANSI standard<br /><br />Les dates entrées dans le [volet Critères](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) sont converties dans un format compatible avec la base de données à l’intérieur du volet SQL.|  
|SQL|Données spécifique à la base de données, ANSI standard|  
|Résultats|Données locales spécifiques|  
  
## <a name="searching-on-logical-values"></a>Recherche sur des valeurs logiques  
Le format des données logiques varie d'une base de données à l'autre. Très souvent, la valeur False est stockée en tant que zéro (0). La valeur True est en général stockée en tant que 1, parfois en tant que -1. Les indications suivantes s'appliquent lorsque vous entrez des valeurs logiques dans les conditions de recherche :  
  
-   Pour rechercher la valeur False, utilisez un zéro, comme dans l'exemple suivant :  
  
    ```  
    SELECT * FROM authors  
    WHERE contract = 0  
    ```  
  
-   Si vous avez des doutes sur le format à utiliser pour rechercher la valeur True, essayez d'utiliser 1, comme dans l'exemple suivant :  
  
    ```  
    SELECT * FROM authors  
    WHERE contract = 1  
    ```  
  
-   Vous pouvez également élargir la portée de la recherche en recherchant toutes les valeurs différentes de zéro, comme dans l'exemple suivant :  
  
    ```  
    SELECT * FROM authors  
    WHERE contract <> 0  
    ```  
  
## <a name="see-also"></a> Voir aussi  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
