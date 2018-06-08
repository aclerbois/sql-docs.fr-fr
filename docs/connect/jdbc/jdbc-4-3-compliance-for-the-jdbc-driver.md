---
title: JDBC 4.3 conformité pour le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 36025ec0-3c72-4e68-8083-58b38e42d03b
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: be781d51418184aa519854c1ebd6bb59c19ff5b8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jdbc-43-compliance-for-the-jdbc-driver"></a>JDBC 4.3 conformité pour le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

    
> [!NOTE]  
>  Versions antérieures de Microsoft JDBC Driver 6.4 pour SQL Server sont conformes pour les spécifications de l’API Java Database Connectivity 4.2. Cette section ne s’applique pas pour les versions antérieures à la version 6.4.  
  
 Actuellement, 6.4 de pilote JDBC Microsoft pour SQL Server est Java 9 compatibles, mais il n’est pas entièrement conforme pour les spécifications de 4.3 de API de connectivité de base de données Java. Pour qui vient d’être introduit l’API JDBC 4.3, si ce n’est pas pris en charge par le pilote, le pilote lève une SQLFeatureNotSupportedException.