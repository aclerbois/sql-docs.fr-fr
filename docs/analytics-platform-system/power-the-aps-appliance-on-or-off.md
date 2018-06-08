---
title: L’application ou désactiver - système de plateforme d’Analytique de l’alimentation | Documents Microsoft
description: L’application ou désactiver l’alimentation pour un système de plateforme d’Analytique
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 54829190d03a889ade31383662bf192516934012
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="power-the-appliance-on-or-off-for-analytics-platform-system"></a>L’application ou désactiver l’alimentation pour un système de plateforme d’Analytique
Cette rubrique décrit comment mettre sous tension ou hors tension votre Systemappliance de plateforme Analytique qui est en cours d’exécution Parallel Data Warehouse et éventuellement une région HDInsight en cours d’exécution. Utilisez cette rubrique lorsqu’un dispositif de système de plateforme Analytique est déplacé, ou pour mettre sur un appareil après une panne catastrophique.  
  
Mise sous tension de l’application et n’est pas le même que le démarrage et arrêt des services d’application. Pour plus d’informations sur ce sujet, consultez [l’état des Services PDW &#40;système de plateforme Analytique&#41;](pdw-services-status.md). Pour plus d’informations sur la mise sous tension ou désactiver un SQL Server 2008 Parallel Data Warehouse, consultez le fichier d’aide de SQL Server 2008 Parallel Data Warehouse. Pour plus d’informations sur la mise sous tension ou désactiver de SQL Server 2012 AU1 AU2 Parallel Data Warehouse, consultez le fichier d’aide pour ces versions.  
  
Lorsque ces instructions spécifient la connexion à un nœud SQL Server PDW, la connexion peut être locale à l’aide de périphériques connectés (KVM) ou à distance à l’aide du Bureau à distance. Certaines actions doivent être physique (activation sur un bouton d’alimentation) et autres (par exemple, d’arrêt) peuvent être physiques ou des commandes à l’aide de Windows.  
  
Connexions aux nœuds de SQL Server PDW peuvent être établies en utilisant les adresses IP attribuées aux nœuds, ou à partir de la **HST01** ordinateur en utilisant le **Gestionnaire du Cluster de basculement** (**cluadmin.msc**) ou **Gestionnaire Hyper-V** (**virtmgmt.msc**) les applications et le droit sur le nom du nœud.  
  
## <a name="PowerOff"></a>Mise hors tension de l’appareil  
  
### <a name="before-you-begin"></a>Avant de commencer  
Avant la mise hors tension l’appliance, vous devez se terminer toutes les activités sur l’appareil. Pour mettre fin à toutes les activités :  
  
-   Utilisez le **Sessions** page de la Console d’administration pour identifier les utilisateurs en cours. Contactez-le et demandez-lui de fermer la session.  
  
-   Si nécessaire vous pouvez utiliser la **KILL** instruction pour forcer l’arrêt d’une connexion client. Soyez prudent lors de l’arrêt de connexions. Lorsque l’interruption survient, certains processus transactionnels, par exemple une mise à jour en cours d’exécution longue, doit activité de restauration SQL Server avant de procéder à la récupération de la base de données. Restauration d’une mise à jour partielle ou une suppression, peut prendre beaucoup de temps.  
  
### <a name="to-power-off-the-appliance"></a>Pour mettre hors tension de l’appareil  
  
> [!WARNING]  
> Toutes les étapes doivent être effectuées dans l’ordre indiqué, et chaque étape doit terminer avant l’exécution de l’étape suivante, sauf indication contraire. Les étapes de manière désordonnée ou sans attendre de chaque étape peuvent générer des erreurs lors de l’application est sous tension à une date ultérieure.  
  
1.  Connectez-vous au nœud de contrôle de PDW (***PDW_region *-CTL01** ) et de la connexion avec le compte d’administrateur système de plateforme Analytique appliance domaine.  
  
2.  Exécutez `C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\dwconfig.exe` pour ouvrir le **Configuration Manager**.  
  
3.  Dans le Gestionnaire de Configuration, sous la **parallèle topologie de l’entrepôt de données** menu, cliquez sur le **l’état des Services** onglet, puis cliquez sur **arrêter la région** pour arrêter les services PDW.  
  
4.  S’il existe une région HDInsight, sous le **HDInsight topologie** menu, cliquez sur le **l’état des Services** onglet, puis cliquez sur **arrêter la région** pour arrêter les services de HDInsight.  
  
5.  Se connecter à ***appliance_domain *-HST01** et de la connexion avec le compte d’administrateur de domaine appliance.  
  
6.  À l’aide de la **Gestionnaire du Cluster de basculement** se connecter à la ***appliance_domain *-WFOHST01** cluster si pas automatiquement connecté et, dans le volet de Navigation, cliquez sur **rôles**. Dans le **rôles** volet :  
  
    1.  Multisélection des ordinateurs virtuels. Effectuez un clic droit, puis sélectionnez **arrêter**.  
  
    2.  Attendez que tous les ordinateurs virtuels sélectionnés à se fermer.  
  
7.  En l’absence d’une région HDInsight :  
  
    1.  Se connecter au cluster HDInsight. Pour ce faire, cliquez sur **Gestionnaire du Cluster de basculement**, sélectionnez **se connecter au Cluster**et spécifiez ***appliance_domain *-WFOHST02** pour le nom du cluster.  
  
    2.  Sous le cluster HDInsight, cliquez sur **rôles**. Dans le **rôles** volet :  
  
        1.  Sélectionner plusieurs toutes les machines virtuelles, effectuez un clic droit, puis sélectionnez **arrêter**.  
  
        2.  Attendez que les ordinateurs virtuels pour l’arrêter.  
  
8.  Fermer le **Gestionnaire du Cluster de basculement** application.  
  
9. Arrêtez tous les serveurs à l’exception ***appliance_domain *-HST01**.  
  
10. Arrêter le ***appliance_domain *-HST01** server.  
  
11. Arrêtez les unités de Distribution d’alimentation (PDU).  
  
## <a name="PowerOn"></a>Le dispositif de mise sous tension  
  
### <a name="to-power-on-the-appliance"></a>Pour mettre sous tension l’appareil  
  
> [!WARNING]  
> Toutes les étapes doivent être effectuées dans l’ordre indiqué, et chaque étape doit terminer avant l’exécution de l’étape suivante, sauf indication contraire. Les étapes de manière désordonnée ou sans attendre de chaque étape peuvent entraîner des erreurs de démarrage.  
  
1.  Mise sous tension les unités d’alimentation (PDU) et attendez que les commutateurs pour démarrer automatiquement.  
  
2.  Mettez sous tension le ***appliance_domain *-HST01** server.  
  
3.  Connectez-vous à ***appliance_domain *-HST01** en tant que l’administrateur de domaine d’application.  
  
4.  Démarrer le **Gestionnaire Hyper-V** programme (**virtmgmt.msc**) et connectez-vous à ***appliance_domain *-HST01** si ne pas connecté par défaut.  
  
    1.  Si vous ne peut pas se connecter par nom, car le ***PDW_region *-AD01** est ne pas en cours d’exécution, essayez de vous connecter à l’aide de l’adresse IP.  
  
    2.  Dans le **virtuels** volet, recherchez ***PDW_region *-AD01** et vérifiez qu’il s’exécute. Si ce n’est pas le cas, démarrer cet ordinateur virtuel et attendez qu’elle doit être entièrement démarré.  
  
5.  Mise sous tension le reste des serveurs dans l’appliance.  
  
6.  Sur **HST01** connecté comme administrateur de domaine d’application, à partir de **Gestionnaire Hyper-V**:  
  
    1.  Se connecter à ***appliance_domain *-HST02**.  
  
    2.  Dans le **virtuels** volet, recherchez ***PDW_region *-AD02** et vérifiez qu’il s’exécute.  Si ce n’est pas le cas, démarrer cet ordinateur virtuel et attendez qu’elle doit être entièrement démarré.  
  
7.  À l’aide de la **Gestionnaire du Cluster de basculement** se connecter à la ***appliance_domain *-WFOHST01** de cluster, si pas automatiquement connecté, puis, dans le **Navigation** volet, cliquez sur **Rôles**. Dans le **rôles** volet :  
  
    1.  Sélectionner plusieurs toutes les machines virtuelles, effectuez un clic droit, puis cliquez sur **Démarrer**.  
  
    2.  Attendez que tous les ordinateurs virtuels sélectionnés démarrer avant de passer à l’étape suivante.  
  
    3.  Si nécessaire pour les machines virtuelles ayant basculé, les arrêter, déplacez-les et les redémarrer sur l’hôte principal approprié.  
  
8.  Si l’application a une région HDInsight, connectez-vous au cluster HDInsight. (Pour ce faire, cliquez sur **Gestionnaire du Cluster de basculement**, sélectionnez **se connecter au Cluster**et spécifiez ***appliance_domain *-WFOHST01** pour le nom du cluster.)  
  
    1.  Sous le cluster HDInsight, cliquez sur **rôles**. Dans le **rôles** volet.  
  
        1.  Sélectionner plusieurs toutes les machines virtuelles, effectuez un clic droit, puis sélectionnez **Démarrer**,  
  
        2.  Attendez que tous les ordinateurs virtuels sélectionnés démarrer avant de passer à l’étape suivante.  
  
        3.  Si nécessaire pour les machines virtuelles ayant basculé, les arrêter, déplacez-les et les redémarrer sur l’hôte principal approprié.  
  
9. Déconnectez **HST01** si vous le souhaitez.  
  
10. Se connecter à ***PDW_region *-CTL01** utilisant le compte d’administrateur de domaine appliance.  
  
11. Exécutez `C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\dwconfig.exe` pour lancer le **Configuration Manager**.  
  
12. Dans le **Configuration Manager**, dans le **parallèle topologie de l’entrepôt de données** menu, cliquez sur le **l’état des Services** onglet, puis cliquez sur **démarrer région**pour démarrer les services PDW.  
  
13. Si l’application possède une région HDInsight, dans le menu de la topologie de HDInsight, cliquez sur le **l’état des Services** onglet, puis cliquez sur **démarrer la région** pour démarrer les services HDInsight.  
  
### <a name="to-verify-the-appliance-health"></a>Pour vérifier l’intégrité de l’équipement  
Une fois l’application a démarré, ouvrez le **Console d’administration** et vérifiez la page de contrôle d’intégrité pour les alertes susceptibles d’indiquer les conditions d’échec. Pour plus d’informations, consultez [contrôler le matériel à l’aide de la Console d’administration &#40;système de plateforme Analytique&#41;](monitor-the-appliance-by-using-the-admin-console.md).  
  
## <a name="see-also"></a>Voir aussi  
[Tâches de gestion appliance &#40;Analytique plate-forme système&#41;](appliance-management-tasks.md)  
  
