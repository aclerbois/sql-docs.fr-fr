---
title: SQL Server Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords:
- SSIS
helpviewer_keywords:
- SSIS
- DTS [Integration Services]
- SQL Server Integration Services
- Integration Services
- DTS [Integration Services], about Integration Services
- data integration [Integration Services]
- Data Transformation Services
ms.assetid: c4398655-5657-4ae4-a690-a380790fe84f
caps.latest.revision: 51
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0cbe57fde9348e096ff365189ae82558601b724f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-integration-services"></a>SQL Server Integration Services

 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [SQL Server Integration Services](https://msdn.microsoft.com/library/ms141026(SQL.120).aspx).

[!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] est une plateforme qui permet de créer des solutions de transformation de données et d’intégration de données au niveau de l’entreprise. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] vous permet de résoudre des problèmes professionnels complexes en copiant ou en téléchargeant des fichiers, en envoyant des messages électroniques en réponse à des événements, en mettant à jour des entrepôts de données, en nettoyant et en explorant des données et en gérant des données et des objets [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Les packages peuvent fonctionner en mode autonome ou de concert avec d'autres packages en réponse à des besoins professionnels complexes. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] peut extraire et transformer des données provenant d’une grande variété de sources, par exemple des fichiers de données XML, des fichiers plats et des sources de données relationnelles, puis charger les données dans une ou plusieurs destinations.<br /><br /> [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] inclut un ensemble étendu de tâches et de transformations intégrées, des outils pour construire des packages, et le service [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] permettant d’exécuter et de gérer des packages. Vous pouvez utiliser les outils graphiques de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour créer des solutions sans écrire une seule ligne de code. Vous pouvez également programmer le modèle objet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] étendu pour créer des packages par programmation, et coder des tâches personnalisées et d’autres objets de package.

## <a name="try-sql-server-and-sql-server-integration-services"></a>Essayer SQL Server et SQL Server Integration Services
- [![Télécharger à partir du Centre d’évaluation](../includes/media/download2.png)](http://go.microsoft.com/fwlink/?LinkID=829477) [Télécharger SQL Server 2017 ou 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server)
- [![Télécharger à partir du Centre d’évaluation](../includes/media/download2.png)](../ssdt/download-sql-server-data-tools-ssdt.md) [Télécharger SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [![Télécharger à partir du Centre d’évaluation](../includes/media/download2.png)](../ssms/download-sql-server-management-studio-ssms.md) [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

##  <a name="infotipsql-servermediainfo-tippng-resources"></a>![info_tip](../sql-server/media/info-tip.png) Ressources
-   [Obtenir de l’aide sur le forum SSIS](https://social.msdn.microsoft.com/Forums/home?forum=sqlintegrationservices)
-   [Obtenir de l’aide sur Stack Overflow](http://stackoverflow.com/questions/tagged/ssis)  
-   [Suivre le blog de l’équipe SSIS](https://blogs.msdn.microsoft.com/ssis/)
-   [Signaler des problèmes et demander des fonctionnalités](https://feedback.azure.com/forums/908035-sql-server)
-   [Obtenir les documents sur votre PC](../sql-server/sql-server-help-installation.md)
