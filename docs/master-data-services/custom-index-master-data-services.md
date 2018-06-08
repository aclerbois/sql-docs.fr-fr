---
title: Index personnalisé (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c57bf8b8-55a6-4b6c-9adb-91b5f4f1ee3c
caps.latest.revision: 9
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 4fceac85efd8c53a178d2861ca3b528ff6d8b730
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-index-master-data-services"></a>Index personnalisé (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Les index personnalisés créent un index non cluster sur un attribut spécifique (index unique) ou sur une liste d’attributs (index composite) dans une entité. En règle générale, les index améliorent les performances du processus d’exécution de requêtes. Pour plus d’informations sur les index SQL Server, consultez [Index](../relational-databases/indexes/indexes.md).  
  
## <a name="type-of-indexes"></a>Type d’index  
 Les types d’index personnalisés multiples que vous pouvez créer pour chaque entité sont les suivants :  
  
-   Index unique  
  
-   Index non unique  
  
 Un index unique garantit le fait que la colonne indexée ne contient aucune valeur en double. Dans le cas des index composites uniques, l’index garantit l’unicité de chaque combinaison de valeurs dans la liste d’attributs sélectionnés. La création d’un index unique est impossible s’il existe des valeurs en double pour les attributs sélectionnés.  
  
## <a name="rules"></a>Règles  
 Les règles applicables aux index personnalisés, qu’ils soient uniques ou non uniques, sont répertoriées ci-après.  
  
-   Pour créer un index personnalisé, veillez à sélectionner au moins un attribut.  
  
-   Si vous essayez d’enregistrer un index présentant la même liste d’attributs et le même indicateur d’unicité qu’un autre index, l’enregistrement échouera en affichant un message d’erreur.  
  
    > [!NOTE]  
    >  Master Data Services (MDS) crée automatiquement des index pour certains attributs (par exemple, DBA et Code). Cela signifie que vous ne pouvez pas créer un autre index contenant l’un de ces attributs sans aucun autre attribut.  
  
-   Vous pouvez inclure des attributs dans plusieurs index personnalisés à condition que chacun des autres index comprenne au moins un attribut différent. Dans le cas contraire, les index seront identiques.  
  
-   Si vous créez un index contenant de nombreux attributs ou des attributs de grande taille, et que la taille totale des attributs sélectionnés dépasse la taille de clé d’index maximale (900 octets), vous ne pourrez pas enregistrer l’index.  
  
-   Vous pouvez créer un index personnalisé sur les attributs de membre feuille, à l’exclusion des attributs de fichier.  
  
-   Si vous souhaitez supprimer un attribut inclus dans un index personnalisé, les règles suivantes s’appliquent :  
  
    -   Si l’index n’est créé que sur un seul attribut (index unique), l’attribut et l’index sont tous deux supprimés.  
  
    -   Si l’index est créé sur plusieurs attributs (index composite), vous ne pourrez pas supprimer l’attribut tant que vous n’aurez pas modifié l’index.  
  
-   Le type d’un attribut inclus dans un index personnalisé n’est pas modifiable.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer un index|[Créer un index &#40;Master Data Services&#41;](../master-data-services/create-an-index-master-data-services.md)|  
|Modifier et supprimer un index|[Modifier et supprimer un index &#40;Master Data Services&#41;](../master-data-services/edit-and-delete-an-index-master-data-services.md)|  
  
  
