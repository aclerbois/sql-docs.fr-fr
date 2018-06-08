---
title: Ce que&#39;nouveauté dans l’intégration du CLR | Documents Microsoft
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 871fcccd-b726-4b13-9f95-d02b4b39d8ab
caps.latest.revision: 7
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 040f31cc5cfd3cc06cb20e298a275188fc616185
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-integration---what39s-new"></a>Intégration du CLR - ce que&#39;s New
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Voici les nouvelles fonctionnalités apportées à l'intégration du CLR dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] :  
  
-   Dans la version 4 du CLR, les objets de base de données CLR n'interceptent plus des exceptions d'état endommagées. Ces exceptions sont maintenant interceptées dans l'intégration du CLR qui héberge la couche. Ces exceptions peuvent toujours être interceptées par les composants de base de données CLR en définissant un attribut de code ([\<legacyCorruptedStateExceptionsPolicy > élément](http://go.microsoft.com/fwlink/?LinkId=204954)). Toutefois, cette opération n'est pas recommandée car les résultats ne sont pas fiables lorsqu'une exception d'état endommagée se produit.  
  
-   En raison des strictes conditions de sécurité de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les composants de base de données CLR continueront à utiliser le modèle de sécurité d'accès du code défini dans la version 2.0 du CLR.  
  
-   Dans le CLR version 4, une erreur de format dans un **System.TimeSpan** valeur génère une **System.FormatExceptions**. Avant la version 4 du CLR, une erreur de format dans un **System.TimeSpan** valeur a été ignorée. Les applications de base de données qui s’appuient sur le comportement antérieur à la version 4 du CLR doivent s’exécuter avec un niveau de compatibilité de base de données (**ALTER DATABASE Compatibility Level**) inférieure ou égale à 100. Pour plus d’informations, consultez [< TimeSpan_LegacyFormatMode > élément](http://go.microsoft.com/fwlink/?LinkId=205109).  
  
-   La version 4 du CLR prend en charge Unicode 5.1. Les opérations de tri qui impliquent des accents et des symboles vont faire l'objet d'améliorations. Des problèmes de compatibilité peuvent survenir si votre application s'appuie sur un comportement de tri hérité. Pour activer le tri hérité, le niveau de compatibilité de base de données (**ALTER DATABASE Compatibility Level**) doit être défini à 100 ou inférieure. Pour prendre en charge cette fonctionnalité, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] installe sort00001000.dll dans le répertoire du .NET Framework 4 (C:\Windows\Microsoft.NET\Framework\v4.0.30319). Pour plus d’informations, consultez [ \<CompatSortNLSVersion > élément](http://go.microsoft.com/fwlink/?LinkId=205110).  
  
-   Les colonnes suivantes ont été ajoutées à [sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md): **total_processor_time_ms**, **total_allocated_memory_kb**, et **survived_memory_kb**.  
  
  
