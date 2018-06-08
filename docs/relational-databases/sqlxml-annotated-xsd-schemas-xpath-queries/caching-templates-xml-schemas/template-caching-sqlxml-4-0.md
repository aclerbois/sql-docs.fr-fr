---
title: Modèle de mise en cache (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- registry keys [SQLXML]
- cache [SQLXML]
- templates [SQLXML], caching
ms.assetid: 73e151c6-b24e-4422-a116-51e0846bc6f5
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f4ba78383ac3b0b8b1065ae27aa064a99b3566d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="template-caching-sqlxml-40"></a>Mise en cache de modèle (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La mise en cache de modèle améliore considérablement les performances. Si la mise en cache de modèle est définie, le modèle reste en mémoire lors de sa première exécution. Il s'ensuit une amélioration des performances de la prochaine exécution du modèle.  
  
 Vous pouvez définir la taille du cache du modèle en ajoutant la clé suivante au Registre :  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\TemplateCacheSize  
```  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteRegistry](../../../includes/ssnoteregistry-md.md)]  
  
 La taille du modèle doit être définie d'après la mémoire disponible et le nombre de modèles que vous utilisez. La valeur par défaut de **TemplateCacheSize** est 31. Vous pouvez augmenter la taille du cache si l'accès au modèle semble lent ou diminuer la taille du cache si la mémoire est insuffisante.  
  
 Pour de meilleures performances, il est recommandé de définir **TemplateCacheSize** supérieure au nombre de modèles que vous utilisez généralement. Si **TemlateCacheSize** est inférieur au nombre de modèles que vous avez, les performances se dégradent en tant que le nombre de modèles augmente. Le **TemplateCacheSize** peut être définie à un maximum de 128.  
  
 Chaque fois qu'un modèle mis en cache est utilisé, l'heure de modification du fichier modèle est vérifiée pour déterminer s'il doit être actualisé. En effet, la copie du disque est plus récente que la copie du cache.  
  
> [!NOTE]  
>  Les paramètres de modèle et les propriétés de commande ne sont pas mis en cache.  
  
## <a name="see-also"></a>Voir aussi  
 [La mise en cache de schéma & #40 ; SQLXML 4.0 & #41 ;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)   
 [Mise en cache XSL & #40 ; SQLXML 4.0 & #41 ;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/xsl-caching-sqlxml-4-0.md)  
  
  
