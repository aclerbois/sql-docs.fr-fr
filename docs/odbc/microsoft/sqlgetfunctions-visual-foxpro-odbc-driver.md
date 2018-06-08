---
title: SQLGetFunctions (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- SQLGetFunctions function [ODBC], Visual FoxPro ODBC Driver
ms.assetid: 8102932a-88b3-49d8-bf7a-c766f54878c0
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bc3350e8b6a7a4ddcf505fed14a056422cbbbe41
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetfunctions-visual-foxpro-odbc-driver"></a>SQLGetFunctions (le pilote ODBC Visual FoxPro)
> [!NOTE]  
>  Cette rubrique contient des informations spécifiques au pilote ODBC Visual FoxPro. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Prise en charge : complet  
  
 Conformité d’API ODBC : Niveau 1  
  
 Renvoie la valeur TRUE pour toutes les fonctions prises en charge.  
  
 Le pilote ODBC Visual FoxPro prend en charge les fonctions de tous les principaux d’API ODBC et niveau 1. Le tableau suivant indique si le pilote prend en charge une fonction spécifique de niveau 2.  
  
|*Fonction*|Pris en charge|  
|----------------|---------------|  
|SQL_API_SQLBROWSECONNECT|non|  
|SQL_API_SQLCOLUMNPRIVELEGES|non|  
|SQL_API_SQLDATASOURCES|Oui|  
|SQL_API_SQLDESCRIBEPARAM|non|  
|SQL_API_SQLDRIVERS|Oui|  
|SQL_API_SQLEXTENDEDFETCH|Oui|  
|SQL_API_SQLFOREIGNKEYS|non|  
|SQL_API_SQLMORERESULTS|Oui|  
|SQL_API_SQLNATIVESQL|non|  
|SQL_API_SQLNUMPARAMS|Oui|  
|SQL_API_SQLPARAMOPTIONS|Oui|  
|SQL_API_SQLPRIMARYKEYS|Oui|  
|SQL_API_SQLPROCEDURECOLUMNS|non|  
|SQL_API_SQLPROCEDURES|non|  
|SQL_API_SQLSETPOS|Oui|  
|SQL_API_SQLSETSCROLLOPTIONS|Oui|  
|SQL_API_SQLTABLEPRIVILEGES|non|  
  
 Pour plus d’informations, consultez [SQLGetFunctions](../../odbc/reference/syntax/sqlgetfunctions-function.md) dans les *de référence du programmeur ODBC*.
