---
title: Configurer et gérer des filtres pour la recherche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], filters
- filters [full-text search]
ms.assetid: 7ccf2ee0-9854-4253-8cca-1faed43b7095
caps.latest.revision: 68
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d5581011c38ace01ab61acf62c1d084893d8b9de
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="configure-and-manage-filters-for-search"></a>Configurer et gérer des filtres pour la recherche
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  L’indexation de documents dans une colonne de type de données **varbinary**, **varbinary(max)**, **image** ou **xml** nécessite un traitement supplémentaire. Ce traitement doit être effectué par un filtre. Le filtre extrait les informations textuelles du document (en supprimant la mise en forme). Le filtre envoie ensuite le texte au composant d'analyseur lexical pour la langue associée à la colonne de table.  
 
## <a name="filters-and-document-types"></a>Filtres et types de documents
Un filtre donné est spécifique à un type de document donné (.doc, .pdf, .xls, .xml, etc.). Ces filtres implémentent l'interface IFilter. Pour plus d’informations sur ces types de document, interrogez l’affichage catalogue [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) .  
  
Des documents binaires peuvent être stockés dans une colonne **varbinary(max)** ou **image** . Pour chaque document, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] choisit le filtre correct en fonction de l'extension de fichier. Dans la mesure où cette dernière n’est pas visible lorsque le fichier est stocké dans une colonne **varbinary(max)** ou **image** , l’extension de fichier (.doc, .xls, .pdf, etc.) doit figurer dans une autre colonne de la table, que l’on nomme colonne de type. Cette colonne de type peut être composée de n'importe quel type de données de caractères ; par ailleurs, elle contient l'extension de fichier du document, par exemple « .doc » pour un document [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word. Dans la table **Document** contenue dans [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)], la colonne **Document** est de type **varbinary(max)** alors que la colonne de type **FileExtension**est de type **nvarchar(8)**.  

**Pour voir la colonne de type dans un index de recherche en texte intégral existant**  
  
-   [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
  
> [!NOTE]  
>  Un filtre peut être en mesure de gérer des objets incorporés dans l'objet parent, selon son implémentation. Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne configure pas de filtres pour suivre des liens vers d'autres objets.  

## <a name="installed-filters"></a>Filtres installés 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe ses propres filtres XML et HTML. De plus, tous les filtres des formats propriétaires [!INCLUDE[msCoName](../../includes/msconame-md.md)] (.doc, .xdoc, .ppt, etc.) qui sont déjà installés sur le système d’exploitation sont également chargés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour identifier les filtres actuellement chargés sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez la procédure stockée [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md) , comme suit :  
  
```sql
EXEC sp_help_fulltext_system_components 'filter';   
```  
## <a name="non-microsoft-filters"></a>Filtres non-Microsoft
Toutefois, avant de pouvoir utiliser des filtres pour des formats non-[!INCLUDE[msCoName](../../includes/msconame-md.md)] , vous devez les charger manuellement dans l’instance de serveur. Pour plus d’informations sur l’installation de filtres supplémentaires, consultez [Afficher ou modifier des filtres et des analyseurs lexicaux inscrits](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)   
 [Compatibilité de FILESTREAM avec d'autres fonctionnalités SQL Server](../../relational-databases/blob/filestream-compatibility-with-other-sql-server-features.md)  
  
  
