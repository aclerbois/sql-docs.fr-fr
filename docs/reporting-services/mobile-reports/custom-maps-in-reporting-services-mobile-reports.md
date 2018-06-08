---
title: Cartes personnalisées dans les rapports mobiles Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: mobile-reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 59a4ebad-587a-4770-afcd-c69216b8afd9
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: ea22c2ea60a681accc747e9426fbecb4aad7b515
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-maps-in-reporting-services-mobile-reports"></a>Custom maps in Reporting Services mobile reports
Dans l’Éditeur de rapports mobiles SQL Server, les cartes géographiques sont définies dans un format appelé *fichiers de forme ESRI*.  
  
Ce format semi-ouvert étendu initialement conçu par une société privée est utilisé dans une grande partie des applications GIS. Pour se conformer à ce format, l’Éditeur de rapports mobiles requiert la fourniture de deux fichiers lors de la définition d’une carte :  
  
- Un fichier .SHP pour les géométries de forme  
- Un fichier .DBF pour les métadonnées  
  
Les noms des fichiers de base doivent correspondre (par ex., *canada.shp* et *canada.dbf*). Les métadonnées doivent inclure le champ *NAME* avec la valeur du nom de la forme correspondante (key), pour être utilisées lors de l’ajout des données dans la carte.  
  
> **Remarque**: la taille cumulée des deux fichiers de mappage (SHP et DBF) ne doit pas dépasser 512 Ko. Si les fichiers de mappage sont trop volumineux, vous pouvez réduire leur taille à l’aide d’un outil tel que [http://mapshaper.org/](http://mapshaper.org/).  
  
Découvrez comment [ajouter des mappages personnalisés aux rapports mobiles](../../reporting-services/mobile-reports/add-a-custom-map-to-a-reporting-services-mobile-report.md).  
  
## <a name="technical-information"></a>Informations techniques  
  
- La spécification officielle : [http://www.esri.com/library/whitepapers/pdfs/shapefile.pdf](http://www.esri.com/library/whitepapers/pdfs/shapefile.pdf)  
- L’article Wikipedia sur le fichier de forme : [http://en.wikipedia.org/wiki/Shapefile](http://en.wikipedia.org/wiki/Shapefile)  
  
## <a name="creating--editing-map-geometry"></a>Création et modification d’une géométrie de carte  
  
La création et la modification des fichiers de forme sont un processus complexe qui n’entre pas dans le cadre de ce document. Voici quelques ressources et applications qui vous aideront à démarrer :  
  
- ArcGIS : [http://www.arcgis.com/](http://www.arcgis.com/)  
- Plug-in MAPublisher pour Adobe Illustrator : [http://www.avenza.com/mapublisher](http://www.avenza.com/mapublisher)  
- QuantumGIS (gratuit) : [http://www.qgis.org/](http://www.qgis.org/)  
- Manco ShapeFile Editor : [http://www.mancosoftware.com/ShapeFileEditor](http://www.mancosoftware.com/ShapeFileEditor)  
  
## <a name="existing-shapefiles"></a>Fichiers de forme existants  
  
Il est possible de télécharger plusieurs fichiers de forme existants sur Internet, à partir de sites comme ceux qui suivent :  
  
- Diva-GIS : [http://www.diva-gis.org/Data](http://www.diva-gis.org/Data)  
- OpenStreetMap : [http://openstreetmapdata.com/data](http://openstreetmapdata.com/data)  
  
### <a name="see-also"></a>Voir aussi  
- [Maps in Reporting Services mobile reports](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)  
- [Créer et publier des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)   
  
  
  
