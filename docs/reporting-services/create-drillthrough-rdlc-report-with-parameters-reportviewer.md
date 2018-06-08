---
title: Créer un rapport d’extraction (RDLC) avec des paramètres - ReportViewer | Microsoft Docs
ms.custom: ''
ms.date: 05/18/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
applies_to:
- SQL Server 2016
ms.assetid: 628c8775-c62d-45ac-b349-23db86fa4e6c
caps.latest.revision: 8
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f90ac792fe849ca91735e5a9b22a399d7798cf23
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-drillthrough-rdlc-report-with-parameters---reportviewer"></a>Créer un rapport d’extraction (RDLC) avec des paramètres - ReportViewer
Un rapport [d’extraction](http://technet.microsoft.com/library/ff519554.aspx) est un rapport que l’utilisateur ouvre en cliquant sur un lien situé dans un autre rapport. Il contient en général des détails sur un élément figurant dans le rapport de synthèse d'origine. Ce didacticiel vous guide tout au long des leçons suivantes pour créer un rapport d’extraction avec des paramètres et une requête, en [mode local](http://msdn.microsoft.com/library/ff487969.aspx).  
  
## <a name="requirements"></a>Spécifications  
Pour suivre cette procédure pas à pas, vous devez avoir accès à l’exemple de base de données **AdventureWorks2014** . Pour plus d’informations sur la façon d’obtenir l’exemple de base de données **AdventureWorks2014**, consultez [Exemples de bases de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  
  
Cette procédure pas à pas part du principe que vous connaissez les requêtes Transaction-SQL et les objets ADO.NET [DataSet](https://msdn.microsoft.com/library/system.data.dataset.aspx) et [DataTable](http://msdn.microsoft.com/library/system.data.datatable.aspx) .  
  
Utilisez Visual Studio 2015 et l’application web ASP.NET pour créer une page web ASP.NET avec un contrôle ReportViewer. Le contrôle est configuré en vue d'afficher un rapport que vous créez. Pour cette procédure pas à pas, vous créez l'application dans Microsoft Visual C#.  
  
## <a name="tasks"></a>Tâches  
[Leçon 1 : créer un nouveau site Web](../reporting-services/lesson-1-create-a-new-web-site.md)  
[Leçon 2 : définir une connexion de données et une table de données pour le rapport parent](../reporting-services/lesson-2-define-a-data-connection-and-data-table-for-parent-report.md)  
[Leçon 3 : concevoir le rapport parent à l'aide de l'Assistant Rapport](../reporting-services/lesson-3-design-the-parent-report-using-the-report-wizard.md)  
[Leçon 4 : définir une connexion de données et une table de données pour le rapport enfant](../reporting-services/lesson-4-define-a-data-connection-and-data-table-for-child-report.md)  
[Leçon 5 : concevoir le rapport enfant à l’aide de l’Assistant Rapport](../reporting-services/lesson-5-design-the-child-report-using-the-report-wizard.md)  
[Leçon 6 : ajouter un contrôle ReportViewer à l'application](../reporting-services/lesson-6-add-a-reportviewer-control-to-the-application.md)  
[Leçon 7 : ajouter une action d’extraction dans le rapport parent](../reporting-services/lesson-7-add-drillthrough-action-on-parent-report.md)  
[Leçon 8 : créer un filtre de données](../reporting-services/lesson-8-create-a-data-filter.md)  
[Leçon 9 : générer et exécuter l'application](../reporting-services/lesson-9-build-and-run-the-application.md)  
  
## <a name="see-also"></a> Voir aussi  
[Didacticiels sur Reporting Services &#40;SSRS&#41;](../reporting-services/reporting-services-tutorials-ssrs.md)  
[Concevoir des rapports à l’aide du Concepteur de rapports &#40;SSRS&#41;](../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md)  
  

