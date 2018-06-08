---
title: Composants physiques du matériel - système de plateforme d’Analytique | Documents Microsoft
description: Les noms et descriptions pour les composants physiques de l’ensemble fibre optique PDW et l’équipement.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 0adbd92d1a29a98a80de65268c53ea63e3941d07
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="appliance-physical-components---analytics-platform-system"></a>Composants physiques du matériel - système de plateforme d’Analytique
Les noms et descriptions pour les composants physiques de l’ensemble fibre optique PDW et l’équipement. 
  
<!-- MISSING LINKS See also [HDInsight Physical Components &#40;Analytics Platform System&#41;](hdinsight-physical-components.md).  -->  
  
## <a name="diagrams"></a>Diagrammes de composants  
Affiche les noms des composants physiques et où ils se trouvent dans le rack première d’un appareil de nœud de calcul de 6.  
  
![Noms de composants de région PDW - HP](./media/pdw-and-appliance-fabric-physical-components/APS_HW_ComponentNames-HP.png "APS_HW_ComponentNames-HP")  
  
Le nom réel pour les composants PDW est le nom de région PDW, suivi par un tiret, suivi par le nom du composant. Par exemple, si le nom de région PDW est PDW123, les noms réels sont **PDW123-CTL01**, **PDW123-CMP01**, etc.  
  
De même, le nom réel pour les composants d’infrastructure appliance est le domaine du matériel, suivi d’un tiret, suivi par le nom du composant. Par exemple, si le domaine d’application est FSW123, l’infrastructure d’application les machines virtuelles sont **FSW123-WDS**, **FSW123-AD01**, **FSW123-VMM**, etc.  
  
Voici une vue consolidée d’une région PDW avec 6 nœuds de calcul.  
  
![Noms des composants PDW](./media/pdw-and-appliance-fabric-physical-components/APS_HW_Names.png "APS_HW_Names")  
  
## <a name="pdw"></a>Composants PDW  
Les ordinateurs virtuels PDW font partie de la région PDW.  
  
*PDW_region*-CTL01  
Un ordinateur virtuel qui exécute le nœud du contrôle. Cela s’exécute sur HST01 et peut basculer vers HST02.  
  
> [!WARNING]  
> SQL Server PDW ne prend pas en charge la création d’un instantané de l’ordinateur virtuel CTL01 à l’aide du Gestionnaire Hyper-V. Instantanés reposent sur le stockage local, ce qui entraîne des erreurs si l’ordinateur virtuel tente de basculer vers la sauvegarde. Création d’un instantané peut également entraîner des problèmes de fiabilité avec l’ordinateur que le basculement vers le serveur passif.  
  
*PDW_region*-CMP01 via *PDW_Region*-CMP06  
Un ordinateur virtuel qui exécute le nœud de calcul. Dans ce diagramme de nœud de calcul de 6, les hôtes HSA01 via HSA06 exécuter nœud de calcul CMP01 de machines virtuelles via CMP06 respectivement.  
  
## <a name="fabric"></a>Composants d’infrastructure de matériel  
Ces composants font partie de l’infrastructure d’application.  
  
### <a name="virtual-machines"></a>Machines virtuelles  
*appliance_domain*-WDS  
Ce déploiement de Services Windows (WDS), qui utilise de système de plateforme Analytique hôtes d’ordinateurs virtuels déploiement des systèmes d’exploitation Windows sur le réseau de l’appliance. Il héberge également le service DHCP, ce qui permet les hôtes de l’appliance joindre le réseau de l’application sans avoir une adresse IP préconfigurée.  
  
Le *appliance_domain*- WDS virtual machine s’exécute sur HST01 et peut basculer vers HST02. L’ordinateur virtuel WDS et l’ordinateur virtuel VMM, déployer Windows sur les hôtes physiques lors de l’installation de l’appliance. Pendant le cycle de vie du matériel, WDS et VMM effectuent des opérations telles que le remplacement d’un ordinateur hôte.  
  
*appliance_domain*-VMM  
Virtual Machine Manager (VMM) s’exécute sur une machine virtuelle et peut basculer vers HST02. VMM héberge System Center pour déployer le système d’exploitation sur les ordinateurs hôtes physiques. VMM offre également Windows Server Update Services (WSUS) pour appliquer ou supprimer des mises à jour Windows sur tous les hôtes et les ordinateurs virtuels.  
  
*appliance_domain*-AD01, *appliance_domain*-AD02  
Des Services de domaine Active Directory, qui contient le système DNS (Domain Name), s’exécute sur une machine virtuelle sur HST01 et HST02. Pour la haute disponibilité de l’appliance AD01 et AD02 sont des contrôleurs de domaine répliqué et ils ne basculent pas. Si un échoue, les autres est déjà disponible avec les données correctes.  
  
*appliance_domain*-ISCSI01  
Un ordinateur virtuel ISCSI s’exécute sur chacun des hôtes avec un stockage connecté (HSA01-HSA06). Cet ordinateur virtuel ne bascule pas.  
  
### <a name="hosts"></a>Hôtes  
*appliance_domain*-HST01 via *appliance_domain*-HST06  
Les ordinateurs hôtes pour PDW contrôle nœud et l’équipement fabric virtuels. HST03 est un hôte de passif facultatif.  
  
*appliance_domain*-HSA01 via *appliance_domain*-HSA08  
Les hôtes avec un stockage attaché (HSA). Chaque nœud de calcul une machine virtuelle HAS hôte s’exécute et l’autre ISCSI VM.  
  
### <a name="cluster-for-pdw"></a>Cluster pour PDW  
*appliance_domain*-WFOHST01  
Le cluster PDW est nommé WFOHST01. Il gère tous les hôtes physiques et les machines virtuelles qui appartiennent à PDW.  
  
### <a name="direct-attached-storage"></a>Stockage en attachement direct  
*appliance_domain*-DAS01 via *appliance_domain*-DAS03  
Il s’agit du stockage en attachement direct qui est connecté aux nœuds de calcul. HP a un DAS pour chaque deux nœuds de calcul. Dell et Quanta ont un DAS pour chaque trois nœuds de calcul.  
  
## <a name="see-also"></a>Voir aussi  
<!-- MISSING LINKS [Hardware Configurations &#40;Analytics Platform System&#41;](../architecture/hardware-configurations.md)  -->  
[Configuration du matériel &#40;Analytique plate-forme système&#41;](appliance-configuration.md)  
[Tâches de gestion appliance &#40;Analytique plate-forme système&#41;](appliance-management-tasks.md)  
  
