---
title: Commande SET DELETED | Documents Microsoft
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
- SET DELETED command [ODBC]
ms.assetid: 6b5e0086-156d-471d-8e7f-6c5fa9686cd5
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 74cd0c5752728320acf09768d876bae37b6e193b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-deleted-command"></a>Commande SET DELETED
Spécifie si les enregistrements marqués pour suppression sont traités et si elles sont disponibles pour une utilisation dans d’autres commandes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET DELETED ON | OFF  
```  
  
## <a name="arguments"></a>Arguments  
 ON  
 (Par défaut pour le pilote, la valeur par défaut pour Visual FoxPro est désactivé.) Spécifie que les commandes qui fonctionnent sur les enregistrements (y compris les enregistrements dans les tables associées) à l’aide d’une étendue ignorer les enregistrements marqués pour suppression.  
  
 OFF  
 Spécifie que les enregistrements marqués pour suppression peut être accessible par les commandes qui agissent sur les enregistrements (y compris les enregistrements dans les tables associées) à l’aide d’une étendue.  
  
## <a name="remarks"></a>Notes  
 Interroge que (de DELETED) pour tester le statut des enregistrements d’utilisation peut être optimisée à l’aide de la technologie Visual FoxPro Rushmore si la table est indexée sur (de supprimés).  
  
> [!IMPORTANT]  
>  DÉFINIR supprimé est ignoré si l’étendue par défaut pour la commande est l’enregistrement actif ou si vous incluez une étendue d’un enregistrement unique. L’INDEX ignore les supprimer ensemble toujours et tous les enregistrements dans la table d’index.  
  
## <a name="see-also"></a>Voir aussi  
 [DELETE, commande SQL](../../odbc/microsoft/delete-sql-command.md)
