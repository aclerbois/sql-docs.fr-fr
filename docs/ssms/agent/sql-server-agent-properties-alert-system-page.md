---
title: Propriétés de SQL Server Agent (page Système d’alerte) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ag.agent.alert.f1
ms.assetid: 3e6d3bfd-20ee-4593-86cc-f65b1c08c69d
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 493e81d1d245920b391374ef146d6b4dfb0586a9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-agent-properties-alert-system-page"></a>Propriétés de SQL Server Agent (page Système d'alerte)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette page vous permet d’afficher et de modifier les paramètres des messages envoyés par les alertes de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.  
  
## <a name="options"></a>Options  
**Session de messagerie**  
Les options de cette section permettent de configurer la messagerie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.  
  
**Activer le profil de messagerie**  
Active la messagerie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Par défaut, la messagerie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent est désactivée.  
  
**Système de messagerie**  
Définit le système de messagerie que doit utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Messagerie de base de données  
  
> [!NOTE]  
> Une fois que vous avez changé le système de messagerie, vous devez redémarrer le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent pour que la modification soit prise en compte.  
  
**Profil de la messagerie**  
Définit le profil que doit utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Vous pouvez également sélectionner **\<nouveau profil de messagerie de base de données...>** pour créer un profil.  
  
**Messages de radiomessagerie**  
Les options de cette section vous permettent de configurer les messages électroniques envoyés à des adresses de radiomessagerie pour qu'ils fonctionnent avec votre système de radiomessagerie.  
  
**Format d'adresse pour les messages de radiomessagerie**  
Cette section vous permet de spécifier le format des adresses et la ligne Objet incluse dans les messages de radiomessagerie.  
  
**Ligne À**  
Spécifie les options relatives à la ligne **À** du message.  
  
**Préfixe**  
Tapez le texte prédéfini, requis par votre système au début de la ligne **À** des messages envoyés à un récepteur de radiomessagerie.  
  
**Radiomessagerie**  
Inclut l'adresse de messagerie du message entre le préfixe et le suffixe.  
  
**Suffixee**  
Tapez le texte prédéfini, requis par votre système de radiomessagerie à la fin de la ligne **À** des messages envoyés à un récepteur de radiomessagerie.  
  
**Ligne Cc**  
Spécifie les options relatives à la ligne **Cc** du message.  
  
**Préfixe**  
Tapez le texte prédéfini, requis par votre système au début de la ligne **Cc** des messages envoyés à un récepteur de radiomessagerie.  
  
**Récepteur de radiomessagerie**  
Inclut l'adresse de messagerie du message entre le préfixe et le suffixe.  
  
**Suffixe**  
Tapez le texte prédéfini, requis par votre système de radiomessagerie à la fin de la ligne **Cc** des messages envoyés à un récepteur de radiomessagerie.  
  
**Objet**  
Spécifie les options relatives à l'objet du message  
  
**Prefix**  
Tapez le texte prédéfini, requis par votre système de radiomessagerie au début de la ligne **Objet** des messages envoyés à un récepteur de radiomessagerie.  
  
**Suffixe**  
Tapez le texte prédéfini, requis par votre système de radiomessagerie à la fin de la ligne **Objet** des messages envoyés à un récepteur de radiomessagerie.  
  
**Inclure le corps du message dans le message de notification**  
Inclut le corps du message électronique dans le message envoyé au récepteur de radiomessagerie.  
  
**Opérateur de prévention de défaillance**  
Cette section vous permet de spécifier les options relatives à l'opérateur de prévention de défaillance.  
  
**Activer l'opérateur de prévention de défaillance**  
Spécifie un opérateur de prévention de défaillance.  
  
**Opérateur**  
Définit le nom de l'opérateur qui doit recevoir les notifications pour la prévention de défaillance.  
  
**Notifier en utilisant**  
Définit la méthode à utiliser pour notifier l'opérateur de prévention de défaillance.  
  
**Remplacement des jetons**  
Cette section vous permet d'activer des jetons d'étapes de travail utilisables dans les travaux exécutés par les alertes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Pour plus d’informations sur les jetons d’étapes de travail, consultez [Utiliser des jetons dans les étapes d’un travail](../../ssms/agent/use-tokens-in-job-steps.md).  
  
> [!IMPORTANT]  
> Tout utilisateur Windows doté d'autorisations d'accès en écriture sur le Journal des événements Windows peut accéder aux étapes de travail qui sont activées par les alertes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Pour éviter ce risque de sécurité, les jetons de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent qui peuvent être utilisés dans des travaux activés par des alertes sont désactivés par défaut. Il s’agit des jetons suivants : **$(A-DBN)**, **$(A-SVR)**, **$(A-ERR)**, **$(A-SEV)** et **$(A-MSG)**.  
>   
> Si vous avez besoin de les utiliser, assurez-vous avant de les activer que seuls les membres des groupes de sécurité Windows approuvés, tels que le groupe Administrateurs, possèdent des autorisations d'accès en écriture sur le Journal des événements de l'ordinateur sur lequel réside [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
**Remplacer les jetons pour toutes les réponses de travaux aux alertes**  
Activez cette case à cocher pour permettre le remplacement des jetons pour les travaux qui sont activés par les alertes [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
## <a name="see-also"></a> Voir aussi  
[Opérateurs](../../ssms/agent/operators.md)  
[Configurer la messagerie de SQL Server Agent en vue de l'utilisation de la messagerie de base de données](http://msdn.microsoft.com/en-us/4b8b61bd-4bd1-43cd-b6e5-c6ed2e101dce)  
[Messagerie de base de données](http://msdn.microsoft.com/en-us/9e4563dd-4799-4b32-a78a-048ea44a44c1)  
  
