---
title: Utiliser l’Assistant Ajout d’un réplica Azure (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.azurereplica.f1
ms.assetid: b89cc41b-07b4-49f3-82cc-bc42b2e793ae
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2ec374618e0bab35f77370e3f726e5a1cf9bef92
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770595"
---
# <a name="use-the-add-azure-replica-wizard-sql-server"></a>Utiliser l'Assistant Ajout d’un réplica Azure (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l’Assistant Ajouter un réplica Microsoft Azure pour vous aider à créer une machine virtuelle Microsoft Azure dans un environnement informatique hybride et à le configurer comme réplica secondaire pour un groupe de disponibilité Always On nouveau ou existant.  
  
-   **Avant de commencer :**  
  
     [Conditions préalables](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour ajouter un réplica, consultez**  [Assistant Ajouter un réplica Microsoft Azure (SQL Server Management Studio)](#SSMSProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 Si vous n’avez jamais ajouté de réplica de disponibilité à un groupe de disponibilité, consultez les sections « Instances de serveur » et « Groupes de disponibilité et réplicas » dans [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal actuel.  
  
-   Vous devez travailler dans un environnement informatique hybride dans lequel votre sous-réseau local dispose d'un réseau privé virtuel (VPN) de site à site avec Windows Azure. Pour plus d’informations, consultez [Configurer un réseau privé virtuel (VPN) de site à site dans le portail de gestion](https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-site-to-site-create).  
  
-   Votre groupe de disponibilité doit contenir des réplicas de disponibilité locaux.  
  
-   Les clients de l'écouteur de groupe de disponibilité doivent disposer d'une connectivité à Internet s'ils souhaitent maintenir la connectivité avec l'écouteur lorsque le groupe de disponibilité est basculé vers un réplica Windows Azure.  
  
-   **Conditions préalables requises pour utiliser la synchronisation de données initiale complète** Vous devez spécifier un partage réseau pour que l'assistant crée des sauvegardes et puisse y accéder. Pour le réplica principal, le compte utilisé pour démarrer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] doit disposer d'autorisations de système de fichiers en lecture et en écriture sur un partage réseau. Pour les réplicas secondaires, le compte doit disposer d'une autorisation en lecture sur le partage réseau.  
  
     Si vous ne pouvez pas utiliser l'Assistant pour effectuer la synchronisation des données initiale complète, vous devez préparer vos bases de données secondaires manuellement. Vous pouvez le faire avant ou après l'exécution de l'Assistant. Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Consultez [Security](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md#Security)  
  
##  <a name="SSMSProcedure"></a> Utilisation de l'Assistant Ajouter un réplica Windows Azure (SQL Server Management Studio)  
 L'Assistant Ajouter un réplica Windows Azure peut être lancé depuis la page [Spécifier les réplicas](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md). Il existe deux moyens d'atteindre cette page :  
  
-   [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
 Une fois que vous avez lancé l'Assistant Ajouter un réplica Windows Azure, suivez les étapes ci-dessous :  
  
1.  Commencez par télécharger un certificat de gestion pour votre abonnement Windows Azure. Cliquez sur **Télécharger** pour ouvrir la page de connexion.  
  
2.  Connectez-vous à Microsoft Azure avec votre compte Microsoft ou votre compte professionnel ou scolaire. Votre compte Microsoft ou votre compte d’organisation  a le format d’une adresse e-mail, comme HYPERLINK "mailto:patc@contoso.com" patc@contoso.com. Pour en savoir plus sur les informations d’identification Azure, consultez les rubriques suivantes : [Microsoft Account for Organizations FAQ (FAQ sur les comptes Microsoft pour les organisations)](http://technet.microsoft.com/jj592903) et [Résolution des problèmes de connexion avec votre compte d’organisation](https://support.microsoft.com/kb/2756852).  
  
3.  Connectez-vous ensuite à votre abonnement en cliquant sur **Connexion**. Une fois que vous êtes connecté, les listes déroulantes sont remplies avec vos paramètres Microsoft Azure, tels que **Réseau virtuel** et **Sous-réseau de réseau virtuel**.  
  
4.  Spécifiez les paramètres de l'ordinateur virtuel Windows Azure qui hébergera le nouveau réplica secondaire :  
  
     Image  
     Nom de l'image SQL Server à utiliser pour l'ordinateur virtuel Windows Azure  
  
     Taille de l'ordinateur virtuel  
     Taille de l'ordinateur virtuel Windows Azure  
  
     Nom de l'ordinateur virtuel  
     Nom DNS de l'ordinateur virtuel Windows Azure  
  
     Nom d'utilisateur de l'ordinateur virtuel  
     Nom d'utilisateur de l'administrateur par défaut de l'ordinateur virtuel Windows Azure  
  
     Mot de passe d'administrateur de l'ordinateur virtuel (et Confirmer le mot de passe)  
     Mot de passe de l'administrateur par défaut de l'ordinateur virtuel Windows Azure  
  
     Réseau virtuel  
     Réseau virtuel dans lequel placer l'ordinateur virtuel Windows Azure  
  
     Sous-réseau de réseau virtuel  
     Sous-réseau de réseau virtuel dans lequel placer l'ordinateur virtuel Windows Azure  
  
     Domaine  
     Domaine Active Directory (AD) auquel joindre l'ordinateur virtuel Windows Azure  
  
     Nom d'utilisateur de domaine  
     Nom d'utilisateur AD utilisé pour joindre l'ordinateur virtuel Windows Azure au domaine  
  
     Mot de passe  
     Mot de passe utilisé pour joindre l'ordinateur virtuel Windows Azure au domaine  
  
5.  Cliquez sur **OK** pour valider vos paramètres et quitter l'Assistant Ajouter un réplica Windows Azure.  
  
6.  Effectuez les autres étapes de configuration décrites à la page [Spécifier les réplicas](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md) , comme vous le feriez pour tout nouveau réplica.  
  
     Une fois que vous avez terminé avec l’Assistant de groupe de disponibilité ou l’Assistant Ajouter un réplica au groupe de disponibilité, le processus de configuration effectue toutes les opérations dans Microsoft Azure afin de créer la nouvelle machine virtuelle, de le joindre au domaine AD, de l’ajouter au cluster Windows, d’activer la haute disponibilité Always On et d’ajouter le nouveau réplica au groupe de disponibilité.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
  
