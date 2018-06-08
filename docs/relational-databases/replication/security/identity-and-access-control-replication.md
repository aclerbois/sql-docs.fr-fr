---
title: Identité et contrôle d’accès (réplication) | Microsoft Docs
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
helpviewer_keywords:
- access controls [SQL Server replication]
- security [SQL Server replication], identity and access control
- authentication [SQL Server replication]
- identity [Replication]
ms.assetid: 4da0e793-1ee4-4f69-a80b-45c6732a238d
caps.latest.revision: 8
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ca8cbf2c68de39fc5f918390246a762955cf48a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="identity-and-access-control-replication"></a>Identité et contrôle d'accès (réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'authentification est le processus selon lequel une entité (généralement un ordinateur) vérifie qu'une autre entité, également appelée *entité principale*, (généralement un autre ordinateur ou un autre utilisateur) est bien ce qu'elle prétend être. L'autorisation est le processus selon lequel une entité principale authentifiée bénéficie de l'accès aux ressources, telles qu'un fichier du système de fichiers ou une table d'une base de données.  
  
 La sécurité de réplication fait appel à l'authentification et à l'autorisation pour contrôler l'accès aux objets de base de données répliqués, aux ordinateurs et aux agents impliqués dans le traitement de la réplication. Trois mécanismes régissent son fonctionnement :  
  
-   Sécurité de l'agent  
  
     Le modèle de sécurité de l'agent de réplication permet un contrôle fin sur les comptes sous lesquels les agents de réplication s'exécutent et établissent des connexions. Pour plus d'informations sur le modèle de sécurité de l'agent, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md). Pour plus d’informations sur la définition des connexions et des mots de passe pour les agents, consultez [Gérer les connexions et les mots de passe dans la réplication](../../../relational-databases/replication/security/manage-logins-and-passwords-in-replication.md).  
  
-   Rôles d'administration  
  
     Vérifiez que les rôles de base de données et de serveur appropriés sont utilisés pour la configuration, la maintenance et le traitement de la réplication. Pour plus d’informations, voir [Security Role Requirements for Replication](../../../relational-databases/replication/security/security-role-requirements-for-replication.md).  
  
-   Liste d'accès à la publication  
  
     Octroyez l'accès aux publications par l'intermédiaire de cette liste. La liste d'accès à la publication fonctionne de manière similaire à une liste de contrôle d'accès [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Lorsqu'un Abonné se connecte au serveur de publication ou au serveur de distribution et souhaite accéder à une publication, les informations d'authentification passées par l'agent sont comparées à celles de la liste d'accès à la publication. Pour obtenir plus d’informations et pour connaître les bonnes pratiques concernant la liste d’accès à la publication, consultez [Sécuriser le serveur de publication](../../../relational-databases/replication/security/secure-the-publisher.md).  
  
## <a name="filtering-published-data"></a>Filtrage des données publiées  
 Outre l'utilisation de l'authentification et de l'autorisation pour contrôler l'accès aux données et aux objets répliqués, la réplication comprend deux options permettant de contrôler les données disponibles sur un Abonné : filtrage des colonnes et filtrage des lignes. Pour plus d’informations sur le filtrage, consultez [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md).  
  
 Lorsque vous définissez un article, vous pouvez publier uniquement les colonnes nécessaires pour la publication et omettre celles dont vous n'avez pas besoin ou qui comportent des données sensibles. Par exemple, lorsque vous publiez la table **Customer** de la base de données Adventure Works à l'attention des commerciaux sur le terrain, vous pouvez omettre la colonne **AnnualSales** , qui ne concerne que les cadres de l'entreprise.  
  
 Le filtrage des données publiées vous permet de restreindre l'accès aux données et de spécifier les données qui sont disponibles sur l'Abonné. Par exemple, vous pouvez filtrer la table **Customer** afin que les partenaires de l'entreprise ne reçoivent que les informations sur les clients dont la colonne **ShareInfo** possède la valeur « Oui ». Pour les réplications de fusion, vous devez tenir compte de certains points de sécurité si vous utilisez un filtre paramétré qui inclut HOST_NAME(). Pour plus d'informations, consultez la section « Filtrage avec HOST_NAME() » dans [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurité et protection &#40;Réplication&#41;](../../../relational-databases/replication/security/security-and-protection-replication.md)   
 [Vue d’ensemble de la sécurité &#40;réplication&#41;](../../../relational-databases/replication/security/security-overview-replication.md)   
 [Limitation des menaces et des risques de vulnérabilité &#40;réplication&#41;](../../../relational-databases/replication/security/threat-and-vulnerability-mitigation-replication.md)  
  
  
