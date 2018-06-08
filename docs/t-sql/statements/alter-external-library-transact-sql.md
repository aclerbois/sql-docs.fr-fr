---
title: ALTER EXTERNAL LIBRARY (Transact-SQL) | Microsoft Docs
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
- ALTER EXTERNAL LIBRARY
- ALTER_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL LIBRARY
author: jeannt
ms.author: jeannt
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 5a7a1d38efc0ea14ed00c3e49205c36f5953e73f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-external-library-transact-sql"></a>ALTER EXTERNAL LIBRARY (Transact-SQL)  

[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Modifie le contenu d’une bibliothèque de package externe existante.

## <a name="syntax"></a>Syntaxe

```text
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = 'R' )
[ ; ]

<file_spec> ::=
{
(CONTENT = { <client_library_specifier> | <library_bits> | NONE}
[, PLATFORM = WINDOWS )
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

Spécifie le nom d’une bibliothèque de package existante. Les bibliothèques sont limitées à l’utilisateur. Les noms de bibliothèques doivent être uniques dans le contexte d’un utilisateur ou d’un propriétaire donné.

Le nom de la bibliothèque ne peut pas être assigné arbitrairement. Autrement dit, vous devez utiliser le nom que le runtime appelant attend quand il charge le package.

**owner_name**

Spécifie le nom de l’utilisateur ou du rôle propriétaire de la bibliothèque externe.

**file_spec**

Spécifie le contenu du package pour une plateforme spécifique. Un seul artefact de fichier par plateforme est pris en charge.

Le fichier peut être spécifié sous la forme d’un chemin local ou d’un chemin réseau. Si l’option de source de données est spécifiée, le nom de fichier peut être un chemin relatif au conteneur référencé dans `EXTERNAL DATA SOURCE`.

Si vous le souhaitez, vous pouvez spécifier une plateforme de système d’exploitation pour le fichier. Un seul artefact de fichier ou contenu est autorisé pour chaque plateforme de système d’exploitation pour un langage ou un runtime spécifique.

**library_bits**

Spécifie le contenu du package en tant que littéral hexadécimal, similaire aux assemblys. 

Cette option est utile si vous avez l’autorisation nécessaire pour modifier une bibliothèque, mais que l’accès aux fichiers sur le serveur est restreint et que vous ne pouvez pas enregistrer le contenu à un emplacement accessible au serveur.

Au lieu de cela, vous pouvez passer le contenu du package en tant que variable au format binaire.

**PLATFORM = WINDOWS**

Spécifie la plateforme pour le contenu de la bibliothèque. Cette valeur est nécessaire lors de la modification d’une bibliothèque existante pour ajouter une autre plateforme. Windows est la seule plateforme prise en charge.

## <a name="remarks"></a>Notes 

Pour le langage R, les packages doivent être préparés sous la forme de fichiers d’archives compressés avec l’extension .ZIP pour Windows. Actuellement, seule la plateforme Windows est prise en charge.  

L’instruction `ALTER EXTERNAL LIBRARY` charge uniquement les bits de la bibliothèque vers la base de données. La bibliothèque modifiée est installée quand un utilisateur exécute dans [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) du code qui appelle la bibliothèque.

## <a name="permissions"></a>Autorisations

Par défaut, l’utilisateur **dbo** ou n’importe quel membre du rôle **db_owner** a l’autorisation d’exécuter ALTER EXTERNAL LIBRARY. Par ailleurs, l’utilisateur qui a créé la bibliothèque externe peut la modifier.

## <a name="examples"></a>Exemples

Les exemples suivants modifient une bibliothèque externe nommée `customPackage`.

### <a name="a-replace-the-contents-of-a-library-using-a-file"></a>A. Remplacer le contenu d’une bibliothèque à l’aide d’un fichier

L’exemple suivant modifie une bibliothèque externe nommée `customPackage`, à l’aide d’un fichier compressé qui contient les bits mis à jour.

```sql
ALTER EXTERNAL LIBRARY customPackage 
SET 
  (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip')
WITH (LANGUAGE = 'R');
```

Pour installer la bibliothèque mise à jour, exécutez la procédure stockée `sp_execute_external_script`.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
;
```

### <a name="b-alter-an-existing-library-using-a-byte-stream"></a>B. Modifier une bibliothèque existante à l’aide d’un flux d’octets

L’exemple suivant modifie la bibliothèque existante en passant les nouveaux bits sous forme de littéral hexadécimal.

```SQL
ALTER EXTERNAL LIBRARY customLibrary 
SET (CONTENT = 0xabc123) WITH (LANGUAGE = 'R');
```

> [!NOTE]
> Cet exemple de code montre uniquement la syntaxe ; la valeur binaire de `CONTENT =` a été tronquée pour des raisons de clarté et ne crée pas une bibliothèque fonctionnelle. Son véritable contenu serait beaucoup plus long.

## <a name="see-also"></a>Voir aussi

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md) 
