---
title: Recherche des GUID du jeu de propriétés et des ID d’entier de propriétés pour les propriétés de recherche | Microsoft Docs
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
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], configuring
ms.assetid: 7db79165-8bcc-4be6-8d40-12d44deda79f
caps.latest.revision: 32
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 974155cff19984223d015c5ef38165ece3517c97
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="find-property-set-guids-and-property-integer-ids-for-search-properties"></a>Recherche des GUID du jeu de propriétés et des ID d'entier de propriétés pour les propriétés de recherche
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique explique comment extraire les valeurs requises avant d'ajouter une propriété à une liste de propriétés de recherche et les rendre détectables par la recherche en texte intégral. Ces valeurs incluent le GUID du jeu de propriétés et l'identificateur entier d'une propriété de document.  
  
 Les propriétés de document qui sont extraites par IFilters à partir des données binaires, c’est-à-dire les données stockées dans un type de colonne de données **varbinary**, **varbinary(max)** (y compris **FILESTREAM**) ou **image** , peuvent être rendues disponibles pour la recherche en texte intégral. Pour rendre une propriété extraite détectable, la propriété doit être ajoutée manuellement à une liste de propriétés de recherche. La liste des propriétés de recherche doit également être associée à un ou plusieurs index de recherche en texte intégral. Pour plus d’informations, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 Avant d'ajouter une propriété disponible à une liste de propriétés, vous devez obtenir deux informations sur la propriété :  
  
-   le GUID du jeu de propriétés de la propriété ;  
  
-   l'ID d'entier de la propriété ;  
  
 (Lorsque vous ajoutez une propriété à une liste de propriétés, vous devez également fournir un nom et une description. Toutefois vous ne devez pas utiliser le nom canonique et la description de la propriété.)  
  
 Cette rubrique décrit les méthodes couramment utilisées pour rechercher des informations sur les propriétés disponibles, notamment sur les propriétés définies par Microsoft. Pour plus d'informations sur les propriétés définies par un tiers, reportez-vous à la documentation de ce tiers ou contactez le fournisseur.  
  
##  <a name="wellknown"></a> Recherche d'informations sur les propriétés Microsoft largement répandues et connues  
 Microsoft définit des centaines de propriétés de document utiles dans de nombreux contextes, mais seul un modeste sous-ensemble des propriétés disponibles est utilisé par chaque format de fichier. Parmi les propriétés Windows fréquemment utilisées se trouve un petit jeu de propriétés génériques. Quelques exemples de propriétés génériques connues sont présentés dans le tableau suivant. Le tableau indique le nom connu, le nom canonique Windows (issu de la description de la propriété publiée par Microsoft), le GUID du jeu de propriétés, l'identificateur entier de propriété et une brève description.  
  
|Nom connu|Nom canonique Windows|GUID du jeu de propriétés|ID entier|Description|  
|----------------------|----------------------------|-----------------------|----------------|-----------------|  
|Auteurs|**System.Author**|F29F85E0-4FF9-1068-AB91-08002B27B3D9|4|Auteur ou auteurs d'un élément donné.|  
|Balises|**System.Keywords**|F29F85E0-4FF9-1068-AB91-08002B27B3D9|5|Ensemble de mots clés (également appelé balises) affecté à l'élément.|  
|Type|**System.PerceivedType**|28636AA6-953D-11D2-B5D6-00C04FD918D0|9|Type de fichier perçu selon son type canonique.|  
|Titre|**System.Title**|F29F85E0-4FF9-1068-AB91-08002B27B3D9|2|Titre de l'élément. Par exemple, le titre d'un document, l'objet d'un message, la légende d'une photo ou le nom d'une piste de musique.|  
  
 Pour favoriser la cohérence parmi les formats de fichier, Microsoft a identifié des sous-ensembles de propriétés du document fréquemment utilisées et prioritaires pour plusieurs catégories de documents. Celles-ci incluent des communications, des contacts, des documents, des fichiers de musique, des images et des vidéos. Pour plus d’informations sur les propriétés principales de chaque catégorie, consultez [Propriétés définies par le système pour les formats de fichiers personnalisés](http://go.microsoft.com/fwlink/?LinkId=144336) dans la documentation Windows Search.  
  
 Un format de fichier spécifique peut implémenter des propriétés de trois types :  
  
-   Propriétés génériques définies par [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
-   Propriétés spécifiques à la catégorie définies par [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
-   Propriétés personnalisées, spécifiques à l'application définies par le fournisseur de logiciels.  
  
##  <a name="filtdump"></a> Recherche d'informations sur les propriétés disponibles à l'aide de FILTDUMP.EXE  
 Pour savoir quelles propriétés sont détectées et extraites, le cas échéant, par un IFilter installé, vous pouvez installer et exécuter l’utilitaire **filtdump.exe** , qui fait partie du Kit de développement logiciel (SDK) [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Exécutez **filtdump.exe** à partir de l’invite de commandes et fournissez un argument unique. Cet argument est le nom d'un fichier individuel dont le type de fichier correspond à un IFilter installé. L'utilitaire affiche une liste de toutes les propriétés identifiées par l'IFilter dans le document, avec leurs GUID de jeu de propriétés, leurs ID d'entier et des informations supplémentaires.  
  
 Pour plus d’informations sur l’installation de ce logiciel, consultez [Kit de développement logiciel (SDK) Microsoft Windows pour Windows 7 et .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=212980). Après avoir téléchargé et installé le Kit de développement logiciel (SDK), recherchez l'utilitaire filtdump.exe dans les dossiers suivants.  
  
-   Pour la version 64 bits, recherchez dans `C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin\x64`.  
  
-   Pour la version 32 bits, recherchez dans `C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin`.  
  
##  <a name="propdesc"></a> Recherche de valeurs pour une propriété de recherche à partir d'une description de la propriété Windows  
 Pour une propriété de recherche Windows connue, vous pouvez obtenir ces informations à partir des attributs **formatID** et **propID** de la description de la propriété (**propertyDescription**).  
  
 L'exemple suivant montre la partie pertinente d'une description de propriété Microsoft typique, dans ce cas, la propriété `System.Author` . L'attribut `formatID` spécifie le GUID du jeu de propriétés, `F29F85E0-4FF9-1068-AB91-08002B27B3D9`et l'attribut `propID` spécifie l'ID entier de propriété `4.` . Remarquez que l'attribut `name` spécifie le nom canonique Windows de la propriété `System.Author`. (Cet exemple omet les parties de la description de la propriété qui ne sont pas appropriées.)  
  
```  
.  
propertyDescription  
name = System.Author  
…  
formatID = F29F85E0-4FF9-1068-AB91-08002B27B3D9  
propID = 4  
…  
```  
  
 Pour obtenir la description complète de cette propriété, consultez [System.Author](http://go.microsoft.com/fwlink/?LinkId=144337) dans la documentation Windows Search.  
  
 Pour obtenir une liste complète des propriétés Windows, consultez [Propriétés Windows](http://go.microsoft.com/fwlink/?LinkId=215013)et aussi la documentation Windows Search.  
  
##  <a name="examples"></a> Ajout d'une propriété à une liste de propriétés de recherche  
 L'exemple suivant indique comment ajouter une propriété à une liste de propriétés de recherche. L’exemple utilise une instruction [ALTER SEARCH PROPERTY LIST](../../t-sql/statements/alter-search-property-list-transact-sql.md) pour ajouter la propriété `System.Author` à une liste de propriétés de recherche nommée `PropertyList1`, et fournit un nom convivial à la propriété, `Author`.  
  
```  
ALTER SEARCH PROPERTY LIST PropertyList1   
  ADD 'Author'  
    WITH (  
          PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9',  
          PROPERTY_INT_ID = 4,   
          PROPERTY_DESCRIPTION = 'System.Author - the author or authors of the item'   
         )  
GO  
```  
  
 Pour plus d’informations sur la création d’une liste de propriétés de recherche et son association à un index de recherche en texte intégral, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md)  
  
  
