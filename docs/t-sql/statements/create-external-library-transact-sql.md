---
title: CREATE EXTERNAL LIBRARY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/05/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL LIBRARY
- CREATE_EXTERNAL_LIBRARY_TSQL
- EXTERNAL LIBRARY
- EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL LIBRARY
author: jeannt
ms.author: jeannt
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 45cca2c91b9cbdd870a1883ddec2210928ac9055
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-external-library-transact-sql"></a>CREATE EXTERNAL LIBRARY (Transact-SQL)  

[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]  

Charge des packages R vers une base de données à partir du chemin de fichier ou du flux d’octets spécifié.

Cette instruction sert de mécanisme générique permettant à l’administrateur de base de données de charger des artefacts nécessaires pour tout nouveau runtime de langage externe (R, Python, Java, etc.) et plateformes de système d’exploitation pris en charge par [!INCLUDE[ssnoversion](../../includes/ssnoversion.md)]. 

Actuellement, seul le langage R et la plateforme Windows sont pris en charge. La prise en charge de Python et de Linux est prévue pour une version ultérieure.

## <a name="syntax"></a>Syntaxe

```text
CREATE EXTERNAL LIBRARY library_name  
    [ AUTHORIZATION owner_name ]  
FROM <file_spec> [,…2]  
WITH ( LANGUAGE = 'R' )  
[ ; ]  

<file_spec> ::=  
{  
(CONTENT = { <client_library_specifier> | <library_bits> }  
[, PLATFORM = WINDOWS ])  
}  

<client_library_specifier> :: =  
  '[\\computer_name\]share_name\[path\]manifest_file_name'  
| '[local_path\]manifest_file_name'  
| '<relative_path_in_external_data_source>'  

<library_bits> :: =  
{ varbinary_literal | varbinary_expression }  
```

### <a name="arguments"></a>Arguments

**library_name**

Les bibliothèques sont ajoutées à la base de données étendue à l’utilisateur. Les noms de bibliothèques doivent être uniques dans le contexte d’un utilisateur ou d’un propriétaire donné. Par exemple, deux utilisateurs **RUser1** et **RUser2** peuvent chacun charger la bibliothèque R `ggplot2` individuellement et séparément. Toutefois, si **RUser1** souhaitait charger une version plus récente de `ggplot2`, la deuxième instance devrait être nommée différemment ou remplacer la bibliothèque existante. 

Les noms de bibliothèques ne peuvent pas être assignés arbitrairement. Le nom de la bibliothèque doit être identique au nom requis pour charger la bibliothèque R à partir de R.

**owner_name**

Spécifie le nom de l’utilisateur ou du rôle propriétaire de la bibliothèque externe. En l'absence de spécification, la propriété revient à l'utilisateur actuel.

Les bibliothèques appartenant au propriétaire de la base de données sont considérées comme globales pour la base de données et le runtime. En d’autres termes, les propriétaires de bases de données peuvent créer des bibliothèques qui contiennent un ensemble commun de bibliothèques ou de packages partagés par de nombreux utilisateurs. Quand une bibliothèque externe est créée par un utilisateur autre que l’utilisateur `dbo`, la bibliothèque externe est privée pour cet utilisateur uniquement.

Quand l’utilisateur **RUser1** exécute un script R, la valeur de `libPath` peut contenir plusieurs chemins. Le premier est toujours le chemin de la bibliothèque partagée créée par le propriétaire de la base de données. La deuxième partie de `libPath` spécifie le chemin contenant les packages chargés individuellement par **RUser1**.

**file_spec**

Spécifie le contenu du package pour une plateforme spécifique. Un seul artefact de fichier par plateforme est pris en charge.

Le fichier peut être spécifié sous la forme d’un chemin local ou d’un chemin réseau.

Si vous le souhaitez, vous pouvez spécifier une plateforme de système d’exploitation pour le fichier. Un seul artefact de fichier ou contenu est autorisé pour chaque plateforme de système d’exploitation pour un langage ou un runtime spécifique.

**library_bits**

Spécifie le contenu du package en tant que littéral hexadécimal, similaire aux assemblys. 

Cette option est utile si vous devez créer une bibliothèque ou modifier une bibliothèque existante (et que vous disposez des autorisations nécessaires), mais que le système de fichiers sur le serveur est restreint et vous ne pouvez pas copier les fichiers de bibliothèque vers un emplacement accessible au serveur.

**PLATFORM = WINDOWS**

Spécifie la plateforme pour le contenu de la bibliothèque. La valeur par défaut est la plateforme hôte sur laquelle SQL Server est en cours d’exécution. Par conséquent, l’utilisateur n’a pas à spécifier la valeur. Elle est obligatoire dans le cas où plusieurs plateformes sont prises en charge, ou quand l’utilisateur a besoin de spécifier une autre plateforme. 

Dans SQL Server 2017, Windows est la seule plateforme prise en charge.

## <a name="remarks"></a>Notes 

Pour le langage R, lors de l’utilisation d’un fichier, les packages doivent être préparés sous la forme de fichiers d’archives compressés avec l’extension .ZIP pour Windows. Actuellement, seule la plateforme Windows est prise en charge. 

L’instruction `CREATE EXTERNAL LIBRARY` charge les bits de la bibliothèque vers la base de données. La bibliothèque est installée quand un utilisateur exécute un script externe à l’aide de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) et appelle le package ou la bibliothèque.

Les bibliothèques chargées vers l’instance peuvent être publiques ou privées. Si la bibliothèque est créée par un membre de `dbo`, elle est publique et peut être partagée avec tous les utilisateurs. Dans le cas contraire, la bibliothèque est privée pour cet utilisateur uniquement.

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CREATE EXTERNAL LIBRARY`. Par défaut, les utilisateurs qui possèdent **dbo**, membre du rôle **db_owner**, disposent des autorisations nécessaires pour créer une bibliothèque externe. En ce qui concerne les autres utilisateurs, vous devez leur en donner l’autorisation explicitement à l’aide une instruction [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-database-permissions-transact-sql), en spécifiant le privilège CREATE EXTERNAL LIBRARY.

La modification d’une bibliothèque nécessite l’autorisation distincte `ALTER ANY EXTERNAL LIBRARY`.

## <a name="examples"></a>Exemples

### <a name="a-add-an-external-library-to-a-database"></a>A. Ajouter une bibliothèque externe à une base de données  

L’exemple suivant ajoute une bibliothèque externe nommée `customPackage` à une base de données.

```sql
CREATE EXTERNAL LIBRARY customPackage
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip') WITH (LANGUAGE = 'R');
```

Une fois la bibliothèque chargée vers l’instance, un utilisateur exécute la procédure `sp_execute_external_script` pour installer la bibliothèque.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
```

### <a name="b-installing-packages-with-dependencies"></a>B. Installation de packages avec des dépendances

Si le package que vous souhaitez installer a des dépendances, vous devez impérativement analyser les dépendances de premier niveau et de deuxième niveau, et vérifier que tous les packages nécessaires sont disponibles _avant_ d’essayer d’installer le package cible.

Par exemple, supposez que vous souhaitez installer un nouveau package, `packageA` :

+ `packageA` a une dépendance envers `packageB`.
+ `packageB` a une dépendance envers `packageC`.

Pour que l’installation de `packageA` réussisse, vous devez créer des bibliothèques pour `packageB` et `packageC` en même temps que vous ajoutez `packageA` à SQL Server. Veillez à vérifier également les versions de package nécessaires.

Dans la pratique, les dépendances de package pour les packages populaires sont généralement beaucoup plus compliquées que cet exemple simple. Par exemple, **ggplot2** peut avoir besoin de plus de 30 packages, qui eux-mêmes peuvent réclamer des packages supplémentaires non disponibles sur le serveur. Tout package manquant ou dont la version est incorrecte peut provoquer l’échec de l’installation.

Sachant qu’il peut être difficile de déterminer toutes les dépendances en examinant simplement le manifeste du package, nous vous recommandons d’utiliser un package comme [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) pour identifier tous les packages potentiellement nécessaires à l’installation.

+ Chargez le package cible et ses dépendances. Tous les fichiers doivent être dans un dossier accessible au serveur.

    ```sql
    CREATE EXTERNAL LIBRARY packageA 
    FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageA.zip') 
    WITH (LANGUAGE = 'R'); 
    GO

    CREATE EXTERNAL LIBRARY packageB FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageB.zip') 
    WITH (LANGUAGE = 'R');
    GO

    CREATE EXTERNAL LIBRARY packageC FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageC.zip') 
    WITH (LANGUAGE = 'R');
    GO
    ```

+ Installez d’abord les packages nécessaires.

    Si un package nécessaire a déjà été chargé vers l’instance, vous n’avez pas besoin de l’ajouter à nouveau. Vérifiez simplement si le package existant est la version correcte. 
    
    Les packages nécessaires `packageC` et `packageB` sont installés dans l’ordre correct, quand `sp_execute_external_script` est exécuté pour la première fois afin d’installer le package `packageA`.

    Toutefois, si un package nécessaire n’est pas disponible, l’installation du package cible `packageA` échoue.

    ```sql
    EXEC sp_execute_external_script 
    @language =N'R', 
    @script=N'
    # load the desired package packageA
    library(packageA)
    print(packageVersion("packageA"))
    '
    ```

### <a name="c-create-a-library-from-a-byte-stream"></a>C. Créer une bibliothèque à partir d’un flux d’octets

Si vous n’êtes pas en mesure d’enregistrer les fichiers du package à un emplacement sur le serveur, vous pouvez transmettre son contenu dans une variable. L’exemple suivant crée une bibliothèque en passant les bits sous forme de littéral hexadécimal.

```SQL
CREATE EXTERNAL LIBRARY customLibrary FROM (CONTENT = 0xabc123) WITH (LANGUAGE = 'R');
```

> [!NOTE]
> Cet exemple de code montre uniquement la syntaxe ; la valeur binaire de `CONTENT =` a été tronquée pour des raisons de clarté et ne crée pas une bibliothèque fonctionnelle. Son véritable contenu serait beaucoup plus long.

### <a name="d-change-an-existing-package-library"></a>D. Changer une bibliothèque de package existante

Vous pouvez utiliser l’instruction DDL `ALTER EXTERNAL LIBRARY` pour ajouter du nouveau contenu à la bibliothèque ou modifier le contenu existant de la bibliothèque. La modification d’une bibliothèque existante nécessite l’autorisation `ALTER ANY EXTERNAL LIBRARY`.

Pour plus d’informations, consultez [ALTER EXTERNAL LIBRARY](alter-external-library-transact-sql.md).

## <a name="see-also"></a>Voir aussi

[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)  
