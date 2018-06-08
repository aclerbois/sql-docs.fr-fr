---
title: Programmation de tâches spécifiques | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: smo
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- Visual Basic [SMO]
- Visual C# [SMO]
- programming [SMO]
- SQL Server Management Objects, programming
- SQL Server Management Objects, tasks
- SMO [SQL Server], programming
- SMO [SQL Server], tasks
ms.assetid: a15949ef-88d9-4205-892e-0b66588b4fcc
caps.latest.revision: 32
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6878e0e429bf826d4dc3f6b57f8df42645f22dd5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="programming-specific-tasks"></a>Programmation de tâches spécifiques
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  La programmation de tâches spécifiques à l'aide d'objets SMO inclut des sujets complexes requis uniquement par les programmes ayant une fonction spécifique, par exemple la sauvegarde, l'analyse des statistiques, la réplication, la gestion des objets d'instance et la définition d'options de configuration.  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Utilisation de serveurs liés dans SMO](../../../relational-databases/server-management-objects-smo/tasks/using-linked-servers-in-smo.md)|Explique comment SMO utilise l'objet <xref:Microsoft.SqlServer.Management.Smo.LinkedServer> pour lier des serveurs OLE DB.|  
|[Configuration de SQL Server dans SMO](../../../relational-databases/server-management-objects-smo/tasks/configuring-sql-server-in-smo.md)|Explique comment afficher et modifier les paramètres de configuration de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans SMO.|  
|[Utilisation du partitionnement des tables et des index](../../../relational-databases/server-management-objects-smo/tasks/using-table-and-index-partitioning.md)|Explique comment utiliser le partitionnement d'index et de table dans SMO.|  
|[Utilisation de fichiers ou de groupes de fichiers pour stocker des données](../../../relational-databases/server-management-objects-smo/tasks/using-filegroups-and-files-to-store-data.md)|Explique comment utiliser des groupes de fichiers dans SMO.|  
|[Gestion des services et des paramètres réseau à l’aide du fournisseur WMI](../../../relational-databases/server-management-objects-smo/tasks/managing-services-and-network-settings-by-using-wmi-provider.md)|Décrit plusieurs méthodes permettant d'effectuer le suivi de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en utilisant l'objet <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> qui représente le fournisseur WMI pour la gestion de la configuration.|  
|[Utilisation des objets de base de données](../../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-database-objects.md)|Explique comment créer des classes d'instance qui représentent des objets de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[Gestion des utilisateurs, rôles et connexions](../../../relational-databases/server-management-objects-smo/tasks/managing-users-roles-and-logins.md)|Explique comment utiliser les rôles de sécurité dans SMO.|  
|[Octroi, révocation et refus d'autorisations](../../../relational-databases/server-management-objects-smo/tasks/granting-revoking-and-denying-permissions.md)|Explique comment utiliser SMO pour accorder, révoquer et refuser des autorisations aux utilisateurs ou membres d'un rôle.|  
|[Utilisation du chiffrement](../../../relational-databases/server-management-objects-smo/tasks/using-encryption.md)|Explique comment protéger des données à l'aide du chiffrement dans SMO.|  
|[Planification de tâches administratives automatiques dans SQL Server Agent](../../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md)|Explique comment utiliser l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour analyser, signaler et planifier des travaux dans SMO.|  
|[Sauvegarde et restauration de bases de données et de journaux de transactions](../../../relational-databases/server-management-objects-smo/tasks/backing-up-and-restoring-databases-and-transaction-logs.md)|Explique comment sauvegarder et restaurer des bases de données et des journaux des transactions dans SMO.|  
|[Création de scripts](../../../relational-databases/server-management-objects-smo/tasks/scripting.md)|Explique comment créer des scripts basés sur des objets et découvrir les dépendances entre les objets dans SMO.|  
|[Transfert de données](../../../relational-databases/server-management-objects-smo/tasks/transferring-data.md)|Explique comment transférer des données dans SMO.|  
|[Utilisation de la messagerie de base de données](../../../relational-databases/server-management-objects-smo/tasks/using-database-mail.md)|Explique comment SMO utilise les services de messagerie électronique.|  
|[Gestion de Service Broker](../../../relational-databases/server-management-objects-smo/tasks/managing-service-broker.md)|Explique comment installer Service Broker à l'aide de SMO.|  
|[Utilisation de schémas XML](../../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md)|Explique comment utiliser le type de données XML dans SMO.|  
|[Utilisation de synonymes](../../../relational-databases/server-management-objects-smo/tasks/using-synonyms.md)|Explique comment créer des synonymes dans SMO.|  
|[Utilisation de messages](../../../relational-databases/server-management-objects-smo/tasks/using-messages.md)|Explique comment utiliser des messages système et comment définir vos propres messages utilisateur.|  
|[Implémentation de la recherche en texte intégral](../../../relational-databases/server-management-objects-smo/tasks/implementing-full-text-search.md)|Explique comment implémenter les catalogues et index de recherche en texte intégral dans SMO.|  
|[Implémentation de points de terminaison](../../../relational-databases/server-management-objects-smo/tasks/implementing-endpoints.md)|Explique comment créer des points de terminaison afin de gérer les charges utiles pour la mise en miroir de bases de données, les requêtes SOAP et Service Broker.|  
|[Création et mise à jour des statistiques](../../../relational-databases/server-management-objects-smo/tasks/creating-and-updating-statistics.md)|Explique comment installer et analyser des statistiques dans une base de données, dans SMO.|  
|[Traçage et relecture d’événements](../../../relational-databases/server-management-objects-smo/tasks/tracing-and-replaying-events.md)|Décrit comment utiliser le **Trace** et **relire** objets dans SMO pour les événements de trace et de relecture.|  
  
  
