---
title: Connexion à une Source de données (le pilote ODBC pour Oracle) | Documents Microsoft
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
- connecting to data source [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], connecting to data sources
ms.assetid: f724a9c5-342a-4f4e-a030-ec34f7378eaf
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7c12472544fd843214cab4294311889d0a84010d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-a-data-source-odbc-driver-for-oracle"></a>Connexion à une Source de données (le pilote ODBC pour Oracle)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Une application ODBC peut transmettre des informations de connexion de plusieurs façons. Par exemple, l’application peut avoir le pilote d’inviter l’utilisateur pour les informations de connexion. Ou l’application peut attendre une chaîne de connexion qui spécifie la connexion de source de données. Comment vous connectez à une source de données dépend de la méthode de connexion utilisée par votre application ODBC.  
  
 La boîte de dialogue Source de données est un moyen classique pour se connecter à une source de données. Si votre application ODBC est configurée pour utiliser une boîte de dialogue, cette boîte de dialogue s’affiche et vous demande les informations de connexion de source de données approprié.  
  
 Vous pouvez également vous connecter à une source de données à l’aide du [chaîne de connexion](../../odbc/microsoft/connection-string-format-and-attributes.md).  
  
### <a name="to-connect-to-a-data-source-using-a-dialog-box"></a>Pour vous connecter à une source de données à l’aide d’une boîte de dialogue  
  
1.  La boîte de dialogue Source de données s’affiche, sélectionnez une source de données Oracle, puis cliquez sur OK. La boîte de dialogue de connexion s’affiche.  
  
2.  Renseignez les informations appropriées pour la boîte de dialogue se connecter, puis cliquez sur OK.  
  
 Une fois la connexion le cas échéant, votre application peut utiliser le pilote ODBC pour Oracle pour accéder aux informations de la source de données contient.
