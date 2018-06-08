---
title: sp_execute_external_script (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/22/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_execute_external_script_TSQL
- sys.sp_execute_external_script
- sys.sp_execute_external_script_TSQL
- sp_execute_external_script
dev_langs:
- TSQL
helpviewer_keywords:
- sp_execute_external_script
ms.assetid: de4e1fcd-0e1a-4af3-97ee-d1becc7f04df
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 5660860a3a03a268b0903a0222753f1ea9bc5382
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spexecuteexternalscript-transact-sql"></a>sp_execute_external_script (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Exécute le script fourni en tant qu’argument à un emplacement externe. Le script doit être écrite dans un langage pris en charge et inscrit. Pour exécuter **sp_execute_external_script**, vous devez tout d’abord activer les scripts externes à l’aide de l’instruction, `sp_configure 'external scripts enabled', 1;`.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe

```
sp_execute_external_script   
    @language = N'language',   
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]   
    [ , @input_data_1_name = N'input_data_1_name' ]   
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```

## <a name="arguments"></a>Arguments
 @language = N'*langage*'  
 Indique le langage de script. *langage* est **sysname**.  

 Les valeurs valides sont `Python` ou `R`. 
  
 @script = N'*script*'  
 Script de langage externe spécifié comme une entrée de littéral ou une variable. *script* est **nvarchar (max)**.  
  
 [ @input_data_1_name = N'*input_data_1_name*']  
 Spécifie le nom de la variable utilisée pour représenter la requête définie par @input_data_1. Le type de données de la variable de script externe dépend de la langue. En cas de R, la variable d’entrée est une trame de données. Dans le cas de Python, l’entrée doit être sous forme de tableau. *input_data_1_name* est **sysname**.  
  
 Valeur par défaut est `InputDataSet`.  
  
 [ @input_data_1 = N'*input_data_1*']  
 Spécifie les données d’entrée utilisées par le script externe sous la forme d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] requête. Type de données de *input_data_1* est **nvarchar (max)**.
  
 [ @output_data_1_name = N'*output_data_1_name*']  
 Spécifie le nom de la variable dans le script externe qui contient les données à retourner à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la fin de l’appel de procédure stockée. Le type de données de la variable de script externe dépend de la langue. Pour R, la sortie doit être une trame de données. Pour Python, la sortie doit être une trame de données pandas. *output_data_1_name* est **sysname**.  
  
 Valeur par défaut est « OutputDataSet ».  
  
 [ @parallel = 0 | 1] activer l’exécution parallèle de scripts R en définissant le `@parallel` paramètre 1. La valeur par défaut pour ce paramètre est 0 (Aucun parallélisme).  
  
 Pour les scripts R qui n’utilisent pas les fonctions RevoScaleR, à l’aide de le `@parallel` paramètre peut être utile pour le traitement des jeux de données volumineux, en supposant que le script peut être parallélisé simplement. Par exemple, lors de l’utilisation de R `predict` fonction avec un modèle pour générer de nouvelles prédictions, définissez `@parallel = 1` comme une indication au moteur de requête. Si la requête peut être parallélisée, les lignes sont distribuées en fonction de la **MAXDOP** paramètre.  
  
 Si `@parallel = 1` et la sortie est en cours de diffusion directement à l’ordinateur client, puis le `WITH RESULTS SETS` clause est requise et un schéma de sortie doit être spécifié.  
  
 Pour les scripts R qui utilisent les fonctions RevoScaleR, le traitement parallèle est géré automatiquement et vous ne devez pas spécifier `@parallel = 1` à la **sp_execute_external_script** appeler.  
  
 [ @params = N' *@parameter_name data_type* [OUT | SORTIE] [,.. .n]']  
 Une liste de déclarations de paramètre d’entrée qui sont utilisées dans le script externe.  
  
 [ @parameter1 = '*value1*' [OUT | SORTIE] [,.. .n]]  
 Une liste de valeurs pour les paramètres d’entrée utilisés par le script externe.  

## <a name="remarks"></a>Notes

Utilisez **sp_execute_external_script** pour exécuter des scripts écrits dans un langage pris en charge. Actuellement, les langues prises en charge sont R pour SQL Server 2016 et Python et R pour SQL Server 2017. 

> [!IMPORTANT]
> L’arborescence de la requête est contrôlé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les utilisateurs ne peuvent pas effectuer des opérations arbitraires sur la requête. 

Par défaut, les jeux de résultats retournés par cette procédure stockée s’affichent avec des colonnes sans nom. Noms de colonnes utilisés dans un script locaux de l’environnement de script et ne sont pas répercutées dans le jeu de résultats obtenus. Pour nommer des colonnes du jeu de résultats, utilisez le `WITH RESULTS SET` clause de [ `EXECUTE` ](../../t-sql/language-elements/execute-transact-sql.md).
  
 En plus de retourner un jeu de résultats, vous pouvez retourner des valeurs scalaires à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide des paramètres de sortie. L’exemple suivant illustre l’utilisation du paramètre de sortie pour retourner le modèle R sérialisé qui a été utilisé comme entrée pour le script :  

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] se compose d’un composant de serveur installé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et un ensemble d’outils de la station de travail et les bibliothèques de connectivité qui connectent les données scientifiques à l’environnement hautes performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous devez installer les composants lors d’apprentissage automatique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le programme d’installation pour permettre l’exécution de scripts externes. Pour plus d’informations, consultez [configurer SQL Server Machine Learning Services](../../advanced-analytics/r/set-up-sql-server-r-services-in-database.md).  
  
Vous pouvez contrôler les ressources utilisées par les scripts externes en configurant un pool de ressources externes. Pour plus d’informations, consultez [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md). Plus d’informations sur la charge de travail peuvent être obtenues à partir des affichages de catalogue du gouverneur de ressources, la DMV et des compteurs. Pour plus d’informations, consultez [affichages catalogue du gouverneur de ressources &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md), [Resource Governor connexes vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)et [ Objet de Scripts SQL Server, externe](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md).  

L’exécution du script d’analyse à l’aide [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) et [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md). 

## <a name="streaming-execution-for-r-and-python-scripts"></a>Diffusion en continu de l’exécution des scripts R et Python  

Diffusion en continu permet le script R ou Python travailler avec plus de données que vous pouvez tenir en mémoire. Pour contrôler le nombre de lignes passé lors de la diffusion en continu, spécifiez une valeur entière pour le paramètre, `@r_rowsPerRead` dans le `@params` collection.  Par exemple, si vous formez un modèle qui utilise des données très larges, vous pourriez ajuster la valeur afin de lire moins de lignes, pour vous assurer que toutes les lignes peuvent être envoyées dans un segment de données. Vous pouvez également utiliser ce paramètre pour gérer le nombre de lignes en cours de lecture et traités en même temps, pour atténuer les problèmes de performances de serveur. 
  
Les deux le `@r_rowsPerRead` paramètre de diffusion en continu et la `@parallel` argument doit être considérée comme indicateurs. Pour l’indicateur à appliquer, il doit être possible de générer un plan de requête SQL qui inclut un traitement parallèle. Si ce n’est pas possible, le traitement parallèle ne peut pas être activé.  
  
> [!NOTE]  
>  Traitement parallèle et diffusion en continu sont pris en charge uniquement dans Enterprise Edition. Vous pouvez inclure les paramètres dans vos requêtes dans l’Édition Standard sans déclencher d’erreur, mais les paramètres n’ont aucun effet et les scripts R exécutés dans un processus unique.  
  
## <a name="restrictions"></a>Restrictions  


### <a name="data-types"></a>Types de données

Types de données suivants ne sont pas pris en charge lorsqu’il est utilisé dans la requête d’entrée ou des paramètres de `sp_execute_external_script` procédure et retour d’une erreur de type non pris en charge.  

Pour résoudre ce problème, **CAST** la colonne ou la valeur à un type pris en charge dans [!INCLUDE[tsql](../../includes/tsql-md.md)] avant son envoi au script externe.  
  
-   **cursor**  
  
-   **timestamp**  
  
-   **datetime2**, **datetimeoffset**, **time**  
  
-   **sql_variant**  
  
-   **text**, **image**  
  
-   **xml**  
  
-   **hierarchyid**, **geometry**, **geography**  
  
-   Types CLR définis par l’utilisateur

En règle générale, les résultats qui ne peut pas être mappé à un [!INCLUDE[tsql](../../includes/tsql-md.md)] type de données, est sortie comme NULL.  

### <a name="restrictions-specific-to-r"></a>Restrictions spécifiques à R

Si l’entrée inclut **datetime** les valeurs qui ne répondent pas la plage autorisée des valeurs dans R, les valeurs sont converties en **NA**. Cela est nécessaire car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet une plus grande plage de valeurs qu’est pris en charge dans le langage R.

Valeurs flottantes (par exemple, `+Inf`, `-Inf`, `NaN`) ne sont pas pris en charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] même si les deux langages utilisent la norme IEEE 754. Comportement actuel envoie uniquement les valeurs à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] directement ; par conséquent, le client SQL dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] génère une erreur. Par conséquent, ces valeurs sont converties en **NULL**.

## <a name="permissions"></a>Autorisations

Requiert **EXECUTE ANY EXTERNAL SCRIPT** autorisation de base de données.  

## <a name="examples"></a>Exemples

Cette section contient des exemples d’utilisation de la procédure stockée à exécuter des scripts R ou Python à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)].

### <a name="a-return-an-r-data-set-to-sql-server"></a>A. Retourne un jeu de données R à SQL Server  

L’exemple suivant crée une procédure stockée qui utilise **sp_execute_external_script** pour renvoyer le jeu de données Iris inclus avec R pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

```sql
DROP PROC IF EXISTS get_iris_dataset;  
go  
CREATE PROC get_iris_dataset
AS  
BEGIN  
 EXEC   sp_execute_external_script  
       @language = N'R'  
     , @script = N'iris_data <- iris;'
     , @input_data_1 = N''  
     , @output_data_1_name = N'iris_data'
     WITH RESULT SETS (("Sepal.Length" float not null,   
           "Sepal.Width" float not null,  
        "Petal.Length" float not null,   
        "Petal.Width" float not null, "Species" varchar(100)));  
END;
GO
```

### <a name="b-generate-an-r-model-based-on-data-from-sql-server"></a>B. Générer un modèle R en fonction des données à partir de SQL Server  

L’exemple suivant crée une procédure stockée qui utilise **sp_execute_external_script** pour générer un modèle iris et de retourner le modèle à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

> [!NOTE]
>  Cet exemple requiert l’installation préalable du package e1071. Pour plus d’informations, consultez [installer des packages R supplémentaires sur SQL Server](../../advanced-analytics/r/install-additional-r-packages-on-sql-server.md).

```sql
DROP PROC IF EXISTS generate_iris_model;
GO
CREATE PROC generate_iris_model
AS
BEGIN
 EXEC sp_execute_external_script  
      @language = N'R'  
     , @script = N'  
          library(e1071);  
          irismodel <-naiveBayes(iris_data[,1:4], iris_data[,5]);  
          trained_model <- data.frame(payload = as.raw(serialize(irismodel, connection=NULL)));  
'  
     , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from iris_data'  
     , @input_data_1_name = N'iris_data'  
     , @output_data_1_name = N'trained_model'  
    WITH RESULT SETS ((model varbinary(max)));  
END;
GO
```

Pour générer un modèle semblable à l’aide de Python, vous devez remplacer l’identificateur de langage `@language=N'R'` par `@language = N'Python'` et apporter les modifications nécessaires à l’argument `@script`. Autrement, tous les paramètres fonctionnent de la même manière que pour R.

### <a name="c-create-a-python-model-and-generate-scores-from-it"></a>C. Créer un modèle de Python et générer des scores à partir de celui-ci

Cet exemple illustre l’utilisation de sp\_execute\_external\_script pour générer des scores sur un modèle Python simple. 

```sql
CREATE PROCEDURE [dbo].[py_generate_customer_scores]
AS
BEGIN

-- Input query to generate the customer data
DECLARE @input_query NVARCHAR(MAX) = N'SELECT customer, orders, items, cost FROM dbo.Sales.Orders'

EXEC sp_execute_external_script @language = N'Python', @script = N'
import pandas as pd
from sklearn.cluster import KMeans

# Get data from input query
customer_data = my_input_data

# Define the model
n_clusters = 4
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orders","items","cost"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
, @input_data_1 = @input_query
, @input_data_1_name = N'my_input_data'
WITH RESULT SETS (("CustomerID" int, "Orders" float,"Items" float,"Cost" float,"ClusterResult" float));
END;
GO
```

Les en-têtes de colonne utilisés dans le code Python ne sont pas sortis dans SQL Server. Par conséquent, vous devez utiliser l’instruction WITH RESULTS pour spécifier les noms de colonnes et les types de données que SQL doit utiliser.

Pour calculer les scores, vous pouvez également utiliser la fonction [PREDICT](../../t-sql/queries/predict-transact-sql.md) native, qui est généralement plus rapide car elle évite d’appeler le runtime Python ou R.

## <a name="see-also"></a>Voir aussi

 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Les Types de données et les bibliothèques de Python](../../advanced-analytics/python/python-libraries-and-data-types.md)  
 [Bibliothèques R et les Types de données R](../../advanced-analytics/r/r-libraries-and-data-types.md)  
 [SQL Server R Services](../../advanced-analytics/r/sql-server-r-services.md)   
 [Problèmes connus relatifs à SQL Server d’apprentissage Services](../../advanced-analytics/known-issues-for-sql-server-machine-learning-services.md)   
 [CRÉER une bibliothèque externe &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-library-transact-sql.md)  
 [sp_prepare &#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [external scripts enabled (option de configuration de serveur)](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [SQL Server, objet External Scripts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)  
[sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
[sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md) 
