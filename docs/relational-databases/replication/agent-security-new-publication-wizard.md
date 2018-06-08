---
title: Sécurité de l’agent (Assistant Nouvelle publication) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.agentsecurity.articles.f1
ms.assetid: 05ae44df-8e9f-46ea-95f6-972ad109c6c0
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 039af6ce967310834e2b4853d4dcac86c914814f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="agent-security-new-publication-wizard"></a>Sécurité de l'agent (Assistant Nouvelle publication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Sécurité de l'agent** vous permet de spécifier les comptes sous lesquels les agents ci-après s'exécutent et les connexions sont établies aux ordinateurs dans une topologie de réplication :  
  
-   Agent d'instantané pour toutes les publications ;  
  
-   Agent de lecture du journal pour toutes les publications transactionnelles ;  
  
-   Agent de lecture de la file d'attente pour les publications transactionnelles qui autorisent les abonnements pouvant être mis à jour. Le travail de l'Agent [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour cet agent est créé si vous avez spécifié l'option **Publication transactionnelle avec abonnements pouvant être mis à jour** dans la page **Type de publication** , quel que soit le type d'abonnements pouvant être mis à jour utilisé. Pour plus d’informations sur les abonnements pouvant être mis à jour, consultez [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 Pour plus d'informations sur les autorisations requises par les agents et les méthodes préconisées pour la sécurité de la réplication, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md) et [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="options"></a>Options  
 **Agent d'instantané**  
 Affiché pour toutes les publications. Cliquez sur **Paramètres de sécurité** pour spécifier les paramètres de sécurité dans la boîte de dialogue **Sécurité de l'Agent d'instantané** .  
  
 Cliquez sur **Aide** dans la boîte de dialogue **Sécurité de l'Agent d'instantané** pour obtenir des informations supplémentaires sur les autorisations requises pour les comptes utilisés par l'Agent d'instantané.  
  
 **l'Agent de lecture du journal ;**  
 Affiché pour toutes les publications transactionnelles. Cliquez sur **Paramètres de sécurité** pour spécifier les paramètres de sécurité dans la boîte de dialogue **Sécurité de l'Agent de lecture du journal** .  
  
 Cliquez sur **Aide** dans la boîte de dialogue **Sécurité de l'Agent de lecture du journal** pour obtenir des informations supplémentaires sur les autorisations requises pour les comptes utilisés par l'Agent de lecture du journal.  
  
> [!NOTE]  
>  Il existe un Agent de lecture du journal pour chaque base de données publiée utilisant la réplication transactionnelle. Si une publication transactionnelle existe déjà dans la base de données, les paramètres de sécurité sont en lecture seule. Vous pouvez modifier les paramètres dans la boîte de dialogue **Propriétés de la publication** ; toutefois, toutes les publications transactionnelles seront affectées dans la base de données.  
  
 **Agent de lecture de la file d'attente**  
 Affiché pour les publications transactionnelles qui autorisent les abonnements pouvant être mis à jour. Cliquez sur **Paramètres de sécurité** pour spécifier les paramètres de sécurité dans la boîte de dialogue **Sécurité de l'Agent de lecture de la file d'attente** . Une fois l'exécution de cet Assistant terminée, un Agent de lecture de la file d'attente est créé. Sa création ne dépend aucunement de la création d'abonnements mis à jour en file d'attente. Si vous n'envisagez pas de créer des abonnements mis à jour en file d'attente , vous pouvez désactiver ce travail. Cliquez avec le bouton droit sur ce travail (nommé sous la forme : *[\<serveur_publication>].\<entier>*.) dans le dossier **Travaux de l’Agent** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **Désactiver**.  
  
 Cliquez sur **Aide** dans la la boîte de dialogue **Sécurité de l'Agent de lecture de la file d'attente** pour obtenir des informations supplémentaires sur les autorisations requises pour les comptes utilisés par l'Agent de lecture de la file d'attente.  
  
> [!NOTE]  
>  Il existe un Agent de lecture de la file d'attente pour chaque base de données de distribution (et pour tous les serveurs de publication qu'il sert). Si une publication transactionnelle qui autorise les abonnements de mise à jour en attente existe déjà sur un serveur de publication, qui utilise une base de données de distribution donnée, les paramètres de sécurité sont en lecture seule. Vous pouvez modifier le compte sous lequel l'Agent de lecture de la file d'attente est exécuté et les connexions sont établies dans la boîte de dialogue **Propriétés du serveur de distribution** . Toutefois, les publications sur tous les serveurs correspondants seront affectées dans la base de données de distribution.  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle](publish/create-updatable-subscription-to-transactional-publication.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Gérer les comptes de connexion et les mots de passe dans la réplication](../../relational-databases/replication/security/manage-logins-and-passwords-in-replication.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Présentation des Agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
