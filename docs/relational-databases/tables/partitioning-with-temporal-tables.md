---
title: Partitionnement des tables temporelles | Microsoft Docs
ms.custom: ''
ms.date: 04/26/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 313714b8-4ad1-4c14-93a3-7f628a334a51
caps.latest.revision: 11
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4924a8136570721b775210e1345cc932384cefd4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="partitioning-with-temporal-tables"></a>Partitionnement des tables temporelles
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Vous pouvez utiliser le partitionnement, de manière indépendante, sur la table actuelle et la table de l’historique. Toutefois, le partitionnement ne peut pas être utilisé pour modifier le contenu des données sans contrôle de version du système.  
  
> [!NOTE]  
>  Le partitionnement est une fonctionnalité de l’édition Enterprise dans SQL Server 2016 avant le Service Pack 1 et les versions antérieures. Le partitionnement est pris en charge dans toutes les éditions de SQL Server 2016 Service Pack 1 et des versions ultérieures.
  
-   **Table actuelle :**  
  
    -   **SWITCH IN** sur la table actuelle peut être utilisé pour faciliter le chargement et l’interrogation des données quand **SYSTEM_VERSIONING** a la valeur **ON**  
  
    -   **SWITCH OUT** n’est pas autorisé quand **SYSTEM_VERSIONING** a la valeur **ON**  
  
-   **Table de l’historique :**  
  
    -   **SWITCH OUT** à partir de la table d’historique peut être utilisé quand **SYSTEM_VERSIONING** a la valeur **ON** pour vider les parties de données d’historique qui ne sont plus pertinentes.  
  
    -   **SWITCH IN** n’est pas autorisé quand **SYSTEM_VERSIONING** a la valeur **ON** , car il peut invalider la cohérence des données temporelles.  
  
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)   
 [Considérations et limitations liées aux tables temporelles](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)   
 [Sécurité de la table temporelle](../../relational-databases/tables/temporal-table-security.md)   
 [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)  
  
  
