---
title: Sys.external_libraries (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/05/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- external_libraries
- external_libraries_TSQL
- sys.external_libraries
- sys.external_libraries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_libraries catalog view
author: jeannt
ms.author: jeannt
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 110f514c4688536decfd29c412ce310746cd972e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysexternallibraries-transact-sql"></a>Sys.external_libraries (Transact-SQL)  
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]


Prend en charge la gestion des bibliothèques de package liés à des runtimes externes tels que R ou Python.

## <a name="sysexternallibraries"></a>Sys.external_libraries

Le sys.external_libraries de vue de catalogue répertorie une ligne pour chaque bibliothèque externe qui a été téléchargé dans la base de données.

|Nom de colonne |Type de données |  Description|
|------|------|------|
|external_library_id |int | ID de l’objet de bibliothèque externe. |
|name |sysname |Nom de la bibliothèque externe. Est unique dans la base de données par le propriétaire.|
|principal_id |int |ID du principal qui possède cette bibliothèque externe. |
|langue | sysname | Nom de la langue ou le runtime qui prend en charge de la bibliothèque externe. Les valeurs valides sont « R ». Les runtimes supplémentaires peuvent être ajoutées dans les futures.|
|portée |int |0 pour une portée publique ; 1 pour la portée privée |  
|scope_desc |varchar(7) |Indique si le package est public ou privé|


## <a name="see-also"></a>Voir aussi  
[sys.external_library_files](sys-external-library-files-transact-sql.md)  
[CRÉER UNE BIBLIOTHÈQUE EXTERNE](../../t-sql/statements/create-external-library-transact-sql.md)  
[Gestion des packages pour SQL Server R Services](../../advanced-analytics/r/installing-and-managing-r-packages.md)  
