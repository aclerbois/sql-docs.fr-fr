---
title: Segments (tables sans index cluster) | Microsoft Docs
ms.custom: ''
ms.date: 11/01/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- heaps
ms.assetid: df5c4dfb-d372-4d0f-859a-a2d2533ee0d7
caps.latest.revision: 8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: fef08462d85ef86f76eb7647cadba6063126d7d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="heaps-tables-without-clustered-indexes"></a>Segments (tables sans index cluster)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Un segment est une table sans index cluster. Un ou plusieurs index non cluster peuvent être créés sur des tables stockées comme segment. Les données sont stockées dans le segment sans spécifier d'ordre. Généralement, les données sont stockées initialement dans l'ordre dans lequel les lignes sont insérées dans la table, mais le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut déplacer des données dans le segment pour stocker les lignes efficacement ; l'ordre des données ne peut donc pas être prédit. Pour garantir l'ordre des lignes retournées à partir d'un segment, vous devez utiliser la clause **ORDER BY** . Pour spécifier l'ordre de stockage des lignes, créez un index cluster sur la table, de sorte que la table ne soit pas un segment.  
  
> [!NOTE]  
>  Il existe parfois de bonnes raisons de conserver une table comme segment plutôt que de créer un index cluster, mais l'utilisation de segments est effectivement une compétence avancée. La plupart des tables doivent avoir un index cluster soigneusement choisi, à moins qu'il n'existe une bonne raison de conserver la table comme segment.  
  
## <a name="when-to-use-a-heap"></a>À quel moment utiliser un segment  
 Si une table est un segment et ne possède pas d'index non cluster, la table entière doit être examinée (analyse de table) pour rechercher une ligne quelconque. Cela peut être acceptable lorsque la table est minuscule, par exemple s'il s'agit d'une liste des 12 bureaux régionaux d'une société.  
  
 Lorsqu'une table est stockée en tant que segment, les différentes lignes sont identifiées par référence à un identificateur de ligne (RID) composé d'un numéro de fichier, d'un numéro de page de données et de l'emplacement sur la page. L'ID de ligne est une structure petite et efficace. Parfois, les architectes de données utilisent des segments lorsque l'accès aux données s'effectue toujours par le biais d'index non cluster et que le RID est plus petit que la clé d'index cluster.  
  
## <a name="when-not-to-use-a-heap"></a>À quel moment ne pas utiliser un segment  
 N'utilisez pas de segment lorsque les données sont souvent retournées dans un ordre trié. Un index cluster sur la colonne de tri peut éviter l'opération de tri.  
  
 N'utilisez pas de segment lorsque les données sont souvent regroupées. Les données doivent être triées avant d'être regroupées et un index cluster sur la colonne de tri peut éviter l'opération de tri.  
  
 N'utilisez pas de segment lorsque des plages de données sont souvent interrogées pour la table.  Un index cluster sur la colonne de plages évitera de devoir trier le segment entier.  
  
 N'utilisez pas de segment lorsqu'il n'y a pas d'index non cluster et que la table est grande. Dans un segment, toutes les lignes du segment doivent être lues pour trouver n'importe une ligne quelconque.  
  
## <a name="managing-heaps"></a>Gestion des segments  
 Pour créer un segment, créez une table sans index cluster. Si la table possède déjà un index cluster, supprimez-le afin de reconvertir la table en segment.  
  
 Pour supprimer un segment, créez un index cluster sur le segment.  
  
 Pour reconstruire un segment afin de récupérer de l'espace perdu, créez un index cluster sur le segment, puis supprimez l'index cluster.  
  
> [!WARNING]  
>  La création ou suppression d'index cluster nécessite de réécrire la table entière. Si la table a des index non cluster, tous les index non cluster doivent être recréés à chaque fois que l'index cluster est modifié. Par conséquent, le passage d’un segment à une structure d’index cluster (ou inversement) peut prendre beaucoup de temps et nécessiter de l’espace disque pour réorganiser les données dans tempdb.  

## <a name="heap-structures"></a>Structures des segments


Un segment est une table sans index cluster. Les segments ont une ligne dans [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md), avec `index_id = 0` pour chaque partition utilisée par le segment. Par défaut, un segment comporte une partition unique. Lorsqu'un segment comporte plusieurs partitions, chacune d'elles possède une structure de segment contenant les données la concernant. Par exemple, si un segment comporte quatre partitions, il y a quatre structures de segment, une dans chaque partition.

Selon les types de données du segment, chaque structure contient une ou plusieurs unités d'allocation pour stocker et gérer les données d'une partition spécifique. Chaque segment de mémoire a au minimum une unité d’allocation `IN_ROW_DATA` par partition. Le segment de mémoire a aussi une unité d’allocation `LOB_DATA` par partition s’il contient des colonnes LOB (Large Object). En outre, il a une unité d’allocation `ROW_OVERFLOW_DATA` par partition s’il contient des colonnes de longueur variable qui dépassent la limite de taille de ligne de 8 060 octets.

La colonne `first_iam_page` de la vue système `sys.system_internals_allocation_units` pointe vers la première page IAM de la chaîne de pages IAM gérant l’espace alloué au segment de mémoire dans une partition spécifique. SQL Server utilise les pages IAM pour se déplacer à travers le segment de mémoire. Les pages de données et les lignes qu'elles contiennent ne sont ni classées dans un ordre spécifique, ni liées. La seule connexion logique entre les pages de données concerne les informations enregistrées dans les pages IAM.

> [!IMPORTANT]  
> La vue système `sys.system_internals_allocation_units` est réservée exclusivement à un usage interne de Microsoft SQL Server. La compatibilité future n'est pas garantie.
 
Les analyses de table ou les lectures séquentielles d'un segment explorent les pages IAM pour rechercher les étendues contenant des pages pour le segment. Comme la page IAM représente les étendues dans le même ordre d'apparition que dans les fichiers de données, les analyses en série du segment s'effectuent uniformément de haut en bas de chaque fichier. L'utilisation des pages IAM pour la définition de la séquence d'analyse signifie également que les lignes du segment ne sont généralement pas retournées dans l'ordre de leur insertion.

L’illustration suivante montre comment le moteur de base de données SQL Server utilise les pages IAM pour extraire des lignes de données dans un segment de mémoire de partition unique. 

![iam_heap](../../relational-databases/indexes/media/iam-heap.gif)

  
## <a name="related-content"></a>Contenu associé  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
 [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
  
 [Description des index cluster et non-cluster](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)  
  
  
