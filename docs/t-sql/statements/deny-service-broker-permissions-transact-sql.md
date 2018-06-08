---
title: Autorisations DENY dans Service Broker (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [Service Broker]
- routes [Service Broker], permissions
- Service Broker, permissions
- DENY statement, Service Broker
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- denying permissions [SQL Server], Service Broker
- contracts [Service Broker], permissions
- services [Service Broker], permissions
ms.assetid: 7c6de71b-865c-41db-9413-ad9b3562e579
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1db9fcfc9be2c8db8380f5ecd1f3d21b68ec36b8
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="deny-service-broker-permissions-transact-sql"></a>Autorisations DENY dans Service Broker (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Refus d'autorisations sur un contrat, un type de message, une liaison de service distant, un itinéraire ou un service [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DENY permission  [ ,...n ] ON  
    {    
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    TO database_principal [ ,...n ]   
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
## <a name="arguments"></a>Arguments  
 *permission*  
 Spécifie une autorisation qui peut être refusée sur un élément sécurisable [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 CONTRACT **::***contract_name*  
 Spécifie le contrat sur lequel l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 MESSAGE TYPE **::***message_type_name*  
 Spécifie le type de message sur lequel l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 REMOTE SERVICE BINDING **::***remote_binding_name*  
 Spécifie la liaison de service distant sur laquelle l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 ROUTE **::***route_name*  
 Spécifie l'itinéraire sur lequel l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 SERVICE **::***message_type_name*  
 Spécifie le service sur lequel l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *database_principal*  
 Spécifie le principal auquel l'autorisation est refusée. Il peut s'agir :  
  
-   Utilisateur de base de données  
-   Rôle de base de données  
-   Rôle d'application  
-   Utilisateur de base de données mappé à une connexion Windows  
-   Utilisateur de base de données mappé à un groupe Windows  
-   Utilisateur de base de données mappé à un certificat  
-   Utilisateur de base de données mappé à une clé asymétrique  
-   Utilisateur de base de données mappé à un principal de serveur  
  
CASCADE  
 Indique que l'autorisation à refuser est également refusée pour les autres principaux auxquels elle a été accordée par ce principal.  
  
*denying_principal*  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de refuser l'autorisation. Il peut s'agir :  
  
-   Utilisateur de base de données  
-   Rôle de base de données  
-   Rôle d'application  
-   Utilisateur de base de données mappé à une connexion Windows  
-   Utilisateur de base de données mappé à un groupe Windows  
-   Utilisateur de base de données mappé à un certificat  
-   Utilisateur de base de données mappé à une clé asymétrique  
-   Utilisateur de base de données mappé à un principal de serveur  
  
## <a name="remarks"></a>Notes   
  
## <a name="service-broker-contracts"></a>Contrats Service Broker  
 Un contrat [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être refusées sur un contrat [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un contrat Service Broker|Impliquée par une autorisation d'un contrat Service Broker|Impliquée par une autorisation de base de données|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Types de messages Service Broker  
 Un type de message [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être refusées sur un type de message [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un type de message Service Broker|Impliquée par une autorisation d'un type de message Service Broker|Impliquée par une autorisation de base de données|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Liaisons de service distant Service Broker  
 Une liaison de service distant [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être refusées sur une liaison de service distant [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation de liaison de service distant Service Broker|Impliquée par une autorisation de liaison de service distant Service Broker|Impliquée par une autorisation de base de données|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Itinéraires Service Broker  
 Un itinéraire [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être refusées sur un itinéraire [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un itinéraire Service Broker|Impliquée par une autorisation d'un itinéraire Service Broker|Impliquée par une autorisation de base de données|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Services Service Broker  
 Un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être refusées sur un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un service Service Broker|Impliquée par une autorisation d'un service Service Broker|Impliquée par une autorisation de base de données|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|SEND|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite une autorisation CONTROL sur le contrat, le type de message, la liaison de service distant, l’itinéraire ou le service [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Si vous utilisez la clause AS, le principal spécifié doit être propriétaire de l'élément sécurisable auquel les autorisations sont refusées.  
  
## <a name="see-also"></a> Voir aussi  
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorisations REVOKE dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)  
  
  
