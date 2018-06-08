---
title: Regroupement de connexions (Microsoft Drivers for PHP for SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 07/10/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connection pooling support
ms.assetid: 4d9a83d4-08de-43a1-975c-0a94005edc94
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e4a4718a252a13d6634ce7515b0580b8ce19dd6a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-pooling-microsoft-drivers-for-php-for-sql-server"></a>Regroupement de connexions (Microsoft Drivers for PHP for SQL Server)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Voici des points importants à noter sur le regroupement de connexions dans [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]:  
  
-   [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] utilise le regroupement de connexions ODBC.  
  
-   Par défaut, le regroupement de connexions est activé dans Windows. Linux et Mac OS X, les connexions sont regroupées uniquement si le regroupement de connexions est activé pour ODBC. Lorsque le regroupement de connexions est activé et que vous vous connectez à un serveur, le pilote tente d’utiliser une connexion regroupée avant d’en créer un nouveau. Si une connexion équivalente est introuvable dans le regroupement, une nouvelle connexion est créée et ajoutée à ce regroupement. Le pilote détermine si les connexions sont équivalentes, par comparaison avec des chaînes de connexion.  
  
-   Quand une connexion du regroupement est utilisée, l’état de la connexion est réinitialisé.  
  
-   La fermeture de la connexion retourne la connexion au regroupement.  
  
Pour plus d’informations sur le regroupement de connexions, consultez [regroupement de connexions du Gestionnaire de pilotes](../../odbc/reference/develop-app/driver-manager-connection-pooling.md).  
  
## <a name="enablingdisabling-connection-pooling"></a>Le regroupement de l’activation/désactivation de la connexion
### <a name="windows"></a>Windows
Vous pouvez forcer le pilote pour créer une nouvelle connexion (au lieu de rechercher une connexion équivalente dans le pool de connexions) en définissant la valeur de la *ConnectionPooling* attribut dans la chaîne de connexion à **false**  (ou 0).  
  
Si le *ConnectionPooling* attribut est omis à partir de la chaîne de connexion ou si elle est définie sur **true** (ou 1), le pilote crée uniquement une connexion si aucune connexion équivalente n’existe pas dans le pool de connexions.  
  
Pour plus d’informations sur les autres attributs de connexion, consultez [Connection Options](../../connect/php/connection-options.md).  
### <a name="linux-and-mac-os-x"></a>Linux et Mac OS X
*ConnectionPooling* attribut ne peut pas être utilisé pour activer ou désactiver le regroupement de connexions. 

Le regroupement de connexions peut être activé/désactivé en modifiant le fichier de configuration odbcinst.ini. Le pilote doit être rechargé pour que les modifications prennent effet.

Paramètre `Pooling` à `Yes` et un nombre positif `CPTimeout`valeur dans le fichier odbcinst.ini permet le regroupement de connexions. 
```
[ODBC]
Pooling=Yes

[ODBC Driver 13 for SQL Server]
CPTimeout=<int value>
```
Paramètre `Pooling` à `No` dans le fichier odbcinst.ini force le pilote à créer une nouvelle connexion.
```
[ODBC]
Pooling=No
```
  
## <a name="see-also"></a>Voir aussi  
[Guide pratique pour se connecter à l’aide de l’authentification Windows](../../connect/php/how-to-connect-using-windows-authentication.md)

[Guide pratique pour se connecter à l’aide de l’authentification SQL Server](../../connect/php/how-to-connect-using-sql-server-authentication.md)  
  
