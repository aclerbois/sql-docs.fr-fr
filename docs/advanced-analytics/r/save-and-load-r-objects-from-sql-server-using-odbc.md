---
title: Enregistrer et charger des objets R à partir de SQL Server à l’aide de ODBC | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 17f9ebc151e7112b04766ea1c644aad0a32bc580
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="save-and-load-r-objects-from-sql-server-using-odbc"></a>Enregistrer et charger des objets R à partir de SQL Server à l’aide d’ODBC
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

SQL Server R Services peut stocker des objets R sérialisés dans une table, puis charger l’objet à partir de la table en fonction des besoins, sans que vous ayez à réexécuter le code R ou à reformer le modèle. Cette capacité à enregistrer les objets R dans une base de données est essentielle pour les scénarios tels que la formation et l’enregistrement d’un modèle, puis son utilisation ultérieure à des fins de notation ou d’analyse.

Pour améliorer les performances de cette étape critique, le package **RevoScaleR** inclut désormais de nouvelles fonctions de sérialisation et de désérialisation qui améliorent sensiblement les performances et stockent l’objet de manière plus compacte. Cet article décrit ces fonctions et comment les utiliser.

## <a name="overview"></a>Vue d'ensemble

Le package **RevoScaleR** inclut désormais de nouvelles fonctions qui facilitent l’enregistrement des objets R dans SQL Server, puis la lecture des objets à partir de la table SQL Server. En général, chaque appel de fonction utilise un magasin de valeur de clé simple, dans laquelle la clé est le nom de l’objet, et la valeur associée à la clé est l’objet varbinary R doit être déplacé dans ou hors d’une table.

Pour enregistrer des objets R dans SQL Server directement à partir d’un environnement de R, vous devez :

+ établir une connexion à SQL Server à l’aide de la *RxOdbcData* source de données.
+ Appeler les nouvelles fonctions via la connexion ODBC
+ Si vous le souhaitez, vous pouvez spécifier que l’objet ne pas être sérialisé. Ensuite, sélectionnez un nouvel algorithme de compression à utiliser au lieu de l’algorithme de compression par défaut.

Par défaut, tout objet que vous appelez à partir de R pour passer à SQL Server est sérialisé et compressé. Inversement, quand vous chargez un objet à partir d’une table SQL Server pour l’utiliser dans votre code R, l’objet est désérialisé et décompressé.

## <a name="list-of-new-functions"></a>Liste des nouvelles fonctions

- `rxWriteObject` écrit un objet R dans SQL Server à l’aide de la source de données ODBC.

- `rxReadObject` lit un objet R à partir d’une base de données SQL Server, à l’aide d’une source de données ODBC.

- `rxDeleteObject` supprime un objet R de la base de données SQL Server spécifiée dans la source de données ODBC. Si plusieurs objets sont identifiés par la combinaison clé/version, ils sont tous supprimés.

- `rxListKeys` énumère comme paires clé-valeur tous les objets disponibles. Cela vous permet de déterminer les noms et les versions des objets R.

Pour obtenir des informations détaillées sur la syntaxe de chaque fonction, utilisez l’aide de R. Détails sont également disponibles dans le [référence de ScaleR](https://docs.microsoft.com/r-server/r-reference/revoscaler/revoscaler).

## <a name="how-to-store-r-objects-in-sql-server-using-odbc"></a>Guide pratique pour stocker des objets R dans SQL Server à l’aide d’ODBC

Cette procédure illustre comment utiliser les nouvelles fonctions pour créer un modèle et l’enregistrer dans SQL Server.

1. Configurez la chaîne de connexion pour SQL Server.
   ```R
   conStr <- 'Driver={SQL Server};Server=localhost;Database=storedb;Trusted_Connection=true'
   ```
2. Créez un objet de source de données *rxOdbcData* dans R à l’aide de la chaîne de connexion.
   ```R
   ds <- RxOdbcData(table="robjects", connectionString=conStr)
   ```

3. Supprimez la table si elle existe déjà et que vous ne souhaitez pas effectuer le suivi des anciennes versions des objets.

   ```R
   if(rxSqlServerTableExists(ds@table, ds@connectionString)) {
       rxSqlServerDropTable(ds@table, ds@connectionString)
       }
   ```
   
4. Définissez une table qui peut être utilisée pour stocker des objets binaires.

   ```R
   ddl <- paste(" CREATE TABLE [", ds@table, "] 
      (","  [id] varchar(200) NOT NULL,
       "," [value] varbinary(max),
       "," CONSTRAINT unique_id UNIQUE (id))", 
       sep = "") 
   ```
5. Ouvrez la connexion ODBC pour créer la table puis, quand l’instruction DDL est terminée, fermez la connexion.

   ```R
    rxOpen(ds, "w") 
    rxExecuteSQLDDL(ds, ddl) 
    rxClose(ds)
    ```
6. Générez les objets R que vous souhaitez stocker.

   ```R
   infertLogit <- rxLogit(case ~ age + parity + education + spontaneous + induced, 
     data = infert)
   ```
6. Utilisez l’objet *RxOdbcData* créé précédemment pour enregistrer le modèle dans la base de données.

   ```R
   rxWriteObject(ds, "logit.model", infertLogit)
   ```

## <a name="how-to-read-r-objects-from-sql-server-using-odbc"></a>Guide pratique pour lire des objets R à partir de SQL Server à l’aide d’ODBC

Cette procédure illustre comment utiliser les nouvelles fonctions pour charger un modèle à partir de SQL Server.

1. Configurez la chaîne de connexion pour SQL Server.

   ```R
   conStr2 <- 'Driver={SQL Server};Server=localhost;Database=storedb;Trusted_Connection=true'
   ```
2. Créez un objet de source de données *rxOdbcData* dans R à l’aide de la chaîne de connexion.

   ```R
   ds <- RxOdbcData(table="robjects", connectionString=conStr2)
   ```
3. Lisez le modèle à partir de la table en spécifiant son nom d’objet R.

   ```R
    infertLogit2 <- rxReadObject(ds, "logit.model")
   ```
