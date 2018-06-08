---
title: Spécifier les chemins d’accès et les indicateurs d’optimisation des index XML sélectifs | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 486ee339-165b-4aeb-b760-d2ba023d7d0a
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 18340956a0378a20a4ff5f9c92a47a477d5b3683
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-paths-and-optimization-hints-for-selective-xml-indexes"></a>Spécifier les chemins d'accès et les indicateurs d'optimisation des index XML sélectifs
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment spécifier les chemins d'accès de nœud à indexer et les indicateurs d'optimisation pour l'indexation lorsque vous créez ou modifiez des index XML sélectifs.  
  
 Vous spécifiez les chemins d'accès de nœud et les indicateurs d'optimisation en même temps dans l'une des instructions suivantes :  
  
-   Dans la clause **FOR** d’une instruction **CREATE**. Pour plus d’informations, consultez [CREATE SELECTIVE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
-   Dans la clause **ADD** d’une instruction **ALTER**. Pour plus d’informations, consultez [ALTER INDEX &#40;index XML sélectifs&#41;](../../t-sql/statements/alter-index-selective-xml-indexes.md).  
  
 Pour plus d’informations sur les index XML sélectifs, consultez [Index XML sélectifs &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
##  <a name="untyped"></a> Présentation des types XQuery et SQL Server en XML non typé  
 Les index XML sélectifs prennent en charge deux systèmes de types : les types XQuery et les types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le chemin d'accès indexé peut être utilisé pour correspondre à une expression XQuery, ou pour correspondre au type de retour de la méthode value() de type de données XML.  
  
-   Lorsqu'un chemin d'accès à indexer n'est pas annoté, ou est annoté avec le mot clé XQUERY, il correspond à une expression XQuery. Il existe deux variantes des chemins d'accès de nœud annotés XQUERY :  
  
    -   Si vous ne spécifiez pas le mot clé XQUERY et le type de données XQuery, des mappages par défaut sont utilisés. En général, les performances et le stockage ne sont pas optimaux.  
  
    -   Si vous spécifiez le mot clé XQUERY et le type de données XQuery, et éventuellement d'autres indicateurs d'optimisation, vous pouvez obtenir de meilleures performances et le stockage le plus efficace possible. Toutefois, une conversion peut échouer.  
  
-   Lorsqu'un chemin d'accès à indexer est annoté avec le mot clé SQL, le chemin d'accès correspond au type de retour de la méthode value() de type de données XML. Spécifiez le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] approprié, qui est le type de retour attendue de la méthode value().  
  
 Il existe de légères variantes entre le système de type XML d'expressions XQuery et le système de type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appliqués à la méthode value() de type de données XML. Ces différences sont les suivantes :  
  
-   Le système de type XQuery tient compte des espaces à droite. Par exemple, en fonction de la sémantique de type XQuery, les chaînes "abc" et "abc " sont différentes, alors que dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ces chaînes sont équivalentes.  
  
-   Les types de données à virgule flottante XQuery prennent en charge les valeurs spéciales +/- zéro et +/- infini. Ces valeurs spéciales ne sont pas prises en charge dans les types de données à virgule flottante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="xquery-types-in-untyped-xml"></a>Types XQuery en XML non typé  
  
-   Les types XQuery correspondent aux expressions XQuery dans toutes les méthodes de type de données XML incluant la méthode value().  
  
-   Les types XQuery prennent en charge ces indicateurs d'optimisation : node(), SINGLETON, DATA TYPE et MAXLENGTH.  
  
 Pour les expressions XQuery sur le XML non typé, vous pouvez choisir entre deux modes d'opération :  
  
-   **Mode de mappage par défaut**. Dans ce mode, vous spécifiez uniquement le chemin d'accès lors de la création d'un index XML sélectif.  
  
-   **Mode de mappage défini par l’utilisateur**. Dans ce mode, vous spécifiez le chemin d'accès et les indicateurs facultatifs d'optimisation.  
  
 Le mode de mappage par défaut utilise une option de stockage pessimiste qui est toujours sécurisée et générale. Il peut correspondre à n'importe quel type d'expression. Une limitation du mode de mappage par défaut est inférieure aux performances optimales, car un nombre accru de conversions d'exécution sont requis, et les index secondaires sont pas disponibles.  
  
 Voici un exemple d'index XML sélectif créé avec les mappages par défaut. Pour les trois chemins d’accès, le type de nœud par défaut (**xs:untypedAtomic**) et la cardinalité sont utilisés.  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_UX_default  
ON Tbl(xmlcol)  
FOR  
(  
mypath01 =  '/a/b',  
mypath02 = '/a/b/c',  
mypath03 = '/a/b/d'  
)  
```  
  
 Le mode de mappage défini par l'utilisateur vous permet de spécifier un type et une cardinalité du nœud pour obtenir de meilleures performances. Toutefois, ces performances accrues sont obtenues au détriment de la sécurité, car une conversion peut échouer et en général seul le type spécifié est mis en correspondance avec l'index XML sélectif.  
  
 Les types XQuery pris en charge pour le XML non typé sont les suivants :  
  
-   **xs:boolean**  
  
-   **xs:double**  
  
-   **xs:string**  
  
-   **xs:date**  
  
-   **xs:time**  
  
-   **xs:dateTime**  
  
 Si le type n’est pas spécifié, le nœud est du type de données **xs:untypedAtomic** par défaut.  
  
 Vous pouvez optimiser l'index XML sélectif affiché de la manière suivante :  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_UX_optimized  
ON Tbl(xmlcol)  
FOR  
(  
mypath= '/a/b' as XQUERY 'node()',  
pathX = '/a/b/c' as XQUERY 'xs:double' SINGLETON,  
pathY = '/a/b/d' as XQUERY 'xs:string' MAXLENGTH(200) SINGLETON  
)  
-- mypath – Only the node value is needed; storage is saved.  
-- pathX – Performance is improved; secondary indexes are possible.  
-- pathY - Performance is improved; secondary indexes are possible; storage is saved.  
```  
  
### <a name="sql-server-types-in-untyped-xml"></a>Types SQL Server en XML non typé  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les types correspondent à la valeur de retour de la méthode value().  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les types prennent en charge l’indicateur d’optimisation suivant : SINGLETON.  
  
 Spécifier un type est obligatoire pour les chemins d'accès qui retournent des types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez le même type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous utiliseriez dans la méthode value().  
  
 Considérez la requête suivante :  
  
```sql  
SELECT T.record,  
    T.xmldata.value('(/a/b/d)[1]', 'NVARCHAR(200)')  
FROM myXMLTable T  
```  
  
 La requête spécifiée retourne une valeur du chemin d'accès `/a/b/d` compressé dans un type de données NVARCHAR(200). Ainsi, le type de données à spécifier pour le nœud est évident. Néanmoins, il n'existe aucun schéma pour spécifier la cardinalité du nœud en XML non typé. Pour spécifier que le nœud `d` apparaît au plus une fois sous son nœud parent `b`, créez un index XML sélectif qui utilise l'indicateur d'optimisation de SINGLETON comme suit :  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_US  
ON Tbl(xmlcol)  
FOR  
(  
node1223 = '/a/b/d' as SQL NVARCHAR(200) SINGLETON  
)  
```  
  
  
##  <a name="typed"></a> Présentation de la prise en charge des index XML sélectifs pour le XML typé  
 Le XML typé de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un schéma associé à un document XML donné. Ce schéma définit la structure globale du document et les types de nœuds. S'il existe un schéma, l'index XML sélectif applique la structure de schéma lorsque l'utilisateur promeut les chemins d'accès. Il est donc inutile de spécifier les types XQUERY pour les chemins d'accès.  
  
 Les index XML sélectifs prennent en charge les types XSD suivants :  
  
-   **xs:anyUri**  
  
-   **xs:boolean**  
  
-   **xs:date**  
  
-   **xs:dateTime**  
  
-   **xs:day**  
  
-   **xs:decimal**  
  
-   **xs:double**  
  
-   **xs:float**  
  
-   **xs:int**  
  
-   **xs:integer**  
  
-   **xs:language**  
  
-   **xs:long**  
  
-   **xs:name**  
  
-   **xs:NCName**  
  
-   **xs:negativeInteger**  
  
-   **xs:nmtoken**  
  
-   **xs:nonNegativeInteger**  
  
-   **xs:nonPositiveInteger**  
  
-   **xs:positiveInteger**  
  
-   **xs:qname**  
  
-   **xs:short**  
  
-   **xs:string**  
  
-   **xs:time**  
  
-   **xs:token**  
  
-   **xs:unsignedByte**  
  
-   **xs:unsignedInt**  
  
-   **xs:unsignedLong**  
  
-   **xs:unsignedShort**  
  
 Lorsque l'index XML sélectif est créé sur un document contenant le schéma qui lui est associé, la spécification d'un type XQUERY lors de la création ou de la modification d'un index retourne une erreur. L'utilisateur peut utiliser des annotations de type SQL dans le cadre de la promotion de chemin d'accès. Le type SQL doit être une conversion valide du type XSD défini dans le schéma, sinon une erreur est générée. Tous les types SQL disposant des performances adéquates dans le schéma XSD sont pris en charge, sauf les types date/heure.  
  
> [!NOTE]  
>  L'index sélectif est utilisé si le type spécifié dans la promotion de chemin d'accès d'index XML sélectif est identique à la valeur retournée par la méthode value().  
  
 Les indicateurs d'optimisation suivants peuvent être utilisés avec des documents XML typés :  
  
-   indicateur d'optimisation node().  
  
-   L'indicateur d'optimisation MAXLENGTH peut être utilisé avec types xs:string pour raccourcir la valeur indexée.  
  
 Pour plus d’informations sur les indicateurs d’optimisation, consultez [Spécification des indicateurs d’optimisation](#hints).  
  
##  <a name="paths"></a> Spécification des chemins d'accès  
 Un index XML sélectif vous permet d'indexer uniquement un sous-ensemble de nœuds de données XML stockées en rapport avec les requêtes que vous comptez exécuter. Lorsque le sous-ensemble de nœuds appropriés est beaucoup plus petit que le nombre total de nœuds dans le document XML, l'index XML sélectif stocke uniquement les nœuds appropriés. Pour bénéficier d'un index XML sélectif, identifiez le sous-ensemble correct de nœuds à indexer.  
  
### <a name="choosing-the-nodes-to-index"></a>Sélection des nœuds à indexer  
 Vous pouvez utiliser les deux principes simples suivants pour identifier le sous-ensemble correct de nœuds à ajouter à un index XML sélectif.  
  
1.  **Principe 1**: pour évaluer une expression XQuery donnée, indexez tous les nœuds que vous devez examiner.  
  
    -   Indexez tous les nœuds dont l'existence ou la valeur est utilisée dans l'expression XQuery.  
  
    -   Indexez tous les nœuds de l'expression XQuery sur laquelle les prédicats XQuery sont appliqués.  
  
     Considérez la requête simple suivante sur [l’exemple de document XML](#sample) dans cette rubrique :  
  
    ```sql  
    SELECT T.record FROM myXMLTable T  
    WHERE T.xmldata.exist('/a/b[./c = "43"]') = 1  
    ```  
  
     Pour retourner des instances XML qui satisfont cette requête, un index XML sélectif doit examiner deux nœuds dans chaque instance XML :  
  
    -   Le nœud `c`, car sa valeur est utilisée dans l'expression XQuery.  
  
    -   Le nœud `b`, car un prédicat est appliqué sur le nœud`b` dans l'expression XQuery.  
  
2.  **Principe 2**: pour obtenir de meilleures performances, indexez tous les nœuds requis pour évaluer une expression XQuery donnée. Si vous indexez uniquement certains nœuds, l'index XML sélectif améliore l'évaluation des sous-expressions qui incluent uniquement les nœuds indexés.  
  
 Pour améliorer les performances de l'instruction SELECT ci-dessus, créez l'index XML sélectif suivant :  
  
```sql  
CREATE SELECTIVE XML INDEX simple_sxi  
ON Tbl(xmlcol)  
FOR  
(  
    path123 =  '/a/b',  
    path124 =  '/a/b/c'  
)  
```  
  
### <a name="indexing-identical-paths"></a>Indexation de chemins d'accès identiques  
 Vous ne pouvez pas promouvoir des chemins d'accès identiques de même type de données sous des noms de chemins d'accès différents. Par exemple, la requête suivante génère une erreur, car `pathOne` et `pathTwo` sont identiques :  
  
```sql  
CREATE SELECTIVE INDEX test_simple_sxi ON T1(xmlCol)  
FOR  
(  
    pathOne = 'book/authors/authorID' AS XQUERY 'xs:string',  
    pathTwo = 'book/authors/authorID' AS XQUERY 'xs:string'  
)  
```  
  
 Toutefois, vous pouvez promouvoir des chemins d'accès de types de données différents avec des noms différents. Par exemple, la requête suivante est maintenant acceptable, car les types de données sont différents :  
  
```sql  
CREATE SELECTIVE INDEX test_simple_sxi ON T1(xmlCol)  
FOR  
(  
    pathOne = 'book/authors/authorID' AS XQUERY 'xs:double',  
    pathTwo = 'book/authors/authorID' AS XQUERY 'xs:string'  
)  
```  
  
### <a name="examples"></a>Exemples  
 Voici quelques exemples supplémentaires de sélection des nœuds appropriés pour indexer des types XQuery différents.  
  
 **Exemple 1**  
  
 Voici une requête XQuery simple qui utilise la méthode exist() :  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b/c/d/e/h') = 1  
```  
  
 Le tableau suivant indique les nœuds qui doivent être indexés pour laisser cette requête utiliser les index XML sélectifs.  
  
|Nœd à inclure dans l'index|Raison de l'indexation de ce nœud|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e/h**|L'existence du nœud `h` est évaluée dans la méthode exist().|  
  
 **Exemple 2**  
  
 Voici une variation plus complexe de la requête XQuery précédente, avec un prédicat appliqué :  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b/c/d/e[./f = "SQL"]') = 1  
```  
  
 Le tableau suivant indique les nœuds qui doivent être indexés pour laisser cette requête utiliser les index XML sélectifs.  
  
|Nœd à inclure dans l'index|Raison de l'indexation de ce nœud|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Un prédicat est appliqué sur le nœud `e`.|  
|**/a/b/c/d/e/f**|La valeur du nœud `f` est évaluée au sein du prédicat.|  
  
 **Exemple 3**  
  
 Voici une requête plus complexe avec une clause value() :  
  
```sql  
SELECT T.record,  
    T.xmldata.value('(/a/b/c/d/e[./f = "SQL"]/g)[1]', 'nvarchar(100)')  
FROM myXMLTable T  
```  
  
 Le tableau suivant indique les nœuds qui doivent être indexés pour laisser cette requête utiliser les index XML sélectifs.  
  
|Nœd à inclure dans l'index|Raison de l'indexation de ce nœud|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Un prédicat est appliqué sur le nœud `e`.|  
|**/a/b/c/d/e/f**|La valeur du nœud `f` est évaluée au sein du prédicat.|  
|**/a/b/c/d/e/g**|La valeur du nœud `g` est retournée par la méthode value().|  
  
 **Exemple 4**  
  
 Voici une requête qui utilise une clause FLWOR dans une clause exist(). (Le nom FLWOR provient des cinq clauses qui peuvent composer une expression XQuery FLWOR : FOR, LET, WHERE, ORDER BY et RETURN.)  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('  
  For $x in /a/b/c/d/e  
  Where $x/f = "SQL"  
  Return $x/g  
') = 1  
```  
  
 Le tableau suivant indique les nœuds qui doivent être indexés pour laisser cette requête utiliser les index XML sélectifs.  
  
|Nœd à inclure dans l'index|Raison de l'indexation de ce nœud|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|L'existence du nœud `e` est évaluée dans la clause FLWOR.|  
|**/a/b/c/d/e/f**|La valeur du nœud `f` est évaluée dans la clause FLWOR.|  
|**/a/b/c/d/e/g**|L'existence du nœud `g` est évaluée par la méthode exist().|  
  
  
##  <a name="hints"></a> Spécification des indicateurs d’optimisation  
 Vous pouvez utiliser des indicateurs facultatifs d'optimisation pour spécifier des détails supplémentaires de mappage pour un nœud indexé par un index XML sélectif. Par exemple, vous pouvez spécifier le type de données et la cardinalité du nœud, ainsi que certaines informations sur la structure des données. Ces informations permettent un meilleur mappage. Elles entraînent également des améliorations des performances ou des économies en termes de stockage, ou les deux.  
  
 L'utilisation des indicateurs d'optimisation est facultative. Vous pouvez toujours accepter les mappages par défaut, qui sont fiables mais ne permettent pas des performances et un stockage optimaux.  
  
 Certains indicateurs d'optimisation (par exemple, l'indicateur SINGLETON, introduisent des contraintes sur vos données. Dans certains cas, des erreurs peuvent être générées lorsque ces contraintes ne sont pas satisfaites.  
  
### <a name="benefits-of-optimization-hints"></a>Avantages des indicateurs d'optimisation  
 Le tableau suivant identifie les indicateurs d'optimisation qui prennent en charge un stockage plus efficace ou de meilleures performances.  
  
|indicateur d'optimisation|Stockage plus efficace|Performances améliorées|  
|-----------------------|----------------------------|--------------------------|  
|**node()**|Oui|non|  
|**SINGLETON**|non|Oui|  
|**DATA TYPE**|Oui|Oui|  
|**MAXLENGTH**|Oui|Oui|  
  
### <a name="optimization-hints-and-data-types"></a>Indicateurs d'optimisation et types de données  
 Vous pouvez indexer des nœuds en tant que types de données XQuery ou en tant que types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le tableau suivant illustre les indicateurs d'optimisation pris en charge avec chaque type de données.  
  
|indicateur d'optimisation|Types de données XQuery|Types de données SQL|  
|-----------------------|-----------------------|--------------------|  
|**node()**|Oui|non|  
|**SINGLETON**|Oui|Oui|  
|**DATA TYPE**|Oui|non|  
|**MAXLENGTH**|Oui|non|  
  
### <a name="node-optimization-hint"></a>Indicateur d'optimisation node()  
 S'applique à : types de données XQuery  
  
 Vous pouvez utiliser l'optimisation node() pour spécifier un nœud dont la valeur n'est pas requise pour évaluer la requête classique. Cet indicateur réduit les besoins de stockage lorsque la requête classique doit uniquement évaluer l'existence du nœud. (Par défaut, un index XML sélectif stocke la valeur de tous les nœuds promus, à l'exception des types de nœuds complexes.)  
  
 Prenons l'exemple suivant :  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b[./c=5]') = 1  
```  
  
 Pour utiliser un index XML sélectif pour évaluer cette requête, effectuez la promotion des nœuds `b` et `c`. Toutefois, étant donné que la valeur du nœud `b` n'est pas obligatoire, vous pouvez utiliser l'indicateur node() avec la syntaxe suivante :  
  
 `/a/b/ as node()`  
  
 Si une requête requiert la valeur d'un nœud qui a été indexé avec l'indicateur node(), l'index XML sélectif ne peut pas être utilisé.  
  
### <a name="singleton-optimization-hint"></a>Indicateur d'optimisation SINGLETON  
 S'applique à : types de données XQuery ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 L'indicateur d'optimisation SINGLETON spécifie la cardinalité d'un nœud. Cet indicateur améliore les performances des requêtes, car il sait à l'avance qu'un nœud apparaît au plus une fois dans son parent ou ancêtre.  
  
 Examinez [l’exemple de document XML](#sample) dans cette rubrique.  
  
 Pour utiliser un index XML sélectif pour interroger ce document, spécifiez l'indicateur SINGLETON du nœud `d` , car il apparaît au plus une fois dans son parent.  
  
 Si l'indicateur SINGLETON a été spécifié, mais un nœud apparaît plusieurs fois dans son parent ou ancêtre, une erreur est générée lorsque vous créez l'index (pour les données existantes) ou lorsque vous exécutez une requête (pour les nouvelles données).  
  
### <a name="data-type-optimization-hint"></a>Indicateur d'optimisation DATA TYPE  
 S'applique à : types de données XQuery  
  
 L'indicateur d'optimisation DATA TYPE vous permet de spécifier un type de données XQuery ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du nœud indexé. Le type de données est utilisé pour la colonne dans la table de données de l'index XML sélectif qui correspond au nœud indexé.  
  
 Lors de la conversion d'une valeur existante dans le type de données spécifié échoue, l'opération d'insertion (dans l'index) réussit ; toutefois, une valeur NULL est insérée dans la table de données de l'index.  
  
### <a name="maxlength-optimization-hint"></a>Indicateur d'optimisation MAXLENGTH  
 S'applique à : types de données XQuery  
  
 L'indicateur d'optimisation MAXLENGTH vous permet de limiter la longueur de données xs:string. MAXLENGTH n'est pas pertinente pour les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] étant donné que vous spécifiez la longueur lorsque vous spécifiez les types de dates VARCHAR ou NVARCHAR.  
  
 Lorsqu'une chaîne existante est plus longue que l'indicateur MAXLENGTH spécifié, l'insertion de cette valeur dans l'index échoue.  
  
  
##  <a name="sample"></a> Document XML des exemples  
 Le document XML suivant est référencé dans les exemples de cette rubrique :  
  
```xml  
<a>  
    <b>  
         <c atc="aa">10</c>  
         <c atc="bb">15</c>  
         <d atd1="dd" atd2="ddd">md </d>  
    </b>  
     <b>  
        <c></c>  
        <c atc="">117</c>  
     </b>  
</a>  
```  
  
  
## <a name="see-also"></a> Voir aussi  
 [Index XML sélectifs &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Créer, modifier ou supprimer des index XML sélectifs](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)  
  
  
