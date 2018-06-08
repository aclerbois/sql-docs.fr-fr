---
title: Propriétés de la publication, Articles | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.articles.f1
ms.assetid: bdeea318-a153-44b8-9e51-9155f3bad18b
caps.latest.revision: 29
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7babd81861f5b9f256a1d92ed2ad1f029e5cd13f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publication-properties-articles"></a>Propriétés de la publication, Articles
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Articles** de la boîte de dialogue **Propriétés de la publication** reprend les informations sur les articles contenus dans une publication, vous permet d'ajouter ou de supprimer des articles de publications existantes, ainsi que de modifier les propriétés des articles et le filtrage s'appuyant sur des colonnes.  
  
> [!NOTE]  
>  Après qu'une publication a été créée, certaines modifications de propriétés nécessitent un nouvel instantané. Si une publication dispose d'abonnements, ces abonnements pourraient devoir alors être réinitialisés selon les modifications que vous avez apportées. Pour plus d’informations, consultez [Changer les propriétés de publication et d’article](../../relational-databases/replication/publish/change-publication-and-article-properties.md) et [Ajouter et supprimer des articles de publications existantes](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
 Si vous publiez un objet de base de données qui dépend d'un ou de plusieurs autres objets de base de données, vous devez publier tous les objets référencés. Par exemple, si vous publiez une vue qui dépend d'une table, vous devez publier la table également.  
  
 Une icône rouge apparaît en regard des objets ne pouvant pas être publiés, avec une explication dans le panneau d'information se trouvant en bas de la page de l'Assistant. Les objets suivants ne peuvent pas être publiés :  
  
-   Les objets chiffrés.  
  
-   Les vues indexées contenant des colonnes autorisant la valeur NULL.  
  
-   Les tables ne possédant pas de clé primaire ne peuvent pas être publiées dans des publications transactionnelles.  
  
-   Les tables ne peuvent en effet pas être publiées à la fois dans une publication de fusion et une publication transactionnelle avec l'option de mise à jour d'abonnements en attente activée. Pour plus d’informations sur la publication d’un article dans plusieurs publications, consultez la section « Publication de tables dans plusieurs publications » dans [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="oracle-publishers"></a>Serveurs de publication Oracle  
 Des points supplémentaires sont également à prendre en compte dans le cas d'utilisation de serveur de publication Oracle :  
  
-   Pour obtenir la liste des objets pouvant être publiés d'un serveur Oracle, consultez [Design Considerations and Limitations for Oracle Publishers](../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md). Les objets qui ne peuvent pas être publiés ne sont pas affichés.  
  
-   Pour obtenir la liste des types de données pouvant être publiés, consultez [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md). Les colonnes dont le type de données ne peut pas être publié ne sont pas affichées.  
  
## <a name="column-filters"></a>Filtres de colonnes  
 Vous pouvez filtrer les colonnes de cette page en développant une table dans le volet **Objets à publier** , puis en ne sélectionnant que les colonnes nécessaires (les lignes peuvent en effet être filtrées à partir de la page **Filtrer les lignes de la table** de ce même Assistant). Le filtrage de colonnes s'avère utile pour de nombreuses raisons, notamment en ce qui a trait à la sécurité (contribuant à protéger des données sensibles contre toute réplication) et aux performances (empêchant par exemple la réplication d'objets binaires volumineux, appelés également Blob pour Bynary Large OBjects). Pour plus d’informations sur le filtrage de colonnes, notamment pour connaître la liste des types de colonne éligibles au filtrage, consultez [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md).  
  
## <a name="options"></a>Options  
 Le volet **Objets à publier** vous permet :  
  
-   d'afficher tous les objets disponibles à la réplication ;  
  
-   d'ajouter un article à une publication en activant la case à cocher en regard de l'objet ;  
  
-   de supprimer un article d'une publication en désactivant la case à cocher en regard de l'objet. Pour savoir quand les articles peuvent être supprimés, consultez [Ajouter et supprimer des articles de publications existantes](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
-   d'inclure tous les objets d'un type donné (par exemple, une table) se trouvant dans la publication, en activant la case à cocher en regard du type de l'objet (dans notre cas, **Tables**).  
  
-   de développer les nœuds de la table pour en afficher les colonnes ;  
  
-   de filtrer les colonnes d'une table d'une publication en désactivant la case à cocher en regard de la colonne, ou encore d'inclure les colonnes non publiées en activant au contraire leurs cases à cocher ;  
  
-   enfin, de cliquer sur un objet dans le volet avec le bouton droit de la souris afin d'afficher un menu répertoriant les commandes applicables à cet objet.  
  
 **Propriétés de l'article**  
 Cliquez sur **Propriétés de l'article** , puis effectuez l'une des actions ci-dessous :  
  
-   Cliquez sur **Définir les propriétés de l’article de \<TypeObjet> en surbrillance** pour ouvrir la boîte de dialogue **Propriétés de l’article - \<NomObjet>**. Les modifications de propriétés apportées dans cette boîte de dialogue sont appliquées uniquement à l’objet en surbrillance dans le volet Objet de la page **Articles**.  
  
-   Cliquez sur **Définir les propriétés de tous les articles \<type_objet>** pour ouvrir la boîte de dialogue **Propriétés pour tous les articles \<type_objet>**. Les modifications de propriétés apportées dans cette boîte de dialogue sont appliquées à tous les objets de ce type dans le volet Objet de la page **Articles**, notamment ceux qui ne sont pas encore sélectionnés pour la publication.  
  
    > [!NOTE]  
    >  Les modifications de propriétés apportées dans la boîte de dialogue **Propriétés pour tous les articles \<type_objet>** remplacent celles apportées dans la boîte de dialogue **Propriétés de l’article - \<nom_objet>**. Ainsi, si vous voulez définir un certain nombre de valeurs par défaut applicables à tous les articles d'un type d'objet mais souhaitez également définir des propriétés pour des objets spécifiques, vous devez d'abord définir les valeurs par défaut pour tous les articles. Définissez ensuite les propriétés des objets individuels.  
  
 **La table sélectionnée est en téléchargement seul**  
 Réplication de fusion uniquement. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] and later versions only. Permet d'indiquer que les modifications sont interdites au niveau de l'Abonné si l'abonnement d'un client est utilisé. Étant donné que les articles disponibles uniquement par téléchargement ne peuvent pas être mis à jour sur l'abonné, le suivi des métadonnées n'est pas envoyé aux abonnés. Cela peut aboutir à une réduction du stockage sur les abonnés et à un gain de performances, notamment si la connexion réseau est lente. Cette option équivaut à la valeur **Téléchargement seul pour l'Abonné, interdire les modifications de l'Abonné** pour l'option **Direction de la synchronisation** dans la boîte de dialogue **Propriétés de l'article** . Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
 **Afficher uniquement les objets activés dans la liste**  
 Activez cette case à cocher pour n'afficher que les articles sélectionnés dans le volet des objets.  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Créer et appliquer l’instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [Réinitialiser un abonnement](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
