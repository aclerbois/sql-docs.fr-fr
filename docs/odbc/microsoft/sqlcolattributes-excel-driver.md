---
title: SQLColAttributes (pilote Excel) | Documents Microsoft
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
- Excel driver [ODBC], SQLColAttributes
- SQLColAttribute function [ODBC], Excel Driver
ms.assetid: 7c4833e3-ff0c-4313-9ab8-21379ceab656
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 776fac64a8e0da05964013dced69bc1d4d5e8aae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcolattributes-excel-driver"></a>SQLColAttributes (pilote Excel)
> [!NOTE]  
>  Cette rubrique fournit des informations spécifiques au pilote Excel. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
|Attribut|Commentaires|  
|---------------|--------------|  
|SQL_COLUMN_DISPLAY_SIZE|Pour les données LONGVARBINARY, SQL_COLUMN_DISPLAY_SIZE est la longueur maximale de la colonne, et pas la longueur maximale de la colonne heures 2.|  
|SQL_OWNER_NAME|Une chaîne vide (« ») est retourné dans cette colonne, car le nom du propriétaire n’est pas pris en charge.|  
|SQL_QUALIFIER_NAME|Le chemin d’accès à un répertoire est retournée.|  
|SQL_COLUMN_SEARCHABLE|Colonnes LONGVARBINARY et LONGVARCHAR sont signalés comme SQL_UNSEARCHABLE.<br /><br /> Binaire de longueur fixe et de longueur variable et les types de données caractères sont interrogeables, même si LONGVARBINARY et LONGVARCHAR ne sont pas.|  
  
> [!NOTE]  
>  Ci-dessus n’est pas une liste complète des attributs retourné par **SQLColAttributes**.
