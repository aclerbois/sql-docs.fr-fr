---
title: Prise en charge de SQL Server Integration Services (SSIS) Scale Out pour la haute disponibilité | Microsoft Docs
ms.description: This article describes how to configure SSIS Scale Out for high availability
ms.custom: ''
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: scale-out
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
caps.latest.revision: 1
author: haoqian
ms.author: haoqian
manager: craigg
ms.openlocfilehash: 8cd79327b3733de9f7463f1d5f9d8f924b58a46b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="scale-out-support-for-high-availability"></a>Prise en charge de Scale Out pour la haute disponibilité

Dans SSIS Scale Out, la haute disponibilité côté Scale Out Worker est fournie par l’exécution de packages avec plusieurs Scale Out Workers.

La haute disponibilité côté Scale Out Master est obtenue avec la fonctionnalité [Always On pour le catalogue SSIS](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) et le cluster de basculement Windows. Dans cette solution, plusieurs instances de Scale Out Master sont hébergées dans un cluster de basculement Windows. Quand le service Scale Out Master ou SSISDB est arrêté sur le nœud principal, le service ou SSISDB sur le nœud secondaire continue d’accepter les demandes d’utilisateur et de communiquer avec les Scale Out Workers.

La haute disponibilité côté Scale Out Master peut aussi être obtenue au moyen d’une instance de cluster de basculement SQL Server. Consultez [Prise en charge de Scale Out pour la haute disponibilité au moyen d’une instance de cluster de basculement SQL Server](scale-out-failover-cluster-instance.md).

Pour configurer la haute disponibilité côté Scale Out Master à l’aide de la fonctionnalité Always On pour le catalogue SSIS, suivez les étapes ci-dessous :

## <a name="1-prerequisites"></a>1. Conditions préalables requises
Configurez un cluster de basculement Windows. Pour obtenir des instructions, consultez le billet de blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/04/06/10291601.aspx). Installez la fonctionnalité et les outils sur tous les nœuds de cluster.

## <a name="2-install-scale-out-master-on-the-primary-node"></a>2. Installer Scale Out Master sur le nœud principal
Installez les services SQL Server Moteur de base de données, Integration Services et Scale Out Master sur le nœud principal pour Scale Out Master. 

Pendant l’installation, effectuez les étapes suivantes :

### <a name="21-set-the-account-running-scale-out-master-service-to-a-domain-account"></a>2.1 Définir le compte exécutant le service Scale Out Master sur un compte de domaine
Ce compte doit pouvoir accéder ultérieurement à SSISDB sur le nœud secondaire dans le cluster de basculement Windows. Étant donné que le service Scale Out Master et SSISDB peuvent basculer séparément, ils peuvent se trouver sur des nœuds différents après le basculement.

![Configuration du serveur à haute disponibilité](media/ha-server-config.PNG)

### <a name="22-include-the-dns-host-name-for-the-scale-out-master-service-in-the-cns-of-the-scale-out-master-certificate"></a>2.2 Inclure le nom d’hôte DNS du service Scale Out Master dans les noms communs (CN) du certificat Scale Out Master

Ce nom d’hôte est utilisé dans le point de terminaison de Scale Out Master. 

![Configuration de Master à haute disponibilité](media/ha-master-config.PNG)

## <a name="3-install-scale-out-master-on-the-secondary-node"></a>3. Installer Scale Out Master sur le nœud secondaire
Installez les services SQL Server Moteur de base de données, Integration Services et Scale Out Master sur le nœud secondaire pour Scale Out Master. 

Utilisez le même certificat Scale Out Master que celui utilisé sur le nœud principal. Exportez le certificat SSL Scale Out Master sur le nœud principal avec une clé privée, puis installez-le dans le magasin de certificats racine de l’ordinateur local sur le nœud secondaire. Sélectionnez ce certificat quand vous installez Scale Out Master sur le nœud secondaire.

![Configuration de Master à haute disponibilité 2](media/ha-master-config2.PNG)

> [!NOTE]
> Vous pouvez configurer plusieurs Scale Out Masters de sauvegarde en répétant ces opérations pour le Scale Out Master sur d’autres nœuds secondaires.

## <a name="4-set-up-ssisdb-always-on"></a>4. Configurer Always On pour SSISDB

Pour configurer Always On pour SSISDB, suivez les indications fournies dans [Always On pour le catalogue SSIS (SSISDB)](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb).

Vous devez également créer un écouteur de groupe de disponibilité pour le groupe de disponibilité auquel vous ajoutez SSISDB. Consultez [Créer ou configurer un écouteur de groupe de disponibilité](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Mettre à jour le fichier de configuration du service Scale Out Master
Mettez à jour le fichier de configuration du service Scale Out Master (`\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`) sur le nœud principal et chaque nœud secondaire. Définissez **SqlServerName** sur *nom DNS de l’écouteur de groupe de disponibilité],[port]*.

## <a name="6-enable-package-execution-logging"></a>6. Activer la journalisation de l’exécution des packages

La journalisation dans SSISDB est effectuée par la connexion **##MS_SSISLogDBWorkerAgentLogin##**. Le mot de passe est généré automatiquement pour cette connexion. Pour effectuer la journalisation de tous les réplicas de SSISDB, effectuez les opérations suivantes

### <a name="61-change-the-password-of-msssislogdbworkeragentlogin-on-the-primary-sql-server"></a>6.1 Changer le mot de passe de **##MS_SSISLogDBWorkerAgentLogin##** sur l’instance SQL Server principale

### <a name="62-add-the-login-to-the-secondary-sql-server"></a>6.2 Ajouter la connexion au nœud SQL Server secondaire

### <a name="63-update-the-connection-string-used-for-logging"></a>6.3 Mettre à jour la chaîne de connexion utilisée pour la journalisation
Appelez la procédure stockée `[catalog].[update_logdb_info]` avec les valeurs de paramètre suivantes :

-   `@server_name = '[Availability Group Listener DNS name],[Port]' `

-   `@connection_string = 'Data Source=[Availability Group Listener DNS name],[Port];Initial Catalog=SSISDB;User Id=##MS_SSISLogDBWorkerAgentLogin##;Password=[Password]];'`

## <a name="7-configure-the-scale-out-master-service-role-of-the-windows-failover-cluster"></a>7. Configurer le rôle du service Scale Out Master du cluster de basculement Windows

1.  Dans le Gestionnaire du cluster de basculement, connectez-vous au cluster pour Scale Out. Sélectionnez le cluster. Sélectionnez **Action** dans le menu, puis sélectionnez **Configurer un rôle**.

2.  Dans la boîte de dialogue **Assistant Haute disponibilité**, sélectionnez **Service générique** dans la page **Sélectionner un rôle**. Sélectionnez SQL Server Integration Services Scale Out Master 14.0 dans la page **Sélectionner un service**.

3.  Dans la page **Point d’accès client**, entrez le nom d’hôte DNS du service Scale Out Master.

    ![Assistant Haute disponibilité 1](media/ha-wizard1.PNG)

4.  Terminez l’Assistant.

## <a name="8-update-the-scale-out-master-address-in-ssisdb"></a>8. Mettre à jour l’adresse Scale Out Master dans SSISDB

Sur l’instance SQL Server principale, exécutez la procédure stockée `[catalog].[update_master_address]` avec la valeur de paramètre `@MasterAddress = N'https://[Scale Out Master service DNS host name]:[Master Port]'`. 

## <a name="9-add-the-scale-out-workers"></a>9. Ajouter les Scale Out Workers

Maintenant, vous pouvez ajouter des Scale Out Workers avec [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md). Entrez `[SQL Server Availability Group Listener DNS name],[Port]` dans la page de connexion.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez les articles suivants :
-   [SSIS (SQL Server Integration Services) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [SSIS (SQL Server Integration Services) Scale Out Worker](integration-services-ssis-scale-out-worker.md)