---
title: Gérer une instance CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- manIns
ms.assetid: cfed22c8-c666-40ca-9e73-24d93e85ba92
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2047ee106142d707f8747e15bcabffe537b2d7a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-a-cdc-instance"></a>Gérer une instance de capture de données modifiées
  Vous pouvez utiliser la console du concepteur CDC pour afficher des informations sur les instances que vous créez et gérer le fonctionnement des instances.  
  
 Cliquez sur le nom d'une instance dans le volet gauche pour afficher les informations sur l'instance.  
  
> [!NOTE]  
>  Si vous sélectionnez un service dans le volet gauche, la liste des instances disponibles s'affiche également au centre de la console du concepteur CDC. Si vous sélectionnez l'une des instances dans cette section, vous pouvez effectuer les tâches indiquées dans le volet droit ; toutefois, vous ne pourrez pas afficher les informations dans les onglets de propriétés.  
  
## <a name="what-you-can-do-when-you-display-the-cdc-instance-information"></a>Opérations réalisables lorsque vous affichez les informations d'instance CDC  
 Les actions suivantes s'effectuent à partir du volet droit :  
  
 **Démarrer**  
 Cliquez sur **Démarrer** pour démarrer la capture des modifications pour l’instance de capture des données modifiées sélectionnée.  
  
 **Arrêter**  
 Cliquez sur **Arrêter** pour arrêter de capturer des modifications pour l’instance sélectionnée de capture de données modifiées. Lorsque vous arrêtez l'instance de capture de données modifiées, les modifications qui ont été capturées à ce stade ne sont pas perdues et sont remises lorsque l'instance de capture de données modifiées reprend.  
  
 **Réinitialiser**  
 Cliquez sur **Réinitialiser** pour rétablir l’état (vide) initial de l’instance de capture de données modifiées. Cette option est disponible lorsque l'instance de capture de données modifiées est arrêtée. Toutes les modifications des tables de modifications et de l'état interne de l'instance de capture de données modifiées sont supprimées. Lorsque l'instance de capture de données modifiées est démarrée ultérieurement, la capture de modifications démarre à partir de ce point et inclut uniquement les transactions qui ont démarré après que l'instance de capture de données modifiées a démarré.  
  
 Cliquez sur **OK** dans la boîte de dialogue de confirmation pour confirmer que vous voulez réinitialiser l'instance de capture de données modifiées et supprimer les modifications écrites dans les tables de modifications.  
  
 **Supprimer**  
 Cliquez sur **Supprimer** pour supprimer de façon permanente l’instance de capture des données modifiées. Cette option n'est disponible que lorsque l'instance de capture de données modifiées est arrêtée.  
  
 Cliquez sur **OK** dans la boîte de dialogue de confirmation pour confirmer que vous souhaitez supprimer l'instance de capture de données modifiées.  
  
 **Script de journalisation Oracle**  
 Cliquez sur ce lien pour afficher la boîte de dialogue du script de journalisation Oracle avec le script de journalisation supplémentaire Oracle. Pour plus d'informations sur les opérations réalisables dans cette boîte de dialogue, consultez [Oracle Supplemental Logging Script](../../integration-services/change-data-capture/oracle-supplemental-logging-script.md).  
  
> [!NOTE]  
>  Lorsque vous exécutez des scripts de journalisation supplémentaires, la boîte de dialogue des informations d'identification Oracle pour l'exécution de script s'ouvre et vous permet de spécifier un nom d'utilisateur et un mot de passe Oracle valides. Pour plus d'informations sur la façon de fournir les informations d'identification Oracle appropriées, consultez [Oracle Credentials for Running Script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md).  
  
 **Script de déploiement d'instance CDC**  
 Cliquez sur ce lien pour afficher la boîte de dialogue du script de déploiement d'instance de capture de données modifiées qui affiche le script de déploiement d'instance de capture de données modifiées. Pour plus d'informations sur cette boîte de dialogue, consultez [CDC Instance Deployment Script](../../integration-services/change-data-capture/cdc-instance-deployment-script.md).  
  
 **Propriétés**  
 Cliquez sur ce lien pour ouvrir l'éditeur de propriétés. Vous modifiez la configuration de l'instance de capture de données modifiées à l'aide de l'éditeur de propriétés. Pour plus d'informations sur la modification des propriétés pour une instance de capture de données modifiées, consultez [Edit Instance Properties](../../integration-services/change-data-capture/edit-instance-properties.md).  
  
 **Onglets de la visionneuse**  
  
 Les onglets suivants de la visionneuse sont disponibles lorsque vous affichez des informations pour l'instance de capture de données modifiées. Les informations contenues dans ces onglets sont en lecture seule.  
  
 **État**  
 Cet onglet fournit des informations et des statistiques sur l'état actuel de l'instance CDC. Il contient les informations suivantes :  
  
-   **État**: icône indiquant l'état actuel de l'instance de capture de données modifiées. La section suivante décrit les états.  
  
    |||  
    |-|-|  
    |![Erreur](../../integration-services/change-data-capture/media/error.gif "Erreur")|**Erreur**. L'instance Oracle CDC n'est pas en cours d'exécution, car une erreur non renouvelable s'est produite. Les sous-états suivants sont disponibles :<br /><br /> **Misconfigured**: une erreur de configuration qui nécessite une intervention manuelle s'est produite.<br /><br /> **Password Required**: aucun mot de passe n'est défini pour l'instance Oracle CDC ou le mot de passe n'est pas valide.<br /><br /> **Unexpected**. Toutes les autres erreurs non récupérables.|  
    |![OK](../../integration-services/change-data-capture/media/okay.gif "OK")|**Running**: l'instance de capture de données modifiées s'exécute et traite les enregistrements de modification. Les sous-états suivants sont disponibles :<br /><br /> **Idle**: tous les enregistrements de modification ont été traités et stockés dans les tables de modifications cibles. Il n'y a plus de transactions actives.<br /><br /> **Processing**: il existe des enregistrements de modification en cours de traitement qui ne sont pas encore écrits dans les tables de modifications.|  
    |![Arrêter](../../integration-services/change-data-capture/media/stop.gif "Arrêter")|**Stopped**: l'instance de capture de données modifiées n'est pas en cours d'exécution. L'état Stopped indique que l'instance de capture de données modifiées a été arrêtée de manière régulière.|  
    |![Paused](../../integration-services/change-data-capture/media/paused.gif "Paused")|**Paused**: l'instance de capture de données modifiées s'exécute mais le traitement est interrompu en raison d'une erreur renouvelable. Les sous-états suivants sont disponibles :<br /><br /> **Disconnected**: il est impossible d'établir la connexion à la base de données Oracle source. Le traitement reprend lorsque la connexion est restaurée.<br /><br /> **Storage**: le stockage est saturé. Le traitement reprend lorsque le stockage supplémentaire est disponible.<br /><br /> **Logger**: le journal est connecté à Oracle mais ne peut pas lire les journaux des transactions Oracle en raison d'un problème temporaire, par exemple, un journal des transactions n'est pas disponible.|  
  
-   **Detailed Status**: le sous-statut actuel.  
  
-   **Status Message**: plus d'informations sur l'état actuel.  
  
-   **Timestamp**: heure UTC indiquant la dernière lecture de l'état de capture de données modifiées dans la table d'état.  
  
-   **Currently Processing**: vous surveillez les informations suivantes dans cette section.  
  
    -   **Last transaction timestamp**: heure locale de la dernière transaction écrite dans les tables de modifications.  
  
    -   **Last change timestamp**: heure locale de la modification la plus récente détectée par l'instance Oracle CDC dans les journaux des transactions de la base de données Oracle source. Cela fournit des informations sur la latence actuelle de l'instance de capture de données modifiées lors de la lecture du journal des transactions Oracle.  
  
    -   **Transaction log head CN**: numéro de modification le plus récent (CN) qui a été lu dans le journal des transactions Oracle.  
  
    -   **Transaction log tail CN**: numéro de modification pour la récupération ou le redémarrage de l'instance de capture de données modifiées. L'instance Oracle CDC se replacera à cet emplacement en cas de redémarrage ou de tout autre type d'échec (notamment basculement de cluster).  
  
    -   **Current CN**: dernier numéro de modification (SCN) trouvé dans la base de données Oracle source (pas dans le journal des transactions).  
  
    -   **Active transactions**: nombre actuel de transactions Oracle sources qui sont traitées par l’instance Oracle CDC et pour lesquelles aucune décision n’est encore prise (validation/restauration).  
  
    -   **Staged transactions**: nombre actuel de transactions Oracle sources intermédiaires dans la table [cdc.xdbcdc_staged_transactions](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_staged_transactions) .  
  
-   **Counters**: vous surveillez les informations suivantes dans cette section.  
  
    -   **Completed transactions**: nombre de transactions terminées depuis la dernière réinitialisation de l'instance de capture de données modifiées. Cela n'inclut pas les transactions qui ne contiennent pas les tables d'intérêt.  
  
    -   **Written changes**: nombre de modifications écrites dans les tables de modifications SQL Server.  
  
 **Oracle**  
 Affiche des informations sur l'instance de capture de données modifiées et sa connexion à la base de données Oracle. Cet onglet est en lecture seule. Pour modifier ces propriétés, cliquez avec le bouton droit sur l’instance dans le volet gauche et sélectionnez **Propriétés** ou cliquez sur **Propriétés** dans le volet droit pour ouvrir la boîte de dialogue Propriétés de \<instance>.  
  
 Pour plus d'informations sur ces propriétés et la façon de les modifier, consultez [Edit the Oracle Database Properties](../../integration-services/change-data-capture/edit-the-oracle-database-properties.md).  
  
 **Tables**  
 Affiche des informations sur les tables incluses dans l'instance de capture de données modifiées. Les informations de colonne sont également disponibles ici. Cet onglet est en lecture seule. Pour modifier ces propriétés, cliquez avec le bouton droit sur l’instance dans le volet gauche et sélectionnez **Propriétés** ou cliquez sur **Propriétés** dans le volet droit pour ouvrir la boîte de dialogue Propriétés de \<instance>.  
  
 Pour plus d'informations sur ces propriétés et la façon de les modifier, consultez [Edit Tables](../../integration-services/change-data-capture/edit-tables.md).  
  
 **Avancé**  
 Affiche les propriétés avancées pour l'instance de capture de données modifiées et les valeurs de propriété. Cet onglet est en lecture seule. Pour modifier ces propriétés, cliquez avec le bouton droit sur l’instance dans le volet gauche et sélectionnez **Propriétés** ou cliquez sur **Propriétés** dans le volet droit pour ouvrir la boîte de dialogue Propriétés de \<instance>.  
  
 Pour plus d'informations sur ces propriétés et la façon de les modifier, consultez [Edit the Advanced Properties](../../integration-services/change-data-capture/edit-the-advanced-properties.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Procédure : créer l'instance SQL Server de base de données de modifications](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)   
 [Guide pratique pour afficher les propriétés d’une instance CDC](../../integration-services/change-data-capture/how-to-view-the-cdc-instance-properties.md)   
 [Procédure : modifier les propriétés d'une instance de capture de données modifiées](../../integration-services/change-data-capture/how-to-edit-the-cdc-instance-properties.md)   
 [Utiliser l’Assistant Nouvelle instance](../../integration-services/change-data-capture/use-the-new-instance-wizard.md)  
  
  
