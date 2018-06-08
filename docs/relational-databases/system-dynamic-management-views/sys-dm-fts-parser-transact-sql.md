---
title: Sys.dm_fts_parser (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_parser_TSQL
- dm_fts_parser
- dm_fts_parser_TSQL
- sys.dm_fts_parser
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_parser dynamic management function
- troubleshooting [SQL Server], full-text search
ms.assetid: 2736d376-fb9d-4b28-93ef-472b7a27623a
caps.latest.revision: 37
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 717e97ca38fdc68b0ba807f546a253793234375b
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmftsparser-transact-sql"></a>sys.dm_fts_parser (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne le résultat final de création de jetons après avoir appliqué une donnée [lexical](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md), [dictionnaire des synonymes](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md), et [liste de mots vides](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) combinaison pour une entrée de chaîne de requête. Le résultat de la segmentation du texte en unités lexicales équivaut à la sortie du Moteur d'indexation et de recherche en texte intégral pour la chaîne de requête spécifiée.  
  
 sys.dm_fts_parser est une fonction de gestion dynamique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_fts_parser('query_string', lcid, stoplist_id, accent_sensitivity)  
```  
  
## <a name="arguments"></a>Arguments  
 *query_string*  
 Requête à analyser. *QUERY_STRING* peut être une chaîne chaîne [CONTAINS](../../t-sql/queries/contains-transact-sql.md) prise en charge de la syntaxe. Par exemple, vous pouvez inclure des formes fléchies, un dictionnaire des synonymes et des opérateurs logiques.  
  
 *lcid*  
 Identificateur de paramètres régionaux (LCID) de l’analyseur lexical à utiliser pour l’analyse *query_string*.  
  
 *stoplist_id*  
 ID de la liste de mots vides, cas échéant, pour être utilisé par l’analyseur lexical identifié par *lcid*. *stoplist_id* est **int**. Si vous spécifiez 'NULL', aucune liste de mots vides n'est utilisée. Si vous spécifiez 0, la liste de mots vides système est utilisée.  
  
 Un ID de liste de mots vides est unique dans une base de données. Pour obtenir l’ID de la liste de mots vides pour un index de recherche en texte intégral sur une table donnée, utilisez le [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) affichage catalogue.  
  
 *accent_sensitivity*  
 Valeur booléenne qui indique si la recherche en texte intégral respecte ou non les signes diacritiques. *accent_sensitivity* est **bits**, avec l’une des valeurs suivantes :  
  
|Valeur|Respecte les accents ?|  
|-----------|----------------------------|  
|0|Non-respect de la<br /><br /> Les mots tels que « café » et « cafe » sont traités de la même manière.|  
|1|Sensible<br /><br /> Les mots tels que « café » et « cafe » ne sont pas traités de la même manière.|  
  
> [!NOTE]  
>  Pour afficher le paramètre actuel de cette valeur pour un catalogue de texte intégral, exécutez la commande suivante [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction : `SELECT fulltextcatalogproperty('` *catalog_name*`', 'AccentSensitivity');`.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|mot clé|**varbinary(128)**|Représentation hexadécimale d'un mot clé donné retournée par un analyseur lexical. Cette représentation permet de stocker le mot clé dans l'index de recherche en texte intégral. Cette valeur n’est pas lisible, mais il est utile pour un mot clé donné de sortie retournées par les autres vues de gestion dynamique qui retournent le contenu d’un index de recherche en texte intégral, tel que [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md) et [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md).<br /><br /> **Remarque :** OxFF représente le caractère spécial qui indique la fin d’un fichier ou un jeu de données.|  
|group_id|**int**|Contient une valeur entière qui est utile pour différencier le groupe logique à partir duquel un terme donné a été généré. Par exemple, « `Server AND DB OR FORMSOF(THESAURUS, DB)"` » produit les valeurs group_id suivantes en anglais :<br /><br /> 1 : serveur<br />2 : BASE DE DONNÉES<br />3 : BASE DE DONNÉES|  
|phrase_id|**int**|Contient une valeur entière qui est utile pour différencier les cas dans lesquels les formes alternatives de mots composés, tels que le texte intégral, sont émises par l'analyseur lexical. Il peut arriver qu'en présence de mots composés (« multi-million ») des formes alternatives soient émises par l'analyseur lexical. Ces formes alternatives (expressions) doivent parfois être différenciées.<br /><br /> Par exemple, « `multi-million` » produit les valeurs phrase_id suivantes en anglais :<br /><br /> 1 pour `multi`<br />1 pour `million`<br />2 pour `multimillion`|  
|occurrence|**int**|Indique l'ordre de chaque terme dans le résultat de l'analyse. Par exemple, pour l'expression « `SQL Server query processor` », l'occurrence contiendrait les valeurs d'occurrence suivantes pour les termes de l'expression, en anglais :<br /><br /> 1 pour `SQL`<br />2 pour `Server`<br />3 pour `query`<br />4 pour `processor`|  
|special_term|**nvarchar(4000)**|Contient des informations sur les caractéristiques du terme émis par l'analyseur lexical, informations qui peuvent être l'une des suivantes :<br /><br /> Concordance exacte<br /><br /> Mot parasite<br /><br /> Fin de phrase<br /><br /> Fin de paragraphe<br /><br /> Fin de chapitre|  
|display_term|**nvarchar(4000)**|Contient la forme explicite du mot clé. Comme avec les fonctions conçues pour accéder au contenu de l'index de recherche en texte intégral, ce terme affiché peut ne pas être identique au terme d'origine en raison des limitations inhérentes à la dénormalisation. Toutefois, il doit être suffisamment précis pour vous permettre de l'identifier à partir de l'entrée d'origine.|  
|expansion_type|**int**|Contient des informations sur la nature de l'expansion d'un terme donné, informations qui peuvent être l'une des suivantes :<br /><br /> 0 =Cas de mot unique<br /><br /> 2=Expansion fléchie<br /><br /> 4=Expansion/remplacement du dictionnaire des synonymes<br /><br /> Par exemple, considérez un cas dans lequel le dictionnaire des synonymes définit run comme expansion de `jog` :<br /><br /> `<expansion>`<br /><br /> `<sub>run</sub>`<br /><br /> `<sub>jog</sub>`<br /><br /> `</expansion>`<br /><br /> Le terme `FORMSOF (FREETEXT, run)` génère la sortie suivante :<br /><br /> `run` with expansion_type=0<br /><br /> `runs` with expansion_type=2<br /><br /> `running` with expansion_type=2<br /><br /> `ran` with expansion_type=2<br /><br /> `jog` with expansion_type=4|  
|source_term|**nvarchar(4000)**|Terme ou expression à partir duquel un terme donné à été généré ou analysé. Par exemple, une requête sur '"`word breakers" AND stemmers'` produit les valeurs source_term suivantes en anglais :<br /><br /> `word breakers` pour le display_term`word`<br />`word breakers` pour le display_term`breakers`<br />`stemmers` pour le display_term`stemmers`|  
  
## <a name="remarks"></a>Notes  
 **Sys.dm_fts_parser** prend en charge la syntaxe et les fonctionnalités de prédicats de texte intégral, tel que [contient](../../t-sql/queries/contains-transact-sql.md) et [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)et les fonctions, telles que [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md)et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="using-unicode-for-parsing-special-characters"></a>Utilisation d'Unicode pour l'analyse des caractères spéciaux  
 Lorsque vous analysez une chaîne de requête **sys.dm_fts_parser** utilise le classement de la base de données à laquelle vous êtes connecté, sauf si vous spécifiez la chaîne de requête au format Unicode. Par conséquent, une chaîne non-Unicode qui contient des caractères spéciaux, tels qu’ü ou ç, la sortie peut être inattendue, selon le classement de la base de données. Pour traiter une chaîne de requête indépendamment du classement de base de données, préfixez la chaîne avec `N`, autrement dit, `N'` *query_string*`'`.  
  
 Pour plus d'informations, consultez « C. Affichage de la sortie d'une chaîne qui contient des caractères spéciaux », plus loin dans cette rubrique.  
  
## <a name="when-to-use-sysdmftsparser"></a>Quand utiliser sys.dm_fts_parser  
 **Sys.dm_fts_parser** peuvent être très puissants pour le débogage. Certaines utilisations parmi les plus fréquents sont décrites ci-dessous.  
  
-   Pour comprendre comment un analyseur lexical donné traite une entrée donnée  
  
     Lorsqu'une requête retourne des résultats inattendus, une cause probable est la manière dont l'analyseur lexical analyse et décompose les données. En utilisant sys.dm_fts_parser, vous découvrez le résultat qu'un analyseur lexical passe à l'index de recherche en texte intégral. En outre, vous pouvez voir quels sont les termes utilisés en tant que mots vides, qui ne sont pas recherchés dans l'index de recherche en texte intégral. Si un terme est un mot vide pour une langue donnée varie selon qu’il figure dans la liste de mots vides spécifiée par le *stoplist_id* valeur qui est déclarée dans la fonction.  
  
     Notez également l'indicateur de respect des accents, qui permet à l'utilisateur de voir comment l'analyseur lexical analyse l'entrée en ayant à l'esprit les informations de respect des accents.  
  
-   Pour comprendre le fonctionnement du générateur de formes dérivées sur une entrée donnée  
  
     Vous pouvez déterminer comment l'analyseur lexical et le générateur de formes dérivées analysent un terme de requête et ses formes dérivées en spécifiant une requête CONTAINS ou CONTAINSTABLE qui contient la clause FORMSOF suivante :  
  
    ```  
    FORMSOF( INFLECTIONAL, query_term )  
    ```  
  
     Les résultats vous indiquent les termes qui sont passés à l'index de recherche en texte intégral.  
  
-   Pour comprendre comment le dictionnaire des synonymes étend ou remplace tout ou partie de l'entrée  
  
     Vous pouvez également spécifier :  
  
    ```  
    FORMSOF( THESAURUS, query_term )  
    ```  
  
     Les résultats de cette requête montrent comment l'analyseur lexical et le dictionnaire des synonymes interagissent pour le terme de requête. Vous pouvez voir l'expansion ou les remplacements du dictionnaire des synonymes et identifier la requête obtenue et effectivement émise sur l'index de recherche en texte intégral.  
  
     Notez que si l'utilisateur émet :  
  
    ```  
    FORMSOF( FREETEXT, query_term )  
    ```  
  
     les fonctionnalités des formes fléchies et du dictionnaire des synonymes opèrent automatiquement.  
  
 En plus des scénarios d'utilisation précédents, sys.dm_fts_parser peut être d'une aide précieuse pour comprendre et résoudre nombre d'autres problèmes liés à la requête de texte intégral.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **sysadmin** fixe des droits de rôle et l’accès du serveur à la liste de mots vides spécifiée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-the-output-of-a-given-word-breaker-for-a-keyword-or-phrase"></a>A. Affichage de la sortie d'un analyseur lexical donné pour un mot clé ou une expression  
 L'exemple suivant retourne la sortie produite par l'analyseur lexical anglais, dont le LCID est 1033, utilisé sans liste de mots vides sur la chaîne de requête suivante :  
  
 `The Microsoft business analysis`  
  
 Le respect des accents est désactivé.  
  
```  
SELECT * FROM sys.dm_fts_parser (' "The Microsoft business analysis" ', 1033, 0, 0);  
```  
  
### <a name="b-displaying-the-output-of-a-given-word-breaker-in-the-context-of-stoplist-filtering"></a>B. Affichage de la sortie d'un analyseur lexical donné dans le contexte de filtrage de liste de mots vides  
 L'exemple suivant retourne la sortie produite par l'analyseur lexical anglais, dont le LCID est 1033, utilisé avec une liste de mots vides anglaise, dont l'ID est 77, sur la chaîne de requête suivante :  
  
 `"The Microsoft business analysis" OR "MS revenue"`  
  
 Le respect des accents est désactivé.  
  
```  
SELECT * FROM sys.dm_fts_parser (' "The Microsoft business analysis"  OR " MS revenue" ', 1033, 77, 0);  
```  
  
### <a name="c-displaying-the-output-of-a-string-that-contains-special-characters"></a>C. Affichage de la sortie d'une chaîne qui contient des caractères spéciaux  
 L'exemple suivant utilise Unicode pour analyser la chaîne suivante :  
  
 `français`  
  
 L'exemple spécifie le LCID pour la langue française, `1036` et l'ID d'une liste de mots vides définie par l'utilisateur, `5` Le respect des accents est activé.  
  
```  
SELECT * FROM sys.dm_fts_parser(N'français', 1036, 5, 1);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurer et gérer des fichiers de dictionnaire des synonymes pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurer et gérer des mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [Éléments sécurisables](../../relational-databases/security/securables.md)  
  
  
