---
title: Exemple de Source de données | Documents Microsoft
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
- data sources [ODBC], examples
ms.assetid: cbf15f32-0550-4c74-8088-8f7ac3855469
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 19e5596107f5bae9ceeab8ae105203e89584e854
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-source-example"></a>Exemple de Source de données
Sur les ordinateurs exécutant Microsoft® Windows NT® Server et Windows 2000 Server, Microsoft Windows NT Workstation et Windows 2000 Professionnel ou Microsoft Windows® 95/98, les données des ordinateurs, les informations source sont stockées dans le Registre. En fonction de Registre clés que les informations sont stockées sous, la source de données est appelée un *source de données utilisateur* ou un *source de données système*. Sources de données utilisateur sont stockés sous la clé HKEY_CURRENT_USER et sont disponibles uniquement pour l’utilisateur actuel. Sources de données système sont stockés sous la clé HKEY_LOCAL_MACHINE et peuvent être utilisées par plusieurs utilisateurs sur un ordinateur. Ils peuvent également être utilisés par les services du système, qui peuvent alors accéder à la source de données même si aucun utilisateur n’est connecté à l’ordinateur. Pour plus d’informations sur les sources de données système et utilisateur, consultez [SQLManageDataSources](../../odbc/reference/syntax/sqlmanagedatasources.md).  
  
 Supposons qu’un utilisateur a trois sources de données utilisateur : Personnel et inventaire, qui utilisent un SGBD Oracle ; et paie, qui utilise un système SGBD de Microsoft SQL Server. Les valeurs de Registre pour les sources de données peuvent être :  
  
```  
HKEY_CURRENT_USER  
SOFTWARE  
          ODBC  
               Odbc.ini  
                    ODBC Data Sources  
                    Personnel : REG_SZ : Oracle  
                    Inventory : REG_SZ : Oracle  
                    Payroll : REG_SZ : SQL Server  
```  
  
 et les valeurs de Registre pour la source de données de paie peuvent être :  
  
```  
HKEY_CURRENT_USER  
     SOFTWARE  
          ODBC  
               Odbc.ini  
                    Payroll  
                         Driver : REG_SZ : C:\WINDOWS\SYSTEM\Sqlsrvr.dll  
                         Description : REG_SZ : Payroll database  
                         Server : REG_SZ : PYRLL1  
                         FastConnectOption : REG_SZ : No                          UseProcForPrepare : REG_SZ : Yes  
                         OEMTOANSI : REG_SZ : No  
                         LastUser : REG_SZ : smithjo  
                         Database : REG_SZ : Payroll  
                         Language : REG_SZ :  
```
