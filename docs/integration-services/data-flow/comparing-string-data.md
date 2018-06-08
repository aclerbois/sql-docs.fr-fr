---
title: Comparaison de données de chaînes | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- comparing string data
- comparison options [Integration Services]
- locales [Integration Services]
- converting string data
- string comparisons
ms.assetid: 93aeb5bd-e208-46b7-8979-dea2dcd37d4c
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c60572c425df10ab659239217424d789bfe2abc1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="comparing-string-data"></a>comparaison de données de chaînes
  Les comparaisons de chaînes représentent une partie importante des transformations réalisées par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et peuvent également être utilisées pour l'évaluation d'expressions dans des variables et des expressions de propriétés. Ainsi, la transformation de tri peut comparer les valeurs d'un dataset afin de trier les données dans l'ordre croissant ou décroissant.  
  
## <a name="configuring-transformations-for-string-comparisons"></a>Configuration des transformations pour les comparaisons de chaînes  
 Les transformations de tri, d'agrégation, de regroupement probable et de recherche floue peuvent être personnalisées afin de changer le mode de comparaison des chaînes au niveau de la colonne. Par exemple, vous pouvez spécifier qu'une comparaison ignore la casse, ce qui signifie que les caractères minuscules et majuscules sont considérés comme identiques.  
  
 Les transformations suivantes utilisent des expressions contenant des comparaisons de chaînes.  
  
-   La transformation de fractionnement conditionnel peut utiliser des comparaisons de chaînes dans des expressions afin de déterminer à quelle sortie envoyer la ligne de données. Pour plus d'informations, consultez [Conditional Split Transformation](../../integration-services/data-flow/transformations/conditional-split-transformation.md).  
  
-   La transformation de colonne dérivée peut utiliser des comparaisons de chaînes dans des expressions afin de générer de nouvelles valeurs de colonnes. Pour plus d'informations, consultez [Derived Column Transformation](../../integration-services/data-flow/transformations/derived-column-transformation.md).  
  
 Les variables, les mappages de variables et les contraintes de précédence utilisent également des expressions qui peuvent inclure des comparaisons de chaînes. Pour plus d’informations sur les expressions, consultez [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
## <a name="processing-during-string-comparison"></a>Traitement pendant la comparaison de chaînes  
 En fonction des données et de la configuration de la transformation, le traitement suivant peut être réalisé au cours de la comparaison de données chaînes :  
  
-   conversion des données au format Unicode. Si les données sources ne sont pas au format Unicode, elles sont automatiquement converties dans ce format avant la comparaison ;  
  
-   utilisation de paramètres régionaux afin d'appliquer des règles spécifiques à un pays pour interpréter la date, l'heure, les données décimales et l'ordre de tri ;  
  
-   application d'options de comparaison au niveau de la colonne afin de changer la sensibilité des comparaisons.  
  
## <a name="converting-string-data-to-unicode"></a>Conversion de données de chaîne au format Unicode  
 En fonction des opérations réalisées par la transformation et de sa configuration, les données de chaîne peuvent être converties en type de données DT_WSTR, qui est une représentation Unicode des caractères.  
  
 Les données de chaîne de type DT_STR sont converties en chaînes Unicode à l'aide de la page de codes de la colonne. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge les pages de codes au niveau de la colonne et chaque colonne peut être convertie à l'aide d'une page de codes différente.  
  
 Dans la plupart des cas, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut identifier la page de codes correcte à partir de la source de données. Par exemple, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez définir un classement au niveau de la base de données et de la colonne. La page de codes provient d'un classement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , qui peut être un classement Windows ou SQL.  
  
 Si [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] propose une page de codes non attendue ou si le package accède à une source de données à l'aide d'un fournisseur ne donnant pas suffisamment d'informations pour déterminer la page de codes correcte, vous pouvez spécifier une page de codes par défaut dans la source ou la destination OLE DB. Les pages de codes par défaut sont utilisées à la place des pages de codes fournies par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Les fichiers n'ont pas de pages de codes. À la place, les gestionnaires de connexions de fichiers plats et de fichiers plats multiples utilisés par un package pour se connecter aux données du fichier utilisent une propriété permettant de spécifier la page de codes du fichier. Cette page de codes peut être définie au niveau du fichier uniquement, pas au niveau de la colonne.  
  
## <a name="setting-locale"></a>Définition des paramètres régionaux  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n’utilise pas la page de codes pour déduire les règles spécifiques à un pays servant à trier les données ou à interpréter la date, l’heure et les données décimales. Au lieu de cela, la transformation lit les paramètres régionaux définis par la propriété LocaleId au niveau du composant de flux de données, de la tâche de flux de données, du conteneur ou du package. Par défaut, les paramètres régionaux d'une transformation sont hérités de sa tâche de flux de données, qui elle-même les héritent du package. Si la tâche de flux de données se trouve dans un conteneur comme le conteneur de boucles For, elle hérite ses paramètres régionaux du conteneur.  
  
 Vous pouvez également spécifier des paramètres régionaux pour un gestionnaire de connexions de fichiers plats et un gestionnaire de connexions de fichiers plats multiples.  
  
## <a name="setting-comparison-options"></a>Définition des options de comparaison  
 Les paramètres régionaux déterminent les règles de base pour la comparaison de données chaînes. Ils indiquent par exemple la position de tri de chaque lettre dans l'alphabet. Cependant, ces règles ne suffisent pas toujours dans les comparaisons réalisées par certaines transformations et [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge un ensemble d'options de comparaison avancées extrapolant les règles de comparaison des paramètres régionaux. Ces options de comparaison sont définies au niveau de la colonne. Une de ces options de comparaison permet par exemple d'ignorer les caractères sans espace. Les signes diacritiques comme l'accent sont alors ignorés ce qui conduit à considérer « a » et « á » comme identiques dans les comparaisons.  
  
 Le tableau qui suit décrit les options de comparaison et un style de tri.  
  
|Option de comparaison|Description|  
|-----------------------|-----------------|  
|Ignorer la casse|Indique si la comparaison fait la distinction entre les lettres majuscules et minuscules. Si cette option est définie, la comparaison de chaînes ignore la casse. Par exemple, « ABC » est alors identique à « abc ».|  
|Ignorer le type de caractères Kana|Indique si la comparaison fait la distinction entre les deux types de caractères japonais Kana : Hiragana et Katakana. Si cette option est définie, la comparaison de chaînes ignore le type Kana.|  
|Ignorer la largeur des caractères|Indique si la comparaison fait la distinction entre un caractère sur un octet et le même caractère représenté sur deux octets. Si cette option est définie, la comparaison de chaînes traite les représentations sur un octet et sur deux octets du même caractère comme identiques.|  
|Ignorer les caractères sans espace|Indique si la comparaison fait la distinction entre les caractères avec espace et les caractères diacritiques. Si cette option est définie, la comparaison ignore les caractères diacritiques. Par exemple, « å » est identique à « a ».|  
|Ignorer les symboles|Indique si la comparaison fait la distinction entre les lettres et les symboles comme les espaces blancs, la ponctuation, les symboles de devise et les symboles mathématiques. Si cette option est définie, la comparaison de chaînes ignore les symboles. Par exemple, «  New York »" est identique à « New York » et « *ABC » est identique à « ABC ».|  
|Trier la ponctuation comme des symboles|Indique si la comparaison trie tous les symboles de ponctuation, à l'exception du tiret et de l'apostrophe, avant les caractères alphanumériques. Par exemple, si cette option est définie ; « .ABC » arrive avant « ABC » dans le tri.|  
  
 Les transformations de tri, d'agrégation, de regroupement probable et de recherche floue incluent ces options pour la comparaison de données.  
  
 La balise de comparaison **FullySensitive** s'affiche dans la boîte de dialogue **Éditeur avancé** pour les transformations de regroupement probable et de recherche floue. Le fait de sélectionner l'indicateur de comparaison **FullySensitive** signifie que toutes les options de comparaison s'appliquent.  
  
## <a name="see-also"></a> Voir aussi  
 [Types de données d'Integration Services](../../integration-services/data-flow/integration-services-data-types.md)   
 [Analyse rapide](http://msdn.microsoft.com/library/6688707d-3c5b-404e-aa2f-e13092ac8d95)   
 [Analyse standard](http://msdn.microsoft.com/library/dfe835b1-ea52-4e18-a23a-5188c5b6f013)  
  
  
