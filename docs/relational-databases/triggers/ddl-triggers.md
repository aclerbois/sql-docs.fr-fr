---
title: Déclencheurs DDL | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: triggers
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-ddl
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- DDL triggers, about DDL triggers
ms.assetid: 1a4a6564-9820-4a14-9305-2c0e9ea37454
caps.latest.revision: 35
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 19db6970253548827239adeb46a6ddc797dce59f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ddl-triggers"></a>Déclencheurs DDL
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Les déclencheurs DDL sont activés en réponse à différents événements DDL (Data Definition Language). Ces événements correspondent principalement à des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] commençant par les mots clés CREATE, ALTER, DROP, GRANT, DENY, REVOKE ou UPDATE STATISTICS. Certaines procédures stockées système qui effectuent des opérations de type DDL peuvent également activer des déclencheurs DDL.  
  
 Utilisez des déclencheurs DDL dans les cas suivants :  
  
-   Empêchez certaines modifications sur votre schéma de base de données.  
  
-   Faites en sorte qu'un événement se produise dans la base de données en réponse à une modification du schéma.  
  
-   Enregistrez des modifications ou des événements dans le schéma de la base de données.  
  
> [!IMPORTANT]  
>  Testez vos déclencheurs DDL afin de déterminer leurs réponses aux procédures stockées système qui sont exécutées. Par exemple, l’instruction CREATE TYPE et la procédure stockée **sp_addtype** activeront toutes deux un déclencheur DDL créé sur un événement CREATE_TYPE.  
  
## <a name="types-of-ddl-triggers"></a>Types de déclencheurs DDL  
 Déclencheur Transact-SQL DDL  
 Type spécial de procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] qui exécute une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en réponse à un événement d'étendue de serveur ou de base de données. Par exemple, un déclencheur DDL peut être activé si une instruction comme ALTER SERVER CONFIGURATION est exécutée ou si une table est supprimée à l'aide de DROP TABLE.  
  
 Déclencheur DDL CLR  
 Au lieu d'exécuter une procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] , un déclencheur CLR exécute une ou plusieurs méthodes écrites en code managé que les membres d'un assembly ont créées dans .NET Framework et téléchargées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les déclencheurs DDL ne s'activent qu'après l'exécution des instructions DDL de déclenchement. Les déclencheurs DDL ne peuvent pas être utilisés comme déclencheurs INSTEAD OF. Les déclencheurs DDL ne sont pas activés en réponse à des événements qui concernent les tables et les procédures stockées temporaires locales ou globales.  
  
 Les déclencheurs DDL ne créent pas les tables spéciales **inserted** et **deleted** .  
  
 Les informations sur un événement déclenché par un déclencheur DDL, ainsi que les modifications qui s'ensuivent, sont capturées au moyen de la fonction EVENTDATA.  
  
 Plusieurs déclencheurs à créer pour chaque événement DDL.  
  
 À la différence des déclencheurs DML, le champ d'action des déclencheurs DDL ne correspond pas aux schémas. Par conséquent, les fonctions OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY et OBJECTPROPERTYEX ne sont pas utilisables pour effectuer des requêtes de métadonnées à propos de déclencheurs DDL. Utilisez plutôt les affichages catalogue.  
  
 Les déclencheurs DDL dont l’étendue est le serveur figurent dans le dossier **Déclencheurs** de l’Explorateur d’objets SQL Server Management Studio. Ce dossier se trouve dans le dossier **Objets serveur** . Les déclencheurs DDL au niveau de la base de données figurent dans le dossier **Database Triggers** . Ce dossier se trouve dans le dossier **Programmabilité** de la base de données correspondante.  
  
> [!IMPORTANT]  
>  Un code malveillant présent dans des déclencheurs peut s'exécuter sous des privilèges promus. Pour plus d’informations sur les moyens de lutte contre ces malveillances, consultez [Gérer la sécurité des déclencheurs](../../relational-databases/triggers/manage-trigger-security.md).  
  
## <a name="ddl-trigger-scope"></a>Étendue du déclencheur DDL  
 Les déclencheurs DDL peuvent être activés en réponse à un événement [!INCLUDE[tsql](../../includes/tsql-md.md)] traité dans la base de données actuelle ou sur le serveur actuel. L'étendue du déclencheur dépend de l'événement. Par exemple, un déclencheur DDL créé pour être activé en réponse à un événement CREATE_TABLE se déclenchera à chaque fois qu'un événement CREATE_TABLE se produira dans la base de données ou sur l’instance de serveur. Un déclencheur DDL créé pour être activé en réponse à un événement CREATE_LOGIN ne se déclenchera que si un événement CREATE_LOGIN se produit dans l'instance de serveur.  
  
 Dans l'exemple suivant, le déclencheur DDL `safety` est activé chaque fois qu'un événement `DROP_TABLE` ou `ALTER_TABLE` se produit dans la base de données.  
  
```  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
```  
  
 Dans l'exemple suivant, un déclencheur DDL affiche un message si un événement `CREATE_DATABASE` se produit sur l'instance de serveur actuelle. Cet exemple utilise la fonction `EVENTDATA` pour extraire le texte de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondante. Pour plus d’informations sur l’utilisation de EVENTDATA avec des déclencheurs DDL, consultez [Utiliser la fonction EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
```  
IF EXISTS (SELECT * FROM sys.server_triggers  
    WHERE name = 'ddl_trig_database')  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
CREATE TRIGGER ddl_trig_database   
ON ALL SERVER   
FOR CREATE_DATABASE   
AS   
    PRINT 'Database Created.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
GO  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
  
```  
  
 La liste qui mappe les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] aux étendues qui peuvent leur être spécifiées sont disponibles par l'intermédiaire des liens fournis dans la section « Sélection d'une instruction DDL particulière pour activer un déclencheur DDL », plus loin dans cette rubrique.  
  
 Les déclencheurs DDL dont l'étendue est la base de données sont stockés sous forme d'objets dans la base de données où ils sont créés. Des déclencheurs DDL peuvent être créés dans la base de données **master** ; ils se comportent exactement comme ceux créés dans les bases de données conçues par l’utilisateur. Vous pouvez obtenir des informations concernant les déclencheurs DDL en interrogeant l’affichage catalogue **sys.server_triggers** . Vous pouvez interroger **sys.triggers** dans le contexte de base de données dans lequel ils sont créés, ou en spécifiant le nom de la base de données comme identificateur, par exemple **master.sys.triggers**.  
  
 Les déclencheurs DDL dont l’étendue est le serveur sont stockés sous forme d’objets dans la base de données **master** . Toutefois, vous pouvez obtenir des informations sur les déclencheurs DDL dont l’étendue est limitée au serveur en interrogeant l’affichage catalogue **sys.server_triggers** dans n’importe quel contexte de base de données.  
  
## <a name="specifying-a-transact-sql-statement-or-group-of-statements"></a>Spécification d'une instruction ou d'un groupe d'instructions Transact-SQL  
  
### <a name="selecting-a-particular-ddl-statement-to-fire-a-ddl-trigger"></a>Sélection d'une instruction DDL particulière pour activer un déclencheur DDL  
 Les déclencheurs DDL peuvent être conçus pour être activés après l'exécution d'une ou de plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Dans l'exemple précédent, le déclencheur `safety` est activé après n’importe quel événement `DROP_TABLE` ou `ALTER_TABLE` . Pour obtenir la liste des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à spécifier pour activer un déclencheur et connaître l’étendue selon laquelle ce déclencheur peut être activé, consultez [Événements DDL](../../relational-databases/triggers/ddl-events.md).  
  
### <a name="selecting-a-predefined-group-of-ddl-statements-to-fire-a-ddl-trigger"></a>Sélection d'un groupe prédéfini d'instructions DDL pour activer un déclencheur DDL  
 Un déclencheur DDL peut être activé après l'exécution de n'importe quel événement [!INCLUDE[tsql](../../includes/tsql-md.md)] appartenant à un groupe prédéfini d'événements comparables. Par exemple, si vous souhaitez activer un déclencheur DDL après l'exécution d'une instruction CREATE TABLE, ALTER TABLE ou DROP TABLE quelconque, vous pouvez spécifier FOR DDL_TABLE_EVENTS dans l'instruction CREATE TRIGGER. Après l’exécution de CREATE TRIGGER, les événements qui sont couverts par un groupe d’événements sont ajoutés à l’affichage catalogue **sys.trigger_events** .  
  
 Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], si un déclencheur est créé sur un groupe d’événements, **sys.trigger_events** n’inclut pas d’informations sur le groupe d’événements. **sys.trigger_events** inclut des informations uniquement sur les événements couverts par ce groupe. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et version ultérieure, **sys.trigger_events** rend persistantes les métadonnées relatives au groupe d’événement sur lequel les déclencheurs sont créés, et également celles qui concernent chacun des événements couverts par le groupe. Par conséquent, les modifications apportées aux événements couverts par les groupes d'événement dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions supérieures ne s'appliquent pas aux déclencheurs DDL créés sur ces groupes d'événement dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 Pour obtenir la liste des groupes d’instructions DDL prédéfinis disponibles pour les déclencheurs DDL, connaître les instructions particulières qu’ils couvrent et les étendues selon lesquelles ces groupes d’événements peuvent être programmés, consultez [Groupes d’événements DDL](../../relational-databases/triggers/ddl-event-groups.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Décrit comment créer, modifier, supprimer ou désactiver les déclencheurs DDL.|[Implémenter des déclencheurs DDL](../../relational-databases/triggers/implement-ddl-triggers.md)|  
|Décrit comment créer un déclencheur DDL CLR.|[Créer des déclencheurs CLR](../../relational-databases/triggers/create-clr-triggers.md)|  
|Décrit comment retourner des informations sur les déclencheurs DDL.|[Obtenir des informations sur les déclencheurs DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md)|  
|Décrit comment retourner des informations sur un événement qui lance un déclencheur DDL à l'aide de la fonction EVENTDATA.|[Utiliser la fonction EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md)|  
|Décrit comment gérer la sécurité du déclencheur.|[Gérer la sécurité des déclencheurs](../../relational-databases/triggers/manage-trigger-security.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Déclencheurs DML](../../relational-databases/triggers/dml-triggers.md)   
 [Déclencheurs de connexion](../../relational-databases/triggers/logon-triggers.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
  
  
