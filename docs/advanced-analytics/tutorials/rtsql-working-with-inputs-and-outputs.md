---
title: Utilisation des entrées et sorties (R dans démarrage rapide de SQL) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 9b2a493293f79ed823aa78a5a1d16190b5254449
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="working-with-inputs-and-outputs-r-in-sql-quickstart"></a>Utilisation des entrées et sorties (R dans démarrage rapide de SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Lorsque vous souhaitez exécuter le code R dans SQL Server, vous devez encapsuler le script R dans une procédure stockée système, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Cette procédure stockée est utilisée pour démarrer le runtime R dans le contexte de SQL Server, lequel transmet les données à R, gère les sessions utilisateur R en toute sécurité et retourne les résultats au client.

## <a name="bkmk_SSMSBasics"></a>Créer des données de test simples

Créer une petite table de données de test en exécutant l’instruction T-SQL suivante :

```sql
CREATE TABLE RTestData ([col1] int not null) ON [PRIMARY]
INSERT INTO RTestData   VALUES (1);
INSERT INTO RTestData   VALUES (10);
INSERT INTO RTestData   VALUES (100) ;
GO
```

Une fois la table créée, utilisez l’instruction suivante pour interroger la table :
  
```sql
SELECT * FROM RTestData
```

**Résultats**

|col1|
|------|
|*1*|
|*10*|
|*100*|

## <a name="get-the-same-data-using-r-script"></a>Obtenir les mêmes données avec un script R

Une fois la table créée, exécutez l’instruction suivante :

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N' OutputDataSet <- InputDataSet;'
    , @input_data_1 = N' SELECT *  FROM RTestData;'
    WITH RESULT SETS (([NewColName] int NOT NULL));
```

Obtient les données de la table, effectue un aller-retour via le runtime R et renvoyer les valeurs avec le nom de colonne *NewColName*.

**Résultats**

![rsql_basictut_getsamedataR](media/rsql-basictut-getsamedatar.PNG)


**Commentaires**

+ Dans Management Studio, les résultats sous forme de tableau sont retournés dans le volet **Valeurs**. Les messages retournés par le runtime R sont indiqués dans le volet **Messages**.
+ Le paramètre *@language*  définit l’extension de langage à appeler, dans le cas présent, R.
+ Dans le paramètre *@script*, vous définissez les commandes à passer au runtime R. Votre script R entier doit être placé dans cet argument en tant que texte Unicode. Vous pouvez également ajouter le texte à une variable de type **nvarchar**, puis appeler cette variable.
+ Les données retournées par la requête sont passées au runtime R, qui retourne les données à SQL Server sous la forme d’une trame de données.
+ La clause WITH RESULT SETS définit le schéma de la table de données retournée pour SQL Server.

## <a name="change-input-or-output-variables"></a>Modifier les variables d’entrée ou de sortie

L’exemple précédent utilisait les noms de variables d’entrée et de sortie par défaut, _InputDataSet_ et _OutputDataSet_. Pour définir les données d’entrée associées à _InputDatSet_, vous utilisez la variable *@input_data_1*.

Dans cet exemple, les noms des variables de sortie et d’entrée de la procédure stockée ont été remplacés par *SQLOut* et *SQLIn* :

```sql
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N' SQL_out <- SQL_in;'
  , @input_data_1 = N' SELECT 12 as Col;'
  , @input_data_1_name  = N'SQL_In'
  , @output_data_1_name =  N'SQL_Out'
  WITH RESULT SETS (([NewColName] int NOT NULL));
```

Avez, vous obtenez l’erreur, « SQL de l’objet\_dans introuvable » ? C’est parce que R respecte la casse ! Dans l’exemple, le script R utilise les variables *SQL_in* et *SQL_out*, mais les paramètres de la procédure stockée utilisent les variables *SQL_In* et *SQL_Out*.

C’est parce que R respecte la casse. Dans l’exemple, le script R utilise les variables *SQL_in* et *SQL_out*, mais les paramètres de la procédure stockée utilisent les variables *SQL_In* et *SQL_Out*.
Essayez de corriger **uniquement** le *SQL_In* variable et réexécutez la procédure stockée.

Maintenant vous obtenez un **différents** erreur :

```Error
EXECUTE statement failed because its WITH RESULT SETS clause specified 1 result set(s), but the statement only sent 0 result set(s) at run time.
```

Nous vous présentons cette erreur étant donné que vous souhaitez voir souvent lorsque vous testez le nouveau code R. Cela signifie que le script R s’est correctement exécutée, mais a reçu aucune donnée de SQL Server ou reçu de données incorrecte ou inattendues.

Dans ce cas, le schéma de sortie (la ligne commençant par **WITH**) indique qu’une colonne de données de type entier doit être retournée, mais dans la mesure où R a placé les données dans une variable différente, rien n’est renvoyé à SQL Server ; d’où l’erreur. Pour corriger cette erreur, corrigez le nom de la deuxième variable.

**N’oubliez pas ces exigences !**

- Les noms de variables doivent suivre les règles applicables aux identificateurs SQL.
- L’ordre des paramètres est important. Vous devez commencer par spécifier les paramètres obligatoires *@input_data_1* et *@output_data_1* pour pouvoir utiliser les paramètres facultatifs *@input_data_1_name* et *@output_data_1_name*.
- Vous ne pouvez passer qu’un seul dataset d’entrée comme paramètre, et vous ne pouvez retourner qu’un seul dataset. Toutefois, vous pouvez appeler d’autres datasets à partir de votre code R et retourner des sorties d’autres types en plus du dataset. Vous pouvez aussi ajouter le mot clé OUTPUT à n’importe quel paramètre pour qu’il soit retourné avec les résultats. Vous trouverez un exemple simple plus loin dans ce didacticiel.
- L’instruction `WITH RESULT SETS` définit le schéma des données, au profit de SQL Server. Vous devez fournir des types de données compatibles avec SQL pour chaque colonne que vous retournez à partir de R. Vous pouvez aussi utiliser la définition de schéma pour fournir des nouveaux noms de colonnes. Il n’est pas nécessaire d’utiliser les noms de colonnes de la trame de données R. Dans certains cas, cette clause est facultative. Essayez d’omettant et voir ce qui se passe.

## <a name="generate-results-using-r"></a>Générer des résultats à l’aide de R

Vous pouvez également générer des valeurs en utilisant le script R et en laissant vide la chaîne de requête d’entrée dans _@input_data_1_. Vous pouvez également utiliser une instruction SQL SELECT valide comme espace réservé et ne pas utiliser les résultats SQL dans le script R.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
   , @script = N' mytextvariable <- c("hello", " ", "world");
       OutputDataSet <- as.data.frame(mytextvariable);'
   , @input_data_1 = N' SELECT 1 as Temp1'
   WITH RESULT SETS (([Col1] char(20) NOT NULL));
```

**Résultats**

*Col1*
*hello*
<code>   </code>
*world*

## <a name="next-lesson"></a>Leçon suivante

Vous allez examiner certains des problèmes que vous pouvez rencontrer lors du passage des données entre R et SQL Server, comme les conversions implicites et les différences dans les données tabulaires entre R et SQL.

[Types de données et objets de données R et SQL](../tutorials/rtsql-r-and-sql-data-types-and-data-objects.md)
