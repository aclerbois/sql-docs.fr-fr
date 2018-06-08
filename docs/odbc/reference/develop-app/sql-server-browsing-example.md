---
title: Exemple de navigation de SQL Server | Documents Microsoft
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
- SQLBrowseConnect function [ODBC], example
- connecting to data source [ODBC], SqlBrowseConnect
- connecting to driver [ODBC], SQLBrowseConnect
ms.assetid: 6e0d5fd1-ec93-4348-a77a-08f5ba738bc6
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 113dd18f5ba6c9d2ff8a74e88a920e71bc145893
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-browsing-example"></a>Exemple de navigation de SQL Server
L’exemple suivant montre comment **SQLBrowseConnect** peut être utilisé pour parcourir les connexions disponibles avec un pilote pour SQL Server. Tout d’abord, l’application demande un handle de connexion :  
  
```  
SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
```  
  
 Ensuite, l’application appelle **SQLBrowseConnect** et spécifie le pilote SQL Server, à l’aide de la description du pilote retournée par **SQLDrivers**:  
  
```  
SQLBrowseConnect(hdbc, "DRIVER={SQL Server};", SQL_NTS, BrowseResult,  
                  sizeof(BrowseResult), &BrowseResultLen);  
```  
  
 Comme il s’agit du premier appel à **SQLBrowseConnect**, le Gestionnaire de pilotes charge le pilote SQL Server et appelle du pilote **SQLBrowseConnect** fonction avec les mêmes arguments qu’il a reçu à partir de l’application.  
  
> [!NOTE]  
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier `Trusted_Connection=yes` au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.  
  
 Le pilote détermine qu’il s’agit du premier appel à **SQLBrowseConnect** et retourne le deuxième niveau des attributs de connexion : serveur, nom d’utilisateur, mot de passe, nom de l’application et les ID de station de travail. Pour l’attribut de serveur, il retourne une liste de noms de serveur valide. Le code de retour **SQLBrowseConnect** est SQL_NEED_DATA. Voici la chaîne de résultat Parcourir :  
  
```  
"SERVER:Server={red,blue,green,yellow};UID:Login ID=?;PWD:Password=?;  
   *APP:AppName=?;*WSID:WorkStation ID=?;"  
```  
  
 Chaque mot clé dans la chaîne de résultat Parcourir est suivi par un signe deux-points et d’un ou plusieurs mots avant le signe égal. Ces mots sont le nom convivial dont une application peut utiliser pour générer une boîte de dialogue. Le **application** et **WSID** mots clés sont précédés d’un astérisque, ce qui signifie qu’ils sont facultatifs. Le **SERVER**, **UID**, et **PWD** mots clés ne sont pas préfixés par un astérisque ; doivent être fournies pour eux dans la chaîne de requête de navigation suivante. La valeur de la **SERVER** mot clé peut être un des serveurs retournées par **SQLBrowseConnect** ou un nom fourni par l’utilisateur.  
  
 L’application appelle **SQLBrowseConnect** à nouveau, en spécifiant le serveur de vert et omettez la **application** et **WSID** mots clés et les noms conviviaux après chaque mot clé :  
  
```  
SQLBrowseConnect(hdbc, "SERVER=green;UID=Smith;PWD=Sesame;", SQL_NTS,  
                  BrowseResult, sizeof(BrowseResult), &BrowseResultLen);  
```  
  
 Le pilote tente de se connecter au serveur vert. Si des erreurs se sont récupérables, par exemple une paire mot clé-valeur manquante, **SQLBrowseConnect** retourne SQL_NEED_DATA et reste dans le même état tel qu’il était avant l’erreur. L’application peut appeler **SQLGetDiagField** ou **SQLGetDiagRec** pour déterminer l’erreur. Si la connexion est établie, le pilote retourne SQL_NEED_DATA et retourne la chaîne de résultat Parcourir :  
  
```  
"*DATABASE:Database={master,model,pubs,tempdb};  
   *LANGUAGE:Language={us_english,Franais};"  
```  
  
 Étant donné que les attributs de cette chaîne sont facultatifs, l’application les omettre. Toutefois, l’application doit appeler **SQLBrowseConnect** à nouveau. Si l’application choisit d’omettre le nom de la base de données et la langue, il spécifie une chaîne de requête de navigation vide. Dans cet exemple, l’application choisit la base de données pubs et appelle **SQLBrowseConnect** une dernière fois, l’omission de la **langage** (mot clé) et l’astérisque avant le **base de données** (mot clé) :  
  
```  
SQLBrowseConnect(hdbc, "DATABASE=pubs;", SQL_NTS, BrowseResult,  
                  sizeof(BrowseResult), &BrowseResultLen);  
```  
  
 Étant donné que la **base de données** attribut est l’attribut de connexion finale requise par le pilote, la navigation est terminée, l’application est connectée à la source de données et **SQLBrowseConnect** retourne SQL_SUCCESS. **SQLBrowseConnect** retourne également la chaîne de connexion complète en tant que la chaîne de résultat Parcourir :  
  
```  
"DSN=MySQLServer;SERVER=green;UID=Smith;PWD=Sesame;DATABASE=pubs;"  
```  
  
 La chaîne de connexion finale retournée par le pilote ne contient pas les noms conviviaux après chaque mot clé, ni si elle contient des mots clés facultatifs ne pas spécifiées par l’application. L’application peut utiliser cette chaîne avec **SQLDriverConnect** se reconnecter à la source de données sur le handle de connexion actuel (après déconnexion) ou se connecter à la source de données sur un handle de connexion différents. Par exemple :  
  
```  
SQLDriverConnect(hdbc, hwnd, BrowseResult, SQL_NTS, ConnStrOut,  
                  sizeof(ConnStrOut), &ConnStrOutLen, SQL_DRIVER_NOPROMPT);  
```
