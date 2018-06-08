---
title: Spécifier la copie ou l’interrogation de table (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.specifytablecopyorquery.f1
ms.assetid: 08aa7158-40e6-4ef3-84d3-1265a8ba194c
caps.latest.revision: 69
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b7ca7c6b24514df7d27c42cfaab8aeb304454d5e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-table-copy-or-query-sql-server-import-and-export-wizard"></a>Spécifier la copie ou l'interrogation de table (Assistant Importation et Exportation SQL Server)
  Une fois que vous avez fourni les informations relatives à la destination de vos données et à la façon de s’y connecter, l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche **Spécifier la copie ou l’interrogation de table**. Dans cette page, choisissez l’une des options suivantes.
-   **Copier les données à partir d’une ou plusieurs tables ou vues**. Vous souhaitez sélectionner une ou plusieurs tables dans une liste.
-   **Écrire une requête pour spécifier les données à transférer**. Vous voulez entrer ou coller le texte d’une requête SQL.
    
> [!TIP]
> Si vous devez copier plusieurs bases de données ou des objets de base de données autres que des tables et des vues, utilisez l’Assistant Copie de base de données au lieu de l’Assistant Importation et exportation. Pour plus d’informations, consultez [Utiliser l’Assistant Copie de base de données](../../relational-databases/databases/use-the-copy-database-wizard.md).     
 
## <a name="screen-shot-of-the-specify-table-copy-or-query-page"></a>Capture d’écran de la page Spécifier la copie ou l’interrogation de table    
 La capture d’écran suivante montre la page **Spécifier la copie ou l’interrogation de table** de l’Assistant.    
    
 ![Page Spécifier la copie ou l’interrogation de table de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/table-copy-or-query.png "Page Spécifier la copie ou l’interrogation de table de l’Assistant Importation et Exportation")    
    
## <a name="specify-whether-to-copy-an-entire-table-or-write-a-query"></a>Indiquez s’il faut copier une table entière ou écrire une requête 
 **Copier les données à partir d’une ou plusieurs tables ou vues**    
 Sélectionnez cette option si vous voulez copier les données à partir de la source sans effectuer de filtrage ou de tri sur les enregistrements.   

Quand vous sélectionnez **Copier les données à partir d’une ou plusieurs tables ou vues**, vous pouvez effectuer la copie à partir d’une table ou une vue vers la table de destination, ou à partir de plusieurs tables ou vues vers plusieurs tables de destination.

 Après avoir cliqué sur **Suivant**, vous sélectionnez les tables à copier dans la page **Sélectionner les tables et les vues sources** . Pour plus d’informations, consultez [Sélectionner les tables et les vues sources](../../integration-services/import-export-data/select-source-tables-and-views-sql-server-import-and-export-wizard.md).   
    
 **Écrire une requête pour spécifier les données à transférer**    
 Sélectionnez cette option si vous voulez filtrer ou trier les données sources avant de les copier vers la destination.    
    
Quand vous sélectionnez **Écrire une requête pour spécifier les données à transférer**, vous pouvez uniquement copier les résultats d’une requête dans une table de destination.  

Après avoir cliqué sur **Suivant**, vous fournissez une instruction SQL pour spécifier les colonnes et sélectionner des lignes dans la boîte de dialogue **Fournir une requête source** . Pour plus d’informations, consultez [Fournir une requête source](../../integration-services/import-export-data/provide-a-source-query-sql-server-import-and-export-wizard.md).   
    
## <a name="why-isnt-the-copy-option-available"></a>Pourquoi l’option Copier n’est-elle pas disponible ?    
 L’option **Copier les données à partir d’une ou plusieurs tables ou vues** peut ne pas être disponible quand l’Assistant utilise un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour se connecter à votre source de données. Cela se produit quand l’Assistant ne dispose pas d’informations suffisantes sur le fournisseur de données pour demander une liste de tables et de vues à partir de la source de données. 
 
Dans la mesure où vous connaissez le nom de la table à exporter, vous pouvez toujours utiliser l’option **Écrire une requête**, même si en général vous n’écrivez pas de requêtes SQL. Dans la boîte de dialogue **Fournir une requête source**, visible après avoir cliqué sur **Suivant**, entrez la requête comme suit : `SELECT * FROM <name of table>`. Si le nom de la table contient des espaces ou d’autres caractères spéciaux, placez ce nom entre crochets, comme ceci : `SELECT * FROM [<name of table>]`.

### <a name="more-info"></a>En savoir plus
 L’option **Copier les données à partir d’une ou plusieurs tables ou vues** est disponible uniquement pour les fournisseurs qui disposent d’une section ProviderDescription dans le fichier ProviderDescriptors.xml. (Par défaut, ce fichier se trouve dans \<*lecteur*>:\Program Files\Microsoft SQL Server\130\DTS\ProviderDescriptors.) Chaque section ProviderDescription de ce fichier contient les informations nécessaires pour récupérer les métadonnées du fournisseur correspondant.    
    
 Par défaut, le fichier ProviderDescriptors.xml contient une section ProviderDescription uniquement pour les fournisseurs figurant dans la liste suivante.    
    
-   Fournisseur de données .NET Framework pour SQL Server (System.Data.SqlClient)    
    
-   Fournisseur de données .NET Framework pour Oracle (System.Data.OracleClient)    
    
-   Fournisseur de données .NET Framework pour ODBC (System.Data.Odbc)    
    
-    System.Data.OleDb (qui s’applique à tous les fournisseurs OLE DB)    
    
-   Fournisseur Microsoft pour DB2 installé par Microsoft Host Integration Server (Microsoft.HostIntegration.MsDb2Client.MsDb2Connection)    
    
 Les développeurs tiers peuvent rendre l’option **Copier les données à partir d’une ou plusieurs tables ou vues** disponible pour leurs fournisseurs en ajoutant une section ProviderDescriptor au fichier ProviderDescriptors.xml. Pour vérifier les spécifications de la section ProviderDescriptor, consultez le fichier de schéma ProviderDescriptors.xsd qui figure par défaut dans le même dossier que le fichier ProviderDescriptors.xml.    
    
## <a name="whats-next"></a>Quelle est l’étape suivante ?    
 Une fois que vous avez indiqué si vous voulez copier la totalité d’une table ou fournir une requête, la page suivante varie en fonction de l’option que vous avez choisie sur cette page, ainsi que de la destination de vos données.    
    
-   Si vous avez sélectionné **Copier les données à partir d’une ou plusieurs tables ou vues**, pour la plupart des destinations, la page suivante est **Sélectionner les tables et les vues sources**. Dans cette page, vous sélectionnez les tables et les vues existantes à copier à partir de la source de données vers la destination. Pour plus d’informations, consultez [Sélectionner les tables et les vues sources](../../integration-services/import-export-data/select-source-tables-and-views-sql-server-import-and-export-wizard.md).    
    
-   Si vous avez sélectionné **Copier les données à partir d’une ou plusieurs tables ou vues** et que votre destination est un fichier plat, la page suivante est **Configurer la destination du fichier plat**. Dans cette page, vous spécifiez les options de mise en forme du fichier plat de destination. (Une fois le fichier plat configuré, la page suivante est **Sélectionner les tables et les vues sources**.) Pour plus d’informations, consultez [Configurer la destination du fichier plat](../../integration-services/import-export-data/configure-flat-file-destination-sql-server-import-and-export-wizard.md).    
    
-   Si vous avez sélectionné **Écrire une requête pour spécifier les données à transférer**, la page suivante est **Fournir une requête source**. Dans cette page, vous écrivez et testez l’instruction SQL qui sélectionne les données à copier de la source de données vers la destination. (Une fois que vous avez fourni une requête, la page suivante est **Sélectionner les tables et les vues sources**.) Pour plus d’informations, consultez [Fournir une requête source](../../integration-services/import-export-data/provide-a-source-query-sql-server-import-and-export-wizard.md).

## <a name="see-also"></a>Voir aussi
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)


