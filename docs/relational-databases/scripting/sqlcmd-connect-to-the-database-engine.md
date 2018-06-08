---
title: Se connecter au moteur de base de données avec sqlcmd | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd utility, Database Engine connections
- Named Pipes [SQL Server], sqlcmd utility
- TCP/IP [SQL Server], client protocols
- network protocols [SQL Server], sqlcmd utility
- protocols [SQL Server], sqlcmd utility
- VIA
- client protocols [SQL Server]
ms.assetid: 74b0fb71-7f8e-4171-9431-d07528532524
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8bfba438ec6160b5431dfdd133c37eb4bcd85d7e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sqlcmd---connect-to-the-database-engine"></a>sqlcmd : se connecter au moteur de base de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la communication cliente par le biais du protocole réseau TCP/IP (protocole par défaut) et du protocole des canaux nommés. Le protocole de mémoire partagée est également disponible si le client se connecte à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur le même ordinateur. Il existe trois méthodes courantes de sélection du protocole. Le protocole utilisé par l’utilitaire **sqlcmd** est déterminé dans l’ordre suivant :  
  
-   **sqlcmd** utilise le protocole spécifié dans la chaîne de connexion comme décrit ci-après.  
  
-   Si aucun protocole n’est spécifié dans la chaîne de connexion, **sqlcmd** utilise le protocole défini dans l’alias auquel il se connecte. Pour configurer **sqlcmd** pour utiliser un protocole réseau spécifique en créant un alias, consultez [Créer ou modifier un alias de serveur devant être utilisé par un client &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
-   Sinon, **sqlcmd** utilise le protocole réseau déterminé par l’ordre des protocoles dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Les exemples suivants montrent différentes manières de se connecter à l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur le port 1433 et aux instances nommées du [!INCLUDE[ssDE](../../includes/ssde-md.md)] supposées être à l'écoute sur le port 1691. Certains de ces exemples utilisent l'adresse IP de la carte de bouclage (127.0.0.1). Effectuez un test sur l'adresse IP de la carte réseau de votre ordinateur.  
  
 Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] en spécifiant le nom de l'instance :  
  
```  
sqlcmd -S ComputerA  
sqlcmd -S ComputerA\instanceB  
```  
  
 Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] en spécifiant l'adresse IP :  
  
```  
sqlcmd -S 127.0.0.1  
sqlcmd -S 127.0.0.1\instanceB  
```  
  
 Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] en spécifiant le numéro de port TCP\IP :  
  
```  
sqlcmd -S ComputerA,1433  
sqlcmd -S ComputerA,1691  
sqlcmd -S 127.0.0.1,1433  
sqlcmd -S 127.0.0.1,1691  
```  
  
### <a name="to-connect-using-tcpip"></a>Pour établir la connexion à l'aide du protocole TCP/IP  
  
-   Connectez-vous à l'aide de la syntaxe générale suivante :  
  
    ```  
    sqlcmd -S tcp:<computer name>,<port number>  
    ```  
  
-   Connectez-vous à l'instance par défaut :  
  
    ```  
    sqlcmd -S tcp:ComputerA,1433  
    sqlcmd -S tcp:127.0.0.1,1433  
    ```  
  
-   Connectez-vous à une instance nommée :  
  
    ```  
    sqlcmd -S tcp:ComputerA,1691  
    sqlcmd -S tcp:127.0.0.1,1691  
    ```  
  
### <a name="to-connect-using-named-pipes"></a>Pour établir la connexion à l'aide de canaux nommés  
  
-   Connectez-vous à l'aide de l'une des syntaxes générales suivantes :  
  
    ```  
    sqlcmd -S np:\\<computer name>\<pipe name>  
    ```  
  
-   Connectez-vous à l'instance par défaut :  
  
    ```  
    sqlcmd -S np:\\ComputerA\pipe\sql\query  
    sqlcmd -S np:\\127.0.0.1\pipe\sql\query  
    ```  
  
-   Connectez-vous à une instance nommée :  
  
    ```  
    sqlcmd -S np:\\ComputerA\pipe\MSSQL$<instancename>\sql\query  
    sqlcmd -S np:\\127.0.0.1\pipe\MSSQL$<instancename>\sql\query  
    ```  
  
### <a name="to-connect-using-shared-memory-a-local-procedure-call-from-a-client-on-the-server"></a>Pour établir la connexion à l'aide de la mémoire partagée (appel de procédure local) depuis un client sur le serveur  
  
-   Connectez-vous à l'aide de l'une des syntaxes générales suivantes :  
  
    ```  
    sqlcmd -S lpc:<computer name>  
    ```  
  
-   Connectez-vous à l'instance par défaut :  
  
    ```  
    sqlcmd -S lpc:ComputerA  
    ```  
  
-   Connectez-vous à une instance nommée :  
  
    ```  
    sqlcmd -S lpc:ComputerA\<instancename>  
    ```  
  
  
