---
title: PREDICT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2018
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|queries
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: jeannt
ms.author: jeannt
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: b027530565b9b138d5a8f9559e3e60e0331dd3af
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708887"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)  
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Génère une valeur prédite ou des scores calculés à partir d’un modèle stocké.  

## <a name="syntax"></a>Syntaxe

```
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>  
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

### <a name="arguments"></a>Arguments

**model**

Le paramètre `MODEL` permet de spécifier le modèle utilisé pour calculer les scores ou effectuer une prédiction. Le modèle est spécifié sous forme de variable, de littéral ou d’expression scalaire.

L’objet modèle peut être créé à l’aide d’un outil comme R ou Python.

**data**

Le paramètre DATA permet de spécifier les données utilisées pour calculer les scores ou effectuer une prédiction. Les données sont spécifiées sous la forme d’une source de table dans la requête. La source de table peut être une table, un alias de table, un alias d’expression de table commune (CTE), une vue ou une fonction table.

**parameters**

Le paramètre PARAMETERS permet de spécifier des paramètres définis par l’utilisateur facultatifs, à utiliser pour le calcul des scores ou la prédiction.

Le nom de chaque paramètre est propre au type de modèle. Par exemple, la fonction [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) dans RevoScaleR prend en charge le paramètre `@computeResiduals`, qui indique si les résiduels doivent être calculés en même temps que les scores d’un modèle de régression logistique. Si vous appelez un modèle compatible, vous pouvez passer ce nom de paramètre et une valeur TRUE ou FALSE à la fonction `PREDICT`.

> [!NOTE]
> Cette option ne fonctionne pas dans les préversions de SQL Server 2017.

**WITH ( <result_set_definition> )**

La clause WITH permet de spécifier le schéma de la sortie retournée par la fonction `PREDICT`.

En plus des colonnes retournées par la fonction `PREDICT` proprement dite, toutes les colonnes qui font partie des données d’entrée peuvent être utilisées dans la requête.

### <a name="return-values"></a>Valeurs retournées

Aucun schéma prédéfini n’est disponible. SQL Server ne valide pas le contenu du modèle, ni les valeurs de colonne retournées.

- La fonction `PREDICT` passe par les colonnes en tant qu’entrée.
- La fonction `PREDICT` génère également de nouvelles colonnes, dont le nombre et le type de données dépendent du type de modèle qui a été utilisé pour la prédiction.

Les messages d’erreur liés aux données, au modèle ou au format de colonne sont retournés par la fonction de prédiction sous-jacente qui est associée au modèle.

- Pour RevoScaleR, la fonction équivalente est [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict)  
- Pour MicrosoftML, la fonction équivalente est [rxPredict.mlModel](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxpredict)  

La fonction `PREDICT` ne permet pas d’afficher la structure de modèle interne. Si vous souhaitez comprendre le contenu du modèle lui-même, vous devez charger l’objet modèle, le désérialiser et utiliser le code R approprié pour analyser le modèle.

## <a name="remarks"></a>Notes 

La fonction `PREDICT` est prise en charge dans toutes les éditions de SQL Server, y compris Linux, et dans Azure SQL Database, indépendamment de l’activation d’autres fonctionnalités d’apprentissage automatique. Toutefois, vous devez utiliser SQL Server 2017 ou une version ultérieure. 

Vous n’avez pas besoin d’installer R, Python ou un autre langage d’apprentissage automatique sur le serveur pour utiliser la fonction `PREDICT`. Vous pouvez effectuer l’apprentissage du modèle dans un autre environnement et l’enregistrer dans une table SQL Server pour l’utiliser avec `PREDICT`, ou appeler le modèle à partir d’une autre instance de SQL Server qui contient le modèle enregistré.

### <a name="supported-algorithms"></a>Algorithmes pris en charge

Le modèle que vous utilisez doit avoir été créé à l’aide d’un des algorithmes pris en charge fournis dans le package RevoScaleR. Pour obtenir la liste des modèles pris en charge, consultez [Calcul des scores en temps réel](../../advanced-analytics/real-time-scoring.md).

### <a name="permissions"></a>Autorisations

Aucune autorisation n’est requise pour `PREDICT`. Cependant, l’utilisateur doit avoir l’autorisation `EXECUTE` sur la base de données, ainsi que l’autorisation d’effectuer des requêtes sur les données utilisées comme entrées. L’utilisateur doit également pouvoir lire le modèle à partir d’une table, si le modèle a été stocké dans une table.

## <a name="examples"></a>Exemples

Les exemples suivants illustrent la syntaxe à utiliser pour appeler `PREDICT`.

### <a name="call-a-stored-model-and-use-it-for-prediction"></a>Appeler un modèle stocké et l’utiliser pour la prédiction

Cet exemple appelle un modèle de régression logistique existant, stocké dans la table [models_table]. Il obtient le dernier modèle appris, à l’aide d’une instruction SELECT, puis il passe le modèle binaire à la fonction PREDICT. Les valeurs d’entrée représentent les fonctionnalités et la sortie représente la classification assignée par le modèle.

```sql
DECLARE @logit_model varbinary(max) = "SELECT TOP 1 [model_binary] from [models_table] ORDER BY [trained_date] DESC";
DECLARE @input_qry = "SELECT ID, [Gender], [Income] from NewCustomers";

SELECT PREDICT [class]
FROM PREDICT( MODEL = @logit_model,  DATA = @input_qry
WITH (class string);
```

### <a name="using-predict-in-a-from-clause"></a>Utilisation de PREDICT dans une clause FROM

Cet exemple référence la fonction `PREDICT` dans la clause `FROM` d’une instruction `SELECT` :

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @logit_model, 
  DATA = dbo.mytable AS d) WITH (Score float) AS p;
```

L’alias **d** spécifié pour la source de table dans le paramètre `DATA` est utilisé pour référencer les colonnes de dbo.mytable. L’alias **p** spécifié pour la fonction **PREDICT** est utilisé pour référencer les colonnes retournées par la fonction PREDICT.

### <a name="combining-predict-with-an-insert-statement"></a>Combinaison de PREDICT avec une instruction INSERT

Un cas d’usage courant de la prédiction est de calculer un score pour les données d’entrée, puis d’insérer les valeurs prédites dans une table. L’exemple suivant suppose que l’application appelante utilise une procédure stockée pour insérer une ligne contenant la valeur prédite dans une table :

```sql
CREATE PROCEDURE InsertLoanApplication
(@p1 varchar(100), @p2 varchar(200), @p3 money, @p4 int)
AS
BEGIN
  DECLARE @model varbinary(max) = (select model
  FROM scoring_model
  WHERE model_name = 'ScoringModelV1');
  WITH d as ( SELECT * FROM (values(@p1, @p2, @p3, @p4)) as t(c1, c2, c3, c4) )

  INSERT INTO loan_applications (c1, c2, c3, c4, score)
  SELECT d.c1, d.c2, d.c3, d.c4, p.score
  FROM PREDICT(MODEL = @model, DATA = d) WITH(score float) as p;
END;
```

Si la procédure accepte plusieurs lignes par le biais d’un paramètre table, elle peut être écrite comme suit :

```sql
CREATE PROCEDURE InsertLoanApplications (@new_applications dbo.loan_application_type)
AS
BEGIN
  DECLARE @model varbinary(max) = (SELECT model_bin FROM scoring_models WHERE model_name = 'ScoringModelV1');
  INSERT INTO loan_applications (c1, c2, c3, c4, score)
  SELECT d.c1, d.c2, d.c3, d.c4, p.score
  FROM PREDICT(MODEL = @model, DATA = @new_applications as d)
  WITH (score float) as p;
END;
```

### <a name="creating-an-r-model-and-generating-scores-using-optional-model-parameters"></a>Création d’un modèle R et calcul de scores à l’aide de paramètres de modèle facultatifs

> [!NOTE]
> L’utilisation de l’argument parameters n’est pas pris en charge dans la version Release Candidate 1.

Cet exemple suppose que vous avez créé un modèle de régression logistique avec une matrice de covariance, en appelant RevoScaleR comme ceci :

```R
logitObj <- rxLogit(Kyphosis ~ Age + Start + Number, data = kyphosis, covCoef = TRUE)
```

Si vous stockez le modèle dans SQL Server au format binaire, vous pouvez utiliser la fonction PREDICT pour générer non seulement des prédictions, mais aussi certaines informations supplémentaires prises en charge par le type de modèle, telles qu’une erreur ou des intervalles de confiance.

Le code suivant illustre le même appel de [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) dans R :

```R
rxPredict(logitObj, data = new_kyphosis_data, computeStdErr = TRUE, interval = "confidence")
```

Un appel équivalent à l’aide de la fonction `PREDICT` fournit également le score (valeur prédite), l’erreur et les intervalles de confiance :

```sql
SELECT d.Age, d.Start, d.Number, p.pred AS Kyphosis_Pred, p.stdErr, p.pred_lower, p.pred_higher
FROM PREDICT( MODEL = @logitObj,  DATA = new_kyphosis_data AS d,
  PARAMETERS = N'computeStdErr bit, interval varchar(30)',
  computeStdErr = 1, interval = 'confidence')
WITH (pred float, stdErr float, pred_lower float, pred_higher float) AS p;
```
