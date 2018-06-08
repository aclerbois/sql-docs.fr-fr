---
title: sp_rxPredict | Documents Microsoft
ms.custom: ''
ms.date: 07/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_rxPredict
- sp_rxPredict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_rxPredict procedure
author: jeannt
ms.author: jeannt
manager: craigg
ms.openlocfilehash: ede8232f36f42cc2b9758bdee8f50457ebd58dfe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sprxpredict"></a>sp_rxPredict  
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Génère une valeur prédite basée sur un modèle stocké.

Fournit le score sur les modèles d’apprentissage automatique dans quasiment en temps réel. `sp_rxPredict` est une procédure stockée est fournie comme un wrapper pour le `rxPredict` fonctionner dans [RevoScaleR](https://docs.microsoft.com/r-server/r-reference/revoscaler/revoscaler) et [MicrosoftML](https://docs.microsoft.com/r-server/r-reference/microsoftml/microsoftml-package). Il est écrit en C++ et est spécialement optimisée pour les opérations de calcul de score. Il prend en charge les deux R ou Python modèles d’apprentissage de l’ordinateur.

**Cette rubrique s’applique aux**:  
- SQL Server 2017  
- SQL Server 2016 R Services avec la mise à niveau vers Microsoft R Server  

## <a name="syntax"></a>Syntaxe

```
sp_rxPredict  ( @model, @input )
```

### <a name="arguments"></a>Arguments

**model**

Un modèle préformé dans un format pris en charge. 

**input**

Une requête SQL valide

### <a name="return-values"></a>Valeurs retournées

Une colonne de score est retournée, ainsi que toutes les colonnes SQL directes à partir de la source de données d’entrée.
Autres colonnes, telles que de l’intervalle de confiance de score, peut être retourné si l’algorithme prend en charge la génération de ces valeurs.

## <a name="remarks"></a>Notes

Pour activer l’utilisation de la procédure stockée, SQLCLR doit être activé sur l’instance.

> [!NOTE]
> Prendre en compte les implications de sécurité avant d’activer cette option.

L’utilisateur doit avoir `EXECUTE` autorisation sur la base de données.

### <a name="supported-platforms"></a>Plateformes prises en charge

Requiert l’une des éditions suivantes :  
- Services SQL Server 2017 Machine Learning (y compris Microsoft R Server 9.1.0)  
- Apprentissage de Microsoft Server  
- SQL Server R Services 2016, avec une mise à niveau de l’instance de R Services à Microsoft R Server 9.1.0 ou version ultérieure  

### <a name="supported-algorithms"></a>Algorithmes pris en charge

Pour une liste des algorithmes pris en charge, consultez [en temps réel de calcul de score](../../advanced-analytics/real-time-scoring.md).

Les types de modèle suivants sont **pas** pris en charge :  
- Modèles contenant des autres types de transformations de R non pris en charge  
- Modèles à l’aide de la `rxGlm` ou `rxNaiveBayes` algorithmes dans RevoScaleR  
- Modèles PMML  
- Modèles créés à l’aide d’autres bibliothèques R à partir de CRAN ou autres référentiels  
- Modèles contenant n’importe quel autre type de transformation de R autres que ceux répertoriés ici  

## <a name="examples"></a>Exemples

```sql
DECLARE @model = SELECT @model 
FROM model_table 
WHERE model_name = 'rxLogit trained';

EXEC sp_rxPredict @model = @model,
@inputData = N'SELECT * FROM data';
```

En plus d’être une requête SQL valide, les données d’entrée dans *@inputData* doit inclure des colonnes compatibles avec les colonnes dans le modèle stocké.

`sp_rxPredict` prend en charge uniquement les types de colonne de .NET suivants : double, float, short, ushort, long, ulong et chaîne. Vous devrez peut-être filtrer les types non pris en charge dans vos données d’entrée avant de l’utiliser pour calculer les scores en temps réel. 

  Pour plus d’informations sur les types SQL correspondants, consultez [le mappage de Type SQL-CLR](https://msdn.microsoft.com/library/bb386947.aspx) ou [de mappage de données de paramètre CLR](../clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

