---
title: 'Vue d’ensemble : exportation de données vers Excel (Complément MDS pour Excel) | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: microsoft-excel-add-in
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b628548b-982b-4e45-abf4-c8e83e3ab1c2
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 54d22cd0f649c8a30a4d0eb5c69240ac2da9d4d1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="overview-exporting-data-to-excel-mds-add-in-for-excel"></a>Vue d’ensemble : exportation de données vers Excel (Complément MDS pour Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans le [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], vous devez exporter les données du référentiel MDS dans une feuille de calcul Excel active avant de pouvoir les utiliser. Lorsque vous avez terminé de manipuler les données, importez-les dans le référentiel MDS afin que d’autres utilisateurs puissent les utiliser.  
  
 Les données que vous pouvez exporter sont celles auxquelles vous avez l’autorisation d’accéder. Les autorisations d'accès aux données sont définies dans l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] ou par programme.  
  
 Lorsque vous exportez un grand nombre de données, vous pouvez définir des avertissements qui s’affichent lorsque le chargement risque d’être très long. Pour cela, dans le groupe **Options** , cliquez sur **Paramètres**. Sous l’onglet **Données** , sélectionnez **Afficher l’avertissement de filtrage pour les jeux de données volumineux**.  
  
> [!WARNING]  
>  Un classeur compatible DM doit être ouvert et mis à jour uniquement dans Excel avec le complément MDS pour Excel. L'ouverture d'un classeur compatible DM dans Excel sur un ordinateur sur lequel le complément MDS Excel n'est pas installé n'est pas prise en charge et peut endommager le fichier du classeur. Si vous souhaitez partager des données avec une autre personne, envoyez-lui un fichier de requête de raccourci par courrier électronique au lieu d'enregistrer la feuille de calcul et de la lui envoyer par courrier électronique. Pour plus d’informations sur la requête, consultez [Envoyer par e-mail un fichier de requête de raccourci &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/email-a-shortcut-query-file-mds-add-in-for-excel.md).  
  
## <a name="filtering-data"></a>Filtrage des données  
 Vous pouvez filtrer les données avant l’exportation afin de limiter le volume de données que vous allez télécharger. Par exemple, vous pouvez choisir les attributs (colonnes) que vous souhaitez charger, l'ordre d'affichage des attributs et les membres (lignes de données) que vous souhaitez utiliser. Pour plus d’informations, consultez [Filtrer les données avant l’exportation &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md).  
  
## <a name="connect-automatically-and-load-frequently-used-data"></a>Connexion automatique et chargement de données fréquemment utilisées  
 Si vous souhaitez vous connecter toujours au même serveur et exporter le même jeu de données, vous pouvez créer des fichiers de requête de raccourci contenant les informations de connexion et de filtre. Pour plus d’informations sur les fichiers de requête, consultez [Fichiers de requête de raccourci &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
## <a name="refreshing-data"></a>Actualisation des données  
 Les données du référentiel MDS peuvent être mises à jour par d’autres utilisateurs après que vous les avez exportées. Vous pouvez extraire ces données sans perdre les modifications apportées aux données non managées MDS. Pour plus d’informations, consultez [Actualisation des données &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/refreshing-data-mds-add-in-for-excel.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Filtrez les données MDS avant de les charger dans Excel.|[Filtrer les données avant l’exportation &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)|  
|Chargez des données MDS dans Excel.|[Exporter des données vers Excel à partir de Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
|Modifiez l'ordre des colonnes avant de télécharger les données.|[Réorganiser des colonnes &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/reorder-columns-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Connexions &#40;Complément MDS pour Excel#41;](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [Fichiers de requête de raccourci &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)  
  
-   [Complément Master Data Services pour Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
-   [Sécurité &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md)  
  
  
