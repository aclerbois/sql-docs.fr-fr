---
title: Restaurer des pages (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.restorepage.general.f1
helpviewer_keywords:
- restoring pages [SQL Server]
- pages [SQL Server], restoring
- databases [SQL Server], damaged
- page restores [SQL Server]
- pages [SQL Server], damaged
- restoring [SQL Server], pages
ms.assetid: 07e40950-384e-4d84-9ac5-84da6dd27a91
caps.latest.revision: 67
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ad3206ff6ccec7db89dcf745b4dd03066e076589
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="restore-pages-sql-server"></a>Restaurer des pages (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment restaurer des pages dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Dans la restauration de pages, l'objectif est de restaurer une ou plusieurs pages endommagées sans restaurer toute la base de données. Généralement, les pages candidates à la restauration ont été marquées « suspectes » en raison d'une erreur rencontrée lors de l'accès à la page. Les pages suspectes sont identifiées dans la table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) dans la base de données **msdb** .  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Quand une restauration de pages est-elle utile ?](#WhenUseful)  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour restaurer des pages, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="WhenUseful"></a> Quand une restauration de pages est-elle utile ?  
 La restauration de pages permet de réparer des pages endommagées. La restauration et la récupération de quelques pages individuelles peuvent être plus rapides qu'une restauration de fichiers, ce qui réduit le nombre de données qui restent hors ligne pendant l'opération. Toutefois, si vous avez besoin de restaurer un plus grand nombre de pages d'un fichier, il est généralement plus efficace de restaurer l'ensemble du fichier. Par exemple, si de nombreuses pages sur une unité indiquent une défaillance possible de l'unité, envisagez de restaurer le fichier éventuellement dans un emplacement différent et de réparer l'unité.  
  
 En outre, toutes les erreurs de page n'exigent pas une restauration. Il peut arriver qu'un problème survenant dans les données en cache, dans un index secondaire par exemple, ne puisse pas être résolu en recalculant les données. Par exemple, si l’administrateur de la base de données supprime un index secondaire et le reconstruit, les données endommagées, bien qu’ayant été corrigées, ne sont pas indiquées comme telles dans la table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) .  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   La restauration de pages s'applique aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui utilisent les modes de récupération complète ou de récupération utilisant les journaux de transactions. La restauration de pages n'est prise en charge que pour les groupes de fichiers en lecture-écriture.  
  
-   Seules les pages de bases de données peuvent être restaurées. La restauration de pages ne peut pas être utilisée pour restaurer les éléments suivants :  
  
    -   Journal des transactions  
  
    -   Pages d'allocation : pages GAM (Global Allocation Map), pages SGAM(Shared Global Allocation Map) et pages PFS (Page Free Space).  
  
    -   Page 0 de tous les fichiers de données (page de démarrage des fichiers)  
  
    -   Page 1:9 (page de démarrage de la base de données)  
  
    -   Catalogue de texte intégral  
  
-   La fonction de restauration de pages est soumise aux conditions supplémentaires suivantes lorsque la base de données utilise le mode de récupération utilisant les journaux de transactions.  
  
    -   La sauvegarde d'un groupe de fichiers ou de données de pages hors connexion pose problème au niveau des données journalisées en bloc étant donné que les données hors connexion ne sont pas enregistrées dans le journal. Une page hors connexion peut empêcher la sauvegarde du journal. Dans ce cas, envisagez l'utilisation de DBCC REPAIR car cette solution peut davantage minimiser la perte de données que la restauration de la sauvegarde la plus récente.  
  
    -   Si une sauvegarde de fichier journal d'une base de données journalisée en bloc rencontre une page endommagée, elle échoue à moins que WITH CONTINUE_AFTER_ERROR n'ait été spécifié.  
  
    -   La restauration de pages ne fonctionne généralement pas avec la récupération utilisant les journaux de transactions.  
  
         La méthode recommandée pour effectuer une restauration de pages consiste à affecter à la base de données le mode de restauration complète et à tenter une sauvegarde du journal. Si la sauvegarde du journal fonctionne, vous pouvez poursuivre la restauration de la page. Si elle échoue, soit vous perdez effectivement le travail effectué depuis la dernière sauvegarde du journal, soit vous tentez d'exécuter DBCC avec l'option REPAIR_ALLOW_DATA_LOSS.  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Scénarios de restauration de pages :  
  
     Restauration de pages hors connexion  
     Toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent en charge la restauration des pages lorsque la base de données est hors connexion. Dans une restauration de pages hors connexion, la base de données est hors connexion pendant que les pages endommagées sont restaurées. À la fin de la séquence de restauration, la base de données est mise en ligne.  
  
     Restauration de pages en ligne  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition prend en charge les restaurations de pages en ligne, bien qu’elle utilise la restauration hors ligne si la base de données est actuellement hors connexion. Dans la plupart des cas, une page endommagée peut être restaurée alors que la base de données reste en ligne, notamment le groupe de fichiers dans lequel une page est restaurée. Lorsque le groupe de fichiers primaire est en ligne, même si un ou plusieurs de ses groupes de fichiers secondaires sont hors connexion, les restaurations de pages sont habituellement effectuées en ligne. Cependant, une page endommagée peut parfois nécessiter une restauration hors connexion. Il arrive par exemple que l'état de certaines pages critiques empêche le démarrage de la base de données.  
  
    > [!WARNING]  
    >  Si les pages endommagées stockent des métadonnées de base de données critiques, les mises à jour obligatoires des métadonnées peuvent échouer lors d'une tentative de restauration de pages en ligne. Dans ce cas, vous pouvez effectuer une restauration de pages hors connexion, mais vous devez d’abord créer une [sauvegarde de la fin du fichier journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) (en sauvegardant le journal des transactions à l’aide de RESTORE WITH NORECOVERY).  
  
-   La restauration de pages bénéficie de la fonction améliorée de signalisation des erreurs au niveau de la page (avec sommes de contrôle) et du suivi. Les pages endommagées détectées par somme de contrôle ou une erreur d’écriture, *pages endommagées*, peuvent être restaurées par une opération de restauration de pages. Seules les pages spécifiées explicitement sont restaurées. Chaque page spécifiée est remplacée par la copie de cette page de la sauvegarde de données spécifiée.  
  
     Lorsque vous restaurez les sauvegardes de journaux suivantes, elles s'appliquent uniquement aux fichiers de base de données qui contiennent au moins une page récupérée. Si une chaîne ininterrompue de sauvegardes de journaux doit être appliquée à la dernière restauration complète ou différentielle pour restaurer par progression la page jusqu'au fichier journal actuel. Comme dans le cas d'une restauration de fichiers, le jeu de restauration par progression est une opération avancée qui s'effectue en une seule passe. Pour qu'une restauration de pages aboutisse, les pages restaurées doivent être récupérées dans un état cohérent avec la base de données.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas quand RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 À compter de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] prend en charge les restaurations de pages.  
  
#### <a name="to-restore-pages"></a>Pour restaurer des pages  
  
1.  Connectez-vous à l'instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], puis dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**. Selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système**et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, pointez sur **Restaurer**, puis cliquez sur **Page**, pour ouvrir la boîte de dialogue **Restaurer la page** .  
  
     **Restore**  
     Cette section effectue la même fonction que **Restaurer sur** dans la page [Restaurer la base de données (page Général)](../../relational-databases/backup-restore/restore-database-general-page.md).  
  
     **Sauvegarde de la base de données**  
     Spécifie la base de données à restaurer. Vous pouvez saisir le nom d'une nouvelle base de données ou en sélectionner une existante dans la liste déroulante. La liste comprend toutes les bases de données se trouvant sur le serveur, à l'exception des bases de données système **master** et **tempdb**.  
  
    > [!WARNING]  
    >  Pour restaurer une sauvegarde protégée par mot de passe, vous devez utiliser l’instruction [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) .  
  
     **Sauvegarde de la fin du journal**  
     Entrez ou sélectionnez un nom de fichier dans **Unité de sauvegarde** où la sauvegarde de la fin du journal sera stockée pour la base de données.  
  
     **Jeux de sauvegarde**  
     Cette section affiche les jeux de sauvegarde impliqués dans la restauration.  
  
    |En-tête|Valeurs|  
    |------------|------------|  
    |**Nom**|Nom du jeu de sauvegarde.|  
    |**Composant**|Composant sauvegardé : **Base de données**, **Fichier** ou **\<vide>** (pour les journaux des transactions).|  
    |**Type**|Type de sauvegarde effectué : **Complète**, **Différentielle**ou **Journal des transactions**.|  
    |**Server**|Nom de l'instance [!INCLUDE[ssDE](../../includes/ssde-md.md)] du moteur de base de données qui a effectué l'opération de sauvegarde.|  
    |**Sauvegarde de la base de données**|Nom de la base de données impliquée dans la sauvegarde.|  
    |**Position**|La position du jeu de sauvegarde dans le volume.|  
    |**Premier NSE**|Numéro séquentiel dans le journal (LSN) correspondant à la première transaction dans le jeu de sauvegarde. Vide pour les sauvegardes de fichiers.|  
    |**Dernier NSE**|Numéro séquentiel dans le journal (LSN) correspondant à la dernière transaction dans le jeu de sauvegarde. Vide pour les sauvegardes de fichiers.|  
    |**NSE du point de contrôle**|Numéro séquentiel dans le journal du point de contrôle le plus récent au moment où la sauvegarde a été créée.|  
    |**Tous les NSE**|Numéro séquentiel dans le journal (LSN) correspondant à la sauvegarde complète la plus récente de la base de données|  
    |**Date de début**|Date et heure de lancement de l'opération de sauvegarde, présentée conformément aux paramètres régionaux du client.|  
    |**Date de fin**|Date et heure de fin de l'opération de sauvegarde, exprimée d'après les paramètres régionaux du client.|  
    |**Taille**|Taille du jeu de sauvegarde, exprimée en octets.|  
    |**Nom d'utilisateur**|Nom de l'utilisateur qui a exécuté l'opération de sauvegarde.|  
    |**Expiration**|La date et l'heure d'expiration du jeu de sauvegarde.|  
  
     Cliquez sur **Vérifier** pour vérifier l'intégrité des fichiers de sauvegarde nécessaires pour effectuer l'opération de restauration de pages.  
  
4.  Pour identifier les pages endommagées, avec la base de données correcte sélectionnée dans la zone **Base de données** , cliquez sur **Vérifier les pages de la base de données**. Cette opération est longue.  
  
    > [!WARNING]  
    >  Pour restaurer les pages spécifiques qui ne sont pas endommagées, cliquez sur **Ajouter** et entrez l’ **ID de fichier** et l’ **ID de page** des pages à restaurer.  
  
5.  La grille de pages est utilisée pour identifier les pages à restaurer. Initialement, cette grille est remplie à partir de la table système [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) . Pour ajouter ou supprimer des pages de la grille, cliquez sur **Ajouter** ou **Supprimer**. Pour plus d’informations, consultez [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md).  
  
6.  La grille **Jeux de sauvegarde** répertorie les jeux de sauvegarde dans le plan de restauration par défaut. Éventuellement, cliquez sur **Vérifier** pour vérifier que les sauvegardes sont lisibles et que les jeux de sauvegarde sont complets, sans les restaurer. Pour plus d’informations, consultez [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
     **Pages**  
  
7.  Restaurer les pages répertoriées dans la grille de pages, cliquez sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Pour spécifier une page dans une instruction RESTORE DATABASE, vous avez besoin de l'ID du fichier qui contient la page et l'ID de la page. La syntaxe requise est la suivante :  
  
 `RESTORE DATABASE <database_name>`  
  
 `PAGE = '<file: page> [ ,... n ] ' [ ,... n ]`  
  
 `FROM <backup_device> [ ,... n ]`  
  
 `WITH NORECOVERY`  
  
 Pour plus d’informations sur les paramètres de l’option PAGE, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Pour plus d’informations sur la syntaxe de RESTORE DATABASE, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
#### <a name="to-restore-pages"></a>Pour restaurer des pages  
  
1.  Obtenez les ID des pages endommagées à restaurer. Une somme de contrôle ou une erreur d'écriture renvoie un ID de page fournissant les informations nécessaires pour la spécification des pages. Pour rechercher l'ID de page d'une page endommagée, utilisez une des sources suivantes.  
  
    |Source d'ID de page|Rubrique|  
    |-----------------------|-----------|  
    |**msdb..suspect_pages**|[Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)|  
    |Journal des erreurs|[Afficher le journal des erreurs SQL Server &#40;SQL Server Management Studio&#41;](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md)|  
    |Traces d'événements|[Surveiller et répondre aux événements](http://msdn.microsoft.com/library/f7fbe155-5b68-4777-bc71-a47637471f32)|  
    |DBCC|[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)|  
    |Fournisseur WMI|[Fournisseur WMI pour les concepts des événements de serveur](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)|  
  
2.  Démarrez une restauration de pages avec une sauvegarde complète de base de données, de fichier ou de groupe de fichiers contenant la page. Dans l'instruction RESTORE DATABASE, utilisez la clause PAGE pour énumérer les ID de toutes les pages à restaurer.  
  
3.  Appliquez les sauvegardes différentielles les plus récentes.  
  
4.  Appliquez les sauvegardes des journaux suivants.  
  
5.  Créez une nouvelle sauvegarde de journal de la base de données incluant le NSE final des pages restaurées, c'est-à-dire le point auquel la dernière page restaurée est placée en mode hors connexion. Le NSE final, qui est défini dans le cadre de la première restauration dans la séquence, est le NSE cible de restauration par progression. La restauration en ligne par progression du fichier contenant la page est capable de s'arrêter au NSE cible de restauration par progression. Pour connaître le NSE cible actuel de restauration d’un fichier, consultez la colonne **redo_target_lsn** de **sys.master_files**. Pour plus d’informations, consultez [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md).  
  
6.  Restaurez la nouvelle sauvegarde de fichier Une fois appliquée cette nouvelle sauvegarde de journal, la restauration des pages est terminée et les pages sont désormais utilisables.  
  
    > [!NOTE]  
    >  Cette séquence est analogue à une séquence de restauration de fichier. En fait, la restauration de pages et les restaurations de fichiers peuvent être effectuées dans le cadre de la même séquence.  
  
###  <a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple restaure quatre pages endommagées du fichier `B` à l'aide de `NORECOVERY`. Ensuite, deux sauvegardes de journal sont appliquées à l'aide de `NORECOVERY`, suivies de la sauvegarde de la fin du journal, qui est restaurée à l'aide de `RECOVERY`. Cet exemple effectue une restauration en ligne. Dans l'exemple, l'ID de fichier `B` est `1`, et les ID des pages endommagées sont `57`, `202`, `916`, et `1016`.  
  
```sql  
RESTORE DATABASE <database> PAGE='1:57, 1:202, 1:916, 1:1016'  
   FROM <file_backup_of_file_B>   
   WITH NORECOVERY;  
RESTORE LOG <database> FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG <database> FROM <log_backup>   
   WITH NORECOVERY;   
BACKUP LOG <database> TO <new_log_backup>;   
RESTORE LOG <database> FROM <new_log_backup> WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  
