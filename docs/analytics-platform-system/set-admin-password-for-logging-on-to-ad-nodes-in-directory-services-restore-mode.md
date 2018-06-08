---
title: Définir un mot de passe Active Directory - système de plateforme Analytique | Documents Microsoft
description: Définir le mot de passe d’ouverture de session Active Directory nœuds administrateur en Mode restauration des Services d’annuaire dans le système de plateforme Analytique (APS).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: e74689216c1485fc0c11c588acb151269e2b5d2b
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="set-admin-password-for-logging-on-to-ad-nodes-in-directory-services-restore-mode-dsrm---analytics-platform-system"></a>Définir le mot de passe administrateur pour ouvrir une session sur les nœuds AD dans les Services d’annuaire de restauration en Mode (DSRM) - système de plateforme Analytique
Mode de restauration des Services annuaire (DSRM) est un mode de démarrage pour réparer ou de récupérer les Services de domaine Active Directory (AD DS). Il est utilisé pour se connecter aux nœuds DISPOSITIF AD une fois les services AD DS a échoué ou lorsque les services AD DS doit être restaurée. Le mot de passe DSRM a été initialisée lors de l’installation de l’application sur le site de fournisseur de matériel et doit être modifié par l’administrateur de matériel. Système de plateforme Analytique a deux services AD DS (contrôleurs de domaine) ; ***appliance_domain *-AD01** et ***appliance_domain *-AD02**. Pour chaque nœud DISPOSITIF AD, modifiez le mot de passe DSRM en procédant comme suit.  
  
## <a name="HowToDSRM"></a>Pour réinitialiser le mot de passe administrateur  
  
1.  Ouvrez une fenêtre d’invite de commandes sur un nœud d’application AD ***appliance_domain*AD*xx***machine virtuelle.  
  
2.  À l’invite de commandes, tapez `ntdsutil`.  
  
3.  À la **ntdsutil** tapez `set dsrm password`.  
  
4.  À la **réinitialiser le mot de passe administrateur :** tapez `reset password on server null`.  
  
5.  À l’invite, tapez le nouveau mot de passe.  
  
6.  Répétez les étapes 1 à 5 ci-dessus pour chaque ordinateur virtuel de l’application AD.  
  
    > [!WARNING]  
    > Système de plateforme d’Analytique ne prend pas en charge le signe dollar ($) dans l’administrateur de domaine ou les mots de passe d’administrateur local. Un mot de passe qui contient un signe dollar valide et être utilisable mais peut bloquer les activités de mise à niveau et de maintenance.  
  
> [!NOTE]  
> Si les Services de domaine Active Directory ou de l’ordinateur virtuel est endommagé pour un ordinateur Active Directory spécifique, en cours d’exécution **ReplaceVM** pour Active Directory affecté machine virtuelle est l’action corrective recommandée. CSS contacter pour obtenir une assistance.  
  
## <a name="see-also"></a>Voir aussi  
[Réinitialisation du mot de passe &#40;Analytique plate-forme système&#41;](password-reset.md)  
  
