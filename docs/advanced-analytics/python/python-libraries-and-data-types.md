---
title: Python bibliothèques et types de données dans l’apprentissage de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: f8dfa7f343a3a179b05b624a083238e08011c4a5
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="python-libraries-and-data-types"></a>Python bibliothèques et types de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les bibliothèques Python fournis avec SQL Server Machine Learning Services (de-de base de données) et les installations (autonome).

Cet article répertorie également les types de données non pris en charge, et répertorie le type de données pour les conversions qui peuvent être effectuées implicitement les données passées entre Python et SQL Server.

## <a name="python-version"></a>Version Python

SQL Server 2017 CTP 2.0 comprend une partie de la distribution Anaconda et Python 3.6.

Un sous-ensemble des fonctionnalités RevoScaleR (rxLinMod, rxLogit, rxPredict, rxDTrees, rxBTrees, cette opération peut prendre quelques autres) est fourni à l’aide des API de Python, à l’aide d’un nouveau package Python **revoscalepy**. Vous pouvez utiliser ce package pour travailler avec des données à l’aide des trames de données Pandas, des fichiers XDF ou des requêtes de données SQL.

Pour plus d’informations, consultez [revoscalepy de nouveautés ?](what-is-revoscalepy.md).

## <a name="python-and-sql-data-types"></a>Python et les Types de données SQL

Python prend en charge un nombre limité de types de données par rapport à SQL Server.

Par conséquent, lorsque vous utilisez des données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans les scripts Python, des données peuvent être converties implicitement en un type de données compatible. Toutefois, souvent une conversion exacte ne peut pas être effectuée automatiquement, et une erreur est retournée.

Ce tableau répertorie les conversions implicites sont fournies. Autres types de données ne sont pas pris en charge.

|SQLtype|Type de Python|
|-|-|
|**bigint**|`numeric`|
|**binaire**|`raw`|
|**bit**|`bool`|
|**char**|`str`|
|**float**|`float64`|
|**int**|`int32`|
|**nchar**|`str`|
|**nvarchar**|`str`|
|**nvarchar(max)**|`str`|
|**real**|`float32`|
|**smallint**|`int16`|
|**tinyint**|`uint8`|
|**varbinary**|`bytes`|
|**varbinary(max)**|`bytes`|
|**varchar(n)**|`str`|
|**varchar(max)**|`str`|



