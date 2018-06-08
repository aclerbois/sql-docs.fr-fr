---
title: Désinstaller les mises à jour Microsoft - système de plateforme Analytique | Documents Microsoft »
description: Désinstaller les mises à jour de Microsoft dans le système de plateforme Analytique (APS).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 57d0eb3616cf3567f63d75029f79cea6709ed955
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="uninstall-microsoft-updates-in-analytics-platform-system"></a>Désinstaller les mises à jour Microsoft dans le système de plateforme Analytique
Cet article décrit comment désinstaller une mise à jour Microsoft installé sur le matériel de système de plateforme Analytique.  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="prerequisites"></a>Configuration requise  
Pour effectuer ces étapes, vous devez :  
  
-   Une connexion de système de plateforme Analytique avec des autorisations pour accéder à la Console d’administration pour contrôler le matériel.  
  
-   Base de connaissances du compte d’administrateur de domaine de l’ensemble fibre optique pour vous connecter à la  *<Fabric Domain>***-HST01** nœud.  
  
## <a name="HowToUninstallMSFT"></a>Pour désinstaller les mises à jour Microsoft  
  
1.  Se connecter à la  *<Fabric Domain>***-HST01** nœud en tant que l’administrateur de domaine de l’ensemble fibre optique.  
  
2.  Pour désinstaller les mises à jour sont approuvées pour WSUS désinstaller, ouvrez une fenêtre d’invite de commandes et entrez la commande suivante. Remplacez les éléments de l’espace réservé *< >* avec les informations appropriées.  
  
    ```  
    C:\pdwinst\media\setup.exe /action="RemoveMicrosoftUpdate" /DomainAdminPasswords="<password>"  
    ```  
  
## <a name="next-steps"></a>Étapes suivantes
Pour plus d'informations, consultez :
- [Téléchargez et appliquez les mises à jour Microsoft &#40;Analytique plate-forme système&#41;](download-and-apply-microsoft-updates.md) 
- [Appliquer des correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](apply-analytics-platform-system-hotfixes.md)  
- [Désinstallation de correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](uninstall-analytics-platform-system-hotfixes.md)  
- [Maintenance de logiciel &#40;Analytique plate-forme système&#41;](software-servicing.md)  
  
