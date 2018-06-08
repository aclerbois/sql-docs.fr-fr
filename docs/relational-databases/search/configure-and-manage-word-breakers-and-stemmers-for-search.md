---
title: Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- full-text search [SQL Server], stemmers
- linguistic analysis [full-text search]
- full-text indexes [SQL Server], linguistic analysis
- full-text search [SQL Server], word breakers
- default full-text language option
- stemmers [full-text search]
- conjugating verbs [full-text search]
- word breakers [full-text search]
ms.assetid: d4bdd16b-a2db-4101-a946-583d1c674229
caps.latest.revision: 89
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 0ece22b891139b95d025ccf1f67c6dcac6633b8c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="configure-and-manage-word-breakers-and-stemmers-for-search"></a>Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Ils effectuent une analyse linguistique de l'ensemble des données indexées en texte intégral. L’analyse linguistique effectue les deux opérations suivantes :

-   **Recherche des limites de mots (analyse lexicale)**. L’*analyseur lexical* identifie des mots individuels en déterminant l’emplacement des limites de mots d’après les règles lexicales de cette langue. Chaque mot (également appelé *jeton*) est inséré dans l’index de recherche en texte intégral dans une représentation compressée afin de réduire sa taille.

-   **Conjugaison des verbes (recherche de radical)**. Le *générateur de formes dérivées* génère des formes flexionnelles d’un mot particulier selon les règles spécifiques d’une langue (par exemple, « running », « ran » et « runner » constituent différentes formes du mot « run »).

## <a name="word-breakers-and-stemmers-are-language-specific"></a>Les analyseurs lexicaux et générateurs de formes dérivées sont propres aux langues

Les analyseurs lexicaux et les générateurs de formes dérivées sont spécifiques à la langue, et les règles de l'analyse linguistique diffèrent selon les langues. L’utilisation d’analyseurs lexicaux propres aux langues permet d’obtenir des termes plus précis pour chaque langue.

Pour utiliser les analyseurs lexicaux et générateurs de formes dérivées fournis pour toutes les langues prises en charge par SQL Server, vous n’avez en général aucune action à effectuer.

-   Quand il existe un analyseur lexical pour une famille de langues, et non pour une sous-langue spécifique, la langue principale est utilisée. Par exemple, l'analyseur lexical en français gère le texte écrit en français canadien.
-   Si aucun analyseur lexical n'est disponible pour une langue particulière, l'analyseur lexical en langue neutre est utilisé. Avec l'analyseur lexical en langue neutre, les mots sont décomposés en caractères neutres tels que des espaces et des signes de ponctuation.

## <a name="get-the-list-of-supported-languages"></a>Obtenir la liste des langues prises en charge

Pour afficher la liste des langues prises en charge par la recherche en texte intégral [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante. La présence d’une langue dans cette liste signifie que des analyseurs lexicaux sont inscrits pour cette langue. 
  
```sql
SELECT * FROM sys.fulltext_languages
```

##  <a name="register"></a> Obtenir la liste des analyseurs lexicaux inscrits

Pour que la recherche en texte intégral utilise les analyseurs lexicaux pour une langue, ceux-ci doivent être inscrits. Pour les analyseurs lexicaux inscrits, les ressources linguistiques associées (générateurs de formes dérivées, mots parasites [mots vides] et fichiers de dictionnaire des synonymes) deviennent également disponibles pour des opérations d’indexation et d’interrogation de texte intégral.

Pour afficher la liste des composants d’analyseurs lexicaux inscrits, utilisez l’instruction suivante.

```sql
EXEC sp_help_fulltext_system_components 'wordbreaker';  
GO  
```

Pour plus d’informations et pour obtenir des options supplémentaires, consultez [sp_help_fulltext_system_components &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md).
 
## <a name="if-you-add-or-remove-a-word-breaker"></a>Si vous ajoutez ou supprimez un analyseur lexical  
Si vous ajoutez, supprimez ou modifiez un analyseur lexical, vous devez actualiser la liste des identificateurs de paramètres régionaux Microsoft Windows pris en charge pour l'indexation et les requêtes de texte intégral. Pour plus d’informations, consultez [Afficher ou modifier des filtres et des analyseurs lexicaux inscrits](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md).  
  
##  <a name="default"></a> Définir l’option de langue de texte intégral par défaut  
 Dans le cas d’une version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit l’option **Langue de texte intégral par défaut** en fonction de la langue du serveur s’il existe une correspondance appropriée. Pour une version non localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’anglais est la **langue de texte intégral par défaut** .  
  
 Quand vous créez ou modifiez un index de recherche en texte intégral, vous pouvez spécifier une langue différente pour chaque colonne indexée de texte intégral. Si aucune langue n’est spécifiée pour une colonne, la valeur par défaut est déterminée par l’option de configuration **Langue de texte intégral par défaut**.  
  
> [!NOTE]  
>  Toutes les colonnes répertoriées dans une clause de fonction de requête de texte intégral doivent utiliser la même langue, sauf si l'option LANGUAGE est spécifiée dans la requête. La langue utilisée pour la colonne d’index de recherche en texte intégral faisant l’objet d’une requête détermine l’analyse linguistique menée sur les arguments des prédicats de requête de texte intégral ([CONTAINS](../../t-sql/queries/contains-transact-sql.md) et [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)) et des fonctions de requête de texte intégral ([CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md)).  
  
##  <a name="lang"></a> Choisir la langue pour une colonne indexée  
 Lors de la création d'un index de recherche en texte intégral, nous vous recommandons de spécifier une langue pour chaque colonne indexée. Si tel n'est pas le cas, la langue par défaut du système est utilisée. La langue d'une colonne détermine l'analyseur lexical et le générateur de formes dérivées utilisés pour indexer cette colonne. En outre, le fichier de dictionnaire des synonymes de cette langue est utilisé par les requêtes de texte intégral sur la colonne.  
  
 Plusieurs aspects doivent être pris en considération pour le choix de la langue d'une colonne lors de la création d'un index de recherche en texte intégral. Ces aspects sont liés à la façon dont les unités lexicales de votre texte sont créées et à la façon dont ce texte est ensuite indexé par le Moteur d'indexation et de recherche en texte intégral. Pour plus d’informations, consultez [Choisir une langue lors de la création d’un index de recherche en texte intégral](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md).  
  
Pour afficher la langue d’analyseur lexical de colonnes spécifiques, exécutez l’instruction suivante.
   
```sql 
SELECT 'language_id' AS "LCID" FROM sys.fulltext_index_columns;
```  

Pour plus d’informations et pour obtenir des options supplémentaires, consultez [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md).

##  <a name="tshoot"></a> Dépanner les erreurs de délai d’attente de l’analyse lexicale  
 Une erreur de délai d’attente lors de l’analyse lexicale peut se produire dans un grand nombre de situations. Pour plus d’informations sur ces situations et sur la façon de résoudre le problème correspondant, consultez [MSSQLSERVER_30053](https://msdn.microsoft.com/en-us/library/cc879279.aspx).

### <a name="info-about-the-mssqlserver30053-error"></a>Informations sur l’erreur MSSQLSERVER_30053
  
|Propriété|Valeur|
|-|-|
|Nom du produit|SQL Server|  
|ID d'événement|30053|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|FTXT_QUERY_E_WORDBREAKINGTIMEOUT|  
|Texte du message|L'analyse lexicale a expiré pour la chaîne de requête de texte intégral. Cela peut se produire si l'analyseur lexical a mis beaucoup de temps à traiter la chaîne de requête de texte intégral ou si un grand nombre de requêtes sont exécutées sur le serveur. Essayez de réexécuter la requête en condition de charge moins élevée.|  
  
#### <a name="explanation"></a>Explication  
 Une erreur de dépassement de délai de l'analyse lexicale peut se produire dans les cas de figure suivants :  
  
-   L'analyseur lexical pour le langage de requête est configuré de manière incorrecte ; par exemple, ses paramètres du Registre sont incorrects.  
  
-   L'analyseur lexical présente des dysfonctionnements pour une chaîne de requête spécifique.  
  
-   L'analyseur lexical retourne une trop grande quantité de données pour une chaîne de requête spécifique. Les données excédentaires sont considérées comme une attaque de dépassement de mémoire tampon potentielle, ce qui entraîne l'arrêt du processus de démon de filtre (fdhost.exe), qui héberge les services d'analyse lexicale.  
  
-   La configuration du processus de démon de filtre est incorrecte.  
  
     Les problèmes de configuration les plus courants sont les suivants : expiration du mot de passe ou existence d'une stratégie de domaine qui empêche la connexion du compte de démon de filtre.  
  
-   Une charge de travail de requête très importante s'exécute sur l'instance de serveur ; par exemple, l'analyseur lexical prend beaucoup de temps pour traiter la chaîne de requête de texte intégral, ou un grand nombre de requêtes s'exécutent sur le serveur. Notez qu'il s'agit de la cause la plus improbable.  
  
#### <a name="user-action"></a>Action de l'utilisateur  
 Sélectionnez l'action utilisateur appropriée en fonction de la cause probable du délai d'attente, comme suit :  
  
|Cause probable|Action de l’utilisateur|  
|--------------------|-----------------|  
|L'analyseur lexical du langage de requête est configuré de manière incorrecte.|Si vous utilisez un analyseur lexical tiers, il est peut-être enregistré de manière incorrecte auprès du système d'exploitation. Dans ce cas, réenregistrez l'analyseur lexical. Pour plus d’informations, consultez [Rétablir la version précédente des analyseurs lexicaux utilisés par la recherche](revert-the-word-breakers-used-by-search-to-the-previous-version.md).|  
|L'analyseur lexical présente des dysfonctionnements pour une chaîne de requête spécifique.|Si l'analyseur lexical est pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contactez le service clientèle et le support technique Microsoft.|  
|L'analyseur lexical retourne une trop grande quantité de données pour une chaîne de requête spécifique.|Si l'analyseur lexical est pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contactez le service clientèle et le support technique Microsoft.|  
|La configuration du processus de démon de filtre est incorrecte.|Assurez-vous que vous utilisez le mot de passe actuel et qu'une stratégie de domaine n'empêche pas le compte du démon du filtre de se connecter.|  
|Une charge de traitement de requêtes très importante s'exécute sur l'instance de serveur.|Essayez de réexécuter la requête en condition de charge moins élevée.|  

##  <a name="impact"></a> Comprendre l’impact des analyseurs lexicaux mis à jour  
 Chaque version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut généralement de nouveaux analyseurs lexicaux qui possèdent de meilleures règles linguistiques et sont plus précis que les anciens analyseurs lexicaux. Les nouveaux analyseurs lexicaux peuvent se comporter légèrement différemment des analyseurs lexicaux dans les index de recherche en texte intégral qui ont été importés à partir de versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
 
Cela est significatif si un catalogue de texte intégral a été importé lorsqu'une base de données a été mise à niveau vers la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une ou plusieurs langues utilisées par les index de recherche en texte intégral dans le catalogue de texte intégral peuvent maintenant être associées aux nouveaux analyseurs lexicaux. Pour plus d’informations, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  

## <a name="see-also"></a> Voir aussi  
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)    
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 
  
