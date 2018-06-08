---
title: Extraction des lignes | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-rowsets
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- fetching rows
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- IRowset interface
- SQL Server Native Client OLE DB provider, fetching
ms.assetid: 5e6dbe36-b682-464d-adfa-8e886f9bd452
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 24d8b01218b506e5041ad453a07a501ec615e894
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fetching-rows"></a>Extraction de lignes
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le **IRowset** est l’interface de l’ensemble de lignes de base. Le **IRowset** interface fournit des méthodes pour extraire les lignes séquentiellement, obtenir les données de ces lignes et la gestion des lignes. Les consommateurs utilisent les méthodes de **IRowset** pour toutes les opérations de l’ensemble de lignes de base. Ces opérations incluent l'extraction et la libération des lignes, ainsi que l'obtention des valeurs des colonnes.  
  
 Lorsqu’un consommateur Obtient un pointeur d’interface sur un ensemble de lignes, la première étape consiste généralement à déterminer les fonctionnalités de l’ensemble de lignes à l’aide de la **IRowsetInfo::GetProperties** (méthode). Cette opération retourne les informations sur les interfaces exposées par l'ensemble de lignes, ainsi que les fonctions de l'ensemble de lignes n'apparaissant pas comme interfaces distinctes, telles que le nombre maximal de lignes actives et le nombre de lignes qui peuvent avoir simultanément des mises à jour en attente.  
  
 L'étape suivante pour les consommateurs consiste à déterminer les caractéristiques, ou métadonnées, des colonnes de l'ensemble de lignes. Pour cela, ils utilisent le **IColumnsInfo** méthode pour les informations de colonne simple ou **IColumnsRowset** méthode pour les informations de colonne étendu. Le **GetColumnInfo** méthode retourne les informations suivantes :  
  
-   Nombre de colonnes de l'ensemble de résultats.  
  
-   Tableau de structures DBCOLUMNINFO, une par colonne.  
  
     L'ordre des structures désigne l'ordre dans lequel les colonnes apparaissent dans l'ensemble de lignes. Chaque structure DBCOLUMNINFO inclut les métadonnées des colonnes, telles que le nom de colonne, le numéro ordinal de la colonne, la longueur maximale d'une valeur de la colonne, le type de données de la colonne, la précision et la longueur.  
  
-   Pointeur vers un stockage de toutes les valeurs de chaîne au sein d'un bloc d'allocation unique.  
  
 Le consommateur détermine s'il a besoin des colonnes des métadonnées ou de celles fondées sur la commande de texte ayant généré l'ensemble de lignes. Il détermine les ordinaux des colonnes nécessaires à partir du classement des informations de colonne retournées par **IColumnsInfo** ou à partir des ordinaux dans l’ensemble de lignes de métadonnées de colonne retourné par **IColumnsRowset**.  
  
 Le **IColumnsInfo** et **IColumnsRowset** interfaces sont utilisées pour extraire des informations sur les colonnes de l’ensemble de lignes. Le **IColumnsInfo** interface renvoie un ensemble limité d’informations, tandis que **IColumnsRowset** fournit toutes les métadonnées.  
  
> [!NOTE]  
>  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0 et antérieure, la colonne de métadonnées facultative DBCOLUMN_COMPUTEMODE retournée par **IColumnsInfo::GetColumnsInfo** retourne DBSTATUS_S_ISNULL (au lieu des valeurs décrivant si la colonne est calculée), car il ne peut pas être déterminé si la colonne sous-jacente est calculée.  
  
 Les ordinaux sont utilisés pour spécifier une liaison à une colonne. Une liaison est une structure qui associe un élément de la structure du consommateur et une colonne. La liaison peut lier la valeur des données, la longueur et la valeur d'état de la colonne.  
  
 Un jeu de liaisons est regroupé dans un accesseur. Il est créé à l’aide de la **IAccessor::CreateAccessor** (méthode). Un accesseur peut contenir plusieurs liaisons afin que les données de plusieurs colonnes puissent être extraites ou définies en un seul appel. Le consommateur peut créer plusieurs accesseurs pour correspondre aux divers modèles d'utilisation des différentes parties de l'application. Il peut créer et libérer des accesseurs tandis que l'ensemble de lignes continue à exister.  
  
 Pour extraire les lignes à partir de la base de données, le consommateur appelle une méthode, telles que **IRowset::GetNextRows** ou **IRowsetLocate::GetRowsAt**. Ces opérations d'extraction placent les données de ligne du serveur dans le tampon de ligne du fournisseur. Le consommateur ne peut pas accéder directement au tampon de ligne du fournisseur. Le consommateur utilise **IRowset::GetData** pour copier des données à partir de la mémoire tampon du fournisseur dans la mémoire tampon du consommateur et **IRowsetChange::SetData** pour copier les modifications de données à partir de la mémoire tampon du consommateur dans le tampon de fournisseur.  
  
 Le consommateur appelle la **GetData** méthode et passe le handle à une ligne, le handle à un accesseur et un pointeur vers une mémoire tampon allouée par le consommateur. **GetData** convertit les données et retourne les colonnes comme spécifié dans les liaisons utilisées pour créer l’accesseur. Le consommateur peut appeler **GetData** plusieurs fois pour une ligne, à l’aide de différents accesseurs et mémoires tampons et par conséquent, le consommateur peut obtenir plusieurs copies des mêmes données.  
  
 Les données de colonnes de longueur variable peuvent être traitées de plusieurs façons. En premier lieu, ces colonnes peuvent être liées à une section limitée de la structure du consommateur. Il s'ensuit une troncation quand la longueur des données dépasse la longueur de la mémoire tampon. Le consommateur peut déterminer que la troncation a eu lieu en vérifiant l'état DBSTATUS_S_TRUNCATED. La longueur retournée est toujours la véritable longueur en octets, afin que le consommateur puisse également déterminer la quantité de données tronquées.  
  
 Lorsque le consommateur a fini d’extraire ou de mise à jour des lignes, il les libère avec la **ReleaseRows** (méthode). Cela libère les ressources de la copie des lignes de l'ensemble de lignes et crée de la place pour les nouvelles lignes. Le consommateur peut ensuite répéter son cycle d'extraction ou de création de lignes et d'accès à leurs données.  
  
 Lorsque le consommateur est terminé avec l’ensemble de lignes, elle appelle la **IAccessor::ReleaseAccessor** méthode pour libérer un accesseur. Il appelle le **IUnknown::Release** méthode sur toutes les interfaces exposées par l’ensemble de lignes pour libérer l’ensemble de lignes. Lorsque l'ensemble de lignes est libéré, il force la libération des lignes ou accesseurs restants que le consommateur peut détenir.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Prochaine Position d’extraction](../../relational-databases/native-client-ole-db-rowsets/fetching-rows-next-fetch-position.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
