---
title: Définition de Format pour le Marshaling de flux DCOM | Documents Microsoft
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
helpviewer_keywords:
- dcom stream marshaling format in rds [ADO]
ms.assetid: 46664ac5-d6e6-4457-8bae-3a98300f2a41
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: eed72f16fa58e4dc47486967e615de746e27a2a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-dcom-stream-marshaling-format"></a>Définition de Format pour le Marshaling de flux DCOM
Un ordinateur client à l’aide de composants de RDS 1.5 ou une version antérieure n’est pas compatible avec un serveur à l’aide de composants de RDS 2.0 ou version ultérieure. Lors de l’utilisation de DCOM en tant que protocole sous-jacent, la prise en charge pour les services Bureau à distance 2.0 ou version ultérieure est plus efficace dans le transport [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objets. Si votre client exécute des composants de RDS 1.5 ou une version antérieure, vous pouvez configurer votre serveur pour la prise en charge des services Bureau à distance précédent (appelé RDS 1.0) ou la plus récente prise en charge de services Bureau à distance (RDS 2.0 ou version ultérieure). Définissez une des entrées de Registre suivantes :  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
```  
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS10"  
```  
  
 -ou-  
  
```  
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS20"  
```


