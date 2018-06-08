---
title: Regroupement approximatif, transformation | Microsoft Docs
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
f1_keywords:
- sql13.dts.designer.fuzzygroupingtrans.f1
- sql13.dts.designer.fuzzygroupingtransformation.connection.f1
- sql13.dts.designer.fuzzygroupingtransformation.columns.f1
- sql13.dts.designer.fuzzygroupingtransformation.advanced.f1
helpviewer_keywords:
- cleaning data
- comparing data
- token delimiters [Integration Services]
- temporary indexes [Integration Services]
- Fuzzy Grouping transformation
- temporary tables [Integration Services]
- grouping data
- standardizing data [Integration Services]
- columns [Integration Services], standardizing
- similarity thresholds [Integration Services]
- data cleaning [Integration Services]
- duplicate data [Integration Services]
ms.assetid: e43f17bd-9d13-4a8f-9f29-cce44cac1025
caps.latest.revision: 58
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 94fd84ce9a46537fb90caa47e869f96167493bdc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fuzzy-grouping-transformation"></a>Transformation de regroupement approximatif
  La transformation de regroupement probable effectue des tâches de nettoyage des données en identifiant les lignes de données susceptibles d'être des doublons et en sélectionnant une ligne canonique de données à utiliser pour standardiser les données.  
  
> [!NOTE]  
>  Pour plus d’informations sur la transformation de regroupement floue, y compris les limitations en termes de performances et de mémoire, consultez le livre blanc, [Présentation des transformations Fuzzy Lookup (recherche approximative) et Fuzzy Grouping (regroupement approximatif) dans les services DTS (Data Transformation Services) de SQL Server 2005](http://go.microsoft.com/fwlink/?LinkId=96604).  
  
 La transformation de regroupement approximatif nécessite une connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour créer les tables temporaires [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nécessaires à l'algorithme de transformation. La connexion doit correspondre à un utilisateur disposant de l'autorisation de créer des tables dans la base de données.  
  
 Pour configurer la transformation, vous devez sélectionner les colonnes d'entrée à utiliser lors de l'identification des doublons, et vous devez sélectionner le type de correspondance (approximative ou exacte) pour chaque colonne. Une correspondance exacte garantit que seules les lignes possédant des valeurs identiques dans cette colonne seront regroupées. La correspondance exacte peut être appliquée aux colonnes de n'importe quel type de données [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , à l'exception des types DT_TEXT, DT_NTEXT et DT_IMAGE. Une correspondance approximative regroupe des lignes ayant à peu près les mêmes valeurs. La méthode utilisée pour la correspondance approximative des données est basée sur un score de similarité spécifié par l'utilisateur. Seules les colonnes avec les types de données DT_WSTR et DT_STR peuvent être utilisées dans la correspondance approximative. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
 La sortie de transformation comprend toutes les colonnes d'entrée, une ou plusieurs colonnes avec des données standardisées et une colonne contenant le score de similarité. Le score est représenté par une valeur décimale entre 0 et 1. La ligne canonique obtient le score de 1. Les scores des autres lignes dans le regroupement probable indiquent leur degré de correspondance avec cette ligne canonique. Plus le score se rapproche de 1, plus la ligne correspond à la ligne canonique. Si le regroupement probable comprend des lignes qui sont des doublons exacts de la ligne canonique, ces lignes ont également pour score la valeur 1. La transformation ne supprime pas les lignes dupliquées, mais les regroupe en créant une clé qui associe la ligne canonique à des lignes similaires.  
  
 La transformation produit une ligne de sortie pour chaque ligne d'entrée, avec les colonnes supplémentaires suivantes :  
  
-   **_key_in**, une colonne qui identifie chaque ligne de manière unique.  
  
-   **_key_out**, une colonne qui identifie un groupe de lignes dupliquées. La colonne **_key_out** a la même valeur que la colonne **_key_in** dans la ligne de données canonique. Les lignes ayant une valeur identique dans **_key_out** font partie du même groupe. La valeur **_key_out**pour un groupe correspond à la valeur **_key_in** dans la ligne de données canonique.  
  
-   **_score**, valeur comprise entre 0 et 1, qui indique la similarité de la ligne d’entrée avec la ligne canonique.  
  
 Il s'agit des noms de colonne par défaut ; vous pouvez configurer la transformation de regroupement probable pour utiliser d'autres noms. La sortie fournit également un score de similarité pour chaque colonne participant à un regroupement probable.  
  
 La transformation de regroupement probable comprend deux fonctionnalités de personnalisation du regroupement qu'elle effectue : les séparateurs de jetons et les seuils de similarité. La transformation fournit un ensemble par défaut de délimiteurs utilisés pour marquer des données, mais vous pouvez ajouter de nouveaux délimiteurs pour améliorer la création de jetons pour vos données.  
  
 Le seuil de similarité indique le degré de précision avec lequel la transformation identifie les doublons. Les seuils de similarité peuvent être définis au niveau du composant et de la colonne. Le seuil de similarité n'est disponible que pour les colonnes participant à une correspondance approximative. La plage de similarité va de 0 à 1. Plus le seuil s'approche de 1, plus les lignes et les colonnes doivent être similaires pour être répertoriées comme doublons. Vous spécifiez le seuil de similarité en définissant la propriété MinSimilarity au niveau du composant et de la colonne. Pour obtenir la similarité spécifiée au niveau du composant, toutes les lignes doivent montrer une similarité à travers toutes les colonnes qui est supérieure ou égale au seuil de similarité spécifié au niveau du composant.  
  
 La transformation de regroupement probable calcule les mesures internes de similarité ; les lignes moins similaires à la valeur spécifiée dans MinSimilarity ne sont pas regroupées.  
  
 Pour identifier un seuil de similarité qui fonctionne pour vos données, vous pouvez avoir à appliquer la transformation de regroupement probable plusieurs fois, avec des seuils de similarité minimale différents. Lors de l'exécution, les colonnes de score dans la sortie de la transformation contiennent les scores de similarité pour chaque ligne d'un groupe. Vous pouvez utiliser ces valeurs pour identifier le seuil de similarité approprié pour vos données. Si vous voulez augmenter la similarité, vous devez donner à MinSimilarity une valeur supérieure à celle des colonnes de score.  
  
 Vous pouvez personnaliser le regroupement effectué par la transformation en définissant les propriétés des colonnes dans l'entrée de la transformation du regroupement probable. Par exemple, la propriété FuzzyComparisonFlags spécifie comment la transformation compare les données de chaîne dans une colonne, et la propriété ExactFuzzy indique si la transformation effectue une correspondance approximative ou exacte.  
  
 Vous pouvez configurer la quantité de mémoire utilisée par la transformation de regroupement probable en définissant la propriété personnalisée MaxMemoryUsage. Vous pouvez spécifier le nombre de mégaoctets (Mo) ou utiliser la valeur 0 pour permettre à la transformation d'utiliser une quantité dynamique de mémoire en fonction de ses besoins et de la mémoire physique disponible. La propriété personnalisée MaxMemoryUsage peut être mise à jour par une expression de la propriété au moment du chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Expressions de propriété dans des packages](../../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Cette transformation a une entrée et une sortie. Elle ne prend pas en charge de sortie d'erreur.  
  
## <a name="row-comparison"></a>Comparaison de lignes  
 Lorsque vous configurez la transformation de regroupement probable, vous pouvez spécifier l'algorithme de comparaison utilisé par la transformation pour comparer les lignes dans l'entrée de transformation. Si vous définissez la propriété Exhaustive comme ayant la valeur **true**, la transformation compare chaque ligne de l’entrée à chaque autre ligne de l’entrée. Cet algorithme de comparaison peut produire des résultats plus précis, mais il peut ralentir la transformation, sauf si le nombre de lignes dans l'entrée est peu élevé. Pour éviter les problèmes de performance, il est recommandé de définir la propriété Exhaustive avec la valeur **true** seulement au cours du développement du package.  
  
## <a name="temporary-tables-and-indexes"></a>Tables et index temporaires  
 Au moment de l'exécution, la transformation de regroupement approximatif crée des objets temporaires, tels que des tables et des index, parfois de taille importante, dans la base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à laquelle la transformation se connecte. La taille des tables et des index est proportionnelle au nombre de lignes dans l'entrée de la transformation et au nombre de jetons créés par la transformation de regroupement probable.  
  
 La transformation exécute également des requêtes sur les tables temporaires. Vous devez donc envisager de connecter la transformation de regroupement probable à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]qui n’est pas en production, notamment si le serveur de production dispose d’un espace disque disponible limité.  
  
 Les performances de cette transformation peuvent s'améliorer si les tables et les index qu'elle utilise sont situés sur l'ordinateur local.  
  
## <a name="configuration-of-the-fuzzy-grouping-transformation"></a>Configuration de la transformation de regroupement probable  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d'informations sur la définition des propriétés de cette tâche, cliquez sur l'une des rubriques suivantes :  
  
-   [Identifier des lignes de données semblables à l'aide de la transformation de regroupement probable](../../../integration-services/data-flow/transformations/identify-similar-data-rows-by-using-the-fuzzy-grouping-transformation.md)  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="fuzzy-grouping-transformation-editor-connection-manager-tab"></a>Éditeur de transformation de regroupement probable (onglet Gestionnaire de connexions)
  Utilisez l'onglet **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de transformation de regroupement probable** pour sélectionner une connexion existante ou en créer une.  
  
> [!NOTE]  
>  Le serveur défini par la connexion doit exécuter [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La transformation de regroupement probable crée des objets de données temporaires dans tempdb qui peuvent être aussi volumineux que l’ensemble de l’entrée de la transformation. Au cours de son exécution, la transformation envoie des requêtes serveur par rapport aux objets temporaires, ce qui peut affecter les performances générales du serveur.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez une connexion OLE DB existante en utilisant la zone de liste, ou créez une connexion en utilisant le bouton **Nouvelle** .  
  
 **Nouveau**  
 Crée une connexion en utilisant la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** .  
  
## <a name="fuzzy-grouping-transformation-editor-columns-tab"></a>Éditeur de transformation de regroupement approximatif (onglet Colonnes)
  L'onglet **Colonnes** de la boîte de dialogue **Éditeur de transformation de regroupement approximatif** permet de spécifier les colonnes utilisées pour regrouper les lignes comportant des doublons.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Sélectionnez dans cette liste les colonnes d'entrée utilisées pour regrouper les lignes comportant des doublons.  
  
 **Nom**  
 Permet d'afficher le nom des colonnes d'entrée disponibles.  
  
 **Transfert direct**  
 Permet d'indiquer s'il est nécessaire d'inclure la colonne d'entrée dans la sortie de la transformation. Toutes les colonnes utilisées pour le regroupement sont automatiquement copiées dans la sortie. Vous pouvez inclure des colonnes supplémentaires en activant cette colonne.  
  
 **Colonne d'entrée**  
 Choisissez l’une des colonnes d’entrée précédemment sélectionnées dans la liste **Colonnes d’entrée disponibles** .  
  
 **Alias de sortie**  
 Entrez un nom descriptif pour la colonne de sortie correspondante. Par défaut, cette colonne porte le même nom que la colonne d'entrée.  
  
 **Grouper les alias de sortie**  
 Entrez un nom descriptif pour la colonne qui va contenir la valeur canonique des doublons groupés. Par défaut, cette colonne de sortie porte le nom de la colonne d'entrée suivi de la mention « _clean ».  
  
 **Type de correspondance**  
 Sélectionnez la correspondance floue ou exacte. Avec une correspondance approximative, les lignes sont considérées comme des doublons si elles sont suffisamment similaires dans toutes les colonnes. Si vous spécifiez une correspondance exacte pour certaines colonnes, seules les lignes contenant des valeurs identiques dans les colonnes associées à une correspondance exacte seront considérées comme des doublons probables. Par conséquent, si vous savez qu'une colonne ne contient aucune erreur ni incohérence, vous pouvez opter pour la correspondance exacte sur cette colonne afin d'accroître la précision de la correspondance approximative sur d'autres colonnes.  
  
 **Similarité minimale**  
 Définissez, à l'aide du curseur, le seuil de similarité au niveau de la jointure. Plus la valeur est proche de 1, plus la valeur de recherche doit être proche de la valeur source pour constituer une correspondance. Si vous augmentez le seuil, vous pouvez améliorer la vitesse de correspondance étant donné qu'un plus petit nombre d'enregistrements candidats doit être pris en compte.  
  
 **Alias de sortie de similarité**  
 Spécifiez le nom d'une nouvelle colonne de sortie qui contient le score de similarité de la jointure sélectionnée. Si vous ne définissez pas cette valeur, la colonne de sortie n'est pas créée.  
  
 **Chiffres**  
 Spécifiez l'importance des premiers et derniers chiffres en comparant les données de la colonne. Par exemple, si les premiers chiffres sont significatifs, « 123 Main Street » ne sera pas groupé avec « 456 Main Street ».  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Les premiers et derniers chiffres ne sont pas significatifs.|  
|**Premiers**|Seuls les premiers chiffres sont significatifs.|  
|**Derniers**|Seuls les derniers chiffres sont significatifs.|  
|**LeadingAndTrailing**|Les premiers et derniers chiffres sont significatifs.|  
  
 **Indicateurs de comparaison**  
 Pour plus d’informations sur les options de comparaison de chaînes, consultez [Comparaison des données chaînes](../../../integration-services/data-flow/comparing-string-data.md).  
  
## <a name="fuzzy-grouping-transformation-editor-advanced-tab"></a>Éditeur de transformation de regroupement probable (onglet Avancé).
  Utilisez l'onglet **Avancé** de la boîte de dialogue **Éditeur de transformation de regroupement probable** pour spécifier les colonnes d'entrée et de sortie, définir des seuils de similarité et des séparateurs.  
  
> [!NOTE]  
>  Les propriétés **Exhaustive** et **MaxMemoryUsage** de la transformation de regroupement approximatif ne sont pas disponibles dans l' **Éditeur de transformation de regroupement approximatif**, mais elles peuvent être définies à l'aide de l' **Éditeur avancé**. Pour plus d'informations sur ces propriétés, consultez la section Transformation de regroupement approximatif dans [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
### <a name="options"></a>Options  
 **Nom de la colonne clé d'entrée**  
 Spécifiez le nom d'une colonne de sortie qui contient l'identificateur unique de chaque ligne d'entée. La colonne **_key_in** a une valeur qui identifie chaque ligne de manière unique.  
  
 **Nom de la colonne clé de sortie**  
 Spécifiez le nom d'une colonne de sortie qui contient l'identificateur unique de la ligne canonique d'un groupe de lignes dupliquées. La colonne **_key_out** correspond à la valeur **_key_in** de la ligne de données canonique.  
  
 **Nom de colonne du score de similarité**  
 Spécifiez un nom qui contient le score de similarité. Le score de similarité est une valeur comprise entre 0 et 1 qui indique le niveau de similarité avec la ligne canonique. Plus le score se rapproche de 1, plus la ligne correspond à la ligne canonique.  
  
 **Seuil de similarité**  
 Définissez le seuil de similarité au moyen du curseur. Plus le seuil est proche de 1, plus la similarité entre les lignes est grande pour se qualifier comme lignes dupliquées. L'augmentation du seuil peut accélérer les recherches du fait que moins de candidats doivent être évalués.  
  
 **Séparateurs de jetons**  
 La transformation fournit un ensemble de séparateurs par défaut pour marquer des données, mais vous devez ajouter ou supprimer des séparateurs en modifiant la liste en fonction des besoins.  
  
## <a name="see-also"></a> Voir aussi  
 [Transformation de recherche floue](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
