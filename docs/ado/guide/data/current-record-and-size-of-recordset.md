---
title: Enregistrement en cours et la taille du jeu d’enregistrements | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- record location [ADO]
- current record [ADO]
ms.assetid: e63ff331-8655-4be7-82c6-e6cd6cc9d16d
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 47039bdba7fe5d5a867df4ac7ca8700a7a835b13
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="current-record-and-size-of-recordset"></a>Enregistrement en cours et la taille du jeu d’enregistrements
Cette section décrit comment localiser la position actuelle du curseur dans l’exemple **Recordset** dans [exemple de Code JScript pour retourner un jeu d’enregistrements](../../../ado/guide/data/jscript-code-example-to-return-a-recordset.md).  
  
## <a name="current-record"></a>Enregistrement actif  
 L’enregistrement actif dans le jeu de données correspond à qui pointe de la position du curseur de la **Recordset** objet. Lorsqu’un **Recordset** est retourné à partir de la source de données en tant que résultat de l’appel **Recordset.Open**, **Command.Execute**, ou **Connection.Execute**  (y compris **Connection.NamedCommand** et **Connection.StoredProcedure**), le curseur est défini de façon à pointer vers le premier enregistrement. Dans l’exemple de dataset, l’enregistrement actif initial est les « poires secs organiques d’oncle Bob » élément.  
  
## <a name="size-of-recordset"></a>Taille du jeu d’enregistrements  
 Pour connaître la taille d’un **Recordset** d’objet, d’obtenir la valeur de la **sur Recordset.RecordCount** propriété. Cette valeur est un entier long qui indique le nombre d’enregistrements dans la **Recordset**. Si le jeu de données est retournée à partir du fournisseur OLE DB pour Microsoft SQL Server, cette valeur indique le nombre de lignes retournées. La lecture de la **RecordCount** propriété sur un fermé **Recordset** provoque une erreur.  
  
 Si le nombre d’enregistrements ne peut pas être déterminé, la valeur de la propriété est -1.  
  
 La valeur de la **RecordCount** propriété dépend également des fonctionnalités du fournisseur et le type de curseur utilisé. Un curseur avant uniquement, la valeur est -1. Pour un curseur statique ou jeu de clés, la valeur est le nombre réel d’enregistrements renvoyés dans le **Recordset** objet. Un curseur dynamique, la valeur est -1 ou le nombre réel d’enregistrements, en fonction de la source de données.  
  
 Un curseur qui prend en charge **Recordcount** doit travail plus difficile et par conséquent nécessite davantage de puissance, qu’un curseur ne prend pas en charge **Recordcount**. Si vous n’avez pas besoin de connaître le nombre d’enregistrements, à l’aide du type de curseur différent peut aider à améliorer les performances de votre application, en particulier si vous devez gérer un grand ensemble de données.  
  
 Dans certains cas, un fournisseur ou le curseur ne peut pas déterminer le **RecordCount** valeur sans préalable à une extraction de tous les enregistrements à partir de la source de données. Pour vérifier un inventaire précis, appelez le **Recordset**. **MoveLast** méthode avant d’appeler **sur Recordset.RecordCount**.  
  
 L’exemple **Recordset** objet obtenu à l’aide de la [exemple de Code JScript](../../../ado/guide/data/jscript-code-example-to-return-a-recordset.md) utilise un curseur avant uniquement, par conséquent, l’appel **RecordCount** sur cet objet provoque toujours -1. Si vous modifiez la ligne de code qui appelle le **Recordset**. **Ouvrez** méthode comme indiqué dans l’exemple suivant, la **RecordCount** propriété retourne le nombre réel d’enregistrements extraits.  
  
```  
oRs.Open sSQL, sCnStr, adOpenStatic, adLockOptimistic, adCmdText   
```  
  
 C’est parce que statique curseurs avec la [fournisseur Microsoft OLE DB pour SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) prennent en charge **RecordCount**. Dans cet exemple, il existe cinq enregistrements et par conséquent **RecordCount** doit atteindre la valeur 5.  
  
 Cette section contient les rubriques suivantes.  
  
 [Limites d’un recordset](../../../ado/guide/data/boundaries-of-a-recordset.md)
