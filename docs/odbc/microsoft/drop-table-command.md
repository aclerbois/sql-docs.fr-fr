---
title: Commande DROP TABLE | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- drop table command [ODBC]
ms.assetid: bc50459b-8861-4889-84a9-129ae9065aa8
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 657f175f52f7a098a2c026cf384fdf1b77f43484
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-table-command"></a>Commande DROP TABLE
Supprime une table de la base de données spécifiée avec la source de données et le supprime de disque.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro native pour cette commande. Pour plus d’informations spécifiques au pilote, consultez la section Notes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DROP TABLE TableName | FileName | ?  
```  
  
## <a name="settings"></a>Paramètres  
 *TableName*  
 Spécifie la table à supprimer de la base de données spécifiée avec la source de données et de supprimer à partir du disque.  
  
 *FileName*  
 Spécifie une table libre à supprimer à partir du disque.  
  
 ?  
 Affiche la boîte de dialogue Supprimer à partir de laquelle vous pouvez choisir une table à supprimer de la base de données spécifiée avec la source de données et de supprimer à partir du disque.  
  
## <a name="remarks"></a>Notes  
 Lorsque DROP TABLE est publiée, tous les index principales, les valeurs par défaut et les règles de validation associés à la table sont également supprimés. DROP TABLE affecte également d’autres tables dans la base de données spécifiée avec la source de données si ces tables ont des règles ou relations associées à la table en cours de suppression. Les règles et les relations ne sont plus valides lorsque la table est supprimée de la base de données.  
  
## <a name="driver-remarks"></a>Section Notes de pilote  
 Lorsque votre application envoie l’instruction SQL ODBC DROP TABLE à la source de données, le pilote ODBC Visual FoxPro convertit la commande dans la ligne de commande Visual FoxProDROP TABLE à l’aide de la syntaxe indiquée dans le tableau suivant.  
  
|Syntaxe ODBC|Source de données|Syntaxe de Visual FoxPro|  
|-----------------|-----------------|--------------------------|  
|DROP TABLE *nom de table de base*|Base de données (fichier .dbc)|TABLE de suppression *TableName* supprimer|  
||Répertoire de tables indépendantes (fichiers .dbf)|Effacer *dbfName*<br /><br /> Effacer *cdxName*<br /><br /> Effacer *fptName*|
