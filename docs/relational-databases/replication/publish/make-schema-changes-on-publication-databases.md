---
title: Modifier le schéma dans les bases de données de publication | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
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
- replication [SQL Server], schema changes
- snapshot replication [SQL Server], replicating schema changes
- merge replication [SQL Server replication], replicating schema changes
- transactional replication, replicating schema changes
- schemas [SQL Server replication], replicating changes
- publishing [SQL Server replication], schema changes
ms.assetid: 926c88d7-a844-402f-bcb9-db49e5013b69
caps.latest.revision: 73
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 905725cecea8e2dee08641b49500d23f664f934b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="make-schema-changes-on-publication-databases"></a>Modifier le schéma dans les bases de données de publication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication prend en charge une grande variété de modifications de schéma pour les objets publiés. Lorsque vous effectuez l'une des modifications de schémas qui suit sur l'objet publié approprié sur un serveur de publication [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cette modification est propagée par défaut sur tous les Abonnés [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   ALTER TABLE  
  
-   ALTER TABLE SET LOCK ESCALATION ne doit pas être utilisé si la réplication de modification de schéma est activée et qu’une topologie inclut des abonnés [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou [!INCLUDE[ssEWnoversion](../../../includes/ssewnoversion-md.md)].

-   ALTER VIEW  
  
-   ALTER PROCEDURE  
  
-   ALTER FUNCTION  
  
-   ALTER TRIGGER  
  
     ALTER TRIGGER ne peut être utilisé qu'avec les déclencheurs DML car les déclencheurs DDL ne peuvent pas être répliqués.  
  
> [!IMPORTANT]  
>  Les modifications de schéma apportées aux tables doivent être effectuées à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou des objets SMO ( [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Objects). Lorsque des modifications de schéma sont effectuées dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] tente de supprimer puis de recréer la table. La suppression des objets publiés étant impossible, la modification de schéma échoue.  
  
 Dans le cas de la réplication transactionnelle ou de fusion, les modifications de schéma sont propagées de manière incrémentielle lors de l'exécution de l'Agent de distribution ou de fusion. Avec la réplication d'instantané, les modifications de schéma sont propagées lors de l'application d'un nouvel instantané sur l'Abonné. Dans la réplication d'instantané, une nouvelle copie du schéma est envoyée à l'Abonné à chaque synchronisation. Par conséquent, toutes les modifications de schéma (pas uniquement celles répertoriées ci-dessus) apportées aux objets précédemment publiés sont automatiquement propagées avec chaque synchronisation.  
  
 Pour obtenir des informations sur l’ajout et la suppression d’articles dans les publications, consultez [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
 **Pour répliquer les modifications de schéma**  
  
 Les modifications de schéma répertoriées ci-dessus sont répliquées par défaut. Pour plus d'informations sur la désactivation de la réplication des modifications de schéma, consultez [Replicate Schema Changes](../../../relational-databases/replication/publish/replicate-schema-changes.md).  
  
## <a name="considerations-for-schema-changes"></a>Considérations sur les modifications de schéma  
 Les éléments suivants doivent être pris en compte lors de la réplication des modifications de schéma.  
  
### <a name="general-considerations"></a>Considérations générales  
  
-   Les modifications de schéma sont soumises aux restrictions imposées par [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Par exemple, ALTER TABLE ne vous permet pas de modifier les colonnes clés primaire.  
  
-   Le mappage de type de données est effectué uniquement pour l'instantané initial. Les modifications de schéma ne sont pas mappées aux versions précédentes des types de données. Par exemple, si l'instruction `ALTER TABLE ADD datetime2 column` est utilisée dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], le type de données n'est pas traduit par **nvarchar** pour les Abonnés [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] . Dans certains cas, les modifications de schéma sont bloquées sur le serveur de publication.  
  
-   Si la configuration d'une publication autorise la propagation des modifications de schéma, celles-ci sont propagées quelle que soit la configuration de l'option de schéma associée pour un article de la publication. Si, par exemple, vous décidez de ne pas répliquer les contraintes de clé étrangère pour un article de table mais qu'ensuite vous émettez une commande ALTER TABLE qui ajoute une clé étrangère sur le serveur de publication, la clé étrangère est ajoutée à la table sur l'Abonné. Pour éviter cela, désactivez la propagation des modifications de schéma avant d'émettre la commande ALTER TABLE.  
  
-   Les modifications de schéma doivent être uniquement effectuées sur le serveur de publication et non sur les Abonnés (y compris les Abonnés de republication). La réplication de fusion interdit les modifications de schéma sur l'Abonné, à la différence de la réplication transactionnelle mais dans ce dernier cas, il se peut que les modifications apportées entraînent l'échec de la réplication.  
  
-   Les modifications propagées à un Abonné de republication sont transmises par défaut à ses Abonnés.  
  
-   Si la modification de schéma fait référence à des objets ou des contraintes qui existent sur le serveur de publication mais pas sur l'Abonné, elle est effectuée sur le serveur de publication mais échoue sur l'Abonné.  
  
-   Tous les objets de l'Abonné référencés lors de l'ajout d'une clé étrangère doivent avoir un nom et un propriétaire identiques à l'objet correspondant sur le serveur de publication.  
  
-   L'ajout, la suppression ou la modification explicites d'index ne sont pas pris en charge. En revanche, les index créés explicitement pour les contraintes, par exemple une contrainte de clé primaire, le sont.  
  
-   La modification ou la suppression de colonnes d'identité gérées par la réplication ne sont pas prises en charge. Pour plus d’informations sur la gestion automatique des colonnes d’identité, consultez [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
-   Les modifications de schéma qui comprennent des fonctions non déterministes ne sont pas prises en charge car elles peuvent se traduire par la présence de données différentes sur le serveur de publication et l'Abonné (c'est-à-dire non convergentes). Par exemple, si vous émettez la commande suivante sur le serveur de publication : `ALTER TABLE SalesOrderDetail ADD OrderDate DATETIME DEFAULT GETDATE()`, les valeurs diffèrent lorsque la commande est répliquée sur l'Abonné, puis exécutée. Pour plus d'informations sur les fonctions non déterministes, consultez [Deterministic and Nondeterministic Functions](../../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
-   Il est conseillé de nommer explicitement les contraintes. Si les contraintes ne sont pas nommées explicitement, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] génère des noms pour celles-ci, qui seront différents sur le serveur de publication et sur chaque Abonné. Cela peut occasionner des problèmes pendant la réplication des modifications de schéma. Par exemple, si vous supprimez une colonne sur le serveur de publication et qu'une contrainte dépendante est supprimée, la réplication essaie de supprimer la contrainte sur l'Abonné. La suppression sur l'Abonné échouera car le nom de la contrainte est différent. Si la synchronisation échoue en raison d'un problème de dénomination de contrainte, supprimez manuellement la contrainte sur l'Abonné, puis réexécutez l'Agent de fusion.  
  
-   Si une table est publiée pour la réplication, il n'est pas possible de modifier une colonne de cette table en lui attribuant un type de données XML si un instantané de publication a déjà été généré. Pour modifier la colonne, vous devez d'abord supprimer la réplication.  
  
-   La lecture non validée n'est pas un niveau d'isolement pris en charge lors de l'exécution d'instructions DDL sur une table publiée.  
  
-   **SET CONTEXT_INFO** ne doit pas être utilisée pour modifier le contexte des transactions dans lesquelles des modifications de schéma sont exécutées sur les objets publiés.  
  
#### <a name="adding-columns"></a>Ajout de colonnes  
  
-   Pour ajouter une nouvelle colonne à une table et inclure la colonne dans une publication existante, exécutez ALTER TABLE \<Table> ADD \<Colonne>. Par défaut, la colonne est alors répliquée sur tous les Abonnés. La colonne doit accepter des valeurs NULL ou inclure une contrainte par défaut. Pour plus d'informations sur l'ajout de colonnes, consultez la section « Réplication de fusion » de cette rubrique.  
  
-   Pour ajouter une nouvelle colonne à une table sans inclure cette colonne dans une publication existante, désactivez la réplication des modifications de schéma, puis exécutez ALTER TABLE \<Table> ADD \<Colonne>.  
  
-   Pour inclure une colonne existante dans une publication existante, utilisez [sp_articlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), [sp_mergearticlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) ou la boîte de dialogue **Propriétés de la publication - \<Publication>**.  
  
     Pour plus d'informations, voir [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md). Cette opération exige la réinitialisation des abonnements.  
  
-   L'ajout d'une colonne d'identité à une table publiée n'est pas pris en charge, car la nouvelle colonne peut entraîner une non-convergence quand la colonne est répliquée vers l'Abonné. Les valeurs de la colonne d'identité sur l'Abonné dépendent de l'ordre dans lequel les lignes sont physiquement stockées dans la table affectée. Les lignes sont susceptibles d'être stockées différemment sur l'Abonné ; la valeur de la colonne d'identité peut donc être différente pour les mêmes lignes.  
  
#### <a name="dropping-columns"></a>Suppression de colonnes  
  
-   Pour supprimer une colonne d’une publication existante et de la table sur le serveur de publication, exécutez ALTER TABLE \<Table> DROP \<Colonne>. Par défaut, la colonne est alors supprimée de la table sur tous les Abonnés.  
  
-   Pour supprimer une colonne d’une publication existante, tout en la conservant dans la table sur le serveur de publication, utilisez [sp_articlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), [sp_mergearticlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) ou la boîte de dialogue **Propriétés de la publication - \<Publication>**.  
  
     Pour plus d’informations, voir [Définir et modifier un filtre de colonne](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md). Cette opération exige la génération d'un nouvel instantané.  
  
-   La colonne à supprimer ne peut pas être utilisée dans les clauses de filtrage d'un article quelconque d'une publication de la base de données.  
  
-   La suppression d'une colonne d'un article publié nécessite la prise en compte des éventuels contraintes, index ou propriétés de la colonne susceptibles d'affecter la base de données. Exemple :  
  
    -   Vous ne pouvez pas supprimer les colonnes utilisées dans une clé primaire d'articles de publications transactionnelles car elles sont utilisées par la réplication.  
  
    -   Vous ne pouvez pas supprimer la colonne rowguid d'articles de publications de fusion ou la colonne mstran_repl_version d'articles de publications transactionnelles qui prennent en charge les abonnement mis à jour car elles sont utilisées par la réplication.  
  
    -   Les modifications d'index ne sont pas propagées aux Abonnés : si vous supprimez une colonne sur le serveur de publication et si un index dépendant est supprimé, la suppression de l'index n'est pas répliquée. Vous devez supprimer l'index sur l'Abonné avant de supprimer la colonne sur le serveur de publication, de manière à ce que la suppression de la colonne réussisse lorsqu'elle est répliquée depuis le serveur de publication vers l'Abonné. Si la synchronisation échoue en raison d'un index sur l'Abonné, supprimez manuellement cet index, puis réexécutez l'Agent de fusion.  
  
    -   Les opérations de suppression ne fonctionnent que si les contraintes sont nommées explicitement. Pour plus d'informations, consultez la section « Considérations générales », plus haut dans cette rubrique.  
  
### <a name="transactional-replication"></a>Réplication transactionnelle  
  
-   Les modifications de schéma sont propagées aux Abonnés exécutant des versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], mais l'instruction DDL doit absolument inclure la syntaxe prise en charge par la version installée sur l'Abonné.  
  
     Si l'Abonné republie des données, les seules modifications de schéma prises en charge sont l'ajout et la suppression d'une colonne. Ces modifications doivent être effectuées sur le serveur de publication à l’aide de [sp_repladdcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repladdcolumn-transact-sql.md) et [sp_repldropcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repldropcolumn-transact-sql.md) à la place de la syntaxe ALTER TABLE DDL.  
  
-   Les modifications de schéma ne sont pas répliquées sur les Abonnés non SQL Server.  
  
-   Ces modifications de schéma ne sont pas propagées si elles proviennent de serveurs de publication non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Vous ne pouvez pas modifier des vues indexées répliquées en tant que tables. Les vues indexées répliquées en tant que vues indexées peuvent l'être mais une fois modifiées, elles deviennent des tables régulières et non plus des vues indexées.  
  
-   Si la publication prend en charge les abonnements avec mise à jour immédiate ou les abonnements avec mise à jour en attente, le système doit être suspendu avant toute modification du schéma : toutes les activités relatives à la table publiée doivent être interrompues au niveau du serveur de publication et des Abonnés. Par ailleurs, les modifications de données en attente doivent être propagées à tous les nœuds. Après la propagation des modifications de schéma à tous les nœuds, l'activité peut reprendre dans les tables publiées.  
  
-   Si la publication fait partie d'une topologie d'égal à égal, le système doit être suspendu le temps d'effectuer les modifications de schéma : Pour plus d’informations, consultez [Suspendre une topologie de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).  
  
-   L'ajout d'une colonne d'horodatage à une table et le mappage de l'horodatage à binary(8) entraîne la réinitialisation de l'article pour tous les abonnements actifs.  
  
### <a name="merge-replication"></a>Réplication de fusion  
  
-   La façon dont la réplication de fusion gère des modifications de schéma est déterminée par le niveau de compatibilité de la publication et le fait que l'instantané soit défini en mode natif (par défaut) ou en mode caractère :  
  
    -   Pour répliquer les modifications de schéma, le niveau de compatibilité de la publication doit être au minimum égal à 90RTM. Si les Abonnés exécutent des versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou si le niveau de compatibilité est inférieur à 90RTM, vous pouvez utiliser [sp_repladdcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repladdcolumn-transact-sql.md) et [sp_repldropcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repldropcolumn-transact-sql.md) pour ajouter et supprimer des colonnes. Toutefois, ces procédures sont déconseillées.  
  
    -   Si vous essayez d'ajouter à un article existant une colonne avec un type de données qui a été introduit dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] présente le comportement suivant :  
  
        ||100RTM, instantané natif|100RTM, instantané en mode caractère|Tous les autres niveaux de compatibilité|  
        |-|-----------------------------|--------------------------------|------------------------------------|  
        |**hierarchyid**|Autoriser la modification|Bloquer la modification|Bloquer la modification|  
        |**geography** et **geometry**|Autoriser la modification|Autoriser la modification*|Bloquer la modification|  
        |**flux de fichier**|Autoriser la modification|Bloquer la modification|Bloquer la modification|  
        |**date**, **time**, **datetime2**et **datetimeoffset**|Autoriser la modification|Autoriser la modification*|Bloquer la modification|  
  
         *Les Abonnés SQL Server Compact convertissent ces types de données côté Abonné.  
  
-   Si une erreur se produit lors de l'application d'une modification de schéma (par exemple une erreur résultant de l'ajout d'une clé étrangère référençant une table non disponible sur l'Abonné), la synchronisation échoue et l'abonnement doit être réinitialisé.  
  
-   Si une modification de schéma est apportée à une colonne reprise dans un filtre de jointure ou un filtre paramétré, vous devez réinitialiser tous les abonnements et régénérer l'instantané.  
  
-   La réplication de fusion fournit des procédures stockées qui permettent d'ignorer les modifications de schéma pendant le dépannage. Pour plus d’informations, consultez [sp_markpendingschemachange &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md) et [sp_enumeratependingschemachanges &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumeratependingschemachanges-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-view-transact-sql.md)   
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-procedure-transact-sql.md)   
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-function-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Régénérer des procédures transactionnelles personnalisées pour refléter des modifications de schéma](../../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md)  
  
  
