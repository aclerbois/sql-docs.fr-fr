---
title: La libération de descripteurs | Documents Microsoft
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
- SQLFreeHandle function [ODBC]
- descriptors [ODBC], allocating and freeing
- freeing descriptors [ODBC]
- allocating and freeing descriptors [ODBC]
ms.assetid: 317213f4-0ebb-4bf8-a37a-4d6b1313823f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4c96d5b654d8331f95e3abbe8a3aa8b563bf7a63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="freeing-descriptors"></a>La libération de descripteurs
Descripteurs explicitement alloués peuvent être soit libérée explicitement, en appelant **SQLFreeHandle** avec *HandleType* de SQL_HANDLE_DESC, ou implicitement, lorsque le handle de connexion est libéré. Lorsqu’un descripteur explicitement alloué est libéré, tous les descripteurs d’instruction auquel le descripteur libéré automatiquement appliqué rétablir les descripteurs implicitement leur a été allouées.  
  
 Descripteurs implicitement alloués peuvent être libérées uniquement en appelant **SQLDisconnect**, qui supprime toutes les instructions ou descripteurs sur la connexion ou en appelant **SQLFreeHandle** avec un *HandleType* de SQL_HANDLE_STMT pour libérer un descripteur d’instruction et les descripteurs implicitement alloués est associés à l’instruction. Un descripteur implicitement alloué ne peut pas être libéré en appelant **SQLFreeHandle** avec un *HandleType* de SQL_HANDLE_DESC.  
  
 Même lorsque libéré, un descripteur implicitement alloué reste valide, et **SQLGetDescField** peut être appelée sur ses champs.
