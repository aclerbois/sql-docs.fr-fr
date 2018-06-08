---
title: Implémenter un outil personnalisé de résolution des conflits pour un article de fusion | Microsoft Docs
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
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], stored procedure-based resolvers
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 76bd8524-ebc1-4d80-b5a2-4169944d6ac0
caps.latest.revision: 45
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: d18ca8e63ca80671cd31a524b0c9878121a15e61
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32957524"
---
# <a name="implement-a-custom-conflict-resolver-for-a-merge-article"></a>Implémenter un outil personnalisé de résolution des conflits pour un article de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment implémenter l'outil personnalisé de résolution des conflits pour un article de fusion dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)] ou du [programme de résolution personnalisé COM](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-custom-resolvers.md).  
  
 **Dans cette rubrique**  
  
-   **Pour implémenter l'outil personnalisé de résolution des conflits pour un article de fusion à l'aide de :**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Programme de résolution s'appuyant sur l'architecture COM.](#COM)  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez écrire votre propre outil personnalisé de résolution des conflits sous forme de procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] au niveau de chaque serveur de publication. Au cours de la synchronisation, cette procédure stockée est appelée en cas de conflits dans un article pour lequel ce programme de résolution a été enregistré, et les informations sur la ligne en conflit sont passées par l'Agent de fusion aux paramètres requis de la procédure. Les outils personnalisés de résolution des conflits s'appuyant sur des procédures stockées sont toujours créés au niveau du serveur de publication.  
  
> [!NOTE]  
>  Les programmes de résolution des procédures stockées[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont appelés uniquement pour gérer les conflits de changement de ligne. Ils ne peuvent pas être utilisés pour gérer d'autres types de conflits, comme les échecs d'insertion en raison de violations de clés primaires ou de violations de contraintes d'index unique.  
  
#### <a name="to-create-a-stored-procedure-based-custom-conflict-resolver"></a>Pour créer un outil personnalisé de résolution des conflits s'appuyant sur des procédures stockées  
  
1.  Dans la base de données de publication ou la base de données **msdb** sur le serveur de publication, créez une procédure stockée système qui implémente les paramètres requis suivants :  
  
    |Paramètre|Type de données|Description|  
    |---------------|---------------|-----------------|  
    |**@tableowner**|**sysname**|Nom du propriétaire de la table pour laquelle un conflit est résolu. Il s'agit du propriétaire de la table dans la base de données de publication.|  
    |**@tablename**|**sysname**|Nom de la table pour laquelle un conflit est résolu.|  
    |**@rowguid**|**uniqueidentifier**|Identificateur unique de la ligne en conflit.|  
    |**@subscriber**|**sysname**|Nom du serveur depuis lequel une modification en conflit est propagée.|  
    |**@subscriber_db**|**sysname**|Nom de la base de données depuis laquelle la modification en conflit est propagée.|  
    |**@log_conflict OUTPUT**|**Int**|Indique si le processus de fusion doit enregistrer un conflit en vue de le résoudre ultérieurement :<br /><br /> **0** = ne pas enregistrer le conflit.<br /><br /> **1** = l'Abonné est le perdant du conflit.<br /><br /> **2** = le serveur de publication est le perdant du conflit.|  
    |**@conflict_message OUTPUT**|**nvarchar(512)**|Message accompagnant la résolution si le conflit est enregistré.|  
    |**@destowner**|**sysname**|Propriétaire de la table publiée créée sur l'Abonné.|  
  
     Cette procédure stockée utilise les valeurs passées par l'Agent de fusion à ces paramètres pour implémenter votre logique de résolution des conflits personnalisée. Elle doit retourner un jeu de résultats d'une seule ligne dont la structure est identique à la table de base et qui contient les valeurs de données pour la version gagnante de la ligne.  
  
2.  Accordez les autorisations EXECUTE sur la procédure stockée à toutes connexions utilisées par les Abonnés pour se connecter au serveur de publication.  
  
#### <a name="to-use-a-custom-conflict-resolver-with-a-new-table-article"></a>Pour utiliser un outil de résolution des conflits personnalisé avec un nouvel article de table  
  
1.  Exécutez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour définir un article, en affectant la valeur du **programme de résolution des procédures stockées Microsoft SQL** **Server** au paramètre **@article_resolver** et le nom de la procédure stockée qui implémente la logique de l'outil de résolution des conflits au paramètre **@resolver_info** . Pour plus d'informations, voir [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-custom-conflict-resolver-with-an-existing-table-article"></a>Pour utiliser un outil de résolution des conflits personnalisé avec un article de table existant  
  
1.  Exécutez [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), en spécifiant **@publication**, **@article**, en affectant la valeur **article_resolver** à **@property** et la valeur du **programme de résolution des procédures stockées Microsoft SQL** **Server** à **@value**.  
  
2.  Exécutez [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), en spécifiant **@publication**, **@article**, en affectant la valeur **resolver_info** à **@property**et en spécifiant le nom de la procédure stockée qui implémente la logique de l'outil de résolution des conflits pour **@value**.  
  
##  <a name="COM"></a> Programme de résolution personnalisé basé sur COM  
 L'espace de noms <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> implémente une interface qui vous permet d'écrire une logique métier complexe afin de gérer les événements et de résoudre les conflits qui se produisent au cours du processus de synchronisation de la réplication de fusion. Pour plus d'informations, voir [Implémenter un gestionnaire de logique métier pour un article de fusion](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md). Vous pouvez également écrire votre propre logique métier personnalisée en code natif pour résoudre ces conflits. Cette logique est construite sous la forme d'un composant COM et compilée dans des bibliothèques de liens dynamiques (DLL) à l'aide de produits tels que [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++. Un outil de résolution des conflits personnalisé basé sur COM doit implémenter l'interface **ICustomResolver** , qui est conçue spécifiquement pour la résolution des conflits.  
  
#### <a name="to-create-and-register-a-com-based-custom-conflict-resolver"></a>Pour créer et enregistrer un outil de résolution des conflits personnalisé  basé sur COM  
  
1.  Dans un environnement de création compatible COM, ajoutez des références à la bibliothèque de l'outil de résolution des conflits personnalisé.  
  
2.  Pour un projet Visual C++, utilisez la directive #import pour importer cette bibliothèque dans votre projet.  
  
3.  Créez une classe qui implémente l'interface **ICustomResolver** .  
  
4.  Implémentez certaines méthodes et propriétés.  
  
5.  Générez le projet de manière à créer le fichier bibliothèque de l'outil de résolution des conflits personnalisé.  
  
6.  Déployez la bibliothèque dans le répertoire contenant l'exécutable de l'agent de fusion (généralement \Microsoft SQL Server\100\COM).  
  
    > [!NOTE]  
    >  Un outil de résolution des conflits personnalisé doit être déployé sur l'Abonné pour un abonnement par extraction, sur le serveur de distribution pour un abonnement par émission de données ou sur le serveur Web utilisé avec la synchronisation Web.  
  
7.  Enregistrez la bibliothèque de l'outil de résolution des conflits personnalisé à l'aide de regsvr32.exe à partir du répertoire de déploiement, comme suit :  
  
    ```  
    regsvr32.exe mycustomresolver.dll  
    ```  
  
8.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) pour vérifier que la bibliothèque n’est pas enregistrée en tant qu’outil de résolution des conflits personnalisé.  
  
9. Pour enregistrer la bibliothèque en tant qu’outil de résolution des conflits personnalisé, exécutez [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md) au niveau du serveur de distribution. Spécifiez le nom convivial de l'objet COM pour **@article_resolver**, l'ID de la bibliothèque (CLSID) pour **@resolver_clsid**et la valeur du **false** à **@is_dotnet_assembly**.  
  
    > [!NOTE]  
    >  Quand vous n’en avez plus besoin, vous pouvez annuler l’enregistrement d’un outil de résolution des conflits personnalisé à l’aide de [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md).  
  
10. (Facultatif) Sur un cluster, répétez les étapes 5 à 8 pour enregistrer le programme de résolution personnalisé sur tous les nœuds du cluster. Cette procédure est nécessaire pour garantir que le programme de résolution personnalisé sera en mesure de charger correctement le réconciliateur suite à un basculement.  
  
#### <a name="to-use-a-custom-conflict-resolver-with-a-new-table-article"></a>Pour utiliser un outil de résolution des conflits personnalisé avec un nouvel article de table  
  
1.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) et notez le nom convivial du programme de résolution souhaité.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour définir un article. Spécifiez le nom convivial du programme de résolution d'articles obtenu à l'étape 1 pour **@article_resolver**. Pour plus d'informations, voir [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-custom-conflict-resolver-with-an-existing-table-article"></a>Pour utiliser un outil de résolution des conflits personnalisé avec un article de table existant  
  
1.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) et notez le nom convivial du programme de résolution souhaité.  
  
2.  Exécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), en spécifiant **@publication**, **@article**, en affectant la valeur **article_resolver** à **@property** et le nom convivial du programme de résolution d’articles obtenu à l’étape 1 à **@value**.  
  

## <a name="see-also"></a> Voir aussi  
 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [COM-Based Custom Resolvers](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-custom-resolvers.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
