---
title: SQLParamOptions (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- SQLParamOptions function [ODBC], Visual FoxPro ODBC Driver
ms.assetid: 7f94a6e2-9c34-405c-b2b0-304d94269715
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bdced0993d2efc27a2fb40091576c47b931e892a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlparamoptions-visual-foxpro-odbc-driver"></a>SQLParamOptions (le pilote ODBC Visual FoxPro)
> [!NOTE]  
>  Cette rubrique contient des informations spécifiques au pilote ODBC Visual FoxPro. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Prise en charge : complet  
  
 Conformité d’API ODBC : Niveau 1  
  
 Permet à une application spécifier plusieurs valeurs pour l’ensemble de paramètres affectés par [SQLBindParameter](../../odbc/microsoft/sqlbindparameter-visual-foxpro-odbc-driver.md). La possibilité de spécifier plusieurs valeurs pour un ensemble de paramètres est utile pour les insertions en bloc et d’autres tâches qui nécessite que la source de données pour traiter la même instruction SQL plusieurs fois avec différentes valeurs de paramètre. Par exemple, une application peut spécifier trois jeux de valeurs pour le jeu de paramètres associés à un **insérer** instruction, puis exécutez le **insérer** instruction une fois pour effectuer les trois opérations d’insertion.  
  
 Pour plus d’informations, consultez [SQLParamOptions](../../odbc/reference/syntax/sqlparamoptions-function.md) dans les *de référence du programmeur ODBC*.
