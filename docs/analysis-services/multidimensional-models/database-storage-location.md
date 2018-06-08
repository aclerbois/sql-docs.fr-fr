---
title: Emplacement de stockage de base de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f9f96b5a919a898f987892fb517ed9687b1d2498
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="database-storage-location"></a>Emplacement de stockage de la base de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Il existe souvent des situations où un administrateur de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (dba) souhaite qu'une certaine base de données réside en dehors du dossier des données du serveur. Ces situations sont souvent générées par des exigences opérationnelles, telles que l'amélioration des performances ou le développement du stockage. Pour ces situations, la propriété de base de données **DbStorageLocation** permet au dba [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de spécifier l’emplacement de la base de données sur un disque local ou un périphérique réseau.  
  
## <a name="dbstoragelocation-database-property"></a>Propriété de base de données DbStorageLocation  
 La propriété de base de données **DbStorageLocation** spécifie le dossier où [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] crée et gère l’ensemble des fichiers de données et de métadonnées de la base de données. Tous les fichiers de métadonnées sont stockés dans le dossier **DbStorageLocation** , à l’exception du fichier de métadonnées de la base de données, qui est stocké dans le dossier de données du serveur. Deux considérations importantes doivent être prises en compte lors de la définition de la valeur de la propriété de base de données **DbStorageLocation** :  
  
-   La propriété de base de données **DbStorageLocation** doit être définie avec un chemin d'accès au dossier UNC existant ou avec une chaîne vide. Une chaîne vide est la valeur par défaut pour le dossier de données de serveur. Si le dossier n’existe pas, une erreur est générée lors de l’exécution d’une commande **Create**, **Attach**ou **Alter** .  
  
-   La propriété de base de données **DbStorageLocation** ne peut pas être définie pour pointer sur le dossier de données du serveur ou sur un de ses sous-dossiers. Si l’emplacement pointe vers le dossier de données du serveur ou un de ses sous-dossiers, une erreur est générée quand vous exécutez une commande **Create**, **Attach**ou **Alter** .  
  
> [!IMPORTANT]  
>  Nous vous recommandons de définir votre chemin d'accès UNC pour l'utilisation d'un réseau de zone de stockage (SAN), d'un réseau basé sur iSCSI ou d'un disque attaché localement. Tout chemin d'accès UNC menant à un partage réseau ou toute solution de stockage à distance à latence élevée conduit à une installation non prise en charge.  
  
### <a name="dbstoragelocation-compared-to-storagelocation"></a>Comparaison entre DbStorageLocation et StorageLocation  
 **DbStorageLocation** spécifie le dossier où se trouvent tous les fichiers de données et de métadonnées de la base de données, alors que **StorageLocation** spécifie le dossier où se trouvent une ou plusieurs des partitions d’un cube. **Storagelocation** peut être défini indépendamment de **DbStorageLocation**. Il s’agit d’une décision du dba [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fondée sur les résultats attendus et, dans de nombreux cas, les utilisations d’une propriété ou d’une autre se chevauchent.  
  
## <a name="dbstoragelocation-usage"></a>Utilisation de DbStorageLocation  
 La propriété de base de données **DbStorageLocation** est utilisée dans le cadre d’une commande de base de données **Create** dans une séquence de commandes base de données **Detach**/**Attach** , dans une séquence de commandes base de données **Backup**/**Restore** ou dans une commande de base de données **Synchronize** . La modification de la propriété de base de données **DbStorageLocation** est considérée comme une modification structurelle de l’objet de base de données. Toutes les métadonnées doivent alors être recréées et les données traitées à nouveau.  
  
> [!IMPORTANT]  
>  Vous ne devez pas modifier l’emplacement de stockage de la base de données en utilisant une commande **Alter** . Au lieu de cela, nous vous recommandons d’utiliser une séquence de commandes de base de données **Detach**/**Attach** (consultez [Déplacer une base de données Analysis Services](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)et [Attacher et détacher des bases de données Analysis Services](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)).  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices.Database.DbStorageLocation%2A>   
 [Attacher et détacher des bases de données Analysis Services](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)   
 [Déplacer une base de données Analysis Services](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)   
 [Élément DbStorageLocation](../../analysis-services/xmla/xml-elements-properties/dbstoragelocation-element.md)   
 [Créer, élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md)   
 [Élément Attach](../../analysis-services/xmla/xml-elements-commands/attach-element.md)   
 [Synchroniser, élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md)  
  
  
