---
title: Commande SET BLOCKSIZE | Documents Microsoft
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
- set blocksize command [ODBC]
ms.assetid: 0c11580f-37f5-4a8e-99be-9fb9c44bb433
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 85b9df4f40c68d68ea28a4bfad006105e4a98d7b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-blocksize-command"></a>Commande de taille de bloc SET
Spécifie l’espace disque alloué pour le stockage des champs de type memo.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET BLOCKSIZE TO nBytes  
```  
  
## <a name="arguments"></a>Arguments  
 *nBytes*  
 Spécifie la taille de bloc dans lequel l’espace disque pour les champs Mémo est alloué. Si *nBytes* est 0, l’espace disque alloué dans un octet (blocs de 1 octet). Si *nBytes* est un entier compris entre 1 et 32, de l’espace disque alloué dans des blocs de *nBytes* octets multipliant par 512. Si *nBytes* est supérieur à 32, espace disque alloué dans des blocs de *nBytes* octets. Si vous spécifiez une valeur de taille de bloc supérieure à 32, vous pouvez économiser l’espace disque substantielle.  
  
## <a name="remarks"></a>Notes  
 La valeur par défaut pour définir la taille de bloc est 64. Pour réinitialiser la taille de bloc sur une autre valeur, une fois que le fichier a été créé, affectez-lui une nouvelle valeur, puis utilisez copie pour créer une nouvelle table. La nouvelle table a la taille de bloc spécifié.
