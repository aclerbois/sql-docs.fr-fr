---
title: SQLBindParameter (bibliothèque de curseurs) | Documents Microsoft
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
- SQLBindParameter function [ODBC], Cursor Library
ms.assetid: 04c53e4c-cd1d-40b2-9997-684ebe43499f
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ef5189f1179c5be6b9307feb4dcf72c74838177d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlbindparameter-cursor-library"></a>SQLBindParameter (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation de la **SQLBindParameter** fonction dans la bibliothèque de curseurs. Pour des informations générales sur **SQLBindParameter**, consultez [fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 Une application peut appeler **SQLBindParameter** pour relier des paramètres, tant que le type de données C, taille de la colonne et les chiffres décimaux de la colonne liée restent les mêmes.  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR à utiliser les décalages de liaison. (**SQLBindParameter** n’a pas à être appelée pour cette nouvelle liaison doit avoir lieu.)  
  
 La bibliothèque de curseurs prend en charge les paramètres de data-at-execution de liaison.
