---
title: Gestion des erreurs dans Visual C++ | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- errors [ADO], Visual C++
- Visual C++ error handling [ADO]
ms.assetid: b7576f07-020a-45f7-9e79-b5756f33f7ab
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 33e60542f1354d829f51159b74d6a53a7b549f67
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handling-errors-in-visual-c"></a>Gestion des erreurs dans Visual C++
Dans COM, la plupart des opérations renvoient un code de retour HRESULT qui indique si une fonction a réussi. La directive #import génère du code wrapper autour de chaque méthode « brut » ou une propriété et vérifie le HRESULT retourné. Si la valeur HRESULT indique un échec, le code wrapper génère une erreur COM en appelant _com_issue_errorex() avec le code de retour HRESULT en tant qu’argument. Objets d’erreur COM peuvent être interceptées dans un **try-catch** bloc. (Par souci d’efficacité, interceptez une référence à un objet _com_error.)  
  
 N’oubliez pas, il s’agit d’erreurs ADO : elles résultent de l’échec d’une opération ADO. Les erreurs renvoyées par le fournisseur sous-jacent apparaissent en tant que **erreur** des objets dans le **connexion** l’objet **erreurs** collection.  
  
 La directive #import crée uniquement des routines de gestion des erreurs pour les méthodes et propriétés déclarées dans le fichier .dll ADO. Toutefois, vous pouvez tirer parti de ce même mécanisme de gestion des erreurs en écrivant votre propre fonction inline ou macro la vérification des erreurs. Consultez la rubrique des Extensions Visual C++® pour obtenir des exemples.
