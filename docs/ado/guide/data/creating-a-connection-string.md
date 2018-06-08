---
title: Création d’une chaîne de connexion | Documents Microsoft
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
- connections [ADO]
- connection strings [ADO]
ms.assetid: 14eae122-2d1e-40c8-b88e-b7cb8dfbc93b
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cc700bdc0006a4591e61e15f2796b73c194dc5a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-a-connection-string"></a>Création d’une chaîne de connexion
Une chaîne de connexion se compose d’une liste de paires de valeur d’argument (autrement dit, les paramètres), séparée par des points-virgules. Par exemple :  
  
```  
"arg1=val1; arg2=val2; ... argN=valN;"  
```  
  
 Tous les paramètres doivent être reconnues par ADO ou le fournisseur spécifié.  
  
 ADO reconnaît les cinq arguments dans une chaîne de connexion suivants.  
  
|Argument| Description|  
|--------------|-----------------|  
|*Fournisseur*|Spécifie le nom d’un fournisseur à utiliser pour la connexion.|  
|*Nom de fichier*|Spécifie le nom d’un fichier spécifique au fournisseur (par exemple, un objet de source de données persistantes) contenant des informations de connexion prédéfinies.|  
|*URL*|Spécifie la chaîne de connexion comme une URL absolue identifiant une ressource, comme un fichier ou répertoire.|  
|*Fournisseur distant*|Spécifie le nom d’un fournisseur à utiliser lors de l’ouverture d’une connexion côté client. (Remote Data Service uniquement.)|  
|*Serveur distant*|Spécifie le nom de chemin d’accès du serveur à utiliser lors de l’ouverture d’une connexion côté client. (Remote Data Service uniquement.)|  
  
 Autres arguments sont transmis au fournisseur nommé dans le *fournisseur* argument, sans aucun traitement par ADO.  
  
 L’application HelloData dans [HelloData : une Application ADO Simple](../../../ado/guide/data/hellodata-a-simple-ado-application.md) utilisé la chaîne de connexion suivante :  
  
```  
m_sConnStr = "Provider=SQLOLEDB;Data Source=MySqlServer;" & _  
             "Initial Catalog=Northwind;Integrated Security='SSPI';"  
```  
  
 Dans cette chaîne de connexion ADO reconnaît uniquement le `"Provider=SQLOLEDB"` paramètre qui spécifie le fournisseur Microsoft OLE DB pour SQL Server comme source de données ADO. Le reste des paires de valeur d’argument, `"Data Source=MySqlServer; Initial Catalog=Northwind;Integrated Security='SSPI';"`, sont passés textuel à ce fournisseur. Le type et la validité de ces paramètres sont spécifiques au fournisseur. Pour plus d’informations sur les paramètres valides qui peuvent être passées dans la chaîne de connexion, la documentation du fournisseur individuelles.  
  
 Selon le fournisseur OLE DB pour la documentation de SQL Server, vous pouvez remplacer « Server » par le *Source de données* paramètre et « Database » pour le *Initial Catalog* paramètre. Par conséquent, la chaîne de connexion produirait des résultats identiques à celui illustré ci-dessus :  
  
```  
m_sConnStr = "Provider=SQLOLEDB;Server=MySqlServer;" & _  
             "Database=Northwind;Integrated Security='SSPI';"  
```
