---
title: Propriétés de l’article - &lt;Article&gt; | Microsoft Docs
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
- sql13.rep.newpubwizard.articleproperties.f1
helpviewer_keywords:
- Article Properties dialog box
ms.assetid: 6dd601a4-1233-43d9-a9f0-bc8d84e5d188
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 12c255b06cd56ff27f1ada7f3ca0f0fa36113407
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="article-properties---ltarticlegt"></a>Propriétés de l’article - &lt;Article&gt;
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Propriétés de l'article** est accessible depuis l'Assistant Nouvelle publication et la boîte de dialogue **Propriétés de la publication** . Elle permet d'afficher et de définir les propriétés de tous les types d'articles. Certaines propriétés peuvent être définies uniquement lors de la création de la publication, et d'autres uniquement si la publication n'a pas d'abonnements actifs. Les propriétés qui ne peuvent pas être définies s'affichent en lecture seule.  
  
> [!NOTE]  
>  Après qu'une publication a été créée, certaines modifications de propriétés nécessitent un nouvel instantané. Si une publication dispose d'abonnements, ces abonnements pourraient devoir alors être réinitialisés selon les modifications que vous avez apportées. Pour plus d’informations, consultez [Modifier les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
 Chaque propriété dans la boîte de dialogue **Propriétés de l'article** contient une description. Cliquez sur une propriété pour afficher sa description dans la partie inférieure de la boîte de dialogue. Cette rubrique fournit des informations sur diverses propriétés, Les propriétés sont regroupées selon les catégories suivantes :  
  
-   Propriétés qui s'appliquent à toutes les publications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Propriétés qui s'appliquent aux publications transactionnelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Propriétés qui s'appliquent aux publications de fusion.  
  
-   Propriétés qui s'appliquent aux publications transactionnelles et d'instantané des serveurs de publication Oracle.  
  
## <a name="options-for-all-publications"></a>Options pour toutes les publications  
 **Copier les schémas de partition de table** et **Copier les schémas de partition d'index**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit le partitionnement de table et le partitionnement d'index qui ne sont pas associés au partitionnement que la réplication fournit par le biais des filtres de lignes et de colonnes. Les options **Copier les schémas de partition de table** et **Copier les schémas de partition d'index** permettent d'indiquer si les schémas de partitionnement doivent être copiés vers l'Abonné. Pour plus d'informations sur le partitionnement, consultez [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 **Convertir les types de données**  
 Détermine si les types de données définis par l'utilisateur doivent être convertis en types de données de base lors de la création d'objets au niveau de l'Abonné. Les types de données définis par l'utilisateur incluent les types CLR introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Définissez la valeur **True** si vous répliquez ces types de données vers plusieurs versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]afin qu'ils soient traités correctement au niveau de l'Abonné.  
  
 **Créer des schémas sur l'Abonné**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit des schémas qui sont définis en utilisant l'instruction CREATE SCHEMA. Un schéma est le propriétaire d’un objet ; il est utilisé dans un nom composé, tel que \<Base_de_données>.\<Schéma>.\<Objet>. Si la base de données contient des objets appartenant à des schémas autres que DBO, la réplication peut créer ces schémas au niveau de l'Abonné pour pouvoir créer des objets publiés.  
  
 Si vous répliquez les données vers des versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]:  
  
-   Affectez la valeur **False**à cette option, car les versions précédentes ne prennent pas en charge CREATE SCHEMA.  
  
-   Pour chaque schéma, ajoutez un utilisateur à la base de données d'abonnement ayant le même nom que le schéma.  
  
 **Convertir XML en NTEXT**, **Convertir les types de données MAX en NTEXT et IMAGE**, **Convertir la nouvelle valeur datetime en NVARCHARXUIX**, **Convertir le flux de fichier en types de données MAX**, **Convertir CLR volumineux en types de données MAX**, **Convertir hierarchyId en types de données MAX**et **Convertir spatial en types de données MAX**.  
 Indique si les types de données et les attributs doivent être convertis comme indiqué. Spécifiez la valeur **True** pour répliquer ces types de données vers des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela garantit qu'ils pourront être traités correctement sur l'Abonné.  
  
 **Nom de l'objet de destination**  
 Nom de l'objet créé dans la base de données d'abonnement. Cette option ne peut pas être modifiée pour les articles des publications activées pour la réplication transactionnelle d'égal à égal.  
  
 **Propriétaire de l'objet de destination**  
 Schéma sous lequel l'objet est créé dans la base de données d'abonnement. Le schéma par défaut est le schéma auquel l'objet appartient dans la base de données de publication, avec les exceptions suivantes :  
  
-   Pour les articles de publications de fusion d'un niveau de compatibilité inférieur à 90 : par défaut, le propriétaire est laissé vide et spécifié comme **dbo** pendant la création de l'objet sur l'Abonné.  
  
-   Pour les articles des publications Oracle : par défaut, le propriétaire est spécifié comme **dbo**.  
  
-   Pour les articles de publications utilisant les instantanés en mode caractère (utilisées pour les abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les abonnés [!INCLUDE[ssEW](../../includes/ssew-md.md)] ) : le propriétaire est laissé vide par défaut. Le propriétaire prend les valeurs par défaut du propriétaire associé au compte utilisé par l'Agent de distribution ou l'Agent de fusion pour se connecter à l'Abonné.  
  
 Cette option ne peut pas être modifiée pour les articles des publications activées pour la réplication transactionnelle d'égal à égal.  
  
 **Gérer automatiquement les plages d'identité**  
 Par défaut, la réplication gère toutes les colonnes d'identité au niveau du serveur de publication et de chaque abonné. Chaque nœud de réplication est affecté d'une plage de valeurs d'identité (définie avec les options **Taille de la plage sur le serveur de publication** et **Taille de la plage sur l'Abonné** ) pour qu'une valeur donnée soit utilisée uniquement sur un nœud. Pour plus d’informations, consultez [Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
## <a name="options-for-transactional-publications"></a>Options des publications transactionnelles  
 **Copie des procédures stockées INSERT, UPDATE et DELETE**  
 Si, dans la section **Remise d'instruction** de cette boîte de dialogue, vous décidez d'utiliser des procédures stockées pour propager les modifications vers les abonnés (action par défaut), indiquez si vous voulez copier les procédures au niveau de chaque abonné. Si vous sélectionnez **False**, vous devez copier manuellement les procédures, car sinon, l'Agent de distribution ne peut pas distribuer les modifications.  
  
 **Statement delivery**  
 Les options de cette section s'appliquent à toutes les tables, notamment les vues indexées répliquées en tant que tables. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'utiliser les options par défaut si l'application ne nécessite pas une fonctionnalité différente. Par défaut, la réplication transactionnelle propage les modifications vers les abonnés via un groupe de procédures stockées installées sur chaque abonné. Lorsqu'une insertion, une mise à jour ou une suppression se produit dans une table sur le serveur de publication, l'opération est convertie en appel à une procédure stockée au niveau de l'Abonné.  
  
 Les options d' **instruction de remise** permettent d'indiquer si vous voulez utiliser une procédure stockée et, dans ce cas, de définir le format à utiliser pour les paramètres transmis à la procédure. Les options de **procédure stockée** permettent d'utiliser des procédures que crée automatiquement la réplication, ou de substituer des procédures stockées que vous avez créées.  
  
 Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
 **Répliquer**  
 Cette option s'applique uniquement aux procédures stockées. Elle permet d'indiquer si vous voulez répliquer la définition de la procédure stockée (instruction CREATE PROCEDURE) ou son exécution. Si vous répliquez l'exécution de la procédure, la définition de la procédure est répliquée vers l'Abonné lors de l'initialisation de l'abonnement. Lorsque la procédure est exécutée sur le serveur de publication, la réplication exécute la procédure correspondante au niveau de l'Abonné. Ceci peut améliorer les performances de manière significative lorsque des opérations de traitement par lot volumineuses sont exécutées. Pour plus d’informations, voir [Publishing Stored Procedure Execution in Transactional Replication](../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md).  
  
## <a name="options-for-merge-publications"></a>Options des publications de fusion  
 La boîte de dialogue **Propriétés de l'article** des publications de fusion contient deux onglets : **Propriétés** et **Résolveur**.  
  
### <a name="properties-tab"></a>Onglet Propriétés  
 **Direction de la synchronisation**  
 Indique si les modifications peuvent être téléchargées depuis les abonnés qui utilisent le type d'abonnement client :  
  
-   **Bidirectionnelle** (valeur par défaut) : les modifications peuvent être téléchargées vers l'Abonné et vers le serveur de publication.  
  
-   **Téléchargement seul pour l'Abonné, interdire les modifications de l'Abonné**: les modifications peuvent être téléchargées vers l'Abonné, mais pas vers le serveur de publication. Les déclencheurs empêchent les modifications au niveau de l'Abonné.  
  
-   **Téléchargement seul pour l'Abonné, autoriser les modifications de l'Abonné**: les modifications peuvent être téléchargées vers l'Abonné, mais pas vers le serveur de publication.  
  
 Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
 **Options de la partition**  
 Définit le type des partitions que crée un filtre paramétrable. Pour plus d'informations, consultez la section « Définition des options de partition » de [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Niveau de suivi**  
 Indique si les modifications d'une même ligne ou d'une même colonne doivent être traitées sous la forme d'un conflit.  
  
 **Vérifier l'autorisation INSERT**, **Vérifier l'autorisation UPDATE**et **Vérifier l'autorisation DELETE**  
 Permet d'indiquer si vous voulez vérifier au cours de la synchronisation si la connexion de l'Abonné dispose de l'autorisation INSERT, UPDATE ou DELETE sur les tables publiées dans la base de données de publication. La valeur par défaut est **False** , car la réplication de fusion n'a pas besoin d'accorder ces autorisations ; l'accès aux tables publiées est contrôlé par la liste d'accès à la publication. Pour plus d’informations sur la liste d’accès à la publication, consultez [Sécuriser le serveur de publication](../../relational-databases/replication/security/secure-the-publisher.md).  
  
 Vous pouvez demander la vérification des autorisations si vous voulez autoriser des abonnés à télécharger certaines modifications des données publiées. Vous pouvez, par exemple, ajouter un abonné à une liste d'accès à la publication, et ne fournir à l'abonné aucune autorisation sur les tables de la base de données de publication. Vous pouvez ensuite définir les autorisations DELETE en leur affectant la valeur **True**: l'Abonné peut télécharger les insertions et les modifications, mais pas les suppressions.  
  
 **UPDATE multicolonne**  
 Lorsque la réplication de fusion effectue une mise à jour, elle met à jour toutes les colonnes modifiées dans une instruction UPDATE et réinitialise les valeurs d'origine des colonnes non modifiées. Dans ce cas, l'alternative consiste à émettre plusieurs instructions UPDATE avec une instruction UPDATE pour chaque colonne modifiée. L'instruction UPDATE multicolonne est généralement plus efficace, mais vous devez affecter la valeur **False** à cette option si des déclencheurs de la table sont définis pour répondre à la mise à jour de certaines colonnes et qu'ils répondent incorrectement, car ces colonnes sont réinitialisées lors de la mise à jour.  
  
> [!IMPORTANT]  
>  Cette option est déconseillée et ne figurera plus dans les prochaines versions  
  
### <a name="resolver-tab"></a>Onglet Résolveur  
 **Utiliser le résolveur par défaut**  
 Si vous sélectionnez le résolveur par défaut, les conflits sont résolus en fonction de la priorité affectée à chaque abonné ou de la première modification écrite sur le serveur de publication, selon le type d'abonnement utilisé. Pour plus d’informations, consulter [Détecter et résoudre des conflits de réplication de fusion](../../relational-databases/replication/merge/advanced-merge-replication-resolve-merge-replication-conflicts.md).  
  
 **Utiliser un résolveur personnalisé (inscrit sur le serveur de distribution)**  
 Si vous décidez d'utiliser un résolveur d'article (fourni par [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou que vous avez écrit), vous devez sélectionner un résolveur dans la zone de liste. Pour plus d’informations, voir [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
 Si le résolveur nécessite une entrée, définissez-la dans la zone de texte **Entrez les informations requises par le résolveur** . Pour plus d'informations sur l'entrée nécessaire aux résolveurs personnalisés [!INCLUDE[msCoName](../../includes/msconame-md.md)] , consultez [Microsoft COM-Based Resolvers](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
 **Autoriser l'Abonné à résoudre les conflits de manière interactive au cours de la synchronisation à la demande**  
 Sélectionnez cette option si les abonnés utilisent la synchronisation à la demande (synchronisation par défaut de la réplication de fusion) et que vous voulez résoudre les conflits de manière interactive. Spécifiez la synchronisation à la demande dans la page **Planification de synchronisation** de l'Assistant Nouvel abonnement. Pour résoudre les conflits de manière interactive, utilisez l'interface utilisateur Résolveur interactif. Pour plus d’informations, voir [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
 **Exiger la vérification d'une signature numérique avant la fusion**  
 Tous les résolveurs COM fournis par [!INCLUDE[msCoName](../../includes/msconame-md.md)] sont signés. Sélectionnez cette option pour vérifier que le résolveur est valide lors de la synchronisation.  
  
## <a name="options-for-oracle-publications"></a>Options des publications Oracle  
 La boîte de dialogue **Propriétés de l'article** des publications Oracle contient deux onglets : **Propriétés** et **Mappage de données**. Les publications Oracle ne prennent pas en charge toutes les propriétés que prennent en charge les publications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, voir [Design Considerations and Limitations for Oracle Publishers](../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md).  
  
### <a name="properties-tab"></a>Onglet Propriétés  
 **Copie des procédures stockées INSERT, UPDATE et DELETE**  
 Si l'article se trouve dans une publication transactionnelle et que, dans la section **Remise d'instruction** de cette boîte de dialogue, vous décidez d'utiliser des procédures stockées pour propager les modifications vers les abonnés (action par défaut), indiquez si vous voulez copier les procédures au niveau de chaque abonné. Si vous sélectionnez **False**, vous devez copier manuellement les procédures, car sinon, l'Agent de distribution ne peut pas distribuer les modifications.  
  
 **Propriétaire de l'objet de destination**  
 Si vous entrez une valeur différente de **dbo**:  
  
-   Pour les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou une version ultérieure, vous devez vérifier qu'un schéma est créé au niveau de l'Abonné ayant le même nom que la valeur que vous entrez. Pour plus d’informations, consultez [CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md).  
  
-   Pour chaque abonné qui exécute des versions antérieures à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], pour chaque schéma, ajoutez un utilisateur à la base de données d'abonnement ayant le même nom que le schéma.  
  
 **Nom de l'espace disque logique**  
 Nom de l'espace disque logique dans lequel doivent être créées les tables de suivi des modifications de réplication sur l'instance du serveur Oracle. Pour plus d’informations, consultez [Gérer des espaces disque logiques Oracle](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md).  
  
 **Statement delivery**  
 Les options de cette section s'appliquent à toutes les tables dans les publications transactionnelles. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'utiliser les options par défaut si l'application ne nécessite pas une fonctionnalité différente. Par défaut, la réplication transactionnelle propage les modifications vers les abonnés via un groupe de procédures stockées installées sur chaque abonné. Lorsqu'une insertion, une mise à jour ou une suppression se produit dans une table sur le serveur de publication, l'opération est convertie en appel à une procédure stockée au niveau de l'Abonné.  
  
 Les options d' **instruction de remise** permettent d'indiquer si vous voulez utiliser une procédure stockée et, dans ce cas, de définir le format à utiliser pour les paramètres transmis à la procédure. Les options de **procédure stockée** permettent d'utiliser des procédures que crée automatiquement la réplication, ou de substituer des procédures stockées que vous avez créées.  
  
 Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
### <a name="data-mapping-tab"></a>Onglet Mappage de données  
 **Nom de colonne**  
 Nom de la colonne du serveur de publication (en lecture seule).  
  
 **Type de données du serveur de publication**  
 Type de données Oracle de la colonne du serveur de publication (lecture seule). Le type de données peut être modifié uniquement directement dans la base de données Oracle. Pour plus d'informations, consultez votre documentation Oracle.  
  
 **Type de données de l'Abonné**  
 Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auquel le type de données Oracle est associé lors de la réplication des données :  
  
-   Pour certains types de données, il existe uniquement une seule possibilité de mappage dans laquelle la colonne de la grille de propriétés est en lecture seule.  
  
-   Pour certains types, vous pouvez sélectionner plus d'un type. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'utiliser le mappage par défaut si l'application ne nécessite pas un mappage différent. Pour plus d’informations, voir [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Créer et appliquer l’instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [Réinitialiser un abonnement](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
