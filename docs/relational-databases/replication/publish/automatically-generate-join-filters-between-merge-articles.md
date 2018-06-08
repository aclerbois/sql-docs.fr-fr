---
title: Générer automatiquement des filtres de jointure entre des articles de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- automatic join filter generation
- join filters
ms.assetid: 7ef419f4-c17f-42a5-9068-174a3ec08941
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 70fc107aab8433822e5d674b39b8b4986ec32f4e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="automatically-generate-join-filters-between-merge-articles"></a>Générer automatiquement des filtres de jointure entre des articles de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Générez automatiquement un ensemble de filtres de jointure sur la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>**. Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
> [!NOTE]  
>  Si vous générez automatiquement un ensemble de filtres de jointure dans la boîte de dialogue **Propriétés de la publication - \<Publication>** une fois les abonnements à la publication initialisés, vous devez générer un nouvel instantané et réinitialiser tous les abonnements après avoir effectué la modification. Pour plus d’informations sur les exigences relatives aux changements de propriétés, consultez [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
 Les filtres de jointure peuvent être créés manuellement pour un ensemble de tables, ou la réplication peut les générer automatiquement en fonction des relations de clé étrangère à clé primaire définies sur les tables. Pour plus d’informations sur la création manuelle de filtres de jointure, consultez [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
### <a name="to-automatically-generate-a-set-of-join-filters-between-merge-articles"></a>Pour générer automatiquement un ensemble de filtres de jointure entre articles de fusion  
  
1.  Sur la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication, ou la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la Publication - \<Publication>**, cliquez sur **Ajouter**, puis sur **Générer automatiquement des filtres**.  
  
    > [!NOTE]  
    >  La génération automatique de filtres supprime tout filtre de lignes ou de jointure existant dans la publication. Vous pouvez ajouter des filtres après avoir généré automatiquement un ensemble de filtres.  
  
2.  Suivez le processus de la boîte de dialogue **Générer des filtres** pour créer un filtre de lignes. Le filtre de lignes est ensuite étendu aux tables associées à la table filtrée via les relations de clé primaire et de clé étrangère.  
  
    1.  Sélectionnez une table à filtrer dans la zone de liste déroulante.  
  
    2.  Créez une instruction de filtrage dans la zone de texte **Instruction de filtrage** . Vous pouvez taper directement dans la zone de texte, mais vous pouvez aussi faire glisser et déposer des colonnes depuis la zone de liste **Colonnes** .  
  
         La zone de texte **Instruction de filtrage** comprend un texte par défaut, qui est de la forme suivante :  
  
        ```  
        SELECT <published_columns> FROM [tableowner].[tablename] WHERE  
        ```  
  
         Il est impossible de modifier le texte par défaut ; tapez la clause de filtre pour un filtre de lignes statiques ou un filtrage des lignes paramétrable après le mot clé WHERE à l'aide de la syntaxe SQL standard. La clause de filtre complète pour un filtre de lignes paramétrable peut se présenter comme suit :  
  
        ```  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE LoginID = SUSER_SNAME()  
        ```  
  
         La clause WHERE doit utiliser un nommage en deux parties ; les nommages en trois et en quatre parties ne sont pas pris en charge.  
  
    3.  Spécifiez les options de filtre.  
  
         Sélectionnez l'option correspondant à la façon dont les données seront partagées parmi les abonnés : **Une ligne de cette table ira à plusieurs abonnements** ou **Une ligne de cette table ira à un seul abonnement**. Si vous sélectionnez **Filtre paramétré créant des partitions qui ne se chevauchent pas, avec un seul abonnement par partition**, la réplication de fusion peut optimiser les performances en stockant et en traitant moins de métadonnées. Cependant, vous devez vérifier que les données sont partitionnées de telle façon qu'une ligne ne peut pas être répliquée sur plus d'un Abonné. Pour plus d'informations, consultez la section « Définition de « partition options » » dans la rubrique [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
     Le filtre que vous spécifiez est ensuite analysé et exécuté sur la table indiquée dans la clause SELECT. Si l'instruction de filtrage contient des erreurs de syntaxe ou d'autres erreurs, vous recevrez un message et pourrez modifier l'instruction de filtrage.  
  
     Une fois l'instruction analysée, la réplication crée les filtres de jointure nécessaires et les affiche dans le volet **Tables filtrées** sur la page **Filtrer les lignes de la table** ou **Filtrer les lignes** . Si vous générez des filtres à partir de l'Assistant Nouvelle publication et n'avez pas encore configuré le serveur de distribution pour le serveur de publication sur lequel cet Assistant est exécuté, il vous est demandé de le configurer.  
  
4.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>**, cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
### <a name="to-modify-a-filter-that-was-automatically-generated"></a>Pour modifier un filtre généré automatiquement  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>**, sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Modifier**.  
  
2.  Modifiez le filtre dans la boîte de dialogue **Modifier le filtre** ou **Modifier une jointure** .  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-delete-a-filter-that-was-automatically-generated"></a>Pour supprimer un filtre généré automatiquement  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>**, sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Supprimer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
