---
title: Ajouter un filtre ou Modifier le filtre | Microsoft Docs
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
- sql13.rep.newpubwizard.addeditfilter.f1
ms.assetid: bdd7c71d-1c59-4044-bfe8-c85f908345bb
caps.latest.revision: 27
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 93f66d894dd451c8b6bae3893222b14387d7e803
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-or-edit-filter"></a>Ajouter un filtre ou Modifier le filtre
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les boîtes de dialogue **Ajouter un filtre** et **Modifier le filtre** permettent d'ajouter et de modifier des filtres de lignes statiques ou paramétrés.  
  
> [!NOTE]  
>  Pour pouvoir modifier un filtre dans une publication existante, vous devez procéder au préalable à un nouvel instantané de la publication. Si cette dernière dispose d'abonnements, ces abonnements doivent alors être réinitialisés. Pour plus d’informations sur les modifications de propriétés, consultez [Changer des propriétés de publication et d’article](../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
 Les types de publication incluent notamment les filtres statiques. Les publications de fusion comptent également parmi elles les filtres paramétrés. Un filtre statique s'évalue lors de la création de la publication : tous les Abonnés de la publication reçoivent les mêmes données. Un filtre paramétrable s'évalue, lui, lors d'une synchronisation de réplication : chaque Abonné peut effectivement recevoir différentes partitions de données selon ses informations de connexion ou d'après le nom de son ordinateur. Cliquez sur le lien intitulé **Exemples d'instructions** dans la boîte de dialogue afin de consulter les exemples pour chaque type de filtre. Pour plus d’informations sur les options de filtrage, consultez [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md).  
  
 Les filtres de lignes permettent de spécifier un sous-ensemble de lignes à publier à partir d'une table. Les filtres de lignes peuvent être utilisés pour éliminer des lignes que les utilisateurs n'ont pas besoin de voir (parce qu'elles contiennent par exemple des informations sensibles ou confidentielles) ou pour créer des partitions de données destinées à des abonnés différents. La publication de partitions de données différentes à des abonnés différents peut également contribuer à éviter les conflits qu'entraîne la mise à jour des mêmes valeurs de données par plusieurs abonnés.  
  
## <a name="options"></a>Options  
 Cette boîte de dialogue lance un processus en deux étapes pour les publications transactionnelles et d'instantané d'une part et un processus en trois étapes dans le cas des publications de fusion d'autre part. Quel que soit le type de publication choisi, vous devez choisir une table à filtrer ainsi qu'une ou plusieurs colonnes à ajouter au filtre ; le filtre est défini en tant que clause WHERE standard.  
  
1.  **Sélectionnez la table à filtrer**  
  
     Si vous modifiez un filtre existant, sachez que vous ne pouvez pas changer de table choisie. Si vous ajoutez un nouveau filtre, sélectionnez alors une table à partir de la zone de liste déroulante. Les tables n'y apparaissent que si elles ont été sélectionnées sur la page **Articles** et ne possèdent pas déjà un filtre de lignes. Dans ce dernier cas, vous pouvez toujours en définir un nouveau de la façon suivante :  
  
    1.  Dans la boîte de dialogue **Ajouter un filtre** , cliquez sur **Annuler** .  
  
    2.  Sélectionnez la table dans le volet du filtre se trouvant sur la page **Filtrer les lignes de la table** , puis cliquez sur **Modifier**.  
  
    3.  Modifiez un filtre existant dans la boîte de dialogue **Modifier le filtre** .  
  
2.  **Complétez l'instruction de filtrage pour identifier les lignes de table que les abonnés doivent recevoir**  
  
     Définissez une nouvelle instruction de filtrage ou modifiez-en une existante. La zone de liste déroulante **Colonnes** répertorie toutes les colonnes que vous publiez à partir de la table à choisir dans **Sélectionnez la table à filtrer**. La zone de texte **Instruction de filtrage** comprend un texte par défaut, qui est de la forme suivante :  
  
     `SELECT <published_columns> FROM [schema].[tablename] WHERE`  
  
     Ce texte ne peut pas être modifié. Saisissez dans ce cas la clause de filtrage après le mot clé WHERE en suivant la syntaxe standard [!INCLUDE[tsql](../../includes/tsql-md.md)] . Si le serveur de publication est un serveur Oracle, cette clause doit satisfaire les critères de syntaxe des requêtes Oracle. Évitez les filtres complexes autant que possible. En effet, aussi bien les filtres statiques que les filtres paramétrés rallongent le temps de traitement des publications ; essayez par conséquent de simplifier le plus possible vos instructions de filtrage.  
  
    > [!IMPORTANT]  
    >  Pour optimiser les performances, nous vous recommandons de ne pas appliquer de fonctions aux noms de colonnes dans des clauses de filtre de lignes paramétrés pour des publications de fusion, comme `LEFT([MyColumn]) = SUSER_SNAME()`. Si vous utilisez HOST_NAME dans une clause de filtrage puis en remplacez la valeur, il peut s'avérer judicieux de convertir les types de données avec la fonction CONVERT. Pour plus d'informations sur la conduite à adopter dans cette situation, consultez la section « Substitution de la valeur de HOST_NAME » de la rubrique [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
3.  **Spécifiez combien d'abonnements recevront des données de cette table**  
  
     [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures uniquement. Réplication de fusion uniquement. La réplication de fusion vous permet d'indiquer le type de partitions répondant au mieux à vos données et votre application. Si vous sélectionnez **Une ligne de cette table ira à un seul abonnement**, la réplication de fusion définit l'option de non-chevauchement des partitions. Cette option, qui fonctionne avec les partitions précalculées pour améliorer les performances, permet de réduire au minimum le coût de téléchargement associé aux partitions précalculées. Le gain de performances lié au non-chevauchement des partitions est plus flagrant lorsque les filtres paramétrés et les filtres de jointure utilisés sont plus complexes. Si vous sélectionnez cette option, vous devez veiller à ce que les données soient partitionnées de telle sorte qu'une ligne ne puisse pas être répliquée vers plusieurs abonnés. Pour plus d'informations, consultez la section « Définition de « partition options » » dans la rubrique [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 Après avoir ajouté ou modifié un filtre, cliquez sur **OK** pour enregistrer les modifications et fermer ainsi la boîte de dialogue. Le filtre que vous spécifiez est ensuite analysé et exécuté sur la table indiquée dans la clause SELECT. Si l'instruction de filtrage contient des erreurs de syntaxe ou rencontre tout autre problème, vous en êtes alors informé pour vous donner la possibilité de modifier l'instruction de filtrage.  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
