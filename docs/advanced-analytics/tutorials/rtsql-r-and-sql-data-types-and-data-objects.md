---
title: R et SQL les données et les types d’objets de données (R Guide de démarrage rapide SQL) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: eeed977c3b942f0c23e4036f514018f54b966b70
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="r-and-sql-data-types-and-data-objects-r-in-sql-quickstart"></a>R et SQL les données et les types d’objets de données (R Guide de démarrage rapide SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans cette étape, vous en savoir plus sur certains problèmes courants qui surviennent lors du déplacement des données entre R et SQL Server :

+ Les types de données ne correspondent pas toujours.
+ Les conversions implicites peuvent avoir lieu
+ Des opérations de cast et de conversion sont parfois nécessaires.
+ R et SQL utilisent des objets de données différents.

## <a name="always-return-r-data-as-a-data-frame"></a>Toujours retourner les données R sous forme de trame de données

Quand votre script retourne les résultats de R vers SQL Server, il doit retourner les données sous forme de trame de données (objet **data.frame**). Tout autre type d’objet que vous générez dans votre script (liste, facteur, vecteur ou données binaires) doit être converti en trame de données si vous souhaitez retourner la sortie de l’objet dans les résultats de la procédure stockée. Heureusement, vous avez plusieurs fonctions R à votre disposition pour changer des objets différents en trame de données. Vous pouvez même sérialiser un modèle binaire et le retourner dans une trame de données, ce que vous ferez plus tard dans ce didacticiel.

Tout d’abord, nous allons faire des essais avec certains objets de base R R, vecteurs, matrices et listes et voir comment la conversion en une trame de données modifie la sortie transmise à SQL Server.

Comparer ces deux scripts « Hello World » dans R. Les scripts semblent presque identiques, mais la première retourne une seule colonne de trois valeurs, tandis que la seconde retourne trois colonnes avec une seule valeur chacun.

**Exemple 1**

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
     , @script = N' mytextvariable <- c("hello", " ", "world");
       OutputDataSet <- as.data.frame(mytextvariable);'
     , @input_data_1 = N' ';
```

**Exemple 2**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
      , @input_data_1 = N'  ';
```

## <a name="identifying-the-schema-and-data-types-of-r-data"></a>Identifier le schéma et les types des données R

Pourquoi les résultats sont-ils si différents ?

La réponse est généralement liée à l’utilisation de la commande R `str()`. Ajoutez la fonction `str(object_name)` à n’importe quel endroit dans votre script R pour que le schéma de données de l’objet R spécifié soit retourné sous forme de message d’information. Pour afficher les messages, accédez au volet **Messages** dans Visual Studio Code ou à l’onglet **Messages** dans SSMS.

Pour comprendre pourquoi les exemples 1 et 2 ont des résultats très différents, insérez la ligne `str(OutputDataSet)` à la fin de la définition de variable _@script_ dans chaque instruction, comme ceci :

**Exemple 1 avec la fonction str ajouté**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' mytextvariable <- c("hello", " ", "world");
      OutputDataSet <- as.data.frame(mytextvariable);
      str(OutputDataSet);'
      , @input_data_1 = N'  '
;
```

**Exemple 2 avec la fonction str ajouté**

```sql
EXECUTE sp_execute_external_script
  @language = N'R', 
  @script = N' OutputDataSet <- data.frame(c("hello"), " ", c("world"));
    str(OutputDataSet);' , 
  @input_data_1 = N'  ';
```

Examinez ensuite le texte affiché dans **Messages** pour voir ce qui diffère.

**Résultats de l’exemple 1**

```
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Résultats de l’exemple 2**

```
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Comme vous pouvez le constater, un léger changement dans la syntaxe R transforme le schéma des résultats. Nous n’entrerons dans pourquoi, étant donné que les différences dans les types de données R sont expliquées plus en détail dans cet article par Hadley Wickham : [des Structures de données R](http://adv-r.had.co.nz/Data-structures.html).

Pour l’instant, gardez seulement à l’esprit que vous devez vérifier les résultats attendus quand vous convertissez des objets R en trames de données.

> [!TIP]
> 
> Vous pouvez également utiliser des fonctions R identité, tel que `is.matrix`, `is.vector`, etc.

## <a name="implicit-conversion-of-data-objects"></a>Conversion implicite des objets de données

Chaque objet de données R a ses propres règles pour le traitement des valeurs qui sont combinées avec un autre objet de données ayant le même nombre de dimensions ou avec un objet de données contenant des types de données hétérogènes.

Par exemple, supposez que vous exécutez l’instruction suivante pour effectuer une multiplication de matrice à l’aide de R. Vous multipliez une matrice à une seule colonne de trois valeurs par un tableau de quatre valeurs, et attendez comme résultat une matrice contenant quatre colonnes de trois valeurs.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:15);
    OutputDataSet <- as.data.frame(x %*% y);'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (([Col1] int, [Col2] int, [Col3] int, Col4 int));
```

En réalité, la colonne de trois valeurs est convertie en une matrice à une seule colonne. Étant donné qu’une matrice est simplement un tableau particulier dans R, le tableau `y` est implicitement converti en une matrice à une colonne pour rendre les deux arguments conformes.

**Résultats**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

Toutefois, notez que se passe-t-il lorsque vous modifiez la taille du tableau `y`.

```sql
execute sp_execute_external_script
   @language = N'R'
   , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:14);
   OutputDataSet <- as.data.frame(y %*% x);'
   , @input_data_1 = N' SELECT [Col1]  from RTestData;'
   WITH RESULT SETS (([Col1] int ));
```

R retourne maintenant une seule valeur comme résultat.

**Résultats**
    
|Col1|
|---|
|1542|

Pourquoi ? Dans ce cas, étant donné que les deux arguments peuvent être traités comme des vecteurs de la même longueur que R retourne le produit d’interne comme une matrice.  C’est le comportement attendu selon les règles de l’algèbre linéaire, mais cela peut poser problème si votre application en aval s’attend à ce que le schéma de sortie reste toujours le même !

> [!TIP]
> 
> Obtenez des erreurs ? Ces exemples nécessitent la table **RTestData**. Si vous n’avez pas encore créé la table de données de test, revenez à cette rubrique : [utilisation des entrées et sorties](../tutorials/rtsql-working-with-inputs-and-outputs.md).
> 
> Si vous avez créé la table mais que vous recevez une erreur, assurez-vous que vous exécutez la procédure stockée dans le contexte de la base de données qui contient la table et pas **master** ou une autre base de données.
> 
> En outre, nous vous suggérons d’éviter d’utiliser des tables temporaires pour ces exemples. Certains clients R mettra fin à une connexion entre les traitements, suppression de tables temporaires.

## <a name="merge-or-multiply-columns-of-different-length"></a>Fusionner ou multiplier des colonnes de longueur différente

R fournit une grande souplesse pour l’utilisation des vecteurs de différentes tailles et pour la combinaison de ces structures de colonne de type dans les trames de données. Les listes de vecteurs ressemblent parfois à des tables, mais elles ne suivent pas toutes les règles qui régissent les tables de base de données.

Par exemple, le script suivant définit un tableau numérique de longueur 6 et le stocke dans la variable R `df1`. Le tableau numérique est ensuite combiné avec les entiers de la table RTestData, qui contient les valeurs de trois (3), pour effectuer une nouvelle trame de données, `df2`.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
               df1 <- as.data.frame( array(1:6) );
               df2 <- as.data.frame( c( InputDataSet , df1 ));
               OutputDataSet <- df2'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (( [Col2] int not null, [Col3] int not null ));
```

Pour remplir la trame de données, R répète les éléments récupérés de la table RTestData autant de fois que nécessaire pour obtenir le même nombre d’éléments que dans le tableau `df1`.

**Résultats**
    
|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

N’oubliez pas qu’une trame de données est une liste de vecteurs, même si elle a l’apparence d’une table.

## <a name="cast-or-convert-sql-server-data"></a>Effectuer un cast ou une conversion des données SQL Server

R et SQL Server n’utilisent pas les mêmes types de données. Quand vous exécutez une requête dans SQL Server pour obtenir des données que vous voulez ensuite passer au runtime R, un type de conversion implicite a généralement lieu. D’autres conversions sont effectuées quand vous retournez les données de R vers SQL Server.

- SQL Server envoie les données à partir de la requête au processus R géré par le service Launchpad et le convertit en une représentation interne pour une meilleure efficacité.
- Le runtime R charge les données dans une variable data.frame et effectue ses propres opérations sur les données.
- Le moteur de base de données retourne les données à SQL Server par le biais d’une connexion sécurisée interne et présente les données en termes de types de données SQL Server.
- Vous obtenez les données en vous connectant à SQL Server à l’aide d’une bibliothèque cliente ou réseau qui peut envoyer des requêtes SQL et gérer des jeux de données tabulaires. Cette application cliente est susceptible de modifier les données d’autres manières.

Pour voir comment cela fonctionne, exécutez une requête telle que celle-ci sur l’entrepôt de données AdventureWorksDW. Cette requête retourne les données de ventes utilisées pour effectuer des prévisions.

```sql
SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW2014].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = 'M200 Europe'
           ORDER BY ReportingDate ASC
```

> [!NOTE]
> 
> Vous pouvez utiliser n’importe quelle version d’AdventureWorks, ou créer une requête différente à l’aide d’une base de données de votre choix. Le point consiste à tenter de gérer des données qui contient les valeurs de texte, date/heure et numériques.

Essayez maintenant coller cette requête comme entrée à la procédure stockée.

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
      , @script = N' str(InputDataSet);
      OutputDataSet <- InputDataSet;'
      , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW2014].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Si vous obtenez une erreur, vous devez probablement modifier le texte de la requête. Par exemple, le prédicat de chaîne dans la clause WHERE doit être placé entre deux paires de guillemets simples.

Si la requête réussit, examinez les résultats de la fonction `str` pour voir de quelle manière R traite les données d’entrée.

**Résultats**

```
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

+ La colonne date/heure a été traitée avec le type de données R **POSIXct**.
+ La colonne de texte « ProductSeries » a été identifié comme un **facteur**, ce qui signifie une variable catégorique. Par défaut, les valeurs de chaîne sont gérées comme des facteurs. Si vous passez une chaîne à R, cette chaîne est convertie en entier pour un usage interne, puis la valeur est mappée à la chaîne dans la sortie.

### <a name="summary"></a>Résumé

De même, ces exemples courts, vous pouvez voir la nécessité de vérifier les effets de la conversion de données lors de la transmission SQL interroge en tant qu’entrée. Étant donné que certains types de données SQL Server ne sont pas pris en charge par R, tenez compte des ces méthodes pour éviter les erreurs :

+ Vos données de test à l’avance et vérifiez les colonnes ou valeurs dans votre schéma peut être un problème quand il est passé au code R.
+ Spécifiez les colonnes dans votre source de données d’entrée individuellement, et non en utilisant `SELECT *`. Déterminez de quelle manière gérer chaque colonne.
+ Effectuez les opérations de cast explicites nécessaires lors de la phase de préparation de vos données d’entrée, pour éviter tout comportement inattendu.
+ Évitez les colonnes de transmission de données (par exemple, le GUID ou rowguid) qui provoquent des erreurs et ne sont pas utiles pour la modélisation.

Pour plus d’informations sur les types de données pris en charge et non pris en charge, consultez [R bibliothèques et types de données](../r/r-libraries-and-data-types.md).

Pour plus d’informations sur l’impact sur les performances d’exécution conversion des chaînes de facteurs numériques, consultez [le réglage des performances de SQL Server R Services](../r/sql-server-r-services-performance-tuning.md).

## <a name="next-lesson"></a>Leçon suivante

Dans l’étape suivante, vous apprendrez à appliquer des fonctions R à des données SQL Server.

[Utilisation des fonctions R avec les données SQL Server](../../advanced-analytics/tutorials/rtsql-using-r-functions-with-sql-server-data.md)
