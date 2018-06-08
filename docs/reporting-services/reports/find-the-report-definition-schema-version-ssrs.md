---
title: Rechercher la version du schéma de définition de rapport | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XML schemas [Reporting Services]
- Report Definition Language, XML schema
- schemas [Reporting Services]
ms.assetid: 67954419-1b61-4481-a3b9-23b4ba7a5624
caps.latest.revision: 15
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: ebdae6ba0b9b188256823fd2d682b6163aed7a58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="find-the-report-definition-schema-version-ssrs"></a>Rechercher la version du schéma de définition de rapport (SSRS)

Un fichier de définition de rapport spécifie l'espace de noms RDL de la version du schéma de définition de rapport qui est utilisée pour valider le fichier rdl. Lorsque vous ouvrez un fichier .rdl dans un environnement de création de rapports, tel que le Concepteur de rapports de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou le Générateur de rapports, si le rapport a déjà été créé pour un espace de noms, un fichier de sauvegarde est automatiquement créé et le rapport est mis à niveau d'après l'espace de noms actuel. Si vous enregistrez la définition de rapport mise à niveau, vous enregistrez le fichier .rdl converti. Il s'agit de la seule façon de mettre à niveau une définition de rapport. La définition de rapport proprement dite n'est pas mise à niveau sur un serveur de rapports. Le rapport compilé est mis à niveau sur un serveur de rapports. Pour plus d'informations, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md).  
  
### <a name="how-to-identify-the-rdl-schema-version-of-a-report"></a>Procédure : identifier la version du schéma RDL d'un rapport  
  
1.  Ouvrez le fichier de rapport .rdl dans une application, telle que le Bloc-notes ou XML Notepad 2007, dans laquelle vous pouvez visualiser le fichier xml.  
  
     L'élément de rapport XML indique l'espace de noms du schéma. Par exemple, l'élément de rapport suivant indique l'espace de noms pour le Concepteur de rapports et l'espace de noms pour la définition du rapport.  
  
    ```  
    <Report xmlns:rd=http://schemas.microsoft.com/SQLServer/reporting/reportdesigner   
    xmlns="http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition">  
    ```  
  
     L'espace de noms de la définition de rapport est spécifié par l'URL suivante : `http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition`.  
  
### <a name="how-to-identify-the-rdl-schema-version-of-report-designer"></a>Procédure : identifier la version du schéma RDL du Concepteur de rapports  
  
1.  Ouvrez un nouveau projet. La version du projet que vous choisissez détermine la version du schéma RDL. Dans SQL Server, plusieurs versions de schéma sont prises en charge. Pour plus d’informations, consultez [Déploiement et prise en charge des versions dans SQL Server Data Tools](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
2.  Dans le menu **Projet** , cliquez sur **Ajouter un nouvel élément**. La boîte de dialogue **Ajouter un nouvel élément** s’ouvre.  
  
3.  Dans le volet **Modèles** , cliquez sur **Rapport**.  
  
4.  Dans la zone **Nom**, tapez un nom de rapport ou acceptez la valeur par défaut.  
  
5.  Cliquez sur **Ajouter**. Le Concepteur de rapports ouvre un rapport vide en mode Création.  
  
6.  Dans le menu **Affichage** , cliquez sur **Code**. La définition du rapport s'affiche sous forme de fichier XML.  
  
     L'élément de rapport XML indique l'espace de noms du schéma. Par exemple, l'élément de rapport suivant indique l'espace de noms pour le Concepteur de rapports et l'espace de noms pour la définition du rapport.  
  
    ```  
    <Report xmlns:rd=http://schemas.microsoft.com/SQLServer/reporting/reportdesigner  
    xmlns="http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition">  
    ```  
  
     L'espace de noms de la définition de rapport est spécifié par l'URL suivante : `http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition`  
  
### <a name="how-to-identify-the-rdl-schema-version-on-the-report-server"></a>Procédure : identifier la version du schéma RDL sur Report Server  
  
-   Dans le Gestionnaire de rapports, tapez l'URL du serveur de rapports. Par exemple, l'URL suivante spécifie un serveur de rapports sur l'ordinateur local :  
  
     `http://localhost/reportserver/reportdefinition.xsd`  
  
     Le fichier .xsd s'ouvre dans le navigateur.  
  
     L'élément de schéma XML indique l'espace de noms du schéma. Par exemple, l’élément de schéma suivant indique trois espaces de noms : la référence targetNamespace utilisée en interne par [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], la référence xsd pour le schéma lui-même (xsd) et la référence de définition de rapport.  
  
    ```  
    <xsd:schema   
    targetNamespace="http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
    xmlns="http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition"   
    elementFormDefault="qualified">  
    ```  
  
     L'espace de noms de la définition de rapport est spécifié par l'URL suivante : `http://schemas.microsoft.com/sqlserver/reporting/2009/01/reportdefinition`  

## <a name="next-steps"></a>Étapes suivantes

[Rapports de mise à niveau](../../reporting-services/install-windows/upgrade-reports.md)   
[Langage de définition des rapports](../../reporting-services/reports/report-definition-language-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
