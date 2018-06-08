---
title: Désinstallation des correctifs de système de plateforme Analytique dans | Documents Microsoft
description: Désinstallation des correctifs de système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 83e57a676ee0eff21eb3a736484d8e286cdeee01
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="uninstall-analytics-platform-system-hotfixes"></a>Désinstallation des correctifs de système de plateforme Analytique 
Les étapes suivantes décrivent comment désinstaller un correctif de système de plateforme Analytique précédemment installé.  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="prerequisites"></a>Configuration requise  
Pour effectuer ces étapes, vous devez :  
  
-   Une connexion de système de plateforme Analytique avec des autorisations pour accéder à la Console d’administration pour contrôler le matériel.  
  
-   Le compte d’administrateur de domaine pour vous connecter à la *< appliance_domain > ***-HST01** nœud.  
  
-   Le numéro d’article pour le correctif logiciel à désinstaller.  
  
## <a name="HowToUninstallPDW"></a>Pour désinstaller un correctif logiciel SQL Server PDW  
  
1.  Ouvrez une session sur le *< appliance_domain > ***-HST01** nœud en tant que l’administrateur de domaine de l’ensemble fibre optique.  
  
2.  Pour ouvrir une invite de commandes, utilisez l’exécution en tant qu’option de l’administrateur.  
  
3.  Remplacez les répertoires par `C:\PDWINST\Patches\<kbarticle>\media` où *<kbarticle>* est le numéro d’article de Base de connaissances pour le correctif logiciel à désinstaller.  
  
    ```  
    cd /d c:\PDWINST\Patches\<kbarticle>\media  
    ```  
  
4.  Pour supprimer le correctif logiciel, exécutez la commande suivante.  
  
    ```  
    setup.exe /action="removepatch" /DomainAdminPassword="<password>"  
    ```  
  
## <a name="see-also"></a>Voir aussi  
[Téléchargez et appliquez les mises à jour Microsoft &#40;Analytique plate-forme système&#41;](download-and-apply-microsoft-updates.md)  
[Désinstaller les mises à jour Microsoft &#40;Analytique plate-forme système&#41;](uninstall-microsoft-updates.md)  
[Appliquer des correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](apply-analytics-platform-system-hotfixes.md)  
[Maintenance de logiciel &#40;Analytique plate-forme système&#41;](software-servicing.md)  
  
