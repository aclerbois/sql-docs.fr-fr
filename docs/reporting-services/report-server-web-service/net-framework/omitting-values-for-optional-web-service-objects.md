---
title: Omission de valeurs pour les objets de service web facultatifs | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: report-server-web-service
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- Web service [Reporting Services], omitted values
- XML Web service [Reporting Services], omitted values
- Report Server Web service, omitted values
- omitting values [Reporting Services]
ms.assetid: ceb68b8b-9214-4745-abc9-f47f33ecd6f7
caps.latest.revision: 36
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 70a7301edb8cc316cfc4d590070b23e83339a7d0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="omitting-values-for-optional-web-service-objects"></a>Omission de valeurs pour les objets de service Web facultatifs
  Les propriétés de plusieurs des types complexes du services web Report Server sont souvent suivies d’une propriété appelée Specified. Lorsque c'est le cas, le nom des propriétés se compose de leur nom original suivi de la mention « Specified ». La présence de cette mention signifie que certaines valeurs des propriétés peuvent parfois être omises. Ce phénomène est la conséquence directe de la conversion à partir du langage WSDL (Web Service Description Language) dans une classe proxy du [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Par exemple, la propriété de service Web <xref:ReportService2010.DataSourceDefinition.Enabled%2A> de type complexe <xref:ReportService2010.DataSourceDefinition> est suivie d'une propriété intitulée <xref:ReportService2010.DataSourceDefinition.EnabledSpecified%2A>. Quand vous générez une application et que vous ne souhaitez pas définir de valeur pour la propriété <xref:ReportService2010.DataSourceDefinition.Enabled%2A>, vous n’avez pas à attribuer de valeur à <xref:ReportService2010.DataSourceDefinition.Enabled%2A> ; la valeur par défaut **true** est en effet utilisée. Toutefois, vous devez au préalable définir <xref:ReportService2010.DataSourceDefinition.EnabledSpecified%2A> sur **false**. Quand vous attribuez une valeur à la propriété <xref:ReportService2010.DataSourceDefinition.Enabled%2A>, vous devez définir <xref:ReportService2010.DataSourceDefinition.EnabledSpecified%2A> sur **true**. Seules les propriétés accessibles en écriture nécessitent de respecter cette condition. En revanche, les propriétés en lecture seule ne requièrent aucune action de votre part.  
  
> [!IMPORTANT]  
>  Si vous ne parvenez pas à spécifier une propriété en suivant la procédure mentionnée ci-avant, le comportement du service Web peut se révéler imprévisible.  
  
 Les types de données qui nécessite habituellement que vous gériez la propriété Specified supplémentaire sont **Boolean**, **DateTime** et **Enumeration**.  
  
 Pour obtenir un exemple, consultez la méthode <xref:ReportService2010.ReportingService2010.CreateDataSource%2A>.  
  
## <a name="see-also"></a> Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
