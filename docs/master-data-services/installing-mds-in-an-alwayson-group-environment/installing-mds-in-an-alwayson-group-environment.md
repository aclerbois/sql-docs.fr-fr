---
title: Haute disponibilité et récupération d’urgence pour Master Data Services | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2017
ms.prod: sql
ms.prod_service: mds
ms.component: installing-mds-in-an-alwayson-group-environment
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 7e3bd1f050b1f5652a12e74066345e8d20e8286f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="high-availability-and-disaster-recovery-for-master-data-services"></a>Haute disponibilité et récupération d’urgence pour Master Data Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]


**Résumé :** Cet article décrit une solution pour Master Data Services (MDS) hébergé sur une configuration de groupe de disponibilité AlwaysOn. L’article décrit comment installer et configurer SQL 2016 Master Data Services sur un groupe de disponibilité SQL 2016 AlwaysOn. L’objectif principal de cette solution est d’améliorer la haute disponibilité et la récupération d’urgence des données du serveur principal MDS hébergées sur une base de données SQL Server.

## <a name="introduction"></a>Introduction


Cet article décrit une solution pour Master Data Services (MDS) hébergé sur une configuration de groupe de disponibilité AlwaysOn. L’article explique comment installer et configurer SQL 2016 MDS sur un groupe de disponibilité de SQL 2016 AlwaysOn. L’objectif principal de cette solution est d’améliorer la haute disponibilité et la récupération d’urgence des données du serveur principal MDS hébergées sur une base de données SQL Server.

Pour implémenter la solution, vous devez effectuer les tâches suivantes, abordées dans cet article.

1.  [Installer et configurer Windows Server Failover Cluster (WSFC)](#windows-server-failover-cluster-wsfc).

2.  [Configurer le groupe de disponibilité AlwaysOn](#sql-server-alwayson-availability-group).

3.  [Configurer MDS pour qu’il s’exécute sur un nœud WSFC](#configure-mds-to-run-on-an-wsfc-node).

Les sections ci-dessus vous présentent brièvement les technologies, suivies des instructions. Pour plus d’informations sur les technologies, consultez les documents auxquels renvoient les liens dans chaque section.

La solution décrite dans cet article s’appuie sur le groupe de disponibilité SQL Server AlwaysOn, dans lequel chaque base de données a plusieurs réplicas synchrones ou asynchrones. Un seul réplica accepte la transaction (accepte les demandes utilisateur). Il s’agit du réplica principal.

Chaque réplica ayant son propre stockage, il n’existe aucun stockage partagé centralisé dans cette solution. En cas de défaillance logicielle ou matérielle affectant le réplica principal, ce dernier peut être basculé vers un réplica synchrone ou asynchrone, automatiquement ou manuellement suivant la configuration et les circonstances. Cela garantit la haute disponibilité de la base de données avec une interruption minimale pour les utilisateurs.

Les réplicas asynchrones sont généralement hébergés sur un centre de données distant du centre de données du réplica principal. En cas d’urgence, le réplica principal peut être basculé vers un autre centre de données. Cela garantit la récupération d’urgence de la base de données.

À des fins de démonstration, la solution décrite dans cet article utilise les versions de logiciel suivantes. Les versions antérieures doivent fonctionner de la même manière, avec d’éventuelles différences mineures.

-   Windows Server 2012 R2 avec le cluster de basculement de serveur

-   SQL Server 2016 avec fonctionnalité Master Data Services

En outre, la solution utilise deux machines virtuelles, **MDS-HA1** et **MDS-HA2**, pour héberger les deux réplicas. Suivant les possibilités offertes par le groupe de disponibilité SQL Server AlwaysOn, la fonctionnalité MDS ne limite pas le nombre de réplicas que vous pouvez utiliser.

Cet article suppose que vous disposez des connaissances de base sur Windows Server, le cluster de basculement Windows Server, SQL Server AlwaysOn et SQL Server MDS.

## <a name="what-is-not-covered"></a>Sujets non couverts

Ce document ne couvre pas les sujets suivants :

-   Comment rendre IIS, le serveur web qui héberge l’interface utilisateur du service Master Data Services, hautement disponible et récupérable après un incident. MDS n’imposant pas d’exigences particulières à IIS, les techniques standards pour rendre IIS hautement disponible et l’équilibrage de charge peuvent fonctionner ici également.

-   Comment utiliser un cluster de basculement SQL Server AlwaysOn pour prendre en charge la haute disponibilité sur le backend MDS. Le clustering de basculement SQL Server est une solution de haute disponibilité différente, officiellement prise en charge par SQL Server, qui ne fonctionne pas avec MDS.

-   Comment utiliser une solution hybride combinant le cluster de basculement SQL Server et le groupe de disponibilité AlwaysOn pour prendre en charge la haute disponibilité sur le backend MDS. La solution hybride ne fonctionne pas avec MDS.

## <a name="design-consideration"></a>Remarque sur la conception

La figure 1 montre une configuration classique utilisée principalement dans le groupe de disponibilité AlwaysOn. Dans le centre de données principal, il existe deux réplicas liés par une relation de validation synchrone, et les deux réplicas disposent du privilège VOTE. Cette option sert principalement à améliorer la haute disponibilité si le réplica principal échoue.

Dans le centre de données de récupération d’urgence, il existe un réplica secondaire lié au réplica principal par une relation de validation asynchrone. Ce centre de données se trouve généralement dans une région géographique différente de celle du centre de données principal. Le réplica secondaire ne dispose pas du privilège VOTE.

Cette configuration permet d’effectuer une récupération si le centre de données principal est victime d’un incident, tel qu’un incendie ou un tremblement de terre. La configuration autorise à la fois la haute disponibilité et la récupération d’urgence à relativement peu de frais.

![Configuration classique du groupe de disponibilité AlwaysOn](media/Fig1_TypicalConfig.png)

Figure 1. Configuration classique du groupe de disponibilité AlwaysOn

Si vous n’avez pas besoin d’envisager une récupération d’urgence, un réplica dans un second centre de données est superflu. Si vous devez améliorer la haute disponibilité, vous pouvez avoir davantage de réplicas synchrones dans le même centre de données principal.

Il est donc important de tenir compte des scénarios et des impératifs, et de choisir le nombre de réplicas synchrones et asynchrones nécessaires, ainsi que le centre de données devant les accueillir.

## <a name="windows-server-failover-cluster-wsfc"></a>Cluster de basculement Windows Server (WSFC)

Cette section couvre les tâches suivantes.

1.  [Installer la fonctionnalité Cluster de basculement Windows](#install-failover-cluster-feature).

2.  [Créer un cluster de basculement Windows Server](#create-a-windows-server-failover-cluster).

Comme l’indique la figure 1 de la section précédente, la solution décrite dans cet article inclut le cluster de basculement Windows Server (WSFC). Nous devons configurer le cluster WSFC, car SQL AlwaysOn en dépend pour la détection des défaillances et le basculement.

Le cluster WSFC est une fonctionnalité destinée à améliorer la haute disponibilité des applications et des services. Il se compose d’un groupe d’instances de Windows Server indépendantes sur lesquelles s’exécute le service de cluster de basculement Microsoft. Les instances de Windows Server (parfois appelées « nœuds ») sont connectées afin qu’elles puissent communiquer entre elles, rendant ainsi possible la détection de défaillance. Le cluster WSFC fournit des fonctionnalités de détection de défaillance et de basculement. Si un nœud ou un service échoue dans le cluster, la défaillance est détectée, puis un autre nœud commence automatiquement ou manuellement à fournir les services hébergés sur le nœud défaillant. Ainsi, les utilisateurs ne connaissent que des interruptions minimales des services, dont la disponibilité se trouve améliorée.  

### <a name="prerequisites"></a>Conditions préalables requises

Le système d’exploitation Windows Server est installé sur toutes les instances et toutes les mises à jour sont corrigées.

>[!NOTE] 
>Nous vous **recommandons vivement** d’installer la même version de Windows et le même jeu de fonctionnalités sur toutes les instances afin d’éviter tout problème d’incompatibilité éventuel.

### <a name="install-failover-cluster-feature"></a>Installer la fonctionnalité Cluster de basculement

Effectuez les étapes suivantes pour chaque instance de Windows Server afin d’y installer la fonctionnalité WSFC. Vous avez besoin d’autorisations d’administrateur.

1.  Ouvrez le **Gestionnaire de serveur** dans Windows Server, puis cliquez sur **Ajouter des rôles et des fonctionnalités** dans le volet de droite. Cette action lance l’**Assistant Ajout de rôles et de fonctionnalités**.

2.  Cliquez sur **Suivant** jusqu’à ce que vous atteigniez la page **Fonctionnalités**.

3.  Cochez la case **Clustering de basculement**, puis cliquez sur **Suivant** pour terminer l’installation. Voir figure 2.

    Si vous êtes invité à confirmer l’**ajout de fonctionnalités qui sont requises pour le clustering de basculement**, cliquez sur **Ajouter des fonctionnalités**. Voir figure 3.

    ![Assistant Ajout de rôles et de fonctionnalités, clustering de basculement](media/Fig2_SelectFeatures.png)

    Figure 2

    ![Assistant Ajout de rôles et fonctionnalités, nécessaires pour le cluster de basculement](media/Fig3_RequiredFeaturesFailover.png)

    Figure 3

4.  Dans la page **Confirmation**, cliquez sur **Installer** pour installer la fonctionnalité de clustering avec basculement.

5.  Dans la page **Résultat**, vérifiez que tout a été installé correctement sans erreurs ni avertissements.

### <a name="create-a-windows-server-failover-cluster"></a>Créer un cluster de basculement Windows Server

Une fois la fonctionnalité WSFC installée sur toutes les instances, vous pouvez configurer le cluster WSFC. Vous ne devez effectuer cette opération que sur un seul nœud.

1.  Ouvrez le **Gestionnaire de serveur** dans Windows Server, puis cliquez sur **Gestionnaire du cluster de basculement** dans le menu **Outil** en haut à droite pour lancer le gestionnaire.

2.  Dans **Gestionnaire du cluster de basculement**, cliquez sur **Valider la configuration** dans le volet de droite. Voir figure 4.

    ![Gestionnaire du cluster de basculement, valider la configuration](media/Fig4_ValidateConfig.png)

    Figure 4

3.  Dans l’**Assistant** **Validation d’une configuration**, cliquez sur **Suivant**.

4.  Dans la boîte de dialogue **Sélectionner des serveurs ou un cluster**, ajoutez les noms des serveurs destinés à héberger SQL Server, puis cliquez sur **Suivant**. Voir figure 5.

    Dans cet exemple, nous avons ajouté deux instances : MDS-HA1 et MDS-HA2.

    ![Assistant Validation d’une configuration, page Sélectionner des serveurs ou un cluster](media/Fig5_AddServer.png)

    Figure 5

5.  Dans la page **Options de test**, cliquez sur **Exécuter tous les tests**, puis cliquez sur **Suivant**.

6.  Cliquez sur **Suivant** pour terminer la validation.

    La page **Validation** affiche la progression, tandis que la page **Résumé** affiche un récapitulatif des validations (figures 6 et 7).

7.  Dans la page **Résumé**, recherchez les éventuels messages d’avertissement ou d’erreur.

    Vous devez corriger les erreurs. En revanche, les avertissements ne correspondent pas nécessairement à des problèmes. Un message d’avertissement signifie que « l’élément testé répond éventuellement aux exigences, mais que vous devez vérifier quelque chose ». Par exemple, la figure 7 présente un avertissement « Valider la latence de l’accès au disque », éventuellement lié au fait que le disque est temporairement occupé à d’autres tâches ; vous pouvez ignorer cet avertissement. Pour plus de détails, vous devez consulter la documentation en ligne de chaque avertissement et message d’erreur. Voir figure 7.
 
    ![Assistant Validation d’une configuration, page Validation](media/Fig6_ValidationTests.png)

    Figure 6

    ![Assistant Validation d’une configuration, page Résumé](media/Fig7_ValidationSummary.png)

    Figure 7

8.  Dans la page **Résumé**, vérifiez que la case **Créer le cluster maintenant en utilisant les nœuds validés** est cochée, puis cliquez sur **Terminer** pour démarrer l’**Assistant** **Création d’un cluster**.

9.  Dans l’**Assistant** **Création d’un cluster**, cliquez sur **Suivant**.

10. Dans la page **Point d’accès pour l’administration du cluster**, entrez le nom du cluster WSFC, puis cliquez sur **Suivant**. Dans cet exemple, nous utilisons « MDS-HA » comme nom de cluster. Voir figure 8.

    ![Entrer le nom du cluster](media/Fig8_EnterClusterName.png)

    Figure 8

11. Continuez à cliquer sur **Suivant** pour terminer la création du cluster. La section **Résumé du cluster MDS-HA** affiche les informations du cluster. Voir figure 9.

    ![Afficher les informations récapitulatives sur le cluster](media/Fig9_ClusterSummary.png)

    Figure 9

    Si, ultérieurement, vous devez ajouter un nœud, cliquez sur l’action **Ajouter un nœud** dans le volet de droite dans **Gestionnaire du cluster de basculement**.

Remarques :

-   La fonctionnalité WSFC n’est peut-être pas disponible dans toutes les éditions de Windows Server. Vérifiez que votre édition a cette fonctionnalité.

-   Vérifiez que vous disposez des autorisations appropriées pour configurer le cluster WSFC dans Active Directory. Pour plus d’informations, consultez [Failover Cluster Step-by-Step Guide: Configure Accounts in Active Directory](https://technet.microsoft.com/library/cc731002(v=ws.10).aspx) (Guide pas à pas des clusters de basculement : configurer des comptes dans Active Directory).

Pour plus d’informations sur le cluster WSFC, consultez [Failover Clusters](https://technet.microsoft.com/library/cc732488(v=ws.10).aspx) (Clusters de basculement).

## <a name="sql-server-alwayson-availability-group"></a>Groupe de disponibilité SQL Server AlwaysOn

Cette section couvre les tâches suivantes.

1.  [Activer le groupe de disponibilité SQL Server AlwaysOn](#enable-sql-server-alwayson-availability-group-on-every-sql-server-instance).

2.  [Créer un groupe de disponibilité](#create-an-availability-group).

3.  [Valider et tester le groupe de disponibilité](#validation-and-test-the-availability-group).

Les solutions SQL Server AlwaysOn fournissent la haute disponibilité et la récupération d’urgence pour les bases de données SQL Server. AlwaysOn offre deux solutions possibles.
Les deux s’appuient sur le cluster WSFC.

-   Groupes de disponibilité AlwaysOn

-   Instances de cluster de basculement AlwaysOn

Le groupe de disponibilité améliore la haute disponibilité de niveau base de données. Le groupe de disponibilité (ensemble de bases de données utilisateur) et le nom de son réseau virtuel sont enregistrés en tant que ressources dans le cluster WSFC.

Une instance de cluster de basculement améliore la haute disponibilité de niveau instance. Le service SQL Server et les services associés sont enregistrés en tant que ressources dans le cluster WSFC. De plus, la solution des instances de cluster de basculement requiert un stockage sur disque partagé symétrique, tel que des partages de fichiers SAN ou SMB, qui soit disponible pour tous les nœuds du cluster WFC.


### <a name="prerequisites"></a>Conditions préalables requises

-   Installez SQL Server sur tous les nœuds. Pour plus d’informations, consultez [Installer SQL Server 2016](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server).

-   (Recommandé) Installez exactement les mêmes jeu de fonctionnalités et version SQL Server sur chaque nœud. Vous devez notamment installer MDS.

-   (Recommandé) Utilisez la même configuration sur chaque instance de SQL Server. En particulier, vous devez configurer le même classement du serveur sur toutes les instances de SQL Server.

-   (Recommandé) Utilisez le même compte de service pour exécuter chaque instance de SQL Server. Sinon, vous devez accorder une autorisation sur chaque instance de SQL Server pour que les instances de SQL Server puissent communiquer entre elles.

-   Vérifiez que le paramétrage du pare-feu Windows autorise les instances de SQL Server à communiquer entre elles.

### <a name="enable-sql-server-alwayson-availability-group-on-every-sql-server-instance"></a>Activer le groupe de disponibilité SQL Server AlwaysOn sur chaque instance de SQL Server

1.  Dans le **Gestionnaire de configuration SQL Server**, cliquez sur **Service SQL Server** dans le volet de gauche, cliquez avec le bouton droit sur **SQL Server** dans le volet de droite, puis cliquez sur **Propriétés**. Voir figure 10.

    ![Fenêtre Propriétés de SQL Server](media/Fig10_SQLServerProperties.png)

    Figure 10

2.  Dans la boîte de dialogue **Propriétés de** **SQL Server (MSSQLSERVER)**, cliquez sur l’onglet **Haute disponibilité AlwaysOn**, puis cochez la case **Activer les groupes de disponibilité AlwaysOn**. Quand une valeur s’affiche dans la zone de texte **Nom du cluster de basculement Windows**, cliquez sur **OK** pour continuer. Voir figure 11.

    ![Option Activer les groupes de disponibilité AlwaysOn](media/Fig11_EnableAlwaysOn.png)

    Figure 11

3.  Quand une page d’avertissement s’affiche, cliquez sur **OK** pour continuer. Voir figure 12.

    ![Confirmer pour arrêter et redémarrer le service](media/Fig12_WarningServiceStopStart.png)

    Figure 12

4.  Cliquez sur **Redémarrer** pour redémarrer le service **SQL Server** afin d’appliquer ce changement. Voir figure 10.

>[!NOTE] 
>Vous pouvez changer le compte de service exécutant le service SQL Server en utilisant le **Gestionnaire de configuration SQL Server**. Cliquez sur l’onglet **Se connecter** dans la boîte de dialogue **Propriétés de** **SQL Server (MSSQLSERVER)**. Voir figure 11.

### <a name="create-an-availability-group"></a>Créer un groupe de disponibilité

Une fois la fonctionnalité AlwaysOn activée dans toutes les instances de SQL Server, vous créez un groupe de disponibilité qui contient la base de données MDS sur un nœud.

Un groupe de disponibilité ne peut être créé que sur des bases de données existantes. Ainsi, soit vous créez une base de données MDS sur un nœud, soit vous créez une base de données temporaire (que vous supprimez ensuite). Dans cet exemple, nous créons une base de données MDS vide, puis un groupe de disponibilité sur cette base de données.

1.  Lancez **SQL Server Management Studio** (**SSMS**) sur un nœud, puis connectez-vous à l’instance locale de SQL Server avec les informations d’identification appropriées.

2.  Dans SSMS, ouvrez une fenêtre de **nouvelle requête**, puis exécutez le script suivant pour créer une base de données vide. Remplacez C:\\temp par l’emplacement que vous souhaitez utiliser pour effectuer une sauvegarde complète.

    ```
    CREATE DATABASE MDS\_Sample
    GO
    BACKUP DATABASE MDS\_Sample TO DISK='C:\\temp'
    GO
    ```

    >[!NOTE] 
    >Une sauvegarde de base de données complète est nécessaire pour créer le groupe de disponibilité sur cette base de données.

3.  Dans l’**Explorateur d’objets**, développez le dossier **Haute disponibilité AlwaysOn**, puis cliquez sur **Assistant Nouveau groupe de disponibilité** pour lancer l’**Assistant Nouveau groupe de disponibilité**. Voir figure 13.

    ![Lancer l’Assistant Nouveau groupe de disponibilité](media/Fig13_AvailabilityGroupsFolder.png)

    Figure 13

4.  Dans l’**Assistant Nouveau groupe de disponibilité**, cliquez sur **Suivant** pour afficher la page **Spécifier le nom**. Tapez un nom pour le groupe de disponibilité, puis cliquez sur **Suivant**. Voir figure 14.

    ![Entrer le nom du groupe de disponibilité](media/Fig14_AvailabilityGroupName.png)

    Figure 14

5.  Dans la page **Sélectionner une base de données**, cliquez sur la base de données que vous venez de créer, puis cliquez sur **Suivant**. Voir figure 15.

    ![Sélectionner la base de données](media/Fig15_AvailabilityGroupSelectDatabase.png)

    Figure 15

6.  Dans la page **Spécifier les réplicas**, ajoutez un autre réplica en cliquant sur **Ajouter un réplica**. Cette page liste déjà les instances de SQL Server locales actuelles en tant que réplicas. Voir figure 16.

7.  Dans la boîte de dialogue **Se connecter au serveur**, ajoutez les informations d’identification appropriées, puis cliquez sur **Se connecter**.

    ![Se connecter à une instance de SQL Server](media/Fig16_AddReplicaConnectServer.png)

    Figure 16

    Vous devez maintenant voir deux réplicas dans la liste. Répétez cette étape pour ajouter d’autres nœuds en tant que réplicas. Voir figure 17.

    ![Afficher la liste des réplicas](media/Fig17_AvailabilityGroupSQLReplicas.png)

    Figure 17

    Pour chaque réplica, configurez les paramètres **Validation synchrone**, **Basculement automatique** et **Secondaire accessible en lecture**. Voir figure
17.

    **Validation synchrone** : si une transaction est validée sur le réplica principal d’une base de données, la transaction est également validée sur tous les autres réplicas synchrones. Ne pouvant pas garantir cela, la validation asynchrone risque d’être en retard par rapport au réplica principal.

    En règle générale, vous ne devez activer la validation synchrone que si les deux nœuds se trouvent dans le même centre de données. S’ils se trouvent dans des centres de données différents, la validation synchrone risque de ralentir les performances de la base de données.

    Si cette case n’est pas cochée, la validation asynchrone est utilisée.

    **Basculement automatique** : quand le réplica principal est arrêté et que le basculement automatique est activé, le groupe de disponibilité bascule automatiquement vers son réplica secondaire. Cette option ne peut être activée que sur les réplicas avec validations synchrones.

    **Secondaire accessible en lecture** : par défaut, les utilisateurs ne peuvent se connecter à aucun réplica secondaire. Cette option permet aux utilisateurs de se connecter au réplica secondaire avec un accès en lecture seule.

8.  Dans la page **Spécifier les réplicas**, cliquez sur l’onglet **Écouteur**, puis effectuez la procédure suivante. Voir figure 18.

    A.  Cliquez sur **Créer un écouteur de groupe de disponibilité** pour configurer un écouteur de groupe de disponibilité pour la connexion de base de données MDS.

    B.  Entrez un **Nom DNS de l’écouteur**, tel que MDSSQLServer.

    c.  Entrez le port SQL par défaut, 1433, dans la zone de texte **Port**.

    d.  Entrez DHCP dans la zone de texte **Mode réseau**, puis cliquez sur **Suivant** pour continuer.

    >[!NOTE] 
    >Si vous le souhaitez, vous pouvez choisir « Adresse IP statique » comme **Mode réseau**, puis entrer une adresse IP statique. Vous pouvez également entrer un port autre que 1433. 

    ![Configurer l’écouteur](media/Fig18_AvailabilityGroupCreateListener.png)

    Figure 18

9.  Dans la page **Sélectionner la synchronisation des données**, cliquez sur **Complète** et spécifiez un partage réseau auquel chaque nœud peut accéder. Cliquez sur **Suivant** pour continuer. Voir figure 19.

    Ce partage réseau est destiné à stocker la sauvegarde de base de données en vue de la création des réplicas secondaires. Si cette configuration n’est pas disponible pour votre organisation, choisissez un autre mode de synchronisation des données. Reportez-vous à [Groupes de disponibilité AlwaysOn SQL Server 2016](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) pour savoir comment utiliser d’autres options pour créer des réplicas secondaires. La figure 17 répertorie également les autres options.

    ![Configurer la synchronisation des données](media/Fig19_AvailabilityGroupDataSync.png)

    Figure 19 

10. Dans la page **Validation**, vérifiez que toutes les validations réussissent et corrigez les erreurs éventuelles. Cliquez sur **Suivant** pour continuer.

11. Dans la page **Résumé**, examinez les paramètres de configuration, puis cliquez sur **Terminer**. Le groupe de disponibilité est alors créé, puis configuré.

12. Dans la page **Résultat**, vérifiez que toutes les étapes nécessaires ont été effectuées.

### <a name="validation-and-test-the-availability-group"></a>Validation et test du groupe de disponibilité

1.  Ouvrez SSMS et connectez-vous au nom DNS de l’écouteur que vous venez de créer dans la section [Créer un groupe de disponibilité](#create-an-availability-group). Dans cet exemple, il s’agit de MDSSQLServer.

2.  Dans l’**Explorateur d’objets**, développez le dossier **Haute disponibilité AlwaysOn**, cliquez avec le bouton droit sur le groupe de disponibilité que vous venez de créer dans la section [Créer un groupe de disponibilité](#create-an-availability-group), puis cliquez sur **Afficher le tableau de bord**. Voir figure 20. L’état du nouveau groupe de disponibilité et de ses réplicas s’affiche.

    ![Afficher le tableau de bord](media/Fig20_ShowDashboard.png)

    Figure 20 

3.  Cliquez sur **Basculer** pour effectuer un basculement vers un réplica synchrone et un réplica asynchrone. Vous vérifiez ainsi que le basculement se déroule correctement.

 La configuration du groupe de disponibilité AlwaysOn est terminée.

Pour plus d’informations sur le groupe de disponibilité AlwaysOn, consultez [Groupes de disponibilité AlwaysOn SQL Server 2016](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server).

## <a name="configure-mds-to-run-on-an-wsfc-node"></a>Configurer MDS pour qu’il s’exécute sur un nœud WSFC

Dans le cadre de cet article, cette solution nécessite uniquement que la base de données principale MDS s’exécute sur le cluster WSFC. Les autres parties de MDS, telles que les applications web et le Gestionnaire de configuration de MDS, peuvent être exécutées sur le nœud dans le cluster WSFC ou en dehors de celui-ci, tant que MDS peut se connecter au groupe de disponibilité.

1.  Ouvrez le **Gestionnaire de configuration Master Data Services** sur un nœud, cliquez sur **Configuration de la base de données**, puis cliquez sur **Créer une base de données** pour lancer l’**Assistant Création d’une base de données**.

2.  Dans la page **Serveur de base de données**, tapez le nom DNS de l’écouteur de groupe de disponibilité dans la zone de texte **Instance SQL Server**, cliquez sur **Tester la connexion**, puis cliquez sur **Suivant**. Voir figure 21.

    ![Configurer le serveur de base de données avec l’écouteur de groupe de disponibilité](media/Fig21_MDSDatabaseServerListener.png)

    Figure 21

3.  Dans la page **Base de données**, tapez le nom de la base de données que vous avez créée dans la section [Créer un groupe de disponibilité](#create-an-availability-group), puis cliquez sur **Suivant**. Voir figure 22.

    ![Créer et configurer la base de données](media/Fig22_MDSCreateDatabase.png)

    Figure 22

4.  Exécutez l’**Assistant** **Création d’une base de données**. Pour plus d’informations, consultez [Installation et configuration de Master Data Services](https://docs.microsoft.com/sql/master-data-services/master-data-services-installation-and-configuration).

5.  Cliquez sur **Applications web** dans **Gestionnaire de configuration Master Data Services** pour configurer l’application web, puis cliquez sur **Appliquer** pour appliquer les paramètres à MDS. Voir figure 23. Pour plus d’informations, consultez [Installation et configuration de Master Data Services](https://docs.microsoft.com/sql/master-data-services/master-data-services-installation-and-configuration).

    ![Configurer l’application web](media/Fig23_MDSWebApplication.png)

    Figure 23

    La configuration de MDS est terminée. Vous pouvez répéter les étapes ci-dessus pour configurer MDS afin qu’il s’exécute sur tous les nœuds. La base de données principale est la même sur le même groupe de disponibilité.

6.  Si vous avez créé une base de données temporaire (voir la section [Créer un groupe de disponibilité](#create-an-availability-group)) pour créer un groupe de disponibilité AlwaysOn, vous devez la supprimer.

    Pour plus d’informations sur Master Data Services, consultez [Master Data Services](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds).

## <a name="conclusion"></a>Conclusion

Dans ce livre blanc, nous avons vu comment installer et configurer la base de données principale Master Data Services sur le groupe de disponibilité AlwaysOn SQL Server. Cette configuration offre la haute disponibilité et la récupération d’urgence sur la base de données principale Master Data Services. Pour implémenter cette configuration, vous devez installer et configurer le cluster de basculement Windows Server, un groupe de disponibilité SQL Server AlwaysOn et Master Data Services.

## <a name="feedback"></a>Commentaires

Ce document vous a-t-il été utile ? Envoyez-nous vos commentaires en cliquant sur **Commentaires** en haut de l’article. 

Vos commentaires nous aideront à améliorer la qualité des livres blancs que nous publions. 

