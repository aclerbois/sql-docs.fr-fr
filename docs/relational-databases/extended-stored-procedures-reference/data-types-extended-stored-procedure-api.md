---
title: Type de données (API de procédure stockée étendue) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: extended-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- extended stored procedures [SQL Server], data types
- data types [SQL Server], extended stored procedures
ms.assetid: 37fb86b9-8819-4387-bcdc-9616968e15ad
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 0e703c80db732560a45db72d8f8c0bf2a2ce21fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-types-extended-stored-procedure-api"></a>Type de données (API de procédure stockée étendue)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Pour utiliser les types de données de l'API de procédure stockée étendue, incluez le fichier d'en-tête Srv.h dans votre programme.  
  
|Type de données|Type de données SQL Server|Description|  
|---------------|--------------------------|-----------------|  
|SRVBIGBINARY|**binaire**|Type de données **binary**, de longueur comprise entre 0 et 8000 octets.|  
|SRVBIGCHAR|**char**|Type de données **character**, de longueur comprise entre 0 et 8000 octets.|  
|SRVBIGVARBINARY|**varbinary**|Type de données **binary** de longueur variable comprise entre 0 et 8000 octets.|  
|SRVBIGVARCHAR|**varchar**|Type de données **character** de longueur variable comprise entre 0 et 8000 octets.|  
|SRVBINARY|**binaire**|Type de données **binary**|  
|SRVBIT|**Bit**|type de données **bit**|  
|SRVBITN|**bit null**|Type de données **bit** autorisant les valeurs NULL.|  
|SRVCHAR|**char**|type de données **character**|  
|SRVDATETIME|**datetime**|Type de données **datetime** sur 8 octets|  
|SRVDATETIM4|**smalldatetime**|Type de données **smalldatetime** sur 4 octets|  
|SRVDATETIMN|**datetime null**|Type de données **smalldatetime** ou **datetime** autorisant les valeurs NULL.|  
|SRVDECIMAL|**decimal**|Type de données **decimal**|  
|SRVDECIMALN|**decimal null**|Type de données **decimal** autorisant les valeurs NULL.|  
|SRVFLT4|**real**|Type de données **real** sur 4 octets|  
|SRVFLT8|**float**|Type de données **float** sur 8 octets|  
|SRVFLTN|**real** &#124; **float null**|Type de données **real** ou **float** autorisant les valeurs NULL.|  
|SRVIMAGE|**image**|Type de données **image**|  
|SRVINT1|**tinyint**|Type de données **tinyint** sur 1 octet.|  
|SRVINT2|**smallint**|Type de données **smallint** sur 2 octets.|  
|SRVINT4|**Int**|Type de données **int** sur 4 octets|  
|SRVINTN|**tinyint** &#124; **smallint** &#124; **int null**|Type de données **tinyint**, **smallint** ou **int** autorisant les valeurs NULL.|  
|SRVMONEY4|**smallmoney**|Type de données **smallmoney** sur 4 octets|  
|SRVMONEY|**money**|Type de données **money** sur 8 octets|  
|SRVMONEYN|**money** &#124; **smallmoney null**|Type de données **smallmoney** ou **money** autorisant les valeurs NULL.|  
|SRVNCHAR|**nchar**|Type de données **character** Unicode.|  
|SRVNTEXT|**ntext**|Type de données **text** Unicode.|  
|SRVNUMERIC|**numeric**|Type de données **numeric**|  
|SRVNUMERICN|**numeric null**|Type de données **numeric** autorisant les valeurs NULL.|  
|SRVNVARCHAR|**nvarchar**|Type de données **character** de longueur variable Unicode.|  
|SRVTEXT|**texte**|Type de données **text**|  
|SRVVARBINARY|**varbinary**|Type de données **binary** de longueur variable.|  
|SRVVARCHAR|**varchar**|Type de données **character** de longueur variable.|  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](http://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409http://msdn.microsoft.com/security/).  
  
  
