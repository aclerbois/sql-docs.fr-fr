---
title: Les informations de connexion spécifiques au pilote | Documents Microsoft
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
- SQLConnect function [ODBC], driver-specific connection information
- connecting to driver [ODBC], SQLConnect
- SQLDriverConnect function [ODBC], driver specific connection information
- connecting to data source [ODBC], SQLDriverConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], SQLConnect
- connecting to driver [ODBC], driver-specific information
ms.assetid: 3748758a-f16a-4f3b-9c40-06f2e300704e
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 889f051edb1922aacc6dd304468942eaf1e1aa3b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="driver-specific-connection-information"></a>Informations de connexion spécifiques au pilote
**SQLConnect** part du principe qu’un nom de source de données, un ID utilisateur et un mot de passe sont suffisantes pour se connecter à une source de données et que toutes les autres informations de connexion peuvent être stockées sur le système. Cela n’est souvent pas le cas. Par exemple, un pilote peut-être un ID d’utilisateur et mot de passe pour vous connecter à un serveur et un autre ID d’utilisateur et le mot de passe pour vous connecter à un SGBD. Étant donné que **SQLConnect** accepte un ID utilisateur et un mot de passe, cela signifie que les autres ID d’utilisateur et mot de passe doivent être stockés avec les informations de source de données sur le système si **SQLConnect** doit être utilisé. Cela constitue une violation potentielle de sécurité et doit être évitée, sauf si le mot de passe est chiffré.  
  
 **SQLDriverConnect** permet de définir une quantité arbitraire d’informations de connexion dans les paires mot clé-valeur de la chaîne de connexion au pilote. Par exemple, qu'un pilote nécessite un nom de source de données, un ID utilisateur et le mot de passe pour le serveur et un ID utilisateur et le mot de passe pour le SGBD. Un programme personnalisé qui utilise toujours la source de données XYZ Corp peut inviter l’utilisateur pour l’ID et les mots de passe et générer l’ensemble suivant de paires mot clé-valeur, ou *chaîne de connexion,* à passer à **SQLDriverConnect**:  
  
> [!NOTE]  
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier `Trusted_Connection=yes` au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.  
  
```  
DSN={MyDataSourceName};UID={MyUserID};PWD={MyServerPassword};UIDDBMS={MyDBMSUserID};PWDDBMS={MyDBMSUserPassword};  
```  
  
 Le **DSN** (mot clé) (nom de Source de données) désigne la source de données, le **UID** et **PWD** mots clés spécifient l’ID utilisateur et un mot de passe pour le serveur et le **UIDDBMS** et **PWDDBMS** mots clés spécifient l’ID utilisateur et un mot de passe pour le SGBD. Notez que le point-virgule final est facultatif. **SQLDriverConnect** analyse cette chaîne ; utilise le nom de source de données XYZ Corp pour récupérer les informations de connexion supplémentaires à partir du système, telles que l’adresse du serveur et ouvre une session sur le serveur et un SGBD à l’aide de l’ID d’utilisateur spécifié et les mots de passe.  
  
 Dans des paires mot clé-valeur **SQLDriverConnect** doivent suivre certaines règles de syntaxe. Les mots clés et leurs valeurs ne doivent pas contenir le **[]{}(), ? \*= ! @** caractères. La valeur de la **DSN** mot clé ne peut pas se composer uniquement d’espaces et ne doit pas contenir des espaces. En raison de la grammaire du Registre, les noms de sources de données et les mots clés ne peut pas contenir la barre oblique inverse (\\) caractères. Les espaces ne sont pas autorisés autour du signe égal dans la paire mot clé-valeur.  
  
 Le **FILEDSN** mot clé peut être utilisé dans un appel à **SQLDriverConnect** pour spécifier le nom d’un fichier qui contient des informations de source de données (voir [la connexion à l’aide de Sources de données](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md), plus loin dans cette section). Le **SAVEFILE** mot clé peut être utilisé pour spécifier le nom d’un fichier .dsn dans lequel les paires mot clé-valeur d’une connexion est effectuée par l’appel à **SQLDriverConnect** sera enregistré. Pour plus d’informations sur les sources de données de fichier, consultez le [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) description de fonction.
