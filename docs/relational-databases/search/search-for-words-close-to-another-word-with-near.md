---
title: Recherche de mots dans le voisinage d’autres mots avec NEAR | Microsoft Docs
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
dev_langs:
- TSQL
helpviewer_keywords:
- word searches [full-text search]
- NEAR option [full-text search]
- phrase searches [full-text search]
- proximity searches [full-text search]
- full-text search [SQL Server], proximity searches
- full-text queries [SQL Server], proximity
- queries [full-text search], proximity
ms.assetid: 87520646-4865-49ae-8790-f766b80a41f3
caps.latest.revision: 65
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6259d7685ef0e06855325c6903daa204ee83fbf2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="search-for-words-close-to-another-word-with-near"></a>Recherche de mots dans le voisinage d'autres mots avec NEAR
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous pouvez utiliser le *terme de proximité* **NEAR** dans un prédicat [CONTAINS](../../t-sql/queries/contains-transact-sql.md) ou une fonction [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) pour rechercher des mots ou des expressions à proximité les uns des autres. 
  
##  <a name="Custom_NEAR"></a> Vue d’ensemble de NEAR  
**NEAR** offre les fonctionnalités suivantes :  
-   Vous pouvez spécifier le nombre maximal de termes de non-recherche qui séparent le premier et le dernier terme de recherche.

-   Vous pouvez rechercher des mots ou des expressions dans n’importe quel ordre ou dans un ordre spécifié.
  
-   Vous pouvez spécifier le nombre maximal de termes de non-recherche, ou *distance maximale*, qui sépare le premier et le dernier terme de recherche pour établir une correspondance.  

-   Si vous spécifiez le nombre maximal de termes, vous pouvez également spécifier l'ordre dans lequel les termes de recherche doivent figurer dans les correspondances.

 
 Pour être une correspondance valide, une chaîne de texte doit :  
  
-   commencer par l'un des termes de recherche spécifiés et se terminer par l'un des autres termes de recherche spécifiés ;  
  
-   Contenir tous les termes de recherche spécifiés  
  
-   Le nombre de termes de non-recherche, notamment de mots vides situés entre le premier et le dernier terme de recherche, doit être inférieur ou égal à la distance maximale, si celle-ci est spécifiée.  
  
## <a name="syntax-of-near"></a>Syntaxe de NEAR
La syntaxe de base de **NEAR** est la suivante :  

``` 
 NEAR (  
  
 {  
  
 *search_term* [ ,…*n* ]  
  
 |  
  
 (*search_term* [ ,…*n* ] ) [, <maximum_distance> [, <match_order> ] ]  
  
 }  
  
 )  
```

Pour plus d’informations sur la syntaxe, consultez [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md).  

## <a name="examples"></a>Exemples
### <a name="example-1"></a>Exemple 1
 Par exemple, vous pourriez rechercher « John » à deux termes de distance de « Smith », comme suit :  
  
```sql
... CONTAINS(column_name, 'NEAR((John, Smith), 2)')
```  
  
 Les chaînes qui correspondent sont par exemple «`John Jacob Smith`» et «`Smith, John`». La chaîne «`John Jones knows Fred Smith`» contient trois termes de non-recherche intermédiaires, donc ce n'est pas une correspondance.  
  
 Pour que les termes soient recherchés dans un ordre spécifié, vous pouvez modifier le terme de proximité de l'exemple en `NEAR((John, Smith),2, TRUE).` Cela permet de rechercher «`John`» à deux termes de distance de «`Smith`», mais uniquement à condition que «`John`» précède «`Smith`». Dans une langue qui se lit de gauche à droite, tel que l'anglais, un exemple de chaîne correspondante est`John Jacob Smith`.  
  
 Notez que pour une langue qui se lit de droite à gauche, telle que l'arabe ou l'hébreu, le moteur d'indexation et de recherche en texte intégral applique les termes spécifiés dans l'ordre inverse. De même, l'Explorateur d'objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] inverse automatiquement l'ordre d'affichage des mots spécifiés dans les langues s'écrivant de droite à gauche.   

### <a name="example-2"></a>Exemple 2
 L'exemple suivant recherche dans la table `Production.Document` de l'exemple de base de données `AdventureWorks` tous les résumés de document qui contiennent le mot « reflector » dans le même document que le mot « bracket ».  
  
```sql
SELECT DocumentNode, Title, DocumentSummary  
FROM Production.Document AS DocTable   
INNER JOIN CONTAINSTABLE(Production.Document, Document,  
  'NEAR(bracket, reflector)' ) AS KEY_TBL  
  ON DocTable.DocumentNode = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 50  
ORDER BY KEY_TBL.RANK DESC;  
GO  
``` 
 
## <a name="how-maximum-distance-is-measured"></a>Comment mesurer la distance maximale  
 Une distance maximale spécifique, telle que 10 ou 25, détermine combien de termes de non-recherche, notamment les mots vides, peuvent séparer le premier et le dernier terme de recherche dans une chaîne donnée. Par exemple, `NEAR((dogs, cats, "hunting mice"), 3)` retournerait la ligne suivante, dans laquelle le nombre total de termes de non-recherche est trois («`enjoy`», «`but`» et «`avoid`») :  
  
 «`Cats` `enjoy` `hunting mice``, but avoid` `dogs``.`»  
  
 Le même terme de proximité ne retournerait pas la ligne suivante, car les quatre termes de non-recherche («`enjoy`», «`but`», «`usually`» et «`avoid`») dépassent la distance maximale :  
  
 «`Cats` `enjoy` `hunting mice``, but usually avoid` `dogs``.`»  
  
## <a name="combine-near-with-other-terms"></a>Combiner NEAR avec d’autres termes  
 Vous pouvez combiner NEAR avec d’autres termes. Vous pouvez utiliser AND (&), OR (|) ou AND NOT (&!) pour combiner un terme de proximité personnalisé avec un autre terme de proximité personnalisé, un terme simple ou un terme de préfixe. Exemple :  
  
-   CONTAINS('NEAR((*terme1*, *terme2*),5) AND *terme3*')  
  
-   CONTAINS('NEAR((*terme1*, *terme2*),5) OR *terme3*')  
  
-   CONTAINS('NEAR((*terme1*, *terme2*),5) AND NOT *terme3*')  
  
-   CONTAINS('NEAR((*terme1*, *terme2*),5) AND NEAR((*terme3*, *terme4*),2)')  
  
-   CONTAINS('NEAR((*terme1*, *terme2*),5) OR NEAR((*terme3*, *terme4*),2, TRUE)')  
  
 Par exemple,  
  
```  
CONTAINS(column_name, 'NEAR((term1, term2), 5, TRUE) AND term3')  
```  
  
 Vous ne pouvez pas combiner NEAR avec un terme de génération (ISABOUT …) ou un terme pondéré (FORMSOF …).  
  
##  <a name="Additional_Considerations"></a> Plus d’informations sur les recherches de proximité  
   
-   Chevauchement des occurrences des termes de recherche  
  
     Les recherches de proximité ne cherchent toujours que des occurrences sans chevauchement. Les occurrences des termes de recherche qui se chevauchent ne sont jamais validées comme des correspondances. Par exemple, supposons le terme de proximité suivant, qui recherche «`A`» et «`AA`», dans cet ordre, à une distance maximale de deux termes :  
  
    ```  
    CONTAINS(column_name, 'NEAR((A,AA),2, TRUE')  
    ```  
  
     Les correspondances possibles sont, par exemple, «`AAA`», «`A.AA`» et «`A..AA`». Les lignes contenant simplement «`AA`» ne correspondent pas.  
  
    > [!NOTE]  
    >  Vous pouvez spécifier des termes qui se chevauchent, par exemple, `NEAR("mountain bike", "bike trails")` ou `(NEAR(comfort*, comfortable), 5)`. Spécifier des termes qui se chevauchent rend la requête plus complexe en augmentant les permutations de correspondances possibles. Si vous spécifiez un grand nombre de termes se chevauchant, la requête peut échouer en raison de ressources insuffisantes. Dans ce cas, simplifiez la requête et réessayez.  
  
-   Qu’une distance maximale soit spécifiée ou non, NEAR indique la distance logique entre les termes plutôt que la distance absolue qui les sépare. Par exemple, les termes des différentes expressions ou phrases d'un paragraphe sont considérés comme plus éloignés que les termes situés dans une même expression ou phrase, indépendamment de leur proximité réelle, en supposant que leur relation soit moins étroite. De même, les termes situés dans des paragraphes différents sont considérés comme étant encore plus éloignés. Si une correspondance comprend la fin d'une phrase, d'un paragraphe ou d'un chapitre, l'intervalle utilisé pour le classement d'un document est augmenté respectivement de 8, 128 ou 1024.  
  
-   Impact des termes de proximité sur le classement par la fonction CONTAINSTABLE  
  
    Lorsque NEAR est utilisé dans la fonction CONTAINSTABLE, le nombre de résultats dans un document par rapport à sa longueur, ainsi que la distance entre le premier et le dernier terme de recherche dans chacun des résultats, affectent le classement de chaque document. Pour un terme de proximité générique, si les termes de recherche trouvés sont séparés de >50 termes logiques, le classement retourné sur un document est 0. Pour un terme de proximité personnalisé qui ne spécifie pas d'entier comme distance maximale, un document qui contient uniquement des résultats à intervalle de >100 termes logiques recevra un classement de 0. Pour plus d'informations sur le classement des recherches de proximité personnalisées, consultez [Limit Search Results with RANK](../../relational-databases/search/limit-search-results-with-rank.md).  
  
-   Option de serveur **Transformer les mots parasites**   
  
     La valeur de l’option **Transformer les mots parasites** influe sur la manière dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite les mots vides, s’ils sont spécifiés dans les recherches de proximité. Pour plus d’informations, voir [transform noise words Server Configuration Option](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).   
  
## <a name="see-also"></a> Voir aussi  
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
