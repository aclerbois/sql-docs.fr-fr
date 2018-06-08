---
title: Déploiement d’une extension pour le traitement des données | Microsoft Docs
ms.custom: ''
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- Extension element
- deploying [Reporting Services], extensions
ms.assetid: e5c0b5a9-1386-47cb-aade-96653ecfaa54
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: bd41f79d89e1514736b3453a4f6f038488b1200f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploying-a-data-processing-extension"></a>Déploiement d'une extension pour le traitement des données
  Une fois que vous avez écrit et compilé votre extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] dans une bibliothèque [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], vous devez la rendre détectable par le serveur de rapports et par le Concepteur de rapports. Cette opération est aussi simple que de copier l'extension vers les répertoires appropriés et d'ajouter des entrées aux fichiers de configuration [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] appropriés.  
  
## <a name="configuration-file-extension-element"></a>Élément Extension du fichier de configuration  
 Les extensions pour le traitement des données que vous déployez sur le serveur de rapports ou le Concepteur de rapports doivent être entrées en tant qu’éléments **Extension** dans les fichiers de configuration. Ces fichiers sont RSReportServer.config pour le serveur de rapports et RSReportDesigner.config pour le Générateur de rapports.  
  
 Le tableau suivant décrit les attributs de l’élément **Extension** pour les extensions pour le traitement des données.  
  
|Attribute|Description|  
|---------------|-----------------|  
|**Nom**|Nom unique de l'extension, par exemple, « SQL » pour l'extension pour le traitement des données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou « OLEDB » pour l'extension pour le traitement des données OLE DB. La longueur maximale de l'attribut **Name** est de 255 caractères. Le nom doit être unique au sein de toutes les entrées de l’élément **Extension** d’un fichier de configuration.|  
|**Type**|Liste séparée par des virgules qui inclut l'espace de noms complet, ainsi que le nom de l'assembly.|  
|**Visible**|La valeur **false** indique que l’extension pour le traitement des données ne doit pas être visible dans les interfaces utilisateur. Si cet attribut n'est pas défini, la valeur par défaut est **true**.|  
  
 Pour plus d’informations sur les fichiers RSReportServer.config ou RSReportDesigner.config, consultez [Fichiers de configuration de Reporting Services](../../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Procédure : déployer une extension pour le traitement des données sur un serveur de rapports](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-a-report-server.md)|Décrit comment déployer votre extension pour le traitement des données sur un serveur de rapports.|  
|[Procédure : déployer une extension pour le traitement des données sur le Concepteur de rapports](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-report-designer.md)|Décrit comment déployer votre extension pour le traitement des données sur le Générateur de rapports.|  
  
## <a name="see-also"></a> Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
