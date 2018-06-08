---
title: Prise en charge de SQL Server Integration Services (SSIS) Scale Out pour la haute disponibilité au moyen d’une instance de cluster de basculement SQL Server | Microsoft Docs
ms.description: This article describes how to configure SSIS Scale Out for high availability with SQL Server failover cluster instance
ms.custom: ''
ms.date: 04/10/2018
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
ms.openlocfilehash: d11bd76d4bc8f811cbaa4ea34258b56aaf6d6763
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34470241"
---
# <a name="scale-out-support-for-high-availability-via-sql-server-failover-cluster-instance"></a>Prise en charge de Scale Out pour la haute disponibilité au moyen d’une instance de cluster de basculement SQL Server

Pour configurer la haute disponibilité côté Scale Out Master au moyen d’une instance de cluster de basculement SQL Server, suivez les étapes ci-dessous :

## <a name="1-prerequisites"></a>1. Conditions préalables requises
Configurez un cluster de basculement Windows. Pour obtenir des instructions, consultez le billet de blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/04/06/10291601.aspx). Installez la fonctionnalité et les outils sur tous les nœuds de cluster.

## <a name="2-install-sql-server-failover-cluster"></a>2. Installer un cluster de basculement SQL Server
Installez un cluster de basculement SQL Server. Pour obtenir des instructions, consultez [Installation d’un cluster de basculement SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md). Durant l’installation, dans la page Sélection de fonctionnalités, sélectionnez Services Moteur de base de données. Notez le nom réseau SQL Server, car vous en aurez besoin ultérieurement pour la configuration.

![Nom réseau SQL](media/sql-network-name.PNG)

Ajoutez un nœud secondaire au cluster de basculement SQL Server.

## <a name="3-install-scale-out-master-on-the-primary-node"></a>3. Installer Scale Out Master sur le nœud principal
Installez Integration Services et Scale Out Master sur le nœud principal à l’aide de l’Assistant Installation pour une installation non-cluster. 

Durant le processus d’installation, ajoutez le nom réseau SQL Server aux noms communs (CN) dans le certificat Scale Out Master.

> [!NOTE]
> Si vous souhaitez effectuer le basculement de SSISDB et du service Scale Out Master séparément, suivez les étapes décrites dans [2. Installer Scale Out Master sur le nœud principal](scale-out-support-for-high-availability.md#2-install-scale-out-master-on-the-primary-node) pour configurer Scale Out Master.

## <a name="4-install-scale-out-master-on-the-secondary-node"></a>4. Installer Scale Out Master sur le nœud secondaire
Suivez les étapes décrites dans [3. Installer Scale Out Master sur le nœud secondaire](scale-out-support-for-high-availability.md#3-install-scale-out-master-on-the-secondary-node)

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Mettre à jour le fichier de configuration du service Scale Out Master
Mettez à jour le fichier de configuration du service Scale Out Master (\<lecteur\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config) sur les nœuds principal et secondaire. Changez la valeur **SqlServerName** par [nom réseau SQL Server]//[nom de l’instance] ou [nom réseau SQL Server] pour l’instance par défaut.

## <a name="6-add-scale-out-master-service-to-sql-server-role-in-windows-failover-cluster"></a>6. Ajouter le service Scale Out Master au rôle SQL Server dans le cluster de basculement Windows
Dans le Gestionnaire du cluster de basculement, connectez-vous au cluster pour Scale Out. Sélectionnez Rôles dans l’Explorateur, cliquez avec le bouton droit sur le rôle SQL Server et sélectionnez Ajouter une ressource, Service générique. 

![Service générique](media/generic-service.PNG)

Dans l’Assistant Nouvelle ressource, sélectionnez le service Scale Out Master et terminez l’Assistant. 

Mettez le service Scale Out Master en ligne.

![Mettre en ligne](media/bring-online.PNG)

> [!NOTE]
> Si vous souhaitez effectuer le basculement de SSISDB et du service Scale Out Master séparément, suivez les étapes décrites dans [7. Configurer le rôle du service Scale Out Master du cluster de basculement Windows](scale-out-support-for-high-availability.md#7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster)

## <a name="7-install-scale-out-workers"></a>7. Installer chaque Scale Out Worker
Installez Scale Out Worker sur les nœuds Worker. Durant le processus d’installation, spécifiez https://[nom réseau SQL Server]:[port master] pour le point de terminaison master. 

> [!NOTE]
> Si vous souhaitez effectuer le basculement de SSISDB et du service Scale Out Master séparément, spécifiez le nom d’hôte DNS du service Scale Out Master au lieu du nom réseau SQL Server.

## <a name="8-install-scale-out-worker-client-certificate"></a>8. Installer le certificat client Scale Out Worker
Installez le certificat Worker sur tous les nœuds du cluster de basculement SQL Server. Consultez [Installer le certificat client Scale Out Worker](walkthrough-set-up-integration-services-scale-out.md#InstallCert).

> [!NOTE]
> Scale Out Manager n’a pas de cluster de basculement SQL Server pris en charge. Si vous utilisez Scale Out Manager pour ajouter Scale Out Worker, vous devez toujours installer manuellement le certificat Worker sur tous les nœuds Master.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez les articles suivants :
-   [SSIS (SQL Server Integration Services) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [SSIS (SQL Server Integration Services) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
