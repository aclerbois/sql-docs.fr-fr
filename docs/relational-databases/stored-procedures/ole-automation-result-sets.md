---
title: Jeux de résultats OLE Automation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-ole
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [SQL Server], OLE Automation
- two-dimensional arrays
- one-dimensional arrays
- result sets [SQL Server], OLE Automation
- OLE Automation [SQL Server], result sets
- arrays [SQL Server]
ms.assetid: b2f99e33-2303-427c-94b9-9d55f8e2a6ab
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: dd7c53908291e163c3291afa86c24bd12c0a615b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-automation-result-sets"></a>Jeux de résultats OLE Automation
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Si une propriété ou une méthode OLE Automation retourne des données dans un tableau à une ou deux dimensions, ce tableau est retourné au client sous la forme d'un jeu de résultats :  
  
-   Un tableau à une dimension est retourné au client sous la forme d'un jeu de résultats d'une seule ligne avec autant de colonnes qu'il y a d'éléments dans le tableau. Par exemple, array(10) est retourné sous la forme d'une seule ligne de 10 colonnes.  
  
-   Un tableau à deux dimensions est retourné au client sous la forme d'un jeu de résultats qui contient autant de colonnes qu'il y a d'éléments dans la première dimension du tableau et autant de lignes qu'il y a d'éléments dans la seconde dimension du tableau. Par exemple, array(2,3) est retourné sous la forme de 2 colonnes sur 3 lignes.  
  
 Lorsque la valeur de retour d'une propriété ou d'une méthode est un tableau, sp_OAGetProperty ou sp_OAMethod retourne un jeu de résultats au client. (Les paramètres de sortie de la méthode ne peuvent pas être des tableaux). Ces procédures analysent toutes les valeurs de données dans le tableau pour déterminer les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les longueurs de données appropriés à utiliser pour chaque colonne du jeu de résultats. Pour une colonne particulière, ces procédures utilisent le type de données et la longueur requis pour représenter toutes les valeurs des données de cette colonne.  
  
 Lorsque toutes les valeurs de données d'une colonne partagent le même type de données, ce type est utilisé pour toute la colonne. Lorsque les valeurs de données d'une colonne utilisent des types de données différents, le choix du type de données pour l'ensemble de la colonne est basé sur le tableau suivant. Pour utiliser le tableau suivant, recherchez un type de données le long de l'axe des lignes gauche et un second type de données le long de l'axe des colonnes supérieur. L'intersection de la ligne et de la colonne décrit le type de données de la colonne de jeu de résultats.  
  
||||||||  
|-|-|-|-|-|-|-|  
||**Int**|**float**|**money**|**datetime**|**varchar**|**nvarchar**|  
|**Int**|**Int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="related-content"></a>Contenu associé  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
  
 [Procédures OLE Automation (option de configuration de serveur)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
  
