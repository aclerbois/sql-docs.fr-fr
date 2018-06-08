---
title: Problèmes et limitations de conception des serveurs de publication Oracle | Microsoft Docs
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
helpviewer_keywords:
- Oracle publishing [SQL Server replication], design considerations and limitations
ms.assetid: 8d9dcc59-3de8-4d36-a61f-bc3ca96516b6
caps.latest.revision: 48
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 38df2896997fe55f9072c481eb7678ce6c870aa8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="design-considerations-and-limitations-for-oracle-publishers"></a>Problèmes et limitations de conception des serveurs de publication Oracle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  En termes de conception, la publication à partir d'une base de données Oracle fonctionne de façon similaire à la publication à partir d'une base de données [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Toutefois, soyez conscient des problèmes et limitations suivants :  
  
-   L'option Oracle Gateway offre de meilleures performances que l'option Oracle Complete ; il n'est cependant pas possible de l'utiliser pour publier la même table dans plusieurs publications transactionnelles. Une table peut faire partie d'un nombre quelconque de publications d'instantané mais d'une seule publication transactionnelle uniquement. Si vous devez publier la même table dans plusieurs publications transactionnelles, choisissez l'option Oracle Complete.  
  
-   La réplication prend en charge les vues matérialisées, les index et les tables de publication. Les autres objets ne sont pas répliqués.  
  
-   Il existe certaines différences mineures entre le stockage et le traitement des données effectués par Oracle et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui ont une incidence sur la réplication.  
  
-   Le mode de prise en charge des fonctionnalités de réplication transactionnelle diffère quelque peu lorsque vous utilisez un serveur de publication Oracle.  
  
## <a name="support-for-publishing-objects-from-oracle"></a>Prise en charge des objets de publication d'Oracle  
 La réplication peut répliquer les objets de base de données Oracle suivants :  
  
-   Tables  
  
-   Tables indexées  
  
-   Index  
  
-   Vues matérialisées (répliquées en tant que tables)  
  
 Les éléments suivants peuvent figurer dans les tables publiées mais ne sont pas répliqués :  
  
-   Index basés sur les domaines  
  
-   Index basés sur les fonctions  
  
-   Valeurs par défaut  
  
-   Contraintes de validation  
  
-   Clés étrangères  
  
-   Options de stockage (espaces de table, clusters, etc.)  
  
 Les objets suivants ne peuvent pas être répliqués :  
  
-   Tables imbriquées  
  
-   Vues  
  
-   Packages, corps de package, procédures et déclencheurs  
  
-   Files d'attente  
  
-   Séquences  
  
-   Synonymes  
  
 Pour plus d'informations sur les types de données pris en charge, consultez [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md).  
  
## <a name="differences-between-oracle-and-sql-server"></a>Différences entre Oracle et SQL Server  
  
-   Oracle présente des limites de taille maximale différentes pour certains objets. Tout objet créé dans la base de données de publication Oracle doit respecter les limites de taille maximale des objets correspondants dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour obtenir des informations sur les limites de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Spécifications des capacités maximales pour SQL Server](../../../sql-server/maximum-capacity-specifications-for-sql-server.md).  
  
-   Les noms des objets Oracle sont créés par défaut en majuscules. Vérifiez que les noms des objets Oracle sont bien en majuscules lorsque vous les publiez via un serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'ils figurent en majuscules dans la base de données Oracle. Si vous ne respectez pas la casse, il se peut qu'un message d'erreur indiquant que l'objet est introuvable s'affiche.  
  
-   Oracle utilise une version du langage SQL légèrement différente de celle de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]; les filtres de lignes doivent être écrits dans une syntaxe compatible avec celle d'Oracle.  
  
### <a name="considerations-for-large-objects"></a>Considérations relatives aux objets volumineux  
 Les données d'objets volumineux (LOB, Large Object) ne sont pas stockées dans la table du journal d'articles ; les mises à jour de ces données sont toujours directement extraites de la table publiée. Les mises à jour sont répliquées dans les publications transactionnelles dans le seul cas où l'opération affectant l'objet LOB active le déclencheur de réplication sur la table répliquée. Les déclencheurs Oracle sont activés lorsque des lignes contenant des objets LOB sont insérées ou supprimées. En revanche les mises à jour apportées aux colonnes LOB n'activent pas les déclencheurs. La mise à jour d'une colonne LOB est répliquée immédiatement uniquement si une colonne non-LOB de la même ligne est également mise à jour dans la même transaction Oracle. Sinon, la colonne LOB est actualisée sur l'Abonné lors de la mise à jour suivante d'une colonne non-LOB dans la même ligne. Assurez-vous que ce comportement est accepté par votre application.  
  
 Pour répliquer les mises à jour apportées aux colonnes LOB dans des publications transactionnelles, plusieurs stratégies sont possibles lorsque vous écrivez l'application :  
  
-   Supprimez et réinsérez la ou les lignes dans une transaction au lieu de mettre à jour la ligne : spécifiez le nouvel objet LOB lors de la réinsertion de la ligne. Dans la mesure où les opérations de suppression et d'insertion activent toutes deux les déclencheurs, la ligne sera répliquée.  
  
-   Incluez une colonne non LOB dans la mise à jour de ligne en plus de la colonne LOB ou mettez à jour une colonne non LOB de la ligne au cours de la même transaction Oracle. Dans les deux cas, la mise à jour de la colonne non LOB garantit l'activation du déclencheur.  
  
 Pour plus d'informations sur les objets volumineux, consultez [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md).  
  
### <a name="unique-indexes-and-constraints"></a>Contraintes et index uniques  
 Tant pour la réplication d'instantané que transactionnelle, les colonnes contenues dans des index et des contraintes uniques (y compris les contraintes de clé primaire) doivent respecter certaines restrictions. Si ce n'est pas le cas, la contrainte ou l'index n'est pas répliqué.  
  
-   Le nombre maximal de colonnes autorisé dans un index de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est 16.  
  
-   Toutes les colonnes incluses dans des contraintes uniques doivent contenir des types de données pris en charge. Pour plus d'informations sur les types de données, consultez [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md).  
  
-   Toutes les colonnes incluses dans des contraintes uniques doivent être publiées (elles ne peuvent pas être filtrées).  
  
-   Les colonnes contenues dans des contraintes ou des index uniques ne doivent pas être Null.  
  
 Veillez également à prendre en compte les points suivants :  
  
-   Oracle et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] traitent différemment la valeur NULL : Oracle autorise la présence de plusieurs lignes comportant des valeurs NULL pour les colonnes qui acceptent NULL et sont incluses dans des contraintes ou index uniques. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] applique l'unicité en n'autorisant qu'une seule ligne contenant une valeur NULL pour la même colonne. Vous ne pouvez pas publier une contrainte ou un index unique qui autorise NULL car une violation de contrainte se produit au niveau de l'Abonné si la table publiée contient plusieurs lignes avec des valeurs NULL pour l'une des colonnes incluses dans l'index ou la contrainte.  
  
-   Lors de la vérification de l'unicité, les espaces à droite dans un champ sont ignorés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mais pas par Oracle.  
  
 Comme c'est le cas dans la réplication transactionnelle [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , les tables contenues dans les publications transactionnelles d'Oracle exigent une clé primaire, laquelle doit être unique en vertu des règles énoncées ci-dessus. Si la clé primaire n'adhère pas aux règles définies ci-dessus, la table ne peut pas être publiée pour la réplication transactionnelle.  
  
## <a name="differences-between-oracle-publishing-and-standard-transactional-replication"></a>Différences entre la publication Oracle et la réplication transactionnelle standard  
  
-   Un serveur de publication Oracle ne peut avoir le même nom que : son serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , l'un des serveurs de publication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui utilisent le serveur de distribution ou l'un des Abonnés qui reçoivent la publication. Les publications traitées par le même serveur de distribution doivent avoir chacune un nom unique.  
  
-   Une table publiée dans une publication Oracle ne peut pas recevoir de données répliquées. Par conséquent, la publication Oracle ne prend pas en charge : les publications avec mise à jour immédiate ou abonnements mis à jour en attente, ou les topologies dans lesquelles les tables de publication servent également de tables d'abonnement, comme la réplication d'égal à égal et la réplication bidirectionnelle.  
  
-   Les relations clé primaire-clé étrangère dans la base de données Oracle ne sont pas répliquées sur les Abonnés. En revanche, les relations sont conservées dans les données au moment de la remise des modifications.  
  
-   Les publications transactionnelles standard prennent en charge des tables comprenant jusqu'à 1000 colonnes. Les publications transactionnelles Oracle prennent en charge 995 colonnes (la réplication ajoute cinq colonnes à chaque table publiée).  
  
-   Des clauses COLLATE sont ajoutées aux instructions CREATE TABLE pour permettre l'exécution de comparaisons respectant la casse, ce qui est important pour les clés primaires et les contraintes uniques. Ce comportement est contrôlé par l’option de schéma 0x1000, spécifiée avec le paramètre **@schema_option** de [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
  
-   Si vous utilisez des procédures stockées pour configurer ou gérer un serveur de publication Oracle, ne placez pas les procédures dans une transaction explicite. L'opération n'est pas prise en charge sur le serveur lié utilisé pour la connexion au serveur de publication Oracle.  
  
-   Si vous utilisez un Assistant pour créer un abonnement à une publication Oracle extrait (pull), vous devez utiliser l'Assistant Nouvel abonnement fourni avec [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et les versions ultérieures. Pour les versions précédentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez en revanche utiliser la procédure stockée et les interfaces SQL-DMO pour configurer les abonnements aux publications Oracle extraits.  
  
-   Si vous utilisez des procédures stockées pour propager les modifications apportées aux Abonnés (option par défaut), sachez que la syntaxe MCALL est prise en charge mais se comporte différemment si la publication émane d'un serveur de publication Oracle. MCALL fournit habituellement un fichier bitmap qui montre les colonnes mises à jour sur le serveur de publication. Avec une publication Oracle, le fichier bitmap montre toujours toutes les colonnes mises à jour. Pour plus d’informations sur l’utilisation des procédures stockées, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
### <a name="transactional-replication-feature-support"></a>Prise en charge de la fonctionnalité de réplication transactionnelle  
  
-   Les publications Oracle ne prennent pas en charge toutes les options de schéma prises en charge par les publications SQL Server. Pour plus d’informations sur les options de schéma, consultez [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
  
-   Les Abonnés aux publications Oracle ne peuvent pas utiliser les abonnements mis à jour immédiatement ou en attente ou être des nœuds dans une topologie d'égal à égal ou bidirectionnelle.  
  
-   Les Abonnés aux publications Oracle ne peuvent pas être automatiquement initialisés à partir d'une sauvegarde.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge deux types de validation : binaire et nombre de lignes. Les serveurs de publication Oracle prennent uniquement en charge la validation du nombre de lignes. Pour plus d’informations, consultez [Valider des données répliquées](../../../relational-databases/replication/validate-replicated-data.md).  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] offre deux formats d'instantanés : le mode bcp natif et le mode caractère. Les serveurs de publication Oracle prennent en charge les instantanés en mode caractères.  
  
-   Les modifications de schéma apportées aux tables Oracle publiées ne sont pas prises en charge. Pour apporter des modifications au schéma, commencez par supprimer la publication, apportez vos modifications puis recréez la publication et les abonnements.  
  
    > [!NOTE]  
    >  Si les modifications du schéma ainsi que la suppression et recréation ultérieures de la publication et des abonnements sont effectuées à un moment où il n'y a aucune activité sur les tables publiées, vous pouvez spécifier l'option « Prise en charge de la réplication uniquement » pour les abonnements. Ils peuvent ainsi être synchronisés sans devoir copier un instantané sur chaque Abonné. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
### <a name="replication-security-model"></a>Modèle de sécurité de la réplication  
 Le modèle de sécurité pour la publication Oracle est identique à celui de la réplication transactionnelle standard, à ces différences près :  
  
-   Le compte utilisé par l'Agent d'instantané et l'Agent de lecture du journal pour connecter le serveur de distribution au serveur de publication est spécifié à l'aide de l'une des méthodes suivantes :  
  
    -   Le paramètre **@security_mode** de [sp_adddistpublisher & #40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) (vous spécifiez également les valeurs de **@login** et **@password** si l’authentification Oracle est utilisée)  
  
    -   Dans la boîte de dialogue **Se connecter au serveur** de SQL Server Management Studio, que vous utilisez lorsque vous configurez le serveur de publication Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
     Dans la réplication transactionnelle standard, le compte est spécifié avec [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) et [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md).  
  
-   Le compte utilisé par l’Agent d’instantané et l’Agent de lecture du journal pour la connexion ne peut pas être modifié avec [sp_changedistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md) ou via une feuille de propriétés, mais le mot de passe peut l’être.  
  
-   Si vous spécifiez la valeur 1 (Authentification intégrée de Windows) pour le paramètre **@security_mode** de [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) :  
  
    -   Le compte de processus et le mot de passe associé qui sont utilisés par les Agents d’instantané et de lecture du journal (paramètres **@job_login** et **@job_password** de [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) et [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)) doivent être identiques au compte et au mot de passe utilisés pour la connexion au serveur de publication Oracle.  
  
    -   Vous ne pouvez pas modifier le paramètre **@job_login** via [sp_changepublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md) ou [sp_changelogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md), mais le mot de passe, oui.  
  
 Pour plus d’informations sur la sécurité de la réplication, consultez [Sécurité et protection &#40;réplication&#41;](../../../relational-databases/replication/security/security-and-protection-replication.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Considérations sur l’administration des serveurs de publication Oracle](../../../relational-databases/replication/non-sql/administrative-considerations-for-oracle-publishers.md)   
 [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Vue d’ensemble de la publication Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)  
  
  
