---
title: Un logiciel antivirus - système de plateforme Analytique | Documents Microsoft
description: Si votre centre de données requiert un logiciel antiviru, utilisez ces instructions pour installer un logiciel antivirus sur le système de plateforme d’Analytique. Nous vous recommandons ne pas l’installation de logiciel antivirus, sauf si elle est une exigence d’entreprise de votre centre de données.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 5d9ff6848d2df43408613d41dc7a0e6f8c1b0b8c
ms.sourcegitcommit: fc3cd23685c6b9b6972d6a7bab2cc2fc5ebab5f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="antivirus-software-for-analytics-platform-system"></a>Logiciel antivirus pour système de plateforme Analytique
Si votre centre de données requiert un logiciel antiviru, utilisez ces instructions pour installer un logiciel antivirus sur le système de plateforme d’Analytique. Nous vous recommandons ne pas l’installation de logiciel antivirus, sauf si elle est une exigence d’entreprise de votre centre de données.  
  
> [!WARNING]  
> Nous recommandons vivement qu’individuellement évaluer le risque de sécurité pour chaque ordinateur et de système de plateforme d’Analytique dans son ensemble, et que vous sélectionnez les outils appropriés pour le niveau de risque pour la sécurité de chaque ordinateur. En outre, nous vous recommandons d’avant de mettre un projet de la protection contre les virus, tester l’ensemble du système sous une charge complète pour mesurer les modifications de stabilité et de performances.  
>   
> Protection antivirus nécessite des ressources système à exécuter. Vous devez effectuer le test avant et après l’installation de votre logiciel antivirus pour déterminer s’il y a aucun effet de performances sur le système de plateforme Analytique.  
  
Cette rubrique est basée sur les instructions de [comment choisir un logiciel antivirus s’exécute sur les ordinateurs qui exécutent SQL Server](http://support.microsoft.com/kb/309422) et [961804 de l’Article KB](http://support.microsoft.com/kb/961804/en-us).  
  
## <a name="exclusion-list-for-physical-hosts"></a>Liste d’exclusion pour les ordinateurs hôtes physiques  
Pour installer le logiciel antivirus sur les hôtes physiques, excluez la liste suivante des répertoires et des processus. Ils ne doivent pas être analysés par le logiciel antivirus.  
  
**Exclure les répertoires suivants :**  
  
-   C:\ProgramData\Microsoft\Windows\Hyper-V - répertoire de configuration de machine virtuelle  
  
-   Disques durs C:\Users\Public\Documents\Hyper-V\Virtual - répertoire du lecteur de disque dur virtuel par défaut  
  
-   C:\clusterStorage - répertoires de lecteur de disque dur virtuel  
  
**Exclure les processus :**  
  
-   Gestion d’ordinateurs virtuels (Vmms.exe)  
  
-   Processus de travail de machine virtuelle (Vmwp.exe)  
  
## <a name="exclusion-list-for-virtual-machines-vms"></a>Liste d’exclusion pour les Machines virtuelles (VM)  
Pour installer le logiciel antivirus sur les machines virtuelles, excluez la liste suivante de répertoires et fichiers. Ils ne doivent pas être analysés par le logiciel antivirus.  
  
***PDW_region*-CTL01**  
  
-   C:\windows\cluster\  
  
-   G:\  
  
***appliance_domain *-AD01** et ***appliance_domain *-AD02**  
  
-   Aucune restriction  
  
**Machines virtuelles de nœud de calcul**  
  
-   C:\windows\cluster\  
  
-   G:\  
  
***appliance_domain*-VMM**  
  
-   Aucune restriction  
  
***appliance_domain*-WDS**  
  
-   Aucune restriction  
  
***appliance_domain*-ISCSI01**  
  
-   C:\iscsitarget  
  
## <a name="see-also"></a>Voir aussi  
[Tâches de gestion appliance &#40;Analytique plate-forme système&#41;](appliance-management-tasks.md)  
  
