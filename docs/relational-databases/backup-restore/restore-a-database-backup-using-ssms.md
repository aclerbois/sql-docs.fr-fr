---
title: Restaurer une sauvegarde de base de données à l’aide de SSMS | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.locatebackupfileazure.f1
- sql13.swb.specifybackup.f1
helpviewer_keywords:
- full backups [SQL Server]
- database restores [SQL Server], full backups
- backing up databases [SQL Server], full backups
- database backups [SQL Server], full backups
- restoring databases [SQL Server], full backups
ms.assetid: 24b3311d-5ce0-4581-9a05-5c7c726c7b21
caps.latest.revision: 79
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 01e55085d117541e2ecb8d8c4ee585afec205b03
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="restore-a-database-backup-using-ssms"></a>Restaurer une sauvegarde de base de données à l’aide de SSMS
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment restaurer une sauvegarde complète de base de données à l’aide de SQL Server Management Studio.    
       
### <a name="important"></a>Important !    
Pour pouvoir restaurer une base de données en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, vous devrez peut-être sauvegarder le journal des transactions actif (appelé [fin du journal](tail-log-backups-sql-server.md). Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  

Lors de la restauration d’une base de données d’une autre instance, tenez compte des informations de la page [Gérer les métadonnées lors de la mise à disposition d’une base de données sur une autre instance de serveur (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).   
    
Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique utilisé(e) pour chiffrer la base de données. Sans le certificat ou la clé asymétrique, vous ne pouvez pas restaurer cette base de données. Vous devez conserver le certificat utilisé pour chiffrer la clé de chiffrement de base de données tant que vous en avez besoin pour enregistrer la sauvegarde. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).    
    
Si vous restaurez une base de données de version antérieure vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], cette base de données est mise à niveau automatiquement vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].   
  
En général, la base de données est immédiatement disponible. Toutefois si une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] comprend des index de recherche en texte intégral, la mise à niveau les importe, les réinitialise ou les reconstruit, en fonction du paramètre de la propriété de serveur **Option de mise à niveau du catalogue de texte intégral** . Si vous affectez la valeur **Importer** ou **Reconstruire**à l’option de mise à niveau, les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. En fonction du volume de données indexé, l’importation peut prendre plusieurs heures et la reconstruction jusqu’à dix fois plus longtemps.     
    
Quand vous affectez la valeur **Importer**à l’option de mise à niveau, si un catalogue de texte intégral n’est pas disponible, les index de recherche en texte intégral associés sont reconstruits. Pour plus d’informations sur l’affichage ou la modification du paramètre de la propriété **Option de mise à niveau des index de recherche en texte intégral** , consultez [Gérer et surveiller la recherche en texte intégral pour une instance de serveur](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).    

Pour plus d’informations sur la restauration SQL Server à partir du service de stockage d’objets blob Microsoft Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="examples"></a>Exemples
    
### <a name="a-restore-a-full-database-backup"></a>**A. Restaurer une sauvegarde complète de base de données**    
    
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
    
2.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**.    
    
3.  Dans la page **Général** , utilisez la section **Source** pour préciser la source et l'emplacement des jeux de sauvegarde à restaurer. Sélectionnez l'une des options suivantes :    
    
    -   **Sauvegarde de la base de données**    
    
         Sélectionnez la base de données à restaurer dans la liste déroulante. La liste contient uniquement les bases de données qui ont été sauvegardées selon l'historique de sauvegarde **msdb** .    
    
    > **REMARQUE :** Si la sauvegarde est prise à partir d’un serveur différent, le serveur de destination ne disposera pas des informations d’historique de sauvegarde pour la base de données spécifiée. Dans ce cas, sélectionnez **Unité** pour spécifier manuellement le fichier ou l'unité à restaurer. 
    
    -   **Unité**    
    
         Cliquez sur le bouton Parcourir (**...**) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . 
         
        -   Boîte de dialogue**Sélectionner les unités de sauvegarde**   
        
            **Type de support de sauvegarde**  
         Sélectionnez un type de support dans la liste déroulante **Type de support de sauvegarde** .  Remarque : L’option **Bande** s’affiche uniquement si un lecteur de bande est connecté à l’ordinateur, et l’option **Unité de sauvegarde** seulement si au moins une unité de sauvegarde est connectée.

            **Ajouter**  
            En fonction du type de support sélectionné dans la liste déroulante **Support de sauvegarde** , quand vous cliquez sur **Ajouter** , l’une des boîtes de dialogue suivantes s’ouvre. (Si la liste dans la zone de liste **Support de sauvegarde** est pleine, le bouton **Ajouter** n’est pas disponible.)

            |Type de support|Boîte de dialogue|Description|    
            |----------------|----------------|-----------------|    
            |**Fichier**|**Localiser le fichier de sauvegarde**|Dans cette boîte de dialogue, vous pouvez sélectionner un fichier local depuis l'arborescence ou un fichier distant en utilisant son nom complet UNC (Universal Naming Convention). Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|    
            |**Unité**|**Sélectionner l'unité de sauvegarde**|Dans cette boîte de dialogue, vous pouvez effectuer une sélection à partir d'une liste d'unités logiques de sauvegarde définies sur l'instance de serveur.|    
            |**Bande**|**Sélectionner la bande de sauvegarde**|Dans cette boîte de dialogue, vous pouvez effectuer une sélection à partir d'une liste de lecteurs de bande physiquement connectés à l'ordinateur exécutant l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|    
            |**URL**|**Sélectionner un emplacement de fichier de sauvegarde**|Dans cette boîte de dialogue, vous pouvez sélectionner des informations d’identification SQL Server ou un conteneur de stockage Azure existant, ajouter un nouveau conteneur de stockage Azure avec une signature d’accès partagé, ou générer une signature d’accès partagé et des informations d’identification SQL Server pour un conteneur de stockage existant.  Voir aussi [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).|  
         
             **Supprimer**    
             Supprime un ou plusieurs fichiers, bandes ou unités logiques de sauvegarde sélectionnés.    
                 
             **Sommaire**    
            Affiche le contenu du support d'un fichier, d'une bande ou d'une unité logique de sauvegarde sélectionné(e).  Ce bouton peut ne pas fonctionner si le type de support est **URL**.  

             **Support de sauvegarde**   
             Indique le support sélectionné.
    
             Après avoir ajouté les unités souhaitées à la zone de liste **Support de sauvegarde** , cliquez sur **OK** pour revenir à la page **Général** .    
    
         Dans la zone de liste **Source : Unité : Base de données** , sélectionnez le nom de la base de données à restaurer.    
    
        > **REMARQUE :** Cette liste n’est disponible que quand **Unité** est sélectionné. Seules les bases de données qui ont des copies de sauvegarde sur l'unité sélectionnée seront disponibles.    
     
4.  Dans la section **Destination** , la zone **Base de données** est automatiquement renseignée avec le nom de la base de données à restaurer. Pour changer le nom de la base de données, entrez le nouveau nom dans la zone **Base de données** .    
    
5.  Dans la zone **Restaurer sur** , laissez la valeur par défaut **Vers la dernière sauvegarde prise** ou cliquez sur **Chronologie** pour accéder à la boîte de dialogue **Chronologie de sauvegarde** afin de sélectionner manuellement une limite spécifique pour arrêter l'action de récupération. Pour plus d’informations sur la façon de désigner une limite spécifique, consultez [Chronologie de sauvegarde](../../relational-databases/backup-restore/backup-timeline.md).    
    
6.  Dans la grille **Jeux de sauvegarde à restaurer** , sélectionnez les sauvegardes à restaurer. Cette grille affiche les sauvegardes disponibles pour l'emplacement spécifié. Par défaut, un plan de récupération est suggéré. Pour ignorer le plan de récupération suggéré, vous pouvez modifier les sélections dans la grille. Les sauvegardes qui dépendent de la restauration d'une sauvegarde antérieure sont automatiquement désélectionnées dès lors que la sauvegarde antérieure est désélectionnée. Pour plus d’informations sur les colonnes de la grille **Jeux de sauvegarde à restaurer** , consultez [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md).    
    
7.  Vous pouvez aussi cliquer sur **Fichiers** dans le volet **Sélectionner une page** pour accéder à la boîte de dialogue **Fichiers** . Vous pouvez alors restaurer la base de données vers un nouvel emplacement, en spécifiant une nouvelle destination de restauration pour chaque fichier dans la grille **Restaurer les fichiers de la base de données en tant que**. Pour plus d’informations sur cette grille, consultez [Restaurer la base de données &#40;page Fichiers&#41;](../../relational-databases/backup-restore/restore-database-files-page.md).    
    
8. Pour afficher ou sélectionner les options avancées, dans la page **Options**, dans le volet **Options de restauration**, vous pouvez choisir les options suivantes si elles s’appliquent à votre situation :    
    
    1.  Options**WITH** (non obligatoires) :    
    
        -   **Remplacer la base de données existante (WITH REPLACE)**    
    
        -   **Conserver les paramètres de la réplication (WITH KEEP_REPLICATION)**    
    
        -   **Restreindre l'accès à la base de données restaurée (WITH RESTRICTED_USER)**    
    
    2.  Sélectionnez une option pour la zone **État de récupération** . Cette zone détermine l'état de la base de données à l'issue de l'opération de restauration.    
    
        -   **RESTORE WITH RECOVERY** est le comportement par défaut qui laisse la base de données opérationnelle en annulant les transactions non validées. Les journaux des transactions supplémentaires ne peuvent pas être restaurés. Choisissez cette option si vous restaurez toutes les sauvegardes nécessaires maintenant.    
    
        -   **RESTORE WITH NORECOVERY** qui laisse la base de données non opérationnelle et n’annule pas les transactions non validées. Les journaux des transactions supplémentaires peuvent être restaurés. La base de données ne peut pas être utilisée tant qu'elle n'est pas récupérée.    
    
        -   **RESTORE WITH STANDBY** qui laisse la base de données en lecture seule. Elle annule les transactions non validées, mais enregistre les actions d'annulation dans un fichier afin de rendre réversibles les effets de la récupération.    
    
    3.  **Effectuez la sauvegarde de la fin du journal avant la restauration.** Les scénarios de restauration ne nécessitent pas tous une sauvegarde de la fin du journal.  Pour plus d’informations, consultez **Scénarios qui nécessitent une sauvegarde de la fin du journal** dans [Sauvegardes de la fin du journal (SQL Server).](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)
    
    4.  Les opérations de restauration peuvent échouer s'il existe des connexions actives à la base de données. Activez l'option **Fermer les connexions existantes** pour garantir que toutes les connexions actives entre [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et la base de données sont fermées. Cette case à cocher définit la base de données en mode mono-utilisateur avant d'effectuer les opérations de restauration, et définit la base de données en mode multi-utilisateur une fois l'opération terminée.    
    
    5.  Sélectionnez **Demander confirmation avant chaque restauration de sauvegarde** si vous souhaitez être invité entre chaque opération de restauration. Cela n'est généralement pas nécessaire à moins que la base de données ne soit volumineuse et que vous ne souhaitiez surveiller l'état de l'opération de restauration.    
    
     Pour plus d’informations sur ces options de restauration, consultez [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).    
    
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="b-restore-an-earlier-disk-backup-over-an-existing-database"></a>**B. Restaurer une sauvegarde sur disque antérieure sur une base de données existante**    
L’exemple suivant restaure une sauvegarde sur disque antérieure de `Sales` et remplace la base de données `Sales` existante.

1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
    
2.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**.  

3.  Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .

4.  Cliquez sur le bouton Parcourir (**...**) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . Cliquez sur **Ajouter** et accédez à votre sauvegarde.  Cliquez sur **OK** après avoir sélectionné vos fichiers de sauvegarde sur disque.

5.  Cliquez sur **OK** pour revenir à la page **Général** .

6.  Dans le volet **Sélectionner une page** , cliquez sur **Options** .

7.  Dans la section **Options de restauration** , sélectionnez **Remplacer la base de données existante (WITH REPLACE)**.

    > **REMARQUE :** Si vous ne sélectionnez pas cette option, le message d’erreur suivant risque de s’afficher : «  System.Data.SqlClient.SqlError : Le jeu de sauvegarde contient la sauvegarde d’une base de données qui n’est pas la base de données « `Sales` » existante. (Microsoft.SqlServer.SmoExtended) »

8.  Dans la section **Sauvegarde de la fin du journal** , décochez la case **Effectuer la sauvegarde de la fin du journal avant la restauration**.

    > **REMARQUE :** les scénarios de restauration ne nécessite pas tous une sauvegarde de la fin du journal. Vous n'avez pas besoin d'une sauvegarde de la fin du journal si le point de récupération est contenu dans une sauvegarde de journal antérieure. En outre, une sauvegarde de la fin du journal est inutile si vous déplacez ou remplacez (par écrasement) la base de données et ne souhaitez pas la restaurer à un point donné après la sauvegarde la plus récente.  Pour plus d’informations, consultez [Sauvegardes de la fin du journal (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).
Cette option n’est pas disponible pour les bases de données dans le modèle de récupération SIMPLE.

9.  Dans la section **Connexions au serveur** , sélectionnez **Fermer les connexions existantes à la base de données de destination**.

    > **REMARQUE :** Si vous ne sélectionnez pas cette option, le message d’erreur suivant risque de s’afficher : « System.Data.SqlClient.SqlError : Impossible d’obtenir l’accès exclusif car la base de données est en cours d’utilisation. (Microsoft.SqlServer.SmoExtended) »
    
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="c--restore-an-earlier-disk-backup-with-a-new-database-name-where-the-original-database-still-exists"></a>**C.  Restaurer une sauvegarde sur disque antérieure avec un nouveau nom de base de données et conserver la base de données d’origine**
L’exemple suivant restaure une sauvegarde sur disque antérieure de `Sales` et crée une base de données nommée `SalesTest`.  La base de données d’origine, `Sales`, existe toujours sur le serveur.

1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
    
2.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**.  

3.  Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .

4.  Cliquez sur le bouton Parcourir (**...**) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . Cliquez sur **Ajouter** et accédez à votre sauvegarde.  Cliquez sur **OK** après avoir sélectionné vos fichiers de sauvegarde sur disque.

5.  Cliquez sur **OK** pour revenir à la page **Général** .

6.  Dans la section **Destination** , la zone **Base de données** est automatiquement renseignée avec le nom de la base de données à restaurer.  Pour changer le nom de la base de données, entrez le nouveau nom dans la zone **Base de données** .

7.  Dans le volet **Sélectionner une page** , cliquez sur **Options** .

8.  Dans la section **Sauvegarde de la fin du journal** , décochez la case**Effectuer la sauvegarde de la fin du journal avant la restauration**.

    > **IMPORTANT** Si vous ne sélectionnez pas cette option, la base de données existante ( `Sales`) bascule à l’état de restauration.

9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

    > **REMARQUE :** Si le message d’erreur suivant s’affiche : « System.Data.SqlClient.SqlError : La fin du journal pour la base de données « `Sales` » n’a pas été sauvegardée. Utilisez BACKUP LOG WITH NORECOVERY pour sauvegarder le journal s'il contient des travaux que vous ne voulez pas perdre. Utilisez la clause WITH REPLACE ou WITH STOPAT de l'instruction RESTORE pour remplacer simplement le contenu du journal. (Microsoft.SqlServer.SmoExtended) ».  
Cela signifie probablement que n’avez pas entré le nouveau nom de base de données à l’Étape 6 ci-dessus.  Généralement, la restauration empêche le remplacement accidentel d'une base de données par une autre.  Si la base de données nommée dans l'instruction RESTORE existe déjà sur le serveur actif et que le GUID de famille de la base de données spécifié ne correspond pas à celui qui est enregistré dans le jeu de sauvegarde, la base de données n'est pas restaurée. Cette mesure de sécurité est très importante,

### <a name="d--restore-earlier-disk-backups-to-a-point-in-time"></a>**D.  Restaurer des sauvegardes sur disque antérieures à une limite dans le temps**
L’exemple suivant restaure une base de données à l’état où elle se trouvait le 30 mai 2016 à 13:27:17, et décrit une opération de restauration impliquant plusieurs sauvegardes de fichiers journaux.  La base de données n’existe pas sur le serveur actuellement.

1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
    
2.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**.  

3.  Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .

4.  Cliquez sur le bouton Parcourir (**...**) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . Cliquez sur **Ajouter** et accédez à votre sauvegarde complète et à toutes les sauvegardes des journaux de transactions pertinentes.  Cliquez sur **OK** après avoir sélectionné vos fichiers de sauvegarde sur disque.

5.  Cliquez sur **OK** pour revenir à la page **Général** .

6.  Dans la section **Destination** , cliquez sur **Chronologie** pour accéder boîte de dialogue **Chronologie de sauvegarde** et sélectionner manuellement une limite dans le temps pour arrêter l’action de récupération.

7.  Sélectionnez **Date et heure spécifiques**.
8.  Sélectionnez **Heure** comme unité pour **Chronologie - Intervalle** dans la zone de liste déroulante (facultatif).
9.  Déplacez le curseur à l’heure souhaitée.

10. Cliquez sur **OK** pour revenir à la page Général.

11. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="e--restore-a-backup-from-the-microsoft-azure-storage-service"></a>**E.  Restaurer une sauvegarde à partir du service de stockage Microsoft Azure**
#### <a name="common-steps"></a>**Étapes courantes**
Les deux exemples ci-dessous effectuent une restauration de `Sales` à partir d’une sauvegarde située dans le service de stockage Microsoft Azure.  Le nom du compte de stockage est `mystorageaccount`.  Le conteneur se nomme `myfirstcontainer`.  Par souci de concision, les six premières étapes ne sont répertoriées ici qu’une seule fois et tous les exemples commencent à l’ **Étape 7**.
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**.

3.  Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .

4.  Cliquez sur le bouton Parcourir (...) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** .  
5.  Sélectionnez **URL** dans la liste déroulante **Type de support de sauvegarde** .

6.  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner un emplacement de fichier de sauvegarde** .

    #### <a name="e1---restore-a-striped-backup-over-an-existing-database-and-a-shared-access-signature-exists"></a>**E1.   Restaurer une sauvegarde distribuée sur une base de données existante quand il existe une signature d’accès partagé.**
    Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture, suppression et liste.  Une signature d’accès partagé associée à la stratégie d’accès stockée a été créée pour le conteneur `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.  Les étapes sont essentiellement les mêmes s’il existe déjà des informations d’identification SQL Server.  La base de données `Sales` existe actuellement sur le serveur.  Les fichiers de sauvegarde sont `Sales_stripe1of2_20160601.bak` et `Sales_stripe2of2_20160601.bak`.  
*  
    7.  Sélectionnez `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` dans la liste déroulante **Conteneur de stockage Azure** si les informations d’identification SQL Server existent déjà. Sinon, entrez manuellement le nom du conteneur, `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.
    
    8.  Entrez la signature d’accès partagé dans la zone de texte enrichi **Signature d’accès partagé** .
       9.   Cliquez sur **OK** pour ouvrir la boîte de dialogue **Localiser le fichier de sauvegarde dans Microsoft Azure** .
    10. Développez **Conteneurs** et accédez à `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.
    
    11. Maintenez la touche Ctrl enfoncée et sélectionnez les fichiers `Sales_stripe1of2_20160601.bak` et `Sales_stripe2of2_20160601.bak`.
    12. Cliquez sur **OK**.
    13. Cliquez sur **OK** pour revenir à la page **Général** .
    14. Dans le volet **Sélectionner une page** , cliquez sur **Options** .
    15. Dans la section **Options de restauration** , sélectionnez **Remplacer la base de données existante (WITH REPLACE)**.
    16. Dans la section **Sauvegarde de la fin du journal** , décochez la case **Effectuer la sauvegarde de la fin du journal avant la restauration**.
    17. Dans la section **Connexions au serveur** , sélectionnez **Fermer les connexions existantes à la base de données de destination**.
    18. Cliquez sur **OK**.

    #### <a name="e2---a-shared-access-signature-does-not-exist"></a>**E2.   Il n’existe aucune signature d’accès partagé**
    Dans cet exemple, la base de données `Sales` n’existe pas sur le serveur.
    7.  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Se connecter à un abonnement Microsoft** .  
    
    8.  Terminez la boîte de dialogue **Se connecter à un abonnement Microsoft** et cliquez sur **OK** pour revenir à la boîte de dialogue **Sélectionner un emplacement de fichier de sauvegarde** .  Pour plus d’informations, consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md) .
    9.  Cliquez sur **OK** dans la boîte de dialogue **Sélectionner un emplacement de fichier de sauvegarde** pour ouvrir la boîte de dialogue **Localiser le fichier de sauvegarde dans Microsoft Azure** .
    10. Développez **Conteneurs** et accédez à `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.
    11. Sélectionnez le fichier de sauvegarde, puis cliquez **OK**.
    12. Cliquez sur **OK** pour revenir à la page **Général** .
    13. Cliquez sur **OK**.

#### <a name="f---restore-local-backup-to-microsoft-azure-storage-url"></a>**F.   Restaurer une sauvegarde locale dans le stockage Microsoft Azure (URL)**
La base de données `Sales` sera restaurée dans le conteneur de stockage Microsoft Azure `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` à partir d’une sauvegarde située dans `E:\MSSQL\BAK`.  Les informations d’identification SQL Server pour le conteneur Azure ont déjà été créées.  Il doit déjà exister des informations d’identification SQL Server pour le conteneur de destination, car elles ne peuvent pas être créées par la tâche **Restaurer** .  La base de données `Sales` n’existe pas sur le serveur actuellement.
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**.
3.  Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .
4.  Cliquez sur le bouton Parcourir (...) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** .  
5.  Sélectionnez **Fichier** dans la liste déroulante **Type de support de sauvegarde** .
6.  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Localiser le fichier de sauvegarde** .
7.  Accédez à `E:\MSSQL\BAK`, sélectionnez le fichier de sauvegarde, puis cliquez sur **OK**.
8.  Cliquez sur **OK** pour revenir à la page **Général** .
9.  Dans le volet **Sélectionnez** une page, cliquez sur **Fichiers** .
10. Cochez la case **Déplacer tous les fichiers dans le dossier**.
11. Entrez le conteneur, `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`, dans les zones de texte pour **Dossier des fichiers de données** et **Dossier des fichiers journaux**.
12. Cliquez sur **OK**.


## <a name="see-also"></a> Voir aussi    
 [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)     
 [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)     
 [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)     
 [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)     
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)     
 [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)     
 [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)    
    
  
