---
title: Guide d’architecture des pages et des étendues | Microsoft Docs
ms.custom: ''
ms.date: 10/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: relational-databases-misc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- page and extent architecture guide
- guide, page and extent architecture
ms.assetid: 83a4aa90-1c10-4de6-956b-7c3cd464c2d2
caps.latest.revision: 2
author: rothja
ms.author: jroth
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6f42360e4a0b3a23a7e39d390b711870e855129b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pages-and-extents-architecture-guide"></a>Guide d’architecture des pages et des étendues
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

L’unité fondamentale du stockage des données dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est la page. Une étendue est une collection de huit pages physiquement contiguës. Les étendues sont une aide précieuse pour la gestion des pages. Ce guide décrit les structures de données utilisées pour gérer les pages et les étendues dans toutes les versions de SQL Server. Il est essentiel de comprendre l'architecture des pages et étendues pour concevoir et développer des bases de données performantes.

## <a name="pages-and-extents"></a>Pages et étendues

L’unité fondamentale du stockage des données dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est la page. L’espace disque alloué à un fichier de données (.mdf ou .ndf) d’une base de données est logiquement divisé en pages numérotées consécutivement de 0 à n. Les opérations d'E/S disque sont effectuées au niveau page. En d’autres termes, SQL Server lit ou écrit des pages entières de données.

Les extensions sont une collection de huit pages physiques contiguës ; elles sont utilisées pour gérer les pages de manière efficace. Toutes les pages sont stockées dans des extensions.

### <a name="pages"></a>Pages

Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la taille des pages est de 8 Ko. Autrement dit, les bases de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] disposent de 128 pages par mégaoctet. Chaque page commence par un en-tête de 96 octets qui sert à stocker les informations système relatives à la page. Ces informations sont notamment le numéro de page, le type de page, la quantité d'espace disponible sur la page et l'ID de l'unité d'allocation de l'objet auquel appartient la page.

Le tableau suivant présente les types de page utilisés dans les fichiers de données d’une base de données SQL Server.

|Type de page | Sommaire |
|-------|-------|
|Data |Lignes de données avec toutes les données, sauf text, ntext, image, nvarchar(max), varchar(max), varbinary(max) et xml, lorsque le texte dans la ligne est défini sur ACTIVÉ (ON). |
|Index |Des entrées d'index. |
|Texte/image |Types de données d’objets volumineux : (text, ntext, image, nvarchar(max), varchar(max), varbinary(max) et données xml) <br> Colonnes de longueur variable lorsque la ligne de données dépasse 8 Ko : (varchar, nvarchar, varbinary et sql_variant) |
|GAM (Global Allocation Map), SGAM (Shared Global Allocation Map) |Informations précisant si ces extensions sont allouées. |
|Page Free Space (PFS) |Informations sur l'allocation des pages et sur l'espace disponible sur ces pages. |
|page IAM (Index Allocation Map) |Informations sur les extensions utilisées par une table ou un index par unité l'allocation. |
|mappage modifié en bloc |Informations sur les extensions modifiées par des opérations en bloc depuis la dernière instruction BACKUP LOG par unité d'allocation. |
|mappage de modification différentielle |Informations sur les extensions qui ont été modifiées depuis la dernière instruction BACKUP DATABASE par unité d'allocation. |

> [!NOTE]
> Les fichiers journaux ne contiennent pas de pages mais une série d'enregistrements de fichiers journaux.

Les lignes de données sont placées séquentiellement sur la page, immédiatement à partir de l'en-tête. Une table de décalage de lignes débute à la fin de la page et chaque table de décalage de lignes contient une entrée pour chaque ligne de la page. Chaque entrée enregistre la distance à laquelle se trouve le premier octet de la ligne par rapport au début de la page. Les entrées de la table de décalage de lignes sont inversées par rapport à l'ordre des lignes sur la page.

![page_architecture](../relational-databases/media/page-architecture.gif)

#### <a name="large-row-support"></a>Prise en charge des lignes de grande taille  

Les lignes ne peuvent pas couvrir plusieurs pages, mais des parties d'une ligne peuvent être déplacées hors de la page de la ligne, afin que la ligne puisse atteindre une très grande taille. La quantité maximale de données et de surcharge contenue dans une seule ligne d'une page est de 8 060 octets (8 Ko). Toutefois, cette valeur ne tient pas compte des données stockées dans le type de page Texte/Image. 

Cette restriction est assouplie pour les tables qui contiennent des colonnes varchar, nvarchar, varbinary ou sql_variant. Lorsque la taille totale de ligne de toutes les colonnes de longueur fixe et variable d'une table dépasse la limite des 8 060 octets, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] déplace de manière dynamique une ou plusieurs colonnes de longueur variable dans les pages de l'unité d'allocation ROW_OVERFLOW_DATA, en commençant par la colonne dont la largeur est la plus grande. 

Cette opération est réalisée chaque fois qu'une opération d'insertion ou de mise à jour augmente la taille totale de la ligne au-delà de la limite de 8 060 octets. Lorsqu'une colonne est déplacée dans une page de l'unité d'allocation ROW_OVERFLOW_DATA, un pointeur de 24 octets est conservé sur la page d'origine dans l'unité d'allocation IN_ROW_DATA. Si une opération ultérieure réduit la taille de la ligne, SQL Server redéplace de manière dynamique les colonnes dans la page de données d’origine. 

### <a name="extents"></a>Étendues 

Les extensions constituent l'unité de base dans laquelle l'espace est géré. Une extension est constituée de 8 pages contiguës, soit 64 Ko. Autrement dit, les bases de données SQL Server disposent de 16 étendues par mégaoctet.

Afin d’optimiser la gestion de l’espace, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’affecte pas d’étendues complètes aux tables possédant de petites quantités de données. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contient deux types d’étendues : 

* Les extensions **uniformes** appartiennent à un objet unique ; les huit pages de l’extension ne peuvent être utilisées que par l’objet propriétaire.
* Les extensions **mixtes** sont partagées par huit objets au plus. Chacune des huit pages de l'extension peut être la propriété d'un objet différent.

Une nouvelle table ou un nouvel index est en général affecté de pages issues d'extensions mixtes. Lorsque la table ou l'index atteint huit pages, il bascule à l'utilisation des extensions uniformes pour les allocations suivantes. Si vous créez un index sur une table existante qui possède un nombre de lignes suffisant pour générer huit pages dans l'index, toutes les allocations à l'index se trouvent dans des extensions uniformes.

![Étendues](../relational-databases/media/extents.gif)

## <a name="managing-extent-allocations-and-free-space"></a>Gestion des allocations des extensions et de l'espace libre 

Les structures de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui gèrent les allocations des extensions et l’espace libre ont une structure relativement simple. Elles présentent les avantages suivants : 

* Les informations sur l'espace libre sont très compactes, d'où un nombre de pages d'informations relativement faible.   
  La vitesse s'en trouve augmentée grâce à la réduction du nombre de lectures sur le disque nécessaires à la récupération des informations d'allocation et à l'augmentation de la possibilité de garder en mémoire l'affectation des pages, ce qui réduit encore le nombre de lectures. 

* La plupart des informations d'allocation ne sont pas enchaînées les unes aux autres, ce qui simplifie leur gestion.    
  Chaque allocation ou désallocation de page peut être effectuée rapidement, ce qui diminue les problèmes de contention entre les tâches simultanées nécessitant une allocation ou une désallocation de pages. 

### <a name="managing-extent-allocations"></a>Gestion des allocations des extensions

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise deux types de tables d’allocation pour enregistrer l’allocation des extensions : 

- **Pages GAM (Global Allocation Map)**   
  Les pages GAM enregistrent les extensions qui ont été allouées. Chaque table GAM prend en charge 64 000 extensions, soit près de 4 Go de données. Elle est constituée d'un bit pour chaque extension dans l'intervalle couvert. Si la valeur du bit est 1, l'extension est libre. En revanche, si sa valeur est 0, l'extension est allouée. 

- **Pages SGAM (Shared Global Allocation Map)**   
  Les pages SGAM enregistrent les extensions actuellement utilisées comme extensions mixtes et possédant au moins une page inutilisée. Chaque table SGAM prend en charge 64 000 étendues, soit près de 4 Go de données. Elle est constituée d'un bit pour chaque étendue dans l'intervalle couvert. Si la valeur du bit est 1, l'extension est utilisée comme extension mixte et possède une page libre. Si la valeur du bit est 0, l'extension n'est pas utilisée comme extension mixte ou correspond à une extension mixte dont toutes les pages sont utilisées. 

Chaque extension possède les schémas de bits suivants dans les tables GAM et SGAM, en fonction de son utilisation actuelle. 

|Utilisation actuelle de l'extension | Valeur du bit GAM | Valeur du bit SGAM |
|---------|----------|------| 
|Libre, inutilisée | 1 |0 |
|Extension uniforme ou extension mixte complète |0 |0 |
|Extension mixte avec pages libres |0 | 1 |
 
Ceci se traduit par des algorithmes simples de gestion des extensions. 
-   Pour allouer une extension uniforme, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche un bit 1 dans la table GAM et lui affecte la valeur 0. 
-   Pour trouver une extension mixte comportant des pages libres, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche un bit 1 dans la table SGAM. 
-   Pour allouer une extension mixte, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche un bit 1 dans la table GAM, lui affecte la valeur 0, puis affecte la valeur 1 au bit correspondant dans la table SGAM. 
-   Pour désallouer une extension, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] s’assure que le bit de la table GAM a la valeur 1 et que le bit de la table SGAM a la valeur 0. Les algorithmes qui sont effectivement utilisés en interne par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sont plus complexes que ceux dont il est fait mention ici, du fait que le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] répartit les données uniformément dans une base de données. Toutefois, même les algorithmes réels sont simplifiés afin de ne plus devoir gérer les chaînes d'informations d'allocation des extensions.

### <a name="tracking-free-space"></a>Suivi de l’espace libre

Les pages **PFS (Page Free Space)** enregistrent quand une page individuelle a été allouée, le statut d’allocation et la quantité d’espace libre de chaque page. Chaque page correspond à un octet qui enregistre si la page a été allouée et, le cas échéant, si elle est vide, pleine à 1-50 %, 51-80 %, 81-95 % ou 96-100 %.

Une fois une étendue allouée à un objet, le moteur de base de données utilise les pages PFS pour enregistrer les pages de l’étendue qui sont allouées ou libres. Ces informations sont alors utilisées par le moteur de base de données pour l’allocation de toute nouvelle page. La quantité d'espace libre d'une page n'est conservée que pour les pages de segment, de texte et d'image. Ces informations sont exploitées par le moteur de base de données pour rechercher une page disposant de suffisamment d’espace libre pour accueillir une nouvelle ligne. Pour les index, le suivi de l'espace libre des pages n'est pas nécessaire étant donné que le point d'insertion d'une nouvelle ligne est défini par les valeurs de clés de l'index.

Une page PFS vient juste après la page d’en-tête d’un fichier de données (ID de page 1). Elle est suivie d’une page GAM (ID de page 2), puis d’une page SGAM (ID de page 3). Il y a une page PFS approximativement 8 000 pages après la première page PFS. Il y a une autre page GAM 64 000 extensions après la première page GAM (page 2) et une autre page SGAM 64 000 extensions après la première page SGAM (page 3). L’illustration suivante indique l’ordre des pages utilisées par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour l’allocation et la gestion des extensions.

![manage_extents](../relational-databases/media/manage-extents.gif)

## <a name="managing-space-used-by-objects"></a>Gestion de l’espace utilisé par les objets 

Une page **IAM (Index Allocation Map)** mappe les étendues d’une portion de 4 gigaoctets (Go) d’un fichier de base de données utilisées par une unité d’allocation. Une unité d'allocation peut être de trois types :

- IN_ROW_DATA   
    Contient une partition d'un segment ou d'un index.

- LOB_DATA   
   Contient des types de données d’objets volumineux (LOB), tels que xml, varbinary(max) et varchar(max).

- ROW_OVERFLOW_DATA   
   Contient des données de longueur variable stockées dans des colonnes varchar, nvarchar, varbinary ou sql_variant qui dépassent le seuil de 8 060 octets par ligne. 

Chaque partition d'un segment ou d'un index contient au moins une unité d'allocation IN_ROW_DATA. Elle peut aussi contenir une unité d'allocation LOB_DATA ou ROW_OVERFLOW_DATA, selon le schéma de segment ou d'index. Pour plus d’informations sur les unités d’allocation, consultez Organisation des tables et des index.

Une page IAM couvre une plage de 4 Go dans un fichier, comme une page GAM ou SGAM. Si l'unité d'allocation contient des étendues provenant de plusieurs fichiers, ou plusieurs plages de 4 Go dans un fichier, il y aura plusieurs pages IAM liées entre elles dans une chaîne IAM. Ainsi, chaque unité d'allocation contient au moins une page IAM pour chaque fichier dans lequel elle possède des étendues. Un fichier peut aussi contenir plusieurs pages IAM si la plage d'étendues du fichier allouée à l'unité d'allocation dépasse la plage que peut enregistrer une page IAM unique. 

![iam_pages](../relational-databases/media/iam-pages.gif)

Les pages IAM sont allouées au fur et à mesure des besoins pour chaque unité d'allocation et elles sont placées aléatoirement dans le fichier. La vue système, sys.system_internals_allocation_units, pointe sur la première page IAM d’une unité d’allocation. Toutes les pages IAM de cette unité d'allocation sont liées entre elles et forment une chaîne.

> [!IMPORTANT]
> La vue système sys.system_internals_allocation_units est destinée exclusivement à un usage interne et elle est susceptible de changer. La compatibilité n'est pas garantie.

![iam_chain](../relational-databases/media/iam-chain.gif)
 
Pages IAM liées dans une chaîne par unité d’allocation. Une page IAM possède un en-tête indiquant l’étendue de départ de la plage d’étendues mappée par la page IAM. La page IAM contient aussi une grande image dans laquelle chaque bit représente une étendue. Le premier bit représente la première étendue de la plage, le second la deuxième étendue, et ainsi de suite. Si un bit est égal à 0, l'étendue qu'il représente n'est pas allouée à l'unité d'allocation possédant la page IAM. Si un bit est égal à 1, l'étendue qu'il représente est allouée à l'unité d'allocation possédant la page IAM.

Quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] a besoin d’insérer une nouvelle ligne et qu’il n’y a pas de place sur la page active, il a recours aux pages IAM et PFS pour rechercher une page à allouer ou, pour un segment ou une page de texte/image, une page suffisamment grande pour accueillir la ligne. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les pages IAM pour rechercher les étendues allouées à l’unité d’allocation. Pour chaque étendue, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche les pages PFS afin de vérifier si l’une d’elles peut être utilisée. Chaque page IAM et PFS couvre de nombreuses pages de données de sorte qu'il y a peu de pages IAM et PFS dans une base de données. C’est pourquoi elles se trouvent en général dans la mémoire du pool de mémoires tampons de SQL Server, d’où il est possible de les rechercher plus rapidement. Pour les index, le point d'insertion d'une nouvelle ligne est défini par la clé d'index. Dans ce cas, la recherche précédemment décrite ne se produit pas.

Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] n’alloue une nouvelle étendue à une unité d’allocation que s’il ne trouve pas rapidement une page suffisamment grande dans une étendue existante pour accueillir la ligne à insérer. 

<a name="ProportionalFill"></a> Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] alloue des étendues à partir de celles qui sont disponibles dans le groupe de fichiers à l’aide d’un **algorithme d’allocation de remplissage proportionnel**. Si, dans un même groupe de fichiers composé de deux fichiers, l’un d’entre eux dispose de deux fois plus d’espace disponible que l’autre, deux pages sont allouées à partir du fichier ayant le plus d’espace disponible pour chacune des pages allouées à partir de l’autre fichier. Cela signifie que chaque fichier d'un groupe doit avoir un pourcentage identique d'espace utilisé. 

## <a name="tracking-modified-extents"></a>Suivi des extensions modifiées 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise deux structures de données internes pour suivre les extensions modifiées par les opérations de copie en bloc et les extensions modifiées depuis la dernière sauvegarde complète. Ces structures de données accélèrent considérablement les sauvegardes différentielles. Elles accélèrent également l'enregistrement des opérations de copie en bloc dans le journal lorsqu'une base de données utilise le mode de récupération utilisant les journaux de transactions. À l'instar des pages GAM (Global Allocation Map) et SGAM (Shared Global Allocation Map), ces structures sont des bitmaps où chaque bit représente une extension unique. 

- **DCM (Differential Changed Map)**   
   Ces pages suivent les extensions qui ont été modifiées depuis la dernière instruction `BACKUP DATABASE`. Si le bit d’une extension est à 1, l’extension a été modifiée depuis la dernière instruction `BACKUP DATABASE`. Si le bit est à 0, l'extension n'a pas été modifiée. Les sauvegardes différentielles lisent uniquement les pages DCM pour déterminer les extensions qui ont été modifiées. Cela réduit considérablement le nombre de pages qu'une sauvegarde différentielle doit analyser. La durée d'exécution d'une sauvegarde différentielle est proportionnelle au nombre d'extensions modifiées depuis la dernière instruction BACKUP DATABASE, et non à la taille globale de la base de données. 

- **BCM (Bulk Changed Map)**   
   Ces pages suivent les extensions qui ont été modifiées par des opérations journalisées en bloc depuis la dernière instruction `BACKUP LOG`. Si le bit d’une extension est à 1, l’extension a été modifiée par une opération journalisée en bloc après la dernière instruction `BACKUP LOG`. Si le bit est à 0, l'extension n'a pas été modifiée par les opérations journalisées en bloc. Bien que les pages BCM existent dans toutes les bases de données, elles sont uniquement significatives lorsque la base de données emploie le mode de récupération utilisant les journaux de transactions. Dans ce mode de récupération, quand une procédure `BACKUP LOG` est effectuée, le processus de sauvegarde analyse les pages BCM pour identifier les extensions qui ont été modifiées. Il inclut ensuite ces extensions dans la sauvegarde du journal. Ceci permet de récupérer les opérations journalisées en bloc si la base de données est restaurée à partir d'une sauvegarde de la base de données et d'une séquence de sauvegardes de journaux de transactions. Les pages BCM ne sont pas significatives dans une base de données qui utilise le mode de récupération simple, car aucune opération journalisée en bloc n'est enregistrée dans le journal. Elles ne sont pas significatives dans une base de données utilisant le mode de récupération complet puisque ce dernier traite les opérations journalisées en bloc comme des opérations entièrement enregistrées dans le journal. 

L'intervalle entre les pages DCM et les pages BCM est le même que l'intervalle entre les pages GAM et SGAM : 64 000 extensions. Les pages DCM et BCM sont situées juste derrière les pages GAM et SGAM dans un fichier physique :

![special_page_order](../relational-databases/media/special-page-order.gif)
 
