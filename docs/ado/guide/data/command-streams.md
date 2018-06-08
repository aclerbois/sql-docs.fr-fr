---
title: Commande flux | Documents Microsoft
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
- command streams [ADO]
- streams [ADO], command
ms.assetid: 0ac09dbe-2665-411e-8fbb-d1efe6c777be
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a98dc21338ef492aa126e70cc28bc636acb2b91b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="command-streams"></a>Flux de commande
ADO a toujours pris en charge d’entrée de commande sous forme de chaîne spécifiée par le **CommandText** propriété. En guise d’alternative, avec ADO 2.7 ou version ultérieure, vous pouvez également utiliser un flux d’informations pour l’entrée de commande en affectant le flux à la **CommandStream** propriété. Vous pouvez affecter un ADO **flux** objet, ou tout objet qui prend en charge le modèle COM **IStream** interface.  
  
 Le contenu du flux de commande est simplement transmis à partir d’ADO à votre fournisseur, votre fournisseur doit prendre en charge d’entrée de commande par le flux de données pour cette fonctionnalité soit opérationnelle. Par exemple, SQL Server prend en charge les requêtes sous la forme de modèles XML ou des extensions Transact-SQL OpenXML.  
  
 Étant donné que les détails du flux de données doivent être interprétés par le fournisseur, vous devez spécifier le dialecte de commande en définissant le **dialecte** propriété. La valeur de **dialecte** est une chaîne qui contient un GUID, qui est défini par votre fournisseur. Pour plus d’informations sur les valeurs valides pour **dialecte** pris en charge par votre fournisseur, consultez la documentation du fournisseur.  
  
## <a name="xml-template-query-example"></a>Exemple de requête de modèle XML  
 L’exemple suivant est écrit en VBScript, la base de données Northwind.  
  
 Tout d’abord, initialiser et ouvrir le **flux** objet qui sera utilisé pour contenir le flux de requête :  
  
```  
Dim adoStreamQuery  
Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
adoStreamQuery.Open  
```  
  
 Le contenu du flux de requête soit une requête de modèle XML.  
  
 La requête de modèle requiert une référence à l’espace de noms XML identifié par l’instruction sql : le préfixe de le \<SQL : > balise. Une instruction SQL SELECT est incluse en tant que le contenu du modèle XML et assignée à une variable de chaîne comme suit :  
  
```  
sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME </sql:query>  
</ROOT>"  
```  
  
 Ensuite, écrire la chaîne dans le flux de données :  
  
```  
adoStreamQuery.WriteText sQuery, adWriteChar  
adoStreamQuery.Position = 0  
```  
  
 Affecter adoStreamQuery à la **CommandStream** propriété de ADO **commande** objet :  
  
```  
Dim adoCmd  
Set adoCmd  = Server.CreateObject("ADODB.Command"")  
adoCmd.CommandStream = adoStreamQuery  
```  
  
 Spécifiez le langage de commande **dialecte**, ce qui indique comment le fournisseur OLE DB de SQL Server doit interpréter le flux de commandes. Le dialecte spécifié par un GUID spécifique au fournisseur :  
  
```  
adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
```  
  
 Enfin, exécutez la requête et renvoyer les résultats à un **Recordset** objet :  
  
```  
Set objRS = adoCmd.Execute  
```
