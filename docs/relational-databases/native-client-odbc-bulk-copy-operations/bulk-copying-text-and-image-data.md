---
title: Données Text et Image de la copie en bloc | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-bulk-copy-operations
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], text data
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC], image data
- ODBC, bulk copy operations
ms.assetid: 87155bfa-3a73-4158-9d4d-cb7435dac201
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5a8c6a70a4d1682117fff68cefe38f5d40a1e5d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bulk-copying-text-and-image-data"></a>Copie en bloc de données Text et Image
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Grand **texte**, **ntext**, et **image** les valeurs sont copiées à l’aide de la [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md) (fonction). Vous codez [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) pour le **texte**, **ntext**, ou **image** colonne avec un *pData* pointeur la valeur NULL indiquant les données doivent être fournie avec **bcp_moretext**. Il est important de spécifier la longueur exacte des données fournies pour chaque **texte**, **ntext**, ou **image** colonne dans chaque ligne copiées en bloc. Si la longueur des données pour une colonne est différente de la longueur de la colonne spécifiée dans [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md), utilisez [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) pour définir la longueur de la valeur appropriée. A [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) envoie tous les non -**texte**, non-**ntext**et non-**image** de données ; vous appelez ensuite **bcp_moretext** pour envoyer la **texte**, **ntext**, ou **image** les données dans des unités distinctes. Les fonctions de copie en bloc déterminent que toutes les données a été envoyé pour actuel **texte**, **ntext**, ou **image** colonne lors de la somme des longueurs des données envoyées via **bcp_moretext** est égal à la longueur spécifiée dans la dernière version **bcp_collen** ou **bcp_bind**.  
  
 **bcp_moretext** n’a aucun paramètre pour identifier une colonne. S’il existe plusieurs **texte**, **ntext**, ou **image** colonnes dans une ligne, **bcp_moretext** opère sur les **texte**, **ntext**, ou **image** commençant par la colonne ayant le plus petit numéro ordinal et procéder à la colonne avec le numéro le plus élevé de colonnes. **bcp_moretext** passe d’une colonne à l’autre lorsque la somme des longueurs des données envoyées est égale à la longueur spécifiée dans la dernière version **bcp_collen** ou **bcp_bind** pour la colonne actuelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations de copie en bloc &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
