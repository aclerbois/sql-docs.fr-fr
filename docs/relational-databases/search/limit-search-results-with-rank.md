---
title: Limiter les résultats de la recherche avec RANK | Microsoft Docs
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
- row ranking [full-text search]
- relevance ranking values [full-text search]
- full-text search [SQL Server], rankings
- index rankings [full-text search]
- ranked results [full-text search]
- rankings [full-text search]
- per-row rank values [full-text search]
ms.assetid: 06a776e6-296c-4ec7-9fa5-0794709ccb17
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2ee823af2b6d1c6c9345efc022e138feedd1f74c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="limit-search-results-with-rank"></a>Limiter les résultats de la recherche avec RANK
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Les fonctions [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) retournent une colonne appelée RANK qui contient des valeurs ordinales comprises entre 0 et 1000 (valeurs de classement). Ces valeurs servent à établir le rang des lignes retournées en fonction de leur correspondance par rapport aux critères de sélection. Les valeurs de classement indiquent uniquement un ordre relatif de pertinence pour les lignes du jeu de résultats. Une valeur inférieure indique une pertinence plus faible. Les valeurs réelles sont sans importance et sont généralement différentes d'une exécution de requête à une autre.  
  
> [!NOTE]  
>  Les prédicats CONTAINS et FREETEXT ne retournent aucune valeur de rang.  
  
 Le nombre d'éléments répondant à une condition de recherche est souvent très élevé. Pour éviter que les requêtes CONTAINSTABLE ou FREETEXTTABLE ne retournent un trop grand nombre de correspondances, utilisez le paramètre facultatif *top_n_by_rank* , qui retourne uniquement un sous-ensemble de lignes. *top_n_by_rank* est une valeur entière, *n*, qui spécifie que seules les *n* correspondances de classement le plus élevé doivent être retournées, par ordre décroissant. Si *top_n_by_rank* est associé à d’autres paramètres, la requête peut retourner moins de lignes que le nombre de lignes correspondant effectivement à tous les prédicats.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] classe les correspondances par rang et ne retourne au maximum que le nombre de lignes spécifié. Ce choix peut considérablement améliorer les performances. Par exemple, une requête qui doit normalement retourner 100 000 lignes d'une table en comprenant 1 million est traitée plus rapidement si seules les 100 premières lignes sont demandées.  
  
##  <a name="examples"></a> Exemples d'utilisation de RANK pour limiter les résultats de la recherche  
  
### <a name="example-a-searching-for-only-the-top-three-matches"></a>Exemple A : recherche des trois premières correspondances uniquement  
 L'exemple suivant utilise CONTAINSTABLE pour retourner uniquement les trois premières correspondances.  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT K.RANK, AddressLine1, City  
FROM Person.Address AS A  
  INNER JOIN  
  CONTAINSTABLE(Person.Address, AddressLine1, 'ISABOUT ("des*",  
    Rue WEIGHT(0.5),  
    Bouchers WEIGHT(0.9))',  
    3) AS K  
  ON A.AddressID = K.[KEY]  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
RANK        Address                          City  
----------- -------------------------------- ------------------------------  
172         9005, rue des Bouchers           Paris  
172         5, rue des Bouchers              Orleans  
172         5, rue des Bouchers              Metz  
  
(3 row(s) affected)  
```  
  
  
### <a name="example-b-searching-for-the-top-ten-matches"></a>Exemple B : recherche des dix premières correspondances  
 L'exemple suivant utilise CONTAINSTABLE pour retourner la description des 5 premiers produits dont la colonne `Description` contient les mots « aluminum » à proximité du mot « light » ou « lightweight ».  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
GO  
```  
  
  
##  <a name="how"></a> Classement des résultats d'une requête de recherche  
 Une recherche en texte intégral dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut générer un score facultatif (ou valeur de classement) qui indique la pertinence des données retournées par une requête de texte intégral. Cette valeur de classement est calculée sur chaque ligne et peut être utilisée comme critère de tri pour trier le jeu de résultats d'une requête donnée par pertinence. Les valeurs de classement indiquent uniquement un ordre relatif de pertinence pour les lignes contenues dans le jeu de résultats. Les valeurs réelles sont sans importance et sont généralement différentes d'une exécution de requête à une autre. La valeur de classement n'a pas de signification entre les requêtes.  
  
### <a name="statistics-for-ranking"></a>Statistiques de classement  
 Lors de la création d'un index, des statistiques sont recueillies à des fins de classement. Le processus de création d'un catalogue de texte intégral n'aboutit pas directement à une structure d'index unique. À la place, le Moteur d'indexation et de recherche en texte intégral Microsoft pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des index intermédiaires au fur et à mesure de l'indexation des données. Il fusionne ensuite ces index dans un index de plus grande taille en fonction de vos besoins. Ce processus peut se répéter à plusieurs reprises. Le Moteur d'indexation et de recherche en texte intégral mène ensuite une « fusion principale » qui associe tous les index intermédiaires dans un index principal de plus grande taille.  
  
 Des statistiques sont recueillies à chaque niveau d'index intermédiaire. Elles fusionnent en même temps que les index. Certaines valeurs statistiques ne peuvent être générées que durant la fusion principale.  
  
 Lorsqu'il classe le jeu de résultats d'une requête, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise des statistiques provenant du plus grand index intermédiaire. Cela dépend de la fusion ou non des index intermédiaires. Par conséquent, les statistiques de classement peuvent varier en précision si les index intermédiaires n'ont pas été fusionnés. C'est la raison pour laquelle la même requête peut retourner différents résultats dans le temps en fonction de l'ajout, de la modification et de la suppression des données indexées en texte intégral, et en fonction de la fusion des index plus petits.  
  
 Pour réduire la taille des index et la complexité des calculs, les statistiques sont souvent arrondies.  
  
 La liste ci-dessous comprend des termes et des valeurs statistiques fréquemment utilisés, qui sont d'une grande importance dans le calcul du classement.  
  
 Propriété  
 Colonne indexée en texte intégral de la ligne.  
  
 Document  
 Entité retournée dans les requêtes. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cela correspond à une ligne. Un document peut avoir plusieurs propriétés, de même qu'une ligne peut avoir plusieurs colonnes indexées en texte intégral.  
  
 Index  
 Index inversé unique d'un ou de plusieurs documents. Il peut se trouver entièrement en mémoire ou sur disque. De nombreuses statistiques de requêtes sont relatives à l'index individuel où la correspondance s'est produite.  
  
 Catalogue de texte intégral  
 Collection d'index intermédiaires traités en tant qu'entité unique pour les requêtes. Les catalogues sont l'unité d'organisation visible par l'administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Mot, jeton ou élément  
 Unité de correspondance dans le moteur de texte intégral. Les flux de texte des documents sont convertis en mots ; ils peuvent également être convertis en jetons par des analyseurs lexicaux spécifiques aux langues.  
  
 Occurrence  
 Décalage de mot dans une propriété de document conformément à la définition établie par l'analyseur lexical. Le premier mot se trouve à l'occurrence 1, le suivant se trouve à l'occurrence 2, etc. Pour éviter les affirmations incorrectes dans les requêtes d'expression et de proximité, les fins de phrases et les fins de paragraphes introduisent des écarts d'occurrence plus importants.  
  
 TermFrequency  
 Nombre d'occurrences de la valeur de clé dans une ligne.  
  
 IndexedRowCount  
 Nombre total de lignes indexées. Cette valeur est calculée à partir des nombres conservés dans les index intermédiaires. La précision du résultat peut varier.  
  
 KeyRowCount  
 Nombre total de lignes du catalogue de texte intégral contenant une clé donnée.  
  
 MaxOccurrence  
 Occurrence la plus importante stockée dans un catalogue de texte intégral pour une propriété donnée dans une ligne.  
  
 MaxQueryRank  
 Rang maximal, 1000, retourné par le Moteur d'indexation et de recherche en texte intégral.  
  
  
### <a name="rank-computation-issues"></a>Problèmes de calcul de rang  
 Le processus de calcul du rang dépend d'un certain nombre de facteurs.  Les analyseurs lexicaux des diverses langues créent des jetons de texte de manière différente. Par exemple, la chaîne « après-midi » peut être décomposée en « après » « midi » par un analyseur lexical et en « après-midi » par un autre. En d'autres termes, la correspondance et le classement varient en fonction de la langue spécifiée, car les mots sont différents et la longueur du document l'est également. La différence de longueur d'un document peut affecter le classement pour toutes les requêtes.  
  
 Les statistiques telles que **IndexRowCount** peuvent fortement varier. Par exemple, si un catalogue a 2 milliards de lignes dans l'index principal, un nouveau document est indexé dans un index intermédiaire en mémoire ; par ailleurs, les rangs de ce document qui sont basés sur le nombre de documents dans l'index en mémoire peuvent être incorrects par rapport aux rangs des documents de l'index principal. Par conséquent, lorsqu'un remplissage entraîne l'indexation ou la réindexation d'un grand nombre de lignes, il est recommandé de fusionner les index dans un index principal via l'instruction ALTER FULLTEXT CATALOG ... Instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] REORGANIZE. Le Moteur d'indexation et de recherche en texte intégral fusionne automatiquement les index en fonction de paramètres tels que le nombre et la taille des index intermédiaires.  
  
 Les valeurs**MaxOccurrence** sont normalisées sous forme de 32 plages individuelles. Par exemple, un document de 50 mots est traité de la même façon qu'un document de 100 mots. Vous trouverez ci-dessous le tableau de normalisation utilisé. Les documents ayant une longueur comprise dans la plage située entre les valeurs adjacentes 32 et 128 du tableau, ils sont effectivement traités comme s’ils avaient le même nombre de mots, c’est-à-dire 128 (32 < **docLength** <= 128).  
  
```  
{ 16, 32, 128, 256, 512, 725, 1024, 1450, 2048, 2896, 4096, 5792, 8192, 11585,   
16384, 23170, 28000, 32768, 39554, 46340, 55938, 65536, 92681, 131072, 185363,   
262144, 370727, 524288, 741455, 1048576, 2097152, 4194304 };  
  
```  
  
  
### <a name="ranking-of-containstable"></a>Classement de CONTAINSTABLE  
 Le classement de[CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) utilise l’algorithme suivant :  
  
```  
StatisticalWeight = Log2( ( 2 + IndexedRowCount ) / KeyRowCount )  
Rank = min( MaxQueryRank, HitCount * 16 * StatisticalWeight / MaxOccurrence )  
```  
  
 Les correspondances d’expressions sont classées comme des clés individuelles. Toutefois, **KeyRowCount** (nombre de lignes contenant l’expression) étant estimé, il peut être inexact et supérieur au nombre réel.  
  
 **Classement de NEAR**  
  
 CONTAINSTABLE prend en charge l'interrogation de plusieurs termes de recherche à proximité l'un de l'autre à l'aide de l'option NEAR. La valeur de classement de chaque ligne retournée est basée sur plusieurs paramètres. Un facteur majeur du classement est le nombre total de correspondances (ou *résultats*) par rapport à la longueur du document. Par exemple, si un document de 100 mots et un document de 900 mots contiennent des correspondances identiques, le document de 100 mots a un classement supérieur.  
  
 La longueur totale de chaque résultat dans une ligne contribue également au classement de cette ligne en fonction de la distance entre le premier et le dernier terme de recherche de ce résultat. Plus la distance est faible, plus le résultat contribue à la valeur du classement de la ligne. Si une requête de texte intégral ne spécifie pas d'entier comme distance maximale, un document qui contient uniquement des résultats séparés de distances supérieures à 100 termes logiques, aura un classement de 0.  
  
 **Classement de ISABOUT**  
  
 CONTAINSTABLE prend en charge les requêtes de recherche de termes pondérés à l'aide de l'option ISABOUT. ISABOUT est une requête d'espace vectoriel dans la terminologie traditionnelle de récupération d'informations. L'algorithme de classement par défaut utilisé est celui de Jaccard, une formule très connue. Le classement est calculé pour chaque nouveau terme de la requête et est ensuite combiné comme indiqué ci-dessous.  
  
```  
ContainsRank = same formula used for CONTAINSTABLE ranking of a single term (above).  
Weight = the weight specified in the query for each term. Default weight is 1.  
WeightedSum = Σ[key=1 to n] ContainsRankKey * WeightKey  
Rank =  ( MaxQueryRank * WeightedSum ) / ( ( Σ[key=1 to n] ContainsRankKey^2 )   
      + ( Σ[key=1 to n] WeightKey^2 ) - ( WeightedSum ) )  
  
```  
  
  
### <a name="ranking-of-freetexttable"></a>Classement de FREETEXTTABLE  
 Le classement de[FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) est basé sur la formule de classement OKAPI BM25. Les requêtes FREETEXTTABLE ajoutent des mots à la requête via une génération flexionnelle (formes flexionnelles des mots de la requête d'origine) ; ces mots sont traités comme des mots distincts, sans relation particulière avec les mots à partir desquels ils ont été générés. Les synonymes générés à partir de la fonctionnalité du dictionnaire des synonymes sont traités comme des termes distincts, de même pondération. Chaque mot de la requête contribue au rang.  
  
```  
Rank = Σ[Terms in Query] w ( ( ( k1 + 1 ) tf ) / ( K + tf ) ) * ( ( k3 + 1 ) qtf / ( k3 + qtf ) ) )  
Where:   
w is the Robertson-Sparck Jones weight.   
In simplified form, w is defined as:   
w = log10 ( ( ( r + 0.5 ) * ( N – R + r + 0.5 ) ) / ( ( R – r + 0.5 ) * ( n – r + 0.5 ) )  
N is the number of indexed rows for the property being queried.   
n is the number of rows containing the word.   
K is ( k1 * ( ( 1 – b ) + ( b * dl / avdl ) ) ).   
dl is the property length, in word occurrences.   
avdl is the average length of the property being queried, in word occurrences.   
k1, b, and k3 are the constants 1.2, 0.75, and 8.0, respectively.   
tf is the frequency of the word in the queried property in a specific row.   
qtf is the frequency of the term in the query.   
```  
  
  
## <a name="see-also"></a> Voir aussi  
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)  
  
  
