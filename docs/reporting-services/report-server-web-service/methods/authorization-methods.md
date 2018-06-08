---
title: Méthodes d’autorisation | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
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
- security [Reporting Services], reports
- authorization [Reporting Services]
- reports [Reporting Services], security
- tasks [Reporting Services]
- roles [Reporting Services], methods
ms.assetid: 45e9cf2c-facf-4801-9482-c855403f42a8
caps.latest.revision: 42
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 479c1814a6ce52f3deec42d5ce84f40bc00278a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="authorization-methods"></a>Méthodes d'autorisation
  Vous pouvez utiliser les méthodes suivantes pour gérer les tâches, les rôles et les stratégies sur le serveur de rapports.  
  
|Méthode|Action|  
|------------|------------|  
|<xref:ReportService2010.ReportingService2010.CreateRole%2A>|Ajoute un nouveau rôle à la base de données du serveur de rapports. Cette méthode s’applique uniquement au mode natif.|  
|<xref:ReportService2010.ReportingService2010.DeleteRole%2A>|Supprime un rôle de la base de données du serveur de rapports. Cette méthode s’applique uniquement au mode natif.|  
|<xref:ReportService2010.ReportingService2010.GetPermissions%2A>|Retourne les autorisations utilisateur associées à un élément particulier de la base de données du serveur de rapports ou la bibliothèque SharePoint.|  
|<xref:ReportService2010.ReportingService2010.GetPolicies%2A>|Retourne les stratégies associées à un élément particulier de la base de données du serveur de rapports ou la bibliothèque SharePoint.|  
|<xref:ReportService2010.ReportingService2010.GetRoleProperties%2A>|Retourne les propriétés de métadonnées de rôle et une collection de tâches associées.|  
|<xref:ReportService2010.ReportingService2010.GetSystemPermissions%2A>|Retourne les autorisations système de l'utilisateur. Cette méthode s’applique uniquement au mode natif.|  
|<xref:ReportService2010.ReportingService2010.GetSystemPolicies%2A>|Retourne les stratégies système, y compris les groupes et les rôles auxquels elles sont associées. Cette méthode s’applique uniquement au mode natif.|  
|<xref:ReportService2010.ReportingService2010.InheritParentSecurity%2A>|Supprime les stratégies qui sont associées à un élément particulier de la base de données du serveur de rapports et définit les stratégies de sécurité de l’élément sur celles de son parent.|  
|<xref:ReportService2010.ReportingService2010.IsSSLRequired%2A>|Renvoie une valeur booléenne qui indique si le protocole Secure Socket Layer (SSL) est obligatoire pour utiliser le point de terminaison <xref:ReportService2010>.|  
|<xref:ReportService2010.ReportingService2010.ListRoles%2A>|Retourne les noms et descriptions des rôles gérés par le serveur de rapports.|  
|<xref:ReportExecution2005.ReportExecutionService.ListSecureMethods%2A>|Retourne la liste des méthodes SOAP (Simple Object Access Protocol) dans le point de terminaison <xref:ReportExecution2005>qui requièrent une connexion sécurisée lorsqu'elles sont appelées. Le paramètre **SecureConnectionLevel** du serveur de rapports est utilisé pour déterminer quelles méthodes sont retournées.|  
|<xref:ReportService2010.ReportingService2010.ListTasks%2A>|Retourne les tâches gérées par le serveur de rapports.|  
|<xref:ReportService2010.ReportingService2010.SetPolicies%2A>|Définit les stratégies associées à un élément spécifié.|  
|<xref:ReportService2010.ReportingService2010.SetRoleProperties%2A>|Définit les propriétés de métadonnées de rôle et associe un jeu de tâches à un rôle. Cette méthode s’applique uniquement au mode natif.|  
|<xref:ReportService2010.ReportingService2010.SetSystemPolicies%2A>|Définit la stratégie système qui définit des groupes et leurs rôles associés. Cette méthode s’applique uniquement au mode natif.|  
  
## <a name="see-also"></a> Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service web Report Server](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Méthodes du service web Report Server](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
