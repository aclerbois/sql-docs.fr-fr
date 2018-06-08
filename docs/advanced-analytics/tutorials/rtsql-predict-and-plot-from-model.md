---
title: Prédire et tracer le modèle (R Guide de démarrage rapide SQL) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 3809b8f6dbf84de04b84c7f4a6bdd5c492e2bdcd
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="predict-and-plot-from-model-r-in-sql-quickstart"></a>Prédire et tracer le modèle (R Guide de démarrage rapide SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Pour effectuer _score_ à l’aide de nouvelles données, d’obtenir les modèles formés à partir de la table et appelez ensuite un nouveau jeu de données sur lequel baser vos prévisions. Calcul du score du terme est parfois utilisé dans la science des données pour générer des prédictions, les probabilités ou les autres valeurs basées sur les nouvelles données sont chargées dans un modèle formé.

## <a name="create-the-table-of-new-speeds"></a>Créer la table des nouvelles vitesses

Avez-vous remarqué que les données d’apprentissage d’origine ne vont pas au-delà de la vitesse de 25 km/h ? Cela s’explique par le fait que ces données découlent d’une expérience réalisée en 1920 !

Vous voulez savoir quelle aurait été la distance d’arrêt d’une voiture des années 20 qui aurait pu rouler à 60 km/h voire à 100 km/h ? Pour répondre à cette question, vous devez fournir les nouvelles valeurs de vitesse.

```sql
CREATE TABLE [dbo].[NewCarSpeed]([speed] [int] NOT NULL,
    [distance] [int]  NULL) ON [PRIMARY]
GO
INSERT [dbo].[NewCarSpeed] (speed)
VALUES (40),  (50),  (60), (70), (80), (90), (100)
```

## <a name="predict-stopping-distance"></a>Prédire la distance d’arrêt

Vous utilisez peut-être une table qui contient plusieurs modèles R, créés avec des paramètres ou algorithmes différents, ou formés à partir de plusieurs sous-ensembles de données.

![rsql_basictut_listofmodels](media/rsql-basictut-listofmodels.png)

Pour obtenir des prédictions basées sur un modèle spécifique, vous devez écrire un script SQL qui effectue les opérations suivantes :

1. Obtenir le modèle voulu
2. Obtenir les nouvelles données d’entrée
3. Appeler une fonction de prédiction R compatible avec ce modèle

Dans cet exemple, étant donné que votre modèle est basé sur le **rxLinMod** algorithme fourni dans le cadre de la **RevoScaleR** package, vous appelez le [rxPredict](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxpredict) (fonction), plutôt qu’à le R générique `predict` (fonction).

```sql
DECLARE @speedmodel varbinary(max) = (SELECT model FROM [dbo].[stopping_distance_models] WHERE model_name = 'latest model');
EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'
            current_model <- unserialize(as.raw(speedmodel));
            new <- data.frame(NewCarData);
            predicted.distance <- rxPredict(current_model, new);
            str(predicted.distance);
            OutputDataSet <- cbind(new, ceiling(predicted.distance));
            '
    , @input_data_1 = N' SELECT speed FROM [dbo].[NewCarSpeed] '
    , @input_data_1_name = N'NewCarData'
    , @params = N'@speedmodel varbinary(max)'
    , @speedmodel = @speedmodel
WITH RESULT SETS (([new_speed] INT, [predicted_distance] INT))
```

+ Utilisez une instruction SELECT pour obtenir un seul modèle de la table à passer comme paramètre d’entrée.
+  Après avoir récupéré le modèle de la table, appelez la fonction `unserialize` sur ce modèle.

    > [!TIP] 
    > Consultez également la nouvelle [des fonctions de sérialisation](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxserializemodel) fournie par RevoScaleR, qui prennent en charge [calculer les scores en temps réel](../../advanced-analytics/real-time-scoring.md).
+  Appliquez la fonction `rxPredict` avec les arguments appropriés au modèle et fournissez les nouvelles données d’entrée.
+  Dans l’exemple, le `str` fonction est ajoutée au cours de la phase de test, de vérifier le schéma des données retournées à partir de R. Vous pouvez supprimer l’instruction ultérieurement.
+ Les noms de colonne utilisés dans le script R ne sont pas nécessairement transmises à la sortie de la procédure stockée. Ici, nous avons utilisé la clause avec des résultats pour définir des nouveaux noms de colonnes.

**Résultats**

![rsql_basictut_scoringresults_smalldata](media/rsql-basictut-scoringresults-smalldata.PNG)

## <a name="perform-scoring-in-parallel"></a>Effectuer un calcul de score en parallèle

Les prédictions ont été retournées rapidement en raison de la petite taille du jeu de données de cet exemple. Mais cela aurait-il été aussi rapide avec un très grand nombre de prédictions ? Il existe de nombreuses façons d’accélérer les opérations exécutées dans SQL Server, etc. Si les opérations peuvent être traitées en parallèle. Pour le calcul de score, notamment, une méthode simple consiste à ajouter le paramètre *@parallel* à `sp_execute_external_script`, puis à définir la valeur sur **1**.

Supposez que vous ayez obtenu une table de vitesses de voiture possibles beaucoup plus volumineuse, contenant des centaines de milliers de valeurs. La communauté propose de nombreux exemples de scripts T-SQL pour vous aider à générer des tables de nombres. Nous n’allons donc pas les reproduire ici. Imaginez simplement que vous avez une colonne contenant beaucoup d’entiers et que vous voulez utiliser ces nombres comme entrée pour `speed` dans le modèle.

Pour ce faire, il suffit exécuter la même requête de prédiction, mais remplacer le jeu de données plus volumineux et ajouter la `@parallel = 1` argument.

```sql
DECLARE @speedmodel varbinary(max) = (select model from [dbo].[stopping_distance_models] where model_name = 'default model');
EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'
            current_model <- unserialize(as.raw(speedmodel));
            new <- data.frame(NewCarData);
            predicted.distance <- rxPredict(current_model, new);
            OutputDataSet <- cbind(new, ceiling(predicted.distance));
            '
    , @input_data_1 = N' SELECT [speed] FROM [dbo].[HugeTableofCarSpeeds] '
    , @input_data_1_name = N'NewCarData'
    , @parallel = 1
    , @params = N'@speedmodel varbinary(max)'
    , @speedmodel = @speedmodel
WITH RESULT SETS (([new_speed] INT, [predicted_distance] INT))
```

+ L’exécution parallèle n’offre généralement avantages uniquement lorsque vous travaillez avec des données très volumineuses. Le moteur de base de données SQL peut décider que l’exécution parallèle n’est pas nécessaire. De plus, la requête SQL qui récupère vos données doit être capable de générer un plan de requêtes en parallèle.

+ Si vous utilisez l’option d’exécution parallèle, vous **devez** spécifier le schéma de résultats de sortie au préalable, à l’aide de la clause WITH RESULT SETS. Cela permet à SQL Server d’agréger les résultats de plusieurs jeux de données parallèles, en leur évitant d’avoir des schémas inconnus.

+ Si vous êtes *formation* un modèle à la place de *score*, ce paramètre ne sont pas souvent ont une incidence. Selon le type de modèle, le processus de création de modèle peut avoir besoin de lire toutes les lignes pour générer les résumés.

+ Pour obtenir les avantages du traitement en parallèle lors de l’apprentissage votre modèle, nous vous recommandons d’utiliser un de le **RevoScaleR** algorithmes. Ces algorithmes sont conçus pour distribuer le traitement automatiquement, même si vous ne spécifiez pas <code>@parallel =1</code> dans l’appel à `sp_execute_external_script`. Pour obtenir des conseils sur la façon d’obtenir les meilleures performances avec les algorithmes de RevoScaleR, consultez [distribué et le calcul parallèle avec ScaleR dans Microsoft R](https://docs.microsoft.com/r-server/r/how-to-revoscaler-distributed-computing).

## <a name="create-an-r-plot-of-the-model"></a>Créer un tracé R du modèle

Beaucoup de clients, y compris SQL Server Management Studio, ne peuvent pas afficher directement les tracés créés avec [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Au lieu de cela, le processus général pour générer les tracés R est pour créer le tracé dans le cadre de votre code R, puis écrire l’image dans un fichier.

Ou bien, vous pouvez retourner l’objet sérialisé de traçage binaire à toute application qui peut afficher des images.

L’exemple suivant montre comment créer un graphique simple à l’aide d’une fonction de traçage fournie par défaut avec R. L’image de sortie est retournée dans le fichier spécifié, mais également dans une variable SQL par la procédure stockée.

```sql
 EXECUTE sp_execute_external_script
 @language = N'R'
 , @script = N'
     imageDir <- ''C:\\temp\\plots'';
     image_filename = tempfile(pattern = "plot_", tmpdir = imageDir, fileext = ".jpg")
     print(image_filename);
     jpeg(filename=image_filename,  width=600, height = 800);
     print(plot(distance~speed, data=InputDataSet, xlab="Speed", ylab="Stopping distance", main = "1920 Car Safety"));
     abline(lm(distance~speed, data = InputDataSet));
     dev.off();
     OutputDataSet <- data.frame(data=readBin(file(image_filename, "rb"), what=raw(), n=1e6));
     '
  , @input_data_1 = N'SELECT speed, distance from [dbo].[CarSpeed]'
  WITH RESULT SETS ((plot varbinary(max)));
```

+ Le `tempfile` fonction retourne une chaîne qui peut être utilisée comme nom de fichier, mais le fichier n’a pas encore été généré.
+ Les arguments de `tempfile`, vous pouvez spécifier un préfixe et extension de fichier, ainsi que le répertoire. Pour vérifier le nom complet du fichier et le chemin d’accès, imprimer un message à l’aide de `str()`.
+ La fonction `jpeg` crée un périphérique R avec les paramètres spécifiés.
+ Après avoir créé le traçage, vous pouvez ajouter davantage de fonctionnalités visual à celui-ci. Dans ce cas, une ligne de régression est ajoutée à l’aide de `abline`.
+ Quand vous avez terminé d’ajouter les fonctions de traçage, fermez le périphérique graphique à l’aide de la fonction `dev.off()`.
+ La fonction `readBin` prend un fichier à lire, une spécification de format et le nombre d’enregistrements. Le `rb`**' mot clé indique que le fichier est binaire plutôt que du texte.

**Résultats**

![rsql_basictut_plotresult_small](media/rsql-basictut-plotresult-small.png)

Si vous souhaitez effectuer des tracés plus élaborés, à l’aide de certains packages graphiques avancés de R, nous vous recommandons de lire ces articles. Les deux articles nécessitent le package **ggplot2**.

+ [Loan Classification using SQL Server 2016 R Services](https://blogs.msdn.microsoft.com/microsoftrservertigerteam/2016/09/27/loan-classification-using-sql-server-2016-r-services/) : scénario complet basé sur des données d’assurance. Requiert le **remodeler** package.
+ [Créer des graphiques et des graphiques à l’aide de R](../../advanced-analytics/tutorials/walkthrough-create-graphs-and-plots-using-r.md)

## <a name="conclusions"></a>Conclusions

L’intégration de R avec SQL Server facilite le déploiement de solutions R à grande échelle, en exploitant les fonctionnalités les plus performantes de R et des bases de données relationnelles pour optimiser la gestion des données et accélérer le traitement analytique de R. 

Consultez ces ressources supplémentaires pour d’autres exemples de R :

+  [Didacticiels de SQL Server R](../../advanced-analytics/tutorials/sql-server-r-tutorials.md)

    Continuer à découvrir des solutions à l’aide de R avec SQL Server, via des scénarios de bout en bout créés par les équipes de développement de science des données de Microsoft et de R Services.

+ [Didacticiels de SQL Server Python](../../advanced-analytics/tutorials/sql-server-python-tutorials.md)

    Pour SQL Server 2017, utilisez la puissance de contexte de calcul à distance et l’algorithme évolutive avec le langage Python.

+ [Didacticiels et exemples de données pour Microsoft R](https://docs.microsoft.com/r-server/r/tutorial-introduction)

    Découvrez comment utiliser les nouveaux packages RevoScaleR pour créer des modèles et de transformer des données.

+ [Prise en main MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)

    En savoir plus sur rapide et évolutif, algorithmes d’apprentissage à partir de Microsoft Research.
