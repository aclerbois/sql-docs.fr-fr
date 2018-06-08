---
title: SQLSetConnectOption (pilote Access) | Documents Microsoft
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
- Access driver [ODBC], SQLSetConnectOption
- SQLSetConnectOption function [ODBC], Access Driver
ms.assetid: 58399bc4-d0b1-4eaa-a474-c92b2d5855ea
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 952bfe683dabbcedeb771c0e7f1787f7a0e6f7bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetconnectoption-access-driver"></a>SQLSetConnectOption (pilote Access)
> [!NOTE]  
>  Cette rubrique fournit des informations spécifiques au pilote Access. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
|fOption|Commentaire|  
|-------------|-------------|  
|SQL_ACCESS_MODE|La fOption SQL_ACCESS_MODE peut être définie à SQL_MODE_READ_ONLY ou SQL_MODE_READ_WRITE. Toutefois, le pilote n’empêche pas les mises à jour si SQL_ACCESS_MODE est défini sur SQL_MODE_READ_ONLY.|  
|SQL_AUTOCOMMIT|Lorsque le pilote Microsoft Access est utilisé, l’option de SQL_AUTOCOMMIT peut être définie à SQL_AUTOCOMMIT_ON ou SQL_AUTOCOMMIT_OFF, car le pilote Microsoft Access prend en charge des transactions [1].|  
|SQL_CURRENT_QUALIFIER|Pris en charge.|  
|SQL_LOGIN_TIMEOUT|Non pris en charge.|  
|SQL_OPT_TRACE|Pris en charge.|  
|SQL_OPT_TRACEFILE|Pris en charge.|  
|SQL_PACKET_SIZE|Non pris en charge.|  
|SQL_QUIET_MODE|Non pris en charge.|  
|SQL_TRANSLATE_DLL|Non pris en charge.|  
|SQL_TRANSLATION_OPTION|Non pris en charge.|  
|SQL_TXN_ISOLATION|SQL_TXN_ISOLATION est toujours SQL_TXN_READ_COMMITTED.|  
  
 [1] transactions atomiques ne sont pas pris en charge par le pilote Microsoft Access. Lors de la validation d’une transaction en utilisant le pilote Microsoft Access, un délai fini existe entre l’heure de la transaction est validée et le temps que les valeurs sont écrites sur le disque. Ce délai est déterminé par un délai inhérent dans le moteur Microsoft Jet. Le délai d’expiration de la page ne sera pas être inférieure à une valeur minimale, même si l’option PageTimeout a la valeur inférieure à cette valeur. Par conséquent, il est aucune garantie que la validation des données n’est stable, étant donné que les modifications peuvent être effectuées pendant le délai d’attente.
