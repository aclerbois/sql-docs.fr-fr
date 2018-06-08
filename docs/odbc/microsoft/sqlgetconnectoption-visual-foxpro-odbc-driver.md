---
title: SQLGetConnectOption (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- SQLGetConnectOption function [ODBC], Visual FoxPro ODBC Driver
ms.assetid: 5703eb39-f3b2-4f3a-8676-a5625ae29a41
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 23654dc89260423ce51fcde1fd60e554a3095209
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetconnectoption-visual-foxpro-odbc-driver"></a>SQLGetConnectOption (le pilote ODBC Visual FoxPro)
> [!NOTE]  
>  Cette rubrique contient des informations spécifiques au pilote ODBC Visual FoxPro. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Prise en charge : partielle  
  
 Conformité d’API ODBC : Niveau 1  
  
 Retourne la valeur actuelle de l’option de connexion. Cette fonction est partiellement disponible : le pilote prend en charge toutes les valeurs pour le *fOption* argument, mais ne prend ne pas en charge certaines des *vParam* les valeurs pour le *fOption* argument SQL_TXN_ISOLATION.  
  
 Le tableau suivant décrit uniquement les arguments avec un comportement spécifique à l’implémentation du pilote ODBC Visual FoxPro de **SQLGetConnectOption**.  
  
|*fOption*|Notes|  
|---------------|-------------|  
|SQL_AUTOCOMMIT|Si vous choisissez SQL_AUTOCOMMIT_OFF, votre application doit explicitement valider ou annuler les transactions avec [SQLTransact](../../odbc/microsoft/sqltransact-visual-foxpro-odbc-driver.md); le pilote ODBC Visual FoxPro ne valide pas automatiquement une instruction transactable à l’achèvement. Le pilote commence une transaction si l’instruction est transactable.|  
|SQL_CURRENT_QUALIFIER|Peut être un nom qualifié complet de la base de données (fichier .dbc) ou le chemin d’accès complet à un répertoire qui contient zéro ou plusieurs tables (fichiers .dbf).|  
|SQL_LOGINTIMEOUT|Retourne une erreur « Pilote non compatibles avec ».|  
|SQL_CURSORS|Retourne une erreur « Pilote non compatibles avec ».|  
|SQL_PACKET_SIZE|Retourne une erreur « Pilote non compatibles avec ».|  
|SQL_TXN_ISOLATION|Le pilote permet uniquement de SQL_TXN_READ_COMMITTED.<br /><br /> Les éléments suivants *vParam*s ne sont pas prises en charge :<br /><br /> SQL_TXN_READ_UNCOMMITTED<br /><br /> SQL_TXN_REAPEATABLE_READ<br /><br /> SQL_TXN_SERIALIZABLE|  
  
 Pour plus d’informations, consultez [SQLGetConnectOption](../../odbc/reference/syntax/sqlgetconnectoption-function.md) dans les *de référence du programmeur ODBC*.
