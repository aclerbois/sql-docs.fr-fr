---
title: Notifications d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: service-broker
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event notifications, about
- events [SQL Server], notifications
ms.assetid: 4da73ca1-6c06-4e96-8ab8-2ecba30b6c86
caps.latest.revision: 18
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 37602cb45dabc2c00d47dd3c7be28ed6e8fe63e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="event-notifications"></a>Notifications d'événements
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les notifications d'événements envoient des informations sur les événements à un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Les notifications d'événements sont exécutées en réponse à une variété d'instructions DDL (Data Definition Language) [!INCLUDE[tsql](../../includes/tsql-md.md)] et d'événements Trace SQL, par l'envoi d'informations relatives à ces événements à un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
 Les notifications d'événements peuvent être utilisées pour effectuer les opérations suivantes :  
  
-   enregistrer dans un journal et examiner les modifications ou l'activité de la base de données ;  
  
-   réaliser une action en réponse à un événement de manière asynchrone plutôt que synchrone.  
  
 Les notifications d'événements peuvent constituer une alternative de programmation aux déclencheurs DDL et à Trace SQL.  
  
## <a name="event-notifications-benefits"></a>Avantages des notifications d'événements  
 Elles sont exécutées de manière asynchrone, en dehors de la portée d'une transaction. Ainsi, contrairement aux déclencheurs DDL, les notifications d'événements peuvent être utilisées dans une application de base de données pour répondre à des événements sans recourir aux ressources définies par la transaction immédiate.  
  
 Contrairement à Trace SQL, les notifications d'événements peuvent être utilisées pour exécuter une action dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en réponse à un événement Trace SQL.  
  
 Les données des événements peuvent être utilisées par les applications exécutées avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour en suivre la progression et prendre des décisions. Par exemple, la notification d'événement suivante envoie un avis à un certain service chaque fois qu'une instruction `ALTER TABLE` est émise dans l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
USE AdventureWorks2012;  
GO  
CREATE EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE '//Adventure-Works.com/ArchiveService' ,  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
```  
  
## <a name="event-notifications-concepts"></a>Concepts de notifications d'événements  
 Lorsqu'une notification d'événement est créée, une ou plusieurs conversations [!INCLUDE[ssSB](../../includes/sssb-md.md)] entre une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le service cible spécifié sont ouvertes. Les conversations restent en général ouvertes tant que la notification d'événement existe en tant qu'objet sur l'instance de serveur. Dans certains cas d'erreur, les conversations peuvent être interrompues avant la suppression de la notification d'événement. Ces conversations ne sont jamais partagées entre les notifications d'événements. Toutes les notifications d'événements possèdent leurs propres conversations exclusives. L'interruption d'une conversation interdit explicitement au service cible de recevoir davantage de messages ; en outre, la conversation ne rouvrira pas au déclenchement suivant de la notification d'événement.  
  
 Les informations sur l’événement sont remises au service [!INCLUDE[ssSB](../../includes/sssb-md.md)] sous forme de variable de type **xml** qui fournit des informations sur l’heure à laquelle un événement se produit, l’objet de base de données concerné, l’instruction par lot [!INCLUDE[tsql](../../includes/tsql-md.md)] impliquée, ainsi que d’autres informations. Pour plus d’informations sur le schéma XML produit par les notifications d’événements, consultez [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md).  
  
### <a name="event-notifications-vs-triggers"></a>Notifications d'événements et Déclencheurs  
 Le tableau suivant répertorie les similitudes et les différences entre les déclencheurs et les notifications d'événements.  
  
|Déclencheurs|Notifications d'événements|  
|--------------|-------------------------|  
|Les déclencheurs DML répondent aux événements DML (Data Manipulation Language, ou langage de manipulation de données). Les déclencheurs DDL répondent aux événements DDL (Data Definition Language, ou langage de définition de données).|Les notifications d'événements répondent aux événements DDL et à un sous-ensemble d'événements de Trace SQL.|  
|Les déclencheurs peuvent exécuter Transact-SQL ou le code managé CLR (Common Language Runtime).|Les notifications d'événements n'exécutent aucun code. En fait, elles envoient des messages **xml** à un service Service Broker.|  
|Les déclencheurs sont traités de manière synchrone, dans le cadre des transactions qui provoquent leur déclenchement.|Les notifications d'événements peuvent être traitées de manière synchrone, et elles ne sont pas exécutées dans le cadre des transactions qui provoquent leur déclenchement.|  
|Le consommateur d'un déclencheur est étroitement lié à l'événement qui provoque son déclenchement.|Le consommateur d'une notification d'événement est dissocié de l'événement qui provoque son déclenchement.|  
|Les déclencheurs doivent être traités sur le serveur local.|Les notifications d'événements peuvent être traitées sur un serveur distant.|  
|Les déclencheurs peuvent être restaurés.|Les notifications d'événements ne peuvent pas être restaurées.|  
|Les noms de déclencheurs DML ont une portée de schéma. Les noms de déclencheurs DDL sont limités au niveau de la base de données ou du serveur.|Les noms de notifications d'événements sont limités par le serveur ou la base de données. Les notifications d'événements sur un événement QUEUE_ACTIVATION sont limitées à une file d'attente spécifique.|  
|Les déclencheurs DML sont détenus par le propriétaire des tables sur lesquelles ils sont appliqués.|Le propriétaire d'une notification d'événements sur une file d'attente peut être différent de celui de l'objet auquel la notification est appliquée.|  
|Les déclencheurs prennent en charge la clause EXECUTE AS.|Les notifications d'événements ne prennent pas en charge la clause EXECUTE AS.|  
|Les informations d’événement de déclencheur DDL peuvent être capturées à l’aide de la fonction EVENTDATA, qui retourne un type de données **xml** .|Les notifications d’événements envoient des informations d’événement **xml** à un service Service Broker. Les informations sont mises en forme dans le même schéma que celui de la fonction EVENTDATA.|  
|Les métadonnées concernant les déclencheurs se trouvent dans les affichages catalogue **sys.triggers** et **sys.server_triggers** .|Les métadonnées concernant les notifications d’événements se trouvent dans les affichages catalogue **sys.event_notifications** et **sys.server_event_notifications**.|  
  
### <a name="event-notifications-vs-sql-trace"></a>Notifications d'événements et Trace SQL  
 Le tableau suivant répertorie les similitudes et les différences dans l'utilisation des notifications d'événements et de Trace SQL dans le cadre de la surveillance des événements de serveur.  
  
|Trace SQL|Notifications d'événements|  
|---------------|-------------------------|  
|Trace SQL n'engendre aucun surcroît d'utilisation de temps système lié aux transactions. L'empaquetage des données est efficace.|Les ressources système sont sollicitées davantage lors de la création des données d'événements au format XML et de l'envoi des notifications d'événements.|  
|Trace SQL peut surveiller toutes les classes d'événements de trace.|Les notifications d'événements peuvent surveiller un sous-ensemble des classes d'événements de trace ainsi que tous les événements DDL (Data Definition Language).|  
|Vous pouvez personnaliser les colonnes de données à générer dans l'événement de trace.|Le schéma des données d'événements au format XML renvoyées par les notifications d'événements est fixe.|  
|Les événements de trace générés par DDL sont toujours générés, même si l'instruction DDL est annulée.|Les notifications d'événements ne se déclenchent pas si l'événement de l'instruction DDL correspondante est annulé.|  
|La gestion du flux intermédiaire des données d'événement de trace implique le remplissage et la gestion de fichiers ou de tables de trace.|La gestion intermédiaire des données de notification d'événements est accomplie automatiquement via les files d'attente de Service Broker.|  
|Les traces doivent être redémarrées à chaque démarrage du serveur.|Après avoir été inscrites, les notifications d'événements perdurent au fil des cycles serveur et sont incluses dans une transaction.|  
|Après avoir été lancé, le déclenchement des traces ne peut plus être contrôlé. Les options d'heure d'arrêt et de filtrage permettent de spécifier l'heure de lancement. Les traces sont accessibles par interrogation du fichier de trace correspondant.|Une notification d'événements peut être contrôlée en exécutant l'instruction WAITFOR sur la file d'attente qui reçoit le message généré par la notification. Elle est accessible par interrogation de la file d'attente.|  
|ALTER TRACE est l'autorisation minimale requise pour créer une trace. Une autorisation permettant de créer un fichier de trace sur l'ordinateur est également nécessaire.|L'autorisation minimale dépend du type de notification d'événements créée. L'autorisation RECEIVE sur la file d'attente correspondante est également requise.|  
|Les traces peuvent être reçues à distance.|Les notifications d'événements ne peuvent pas être reçues à distance.|  
|Les événements de trace sont mis en œuvre au moyen de procédures stockées système.|Les notifications d’événements sont implémentées à l’aide d’une combinaison d’instructions [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssSB](../../includes/sssb-md.md)][!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|Les données d’événements de trace sont accessibles par programmation en interrogeant la table de trace correspondante, en analysant le fichier de trace ou au moyen de la classe TraceReader SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects).|Les données d'événements sont accessibles par programme en émettant une requête XQuery sur les données d'événements au format XML ou au moyen de classes Event SMO.|  
  
## <a name="event-notification-tasks"></a>Tâches relatives aux notifications d'événements  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Décrit comment créer et implémenter des notifications d'événements.|[Implémenter des notifications d'événements](../../relational-databases/service-broker/implement-event-notifications.md)|  
|Décrit comment configurer la sécurité du dialogue [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour les notifications d'événements qui envoient des messages à Service Broker sur un serveur distant.|[Configurer la sécurité du dialogue pour les notifications d'événements](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md)|  
|Décrit comment retourner des informations sur les notifications d'événements.|[Obtenir des informations concernant les notifications d'événements](../../relational-databases/service-broker/get-information-about-event-notifications.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md)   
 [Déclencheurs DML](../../relational-databases/triggers/dml-triggers.md)   
 [Trace SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
