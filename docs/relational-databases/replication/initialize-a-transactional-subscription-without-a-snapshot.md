---
title: Initialiser un abonnement transactionnel sans instantané | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, initializing
- replication [SQL Server], initializing
- initializing subscriptions [SQL Server replication], without snapshots
ms.assetid: 75c8c1f8-60bc-44a8-944b-d18d1f6bda11
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: da939645cd7f442f15006d994390bf7df7a40c3f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="initialize-a-transactional-subscription-without-a-snapshot"></a>Initialiser un abonnement transactionnel sans instantané
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Par défaut, un abonnement à une publication transactionnelle est initialisé avec un instantané, lequel est généré par l'Agent d'instantané et appliqué par l'Agent de distribution. Dans certains scénarios, comme ceux impliquant de volumineux datasets initiaux, il est préférable d'initialiser un abonnement à l'aide d'une autre méthode. Autres méthodes d'initialisation d'un abonné :  
  
-   Spécifier une sauvegarde. Restaurez la sauvegarde sur l'Abonné, l'Agent de distribution copie ensuite toutes les métadonnées de réplication et procédures système requises. L'initialisation avec une sauvegarde est le moyen le plus rapide pour remettre les données sur l'Abonné, c'est également un moyen pratique, car vous pouvez utiliser n'importe quelle sauvegarde récente si celle-ci a été effectuée après que la publication ait été activée pour l'initialisation avec une sauvegarde.  
  
-   Copier un dataset initial sur l'Abonné par un autre mécanisme, comme l'attachement d'une base de données. Vous devez vous assurer que les données et schéma corrects se trouvent sur l'Abonné, l'Agent de distribution copie ensuite toutes les métadonnées et procédures système requises.  
  
## <a name="initializing-a-subscription-with-a-backup"></a>Initialisation d'un abonnement avec une sauvegarde  
 Une sauvegarde contient une base de données complète ; de ce fait, chaque base de données d'abonnement contiendra une copie complète de la base de données de publication lorsqu'elle est est initialisée :  
  
-   La sauvegarde inclut des tables n'étant pas spécifiées comme des articles pour la publication.  
  
-   La sauvegarde inclut toutes les données, même si des filtres de lignes ou de colonnes sont spécifiés sur une table.  
  
 L'administrateur ou l'application est responsable de la suppression de tout objet ou donnée non sollicité une fois que la sauvegarde a été restaurée. Dans les synchronisations suivantes, les modifications de données sont répliquées uniquement si elles s'appliquent à des tables spécifiées comme des articles, et que les modifications répondent à tous les critères de filtrage spécifiés.  
  
> [!NOTE]  
>  Lors de la restauration d'une sauvegarde, vous devez vous assurer que la sauvegarde provient d'un serveur de publication si vous voulez que l'Abonné se synchronise automatiquement. Les valeurs du numéro séquentiel dans le journal (LSN) de la sauvegarde (utilisées pour définir le point de démarrage de la synchronisation) sont spécifiques au serveur de publication.  
  
 **Pour initialiser un abonnement avec une sauvegarde**  
  
 Pour initialiser un abonnement avec une sauvegarde, vous devez d'abord activer l'option lorsque vous créez une publication, puis spécifier des valeurs pour plusieurs options lorsque vous créez un abonnement. Les publications peuvent être activées par le biais de l'Assistant Nouvelle publication ou par programme. Cependant, les valeurs requises pour les options d'abonnement ne peuvent être spécifiées que par programme.  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Activer l’initialisation avec une sauvegarde pour les publications transactionnelles &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/enable-initialization-with-backup-for-transactional-publications.md)  
  
-   Programmation Transact-SQL de la réplication : [Initialiser un abonnement transactionnel à partir d’une sauvegarde &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/initialize-a-transactional-subscription-from-a-backup.md)  
  
> [!NOTE]  
>  Si un abonnement est initialisé sans utiliser un instantané, le compte sous lequel fonctionne le service SQL Server sur le serveur de publication doit disposer d'autorisations d'écriture sur le dossier d'instantanés sur le serveur de distribution. Pour plus d'informations sur les autorisations, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
### <a name="ensuring-the-suitability-of-a-backup"></a>Vérification de la pertinence d'une sauvegarde  
 Une sauvegarde est appropriée à l'initialisation d'un abonné si toutes les transactions se produisant après avoir effectué la sauvegarde sont stockées sur le serveur de distribution. La réplication affichera un message d'erreur si la sauvegarde n'est pas appropriée.  
  
 Afin de vous assurer que l'utilisation d'une sauvegarde est appropriée, suivez les instructions suivantes :  
  
-   Utilisez la dernière sauvegarde disponible, et si elle est plus ancienne que la période de rétention de distribution maximale, créez une nouvelle sauvegarde avant de tenter d'initialiser un abonnement avec une sauvegarde. Pour plus d'informations sur la période de rétention, consultez [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
-   Par défaut, les travaux de nettoyage de la distribution effacent les transactions de plus de 72 heures de la base de données de distribution. Le nettoyage est basé sur la période de rétention définie pour la publication. Lors de la synchronisation avec d'anciennes sauvegardes, envisagez de désactiver temporairement le travail avant la sauvegarde que vous souhaitez restaurer, puis de le réactiver une fois l'abonnement créé avec succès. Cela évite la suppression de transactions de la base de données de distribution pouvant s'avérer nécessaires à la réussite de la synchronisation à partir de la sauvegarde. Pour obtenir des informations sur l’exécution des travaux de maintenance, consultez [Exécuter des travaux de maintenance de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md).  
  
 Dans certains cas, vous devez effectuer manuellement des personnalisations dans la base de données restaurée de l'Abonné après avoir configuré des abonnements initialisés par une sauvegarde. En général, les modifications manuelles sur la base de données restaurée de l'Abonné sont nécessaires, si la publication est définie de telle sorte que le contenu de la base de données de l'Abonné est supposé être différent du contenu de la base de données du serveur de publication.  
  
-   Les vues indexées sur la base de données restaurée doivent être converties en tables si elles sont publiées en tant qu'articles de la vue indexée sur la table basés sur un journal  
  
-   Les colonnes de type timestamp abonnées à la base de données restaurée doivent être converties en colonnes **binary(8)** : copiez le contenu des tables contenant les colonnes timestamp vers de nouvelles tables avec des schémas correspondants, sauf pour les colonnes timestamp qui sont remplacées par les colonnes **binary(8)** , supprimez les tables d'origine, et renommez les nouvelles tables avec les mêmes noms que les tables d'origine.  
  
## <a name="initializing-a-subscription-with-an-alternative-method"></a>Initialisation d'un abonnement avec une méthode de remplacement  
 Il est possible d'initialiser un abonnement à l'aide de n'importe quelle méthode vous autorisant à copier le schéma de la base de données de publication et les données sur l'Abonné, comme [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Lorsque vous utilisez une méthode de remplacement pour initialiser l'Abonné, les objets de prise en charge de la réplication sont copiés sur l'Abonné.  
  
 Contrairement à l'initialisation avec une sauvegarde, vous, ou votre application, devez vous assurer que les données et le schéma sont correctement synchronisés au moment d'ajouter l'abonnement. Si par exemple il y a une activité sur le serveur de publication entre le moment où les données et le schéma sont copiés vers l'Abonné et le moment auquel l'abonnement est ajouté, les modifications résultant de cette activité peuvent ne pas être répliquées vers l'Abonné.  
  
 Pour initialiser un abonnement avec une méthode de remplacement, consultez [Initialize a Subscription Manually](../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Initialiser un abonnement](../../relational-databases/replication/initialize-a-subscription.md)  
  
  
