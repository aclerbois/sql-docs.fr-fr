---
title: Choisir une langue lors de la création d’un index de recherche en texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.prod_service: search, sql-database
ms.component: search
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- full-text indexes [SQL Server], languages
- international considerations [full-text search]
- stemmers [full-text search]
- global considerations [full-text search]
- full-text search [SQL Server], international considerations
- languages [SQL Server], full-text indexes
- word breakers [full-text search]
ms.assetid: 670a5181-ab80-436a-be96-d9498fbe2c09
caps.latest.revision: 49
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 90d15ff0623ddef99d5209b4a6f3050ebef33486
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="choose-a-language-when-creating-a-full-text-index"></a>Choisir une langue lors de la création d'un index de recherche en texte intégral

[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Lorsque vous créez un index de recherche en texte intégral, vous devez spécifier une langue au niveau de la colonne pour la colonne indexée. L’ [analyseur lexical et les générateurs de formes dérivées](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) de la langue spécifiée seront utilisés par les requêtes de texte intégral sur la colonne. Plusieurs aspects doivent être pris en considération pour le choix de la langue d'une colonne lors de la création d'un index de texte intégral. Ces aspects sont liés à la façon dont les unités lexicales de votre texte sont créées et à la façon dont ce texte est ensuite indexé par le Moteur d'indexation et de recherche en texte intégral.  
  
> [!NOTE]  
>  Pour spécifier une langue au niveau de la colonne pour une colonne d’index de recherche en texte intégral, utilisez la clause LANGUAGE *language_term* lors de la spécification de la colonne. Pour plus d’informations, consultez [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md) et [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
##  <a name="langsupp"></a> Prise en charge linguistique dans la recherche en texte intégral  
 Cette section fournit une introduction aux analyseurs lexicaux et aux générateur de formes dérivées et indique comment la recherche en texte intégral utilise le LCID de la langue de la colonne.  
  
### <a name="introduction-to-word-breakers-and-stemmers"></a>Introduction aux analyseurs lexicaux et aux générateurs de formes dérivées  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures incluent une nouvelle famille complète d’analyseurs lexicaux et de générateurs de formes dérivées qui sont considérablement plus performants que ceux précédemment disponibles dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Le Microsoft Natural Language Group (MS NLG) a implémenté et prend en charge ces nouveaux composants linguistiques.  
  
 Les avantages de ces nouveaux analyseurs lexicaux sont les suivants :  
  
-   Robustesse  
  
     Les tests ont montré que les nouveaux analyseurs lexicaux sont fiables dans les environnements de requête de haute intensité.  
  
-   Sécurité  
  
     Les nouveaux analyseurs lexicaux sont activés par défaut dans SQL Server grâce aux améliorations de sécurité apportées aux composants linguistiques. Nous recommandons vivement que les composants externes tels que les analyseurs lexicaux et filtres soient signés afin d’améliorer la sécurité globale et la robustesse de SQL Server. Vous pouvez configurer le texte intégral pour vérifier que ces composants sont signés comme suit :  
  
    ```  
    EXEC sp_fulltext_service 'verify_signature';  
    ```  
  
-   Qualité  
  
     Les analyseurs lexicaux ont été repensés, et les tests ont montré que les nouveaux analyseurs lexicaux fournissent une qualité sémantique supérieure à celle des analyseurs lexicaux précédents. Cela augmente l'exactitude de rappel.  
  
-   Pour couvrir une longue liste de langues, les analyseurs lexicaux sont inclus d’emblée dans SQL Server et sont activés par défaut.  
  
 Pour obtenir la liste des langues pour lesquelles SQL Server comprend des analyseurs lexicaux et des générateurs de formes dérivées, consultez [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
  
### <a name="how-full-text-search-uses-the-name-of-the-column-level-language"></a>Comment la recherche en texte intégral utilise le nom de la langue de la colonne  
 Lorsque vous créez un index de recherche en texte intégral, vous devez spécifier un nom de langue valide pour chaque colonne. Si un nom de langue est valide mais n’est pas retourné par l’affichage catalogue [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md) , la recherche en texte intégral revient, le cas échéant, au nom de la langue disponible le plus proche de la même famille de langues. Sinon, la recherche en texte intégral revient à l'analyseur lexical neutre. Ce comportement de repli peut affecter l'exactitude de rappel. Par conséquent, nous vous recommandons vivement de spécifier un nom de langue valide et disponible pour chaque colonne lors de la création d'un index de recherche en texte intégral.  
  
> [!NOTE]  
>  Le LCID est appliqué à tous les types de données pouvant faire l’objet d’une indexation de texte intégral (par exemple **char** ou **nchar**). Si l’ordre de tri d’une colonne de type **char**, **varchar**ou **text** est défini à l’aide d’une langue différente de la langue identifiée par le LCID, ce dernier est néanmoins utilisé durant l’indexation de recherche en texte intégral et l’interrogation de ces colonnes.  
  
  
##  <a name="breaking"></a> Analyse lexicale  
 Un analyseur lexical crée des jetons dans le texte indexé à partir des limites des mots, qui sont spécifiques aux langues. Par conséquent, le comportement d'analyse lexicale diffère d'une langue à l'autre. Si vous utilisez une langue x pour indexer plusieurs langues {x, y et z}, une partie du comportement peut donner lieu à des résultats inattendus. Par exemple, un tiret (-) ou une virgule (,) peut être un élément d'analyseur lexical pouvant être rejeté dans une langue mais pas dans un autre. Un comportement de génération de formes dérivées rarement inattendu peut également se produire parce qu'un mot donné peut être dérivé différemment dans une autre langue. Par exemple, en anglais, les limites des mots sont généralement fixées par des espaces blancs ou un signe de ponctuation. Dans d'autres langues, par exemple l'allemand, les mots ou les caractères peuvent être accolés. Par conséquent, le choix de la langue d'une colonne doit être représentatif de la langue destinée à être stockée dans les lignes de cette colonne.  
  
### <a name="western-languages"></a>Langues occidentales  
 Pour la famille des langues occidentales, si vous êtes incertain quant aux langues qui seront stockées dans une colonne ou si vous pensez en stocker plusieurs, la solution de contournement générale est d'utiliser l'analyseur lexical pour la langue la plus complexe pouvant être stockée dans la colonne. Par exemple, vous pouvez vous attendre à stocker du contenu en anglais, en espagnol et en allemand dans une colonne unique. Ces trois langues occidentales possèdent des modes d'analyse lexicale très semblables, les modes allemands étant les plus complexes. Par conséquent, un bon choix dans ce cas serait d'utiliser l'analyseur lexical allemand, qui doit être en mesure de traiter le texte anglais et espagnol correctement. Par contre, il se peut que l'analyseur lexical anglais ne puisse pas traiter parfaitement le texte allemand à cause des mots composés allemands.  
  
 Notez que l'utilisation de l'analyseur lexical de la langue la plus complexe dans une famille de langues ne garantit pas l'indexation parfaite de chaque langue de la famille. Il peut y avoir des cas où l'analyseur lexical le plus complexe ne peut pas gérer correctement le texte écrit dans une autre langue.  
  
  
### <a name="non-western-languages"></a>Langues non occidentales  
 Pour les langues non occidentales (telles que chinois, japonais, hindi, etc.), la solution de contournement précitée ne fonctionne pas nécessairement, pour des raisons linguistiques. Pour les langues non occidentales, considérez l'une des solutions de contournement suivantes :  
  
-   Pour les langues de familles différentes  
  
     Si une colonne peut contenir des langues complètement différentes, par exemple l'espagnol et le japonais, pensez à stocker le contenu de langues différentes dans des colonnes séparées. Cela vous permettrait d'utiliser l'analyseur lexical spécifique à une langue pour chaque colonne. Si vous choisissez cette solution et que vous ne connaissez pas la langue de la requête à l'heure de la requête, vous pouvez devoir lancer la requête par rapport aux deux colonnes afin de faire en sorte que la requête recherche la bonne ligne ou le bon document.  
  
-   Pour le contenu binaire (par exemple, les documents Microsoft Word)  
  
     Lorsque le contenu indexé est de type **binaire** , le filtre de recherche en texte intégral qui traite le contenu textuel avant de l’envoyer à l’analyseur lexical peut respecter des balises de langue spécifiques existantes dans le fichier binaire. Dans ce cas, au moment de l'indexation, le filtre émettra le bon LCID pour un document ou une section d'un document. Le moteur d'indexation et de recherche en texte intégral appellera ensuite l'analyseur lexical pour la langue correspondant à ce LCID. Toutefois, après avoir indexé un contenu multilingue, nous vous recommandons de vérifier que le contenu a été indexé correctement.  
  
-   Pour un contenu de texte brut  
  
     Lorsque votre contenu est du texte brut, vous pouvez le convertir en type de données **xml** et ajouter les balises de langue qui indiquent la langue qui correspond à chaque document ou section de document spécifique. Pour que cette option fonctionne toutefois, vous devez connaître la langue avant l'indexation de recherche en texte intégral.  
  
  
##  <a name="stemming"></a> Recherche de radical  
 La recherche de radical est un autre point à prendre en considération lors du choix de la langue de votre colonne. La*recherche de radical* dans les requêtes de texte intégral se définit comme la recherche de toutes les formes fléchies d’un mot dans une langue particulière. Lorsque vous utilisez un analyseur lexical générique pour traiter plusieurs langues, le processus de recherche de radical fonctionne uniquement pour la langue spécifiée pour la colonne, et non pour d'autres langues dans la colonne. Par exemple, les générateur de formes dérivées allemands ne fonctionnent pas pour l'anglais et l'espagnol (etc.). Cela peut affecter votre rappel, selon la langue que vous choisissez au moment de la requête.  
  
  
##  <a name="type"></a> Effet du type de colonne sur la recherche en texte intégral  
 Un autre point à prendre en considération dans le choix de la langue est lié au mode de représentation des données. Pour les données non stockées dans une colonne **varbinary(max)** , aucun filtrage particulier n’est effectué. À la place, le texte est généralement traité tel quel par le composant de séparation des mots.  
  
 Les analyseurs lexicaux sont, aussi, principalement conçus pour traiter le texte écrit. Par conséquent, si votre texte contient un balisage quelconque (par exemple du code HTML), vous risquez de ne pas obtenir une précision linguistique importante durant l'indexation et la recherche. Dans ce cas, vous avez deux possibilités : la méthode recommandée consiste simplement à stocker les données de texte dans la colonne **varbinary(max)** et à indiquer le type de document correspondant pour permettre un filtrage. Si ce choix ne vous convient pas, utilisez un analyseur lexical neutre et, si possible, ajoutez des données de balisage (par exemple « br » en langage HTML) à vos listes de mots parasites.  
  
> [!NOTE]  
>  L'identification de la racine linguistique n'intervient pas lorsque vous spécifiez la langue neutre.  
  
  
##  <a name="nondef"></a> Spécification d'une langue de colonne autre que la langue par défaut dans un requête de texte intégral  
 Par défaut, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la recherche en texte intégral analyse les termes de la requête en utilisant la langue spécifiée pour chaque colonne incluse dans la clause de texte intégral. Pour remplacer ce comportement, spécifiez une langue autre que par défaut au moment de la requête. Pour les langues prises en charge dont les ressources sont installées, la clause LANGUAGE *language_term* d’une requête [CONTAINS](../../t-sql/queries/contains-transact-sql.md), [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md), [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)ou [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) est utilisée pour spécifier la langue utilisée pour l’analyse lexicale, la recherche de radical, le dictionnaire des synonymes et le traitement des mots vides des termes de la requête.  
  
  
## <a name="see-also"></a> Voir aussi  
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md)   
 [sp_fulltext_service &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)   
 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)  
  
  
