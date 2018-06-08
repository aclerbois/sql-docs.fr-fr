---
title: Liaison des colonnes | Documents Microsoft
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
- result sets [ODBC], binding columns
- binding columns [ODBC]
ms.assetid: c4407694-c8e1-4b0b-a39d-b007e6c3b54d
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b78a87884e075e4cbe3dcb914335994aba2e4159
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="binding-columns"></a>Liaison de colonnes
Les données lues à partir de la source de données sont retournées à l’application dans des variables de l’application a attribué à cet effet. Avant cela, l’application doit associer, ou *lier*, ces variables pour les colonnes du résultat défini ; le point de vue conceptuel, ce processus est identique à la liaison de variables d’application pour les paramètres de l’instruction. Lorsque l’application lie une variable à un résultat de jeu de colonnes, il décrit cette variable, adresse, type de données et ainsi de suite, pour le pilote. Le pilote stocke ces informations dans la structure, il tient à jour pour cette instruction et utilise les informations pour retourner la valeur de la colonne lorsque la ligne est extraite.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Liaison des colonnes d’un ensemble de résultats](../../../odbc/reference/develop-app/binding-result-set-columns.md)  
  
-   [Utilisation de SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md)
