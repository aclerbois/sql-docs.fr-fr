---
title: Créer un administrateur de domaine - système de plateforme Analytique | Documents Microsoft
description: Certaines opérations requièrent des privilèges d’administrateur de domaine de système de plateforme Analytique. Cette rubrique explique comment créer des administrateurs de domaine de matériel supplémentaire.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 73eff52cb6e583383f13334e78012721a20a3e25
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="create-an-aps-domain-administrator"></a>Créer un administrateur de domaine APS
Certaines opérations requièrent des privilèges d’administrateur de domaine de système de plateforme Analytique. Cette rubrique explique comment créer des administrateurs de domaine de matériel supplémentaire.  
  
## <a name="create-a-domain-administrator"></a>Créer un administrateur de domaine  
Dispose des autorisations appropriées pour configurer tous les nœuds de points d’accès, l’utilisateur qui exécute le **Gestionnaire de Configuration de points d’accès** (`dwconfig.exe`) doit être un membre de la **Admins du domaine** groupe. Pour démarrer et arrêter les services de points d’accès, l’utilisateur doit être un membre de la **PdwControlNodeAccess** groupe.  
  
#### <a name="to-add-a-user-to-the-domain-admins-group"></a>Pour ajouter un utilisateur au groupe Admins du domaine  
  
1.  Ouvrez une session sur le nœud actif AD **(*appliance_domain*-AD01** ou ***appliance_domain*-AD02**) à l’aide d’un compte d’administrateur de domaine appliance existant.  
  
2.  Dans le menu Démarrer, cliquez sur **Exécuter**. Dans le **ouvrir** , tapez **DSA.msc**. Cliquez sur **OK**.  
  
3.  Dans le **Active Directory Users and Computers** programme, avec le bouton droit **utilisateurs**, pointez sur **nouveau**, puis cliquez sur **utilisateur**.  
  
4.  Dans le **nouvel objet – utilisateur** boîte de dialogue, la description du nouvel utilisateur complète, puis cliquez sur **suivant**.  
  
    La boîte de dialogue mot de passe, puis cliquez sur **suivant**.  
  
    > [!WARNING]  
    > SQL Server PDW ne prend pas en charge le caractère de signe dollar ($) dans l’administrateur de domaine ou les mots de passe d’administrateur local. Un mot de passe qui contient un signe dollar est valide et utilisable, mais peut bloquer les activités de mise à niveau et de maintenance  
  
    Confirmer la nouvelle description de l’utilisateur, puis cliquez sur **Terminer**.  
  
5.  Dans la liste des utilisateurs, double-cliquez sur le nouvel utilisateur pour ouvrir la boîte de dialogue Propriétés.  
  
6.  Sur le **membre de** , cliquez sur **ajouter**.  
  
    Type **Admins du domaine ; PdwControlNodeAccess** puis cliquez sur **vérifier les noms**. Cliquez sur **OK**.  
  
    Cette opération ajoute le nouvel utilisateur à la **Admins du domaine** groupe et la **PdwControlNodeAccess** groupe. Cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Lancez le Gestionnaire de Configuration &#40;Analytique plate-forme système&#41;](launch-the-configuration-manager.md)  
  
