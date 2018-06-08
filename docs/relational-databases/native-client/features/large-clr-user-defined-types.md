---
title: Les Types CLR volumineux définis par l’utilisateur | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- large CLR user-defined types
ms.assetid: b65eb61d-ccf6-49c0-98e7-9a4ef4b2f790
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f5a2f97231456dc6a3ea79dca021bfbf51cd7b45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="large-clr-user-defined-types"></a>Types CLR volumineux définis par l'utilisateur
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Dans SQL Server 2005, les types définis par l'utilisateur (UDT) dans le CLR (Common Language Runtime) se limitaient à une taille de 8 000 octets. Cette limite n'est plus d'actualité dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] et versions ultérieures. Les types CLR définis par l'utilisateur sont désormais traités de la même manière que les objets LOB. Ainsi, les types définis par l'utilisateur dont la taille est inférieure ou égale à 8 000 octets adoptent le même comportement que dans SQL Server 2005 mais les types définis par l'utilisateur plus volumineux sont pris en charge et affichent une taille « illimitée ».  
  
 Pour plus d’informations, consultez [Large CLR User-Defined Types &#40;OLE DB&#41; ](../../../relational-databases/native-client/ole-db/large-clr-user-defined-types-ole-db.md) et [Large CLR User-Defined Types &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="use-cases"></a>Cas d'usage  
 Pour ODBC, la prise en charge des types définis par l'utilisateur volumineux incluent la possibilité de transmettre des valeurs UDT en fragments sous forme de paramètres de données en cours d'exécution. Pour cela, vous devez utiliser SQLPutData.  
  
 Pour OLE DB, prise en charge pour les UDT volumineux offre la possibilité de valeurs d’UDT de flux de données vers et depuis le serveur en utilisant une liaison ISequentialStream.  
  
 Les types définis par l'utilisateur dont la taille est inférieure ou égale à 8 000 octets se comporteront de la même manière que dans SQL Server 2005. Pour OLE DB, vous pouvez toujours transmettre en continu petits types UDT à l’aide de liaison ISequentialStream.  
  
 Le code natif doit quelquefois comprendre le contenu des types CLR définis par l'utilisateur mais n'a pas besoin d'instancier les objets managés. Dans ce cas, vous pouvez utiliser la sérialisation personnalisée pour convertir des valeurs de type défini par l'utilisateur (UDT) sur le serveur dans un format bien connu des clients.  
  
 Pour les applications qui disposent d'un code d'accès aux données existant, vous pouvez exploiter le comportement des types CLR définis par l'utilisateur sur le client en extrayant des types définis par l'utilisateur via des API natives et en les instanciant au moyen de l'interopérabilité C++/CLI dans des applications en mode mixte.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
