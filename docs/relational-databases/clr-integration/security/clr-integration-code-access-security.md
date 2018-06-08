---
title: Sécurité d’accès du Code CLR Integration | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- UNSAFE assemblies
- permissions [CLR integration]
- common language runtime [SQL Server], security
- SAFE assemblies
- code access security [CLR integration]
- EXTERNAL_ACCESS assemblies
ms.assetid: 2111cfe0-d5e0-43b1-93c3-e994ac0e9729
caps.latest.revision: 28
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 8b71c017161b2f696872c6d2dd2ba7843a474bdc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-integration-code-access-security"></a>Sécurité d'accès du code de l'intégration du CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le common language runtime (CLR) prend en charge un modèle de sécurité appelé sécurité d’accès du code pour le code managé. Dans ce modèle, les autorisations sont accordées aux assemblys selon l'identité du code. Pour plus d'informations, consultez la section relative à la sécurité d'accès du code dans le kit de développement logiciel (SDK) .NET Framework.  
  
 La stratégie de sécurité qui détermine les autorisations accordée aux assemblys est définie dans trois emplacements différents :  
  
-   Stratégie de l'ordinateur : il s'agit de la stratégie appliquée pour tout le code managé qui s'exécute sur l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est installé.  
  
-   Stratégie de l'utilisateur : il s'agit de la stratégie appliquée pour le code managé hébergé par un processus. Pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la stratégie de l'utilisateur est spécifique au compte Windows sur lequel le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute.  
  
-   Stratégie de l'hôte : il s'agit de la stratégie configurée par l'hôte du CLR (dans ce cas, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) qui est appliquée pour le code managé qui s'exécute sur cet hôte.  
  
 Le mécanisme de sécurité d'accès du code pris en charge par le CLR est basé sur la supposition que le module d'exécution peut héberger à la fois du code d'un niveau de confiance total et d'un niveau de confiance partiel. Les ressources qui sont protégées par la sécurité d’accès du code CLR sont généralement intégrées à interfaces de programmation d’application managée qui nécessitent l’autorisation correspondante avant d’autoriser l’accès à la ressource. La demande d'autorisation est satisfaite seulement si tous les appelants (au niveau de l'assembly) de la pile d'appels ont l'autorisation pour la ressource correspondante.  
  
 Le jeu d'autorisations de sécurité d'accès du code accordées au code managé en cas d'exécution à l'intérieur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est l'intersection du jeu d'autorisations accordées par les trois niveaux de stratégie précités. Même si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] accorde un jeu d'autorisations à un assembly chargé dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le jeu éventuel d'autorisations accordées au code utilisateur peut être restreint davantage par les stratégies de l'utilisateur et de l'ordinateur.  
  
## <a name="sql-server-host-policy-level-permission-sets"></a>Jeux d'autorisations au niveau stratégie de l'hôte de SQL Server  
 Le jeu d'autorisations de sécurité d'accès du code accordées aux assemblys par le niveau de stratégie de l'hôte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est déterminé par le jeu d'autorisations spécifié lors de la création de l'assembly. Il existe trois jeux d’autorisations : **SAFE**, **EXTERNAL_ACCESS** et **UNSAFE** (spécifié à l’aide de la **PERMISSION_SET** option de [ CRÉER l’ASSEMBLY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-assembly-transact-sql.md)).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournit un niveau de stratégie de sécurité au niveau de l'hôte au CLR (Common Language Runtime) quand il l'héberge ; cette stratégie est un niveau de stratégie supplémentaire sous les deux niveaux de stratégie qui sont toujours effectifs. Cette stratégie est définie pour chaque domaine d'application qui est créé par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cette stratégie n'est pas destinée au domaine d'application par défaut appliqué lorsque [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crée une instance du CLR.  
  
 La stratégie de niveau hôte de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est une combinaison de la stratégie fixe de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour les assemblys système et de la stratégie spécifiée par l’utilisateur pour les assemblys utilisateur.  
  
 La stratégie fixe pour les assemblys CLR et les assemblys système [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] leur accorde une confiance totale.  
  
 La partie spécifiée par l'utilisateur de la stratégie de l'hôte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est basée sur le propriétaire de l'assembly spécifiant un des trois compartiments d'autorisation pour chaque assembly. Pour plus d'informations sur les autorisations de sécurité répertoriées ci-dessous, consultez le kit de développement logiciel SDK .NET Framework.  
  
### <a name="safe"></a>SAFE  
 Seul un calcul interne et accès aux données locales sont autorisées. **SAFE** est le jeu d’autorisations plus restrictif. Le code exécuté par un assembly avec **SAFE** autorisations ne peuvent pas accéder aux ressources système externes telles que des fichiers, le réseau, les variables d’environnement ou le Registre.  
  
 **SAFE** les assemblys ont les autorisations et les valeurs suivantes :  
  
|Autorisation|Valeur(s)/description|  
|----------------|-----------------------------|  
|**SecurityPermission**|**L’exécution :** l’autorisation d’exécuter du code managé.|  
|**SqlClientPermission**|**Connexion de contexte = true**, **connexion contextuelle = yes**: uniquement la connexion de contexte peut être utilisée et la chaîne de connexion ne peut spécifier une valeur de « connexion contextuelle = true » ou « connexion contextuelle = yes ».<br /><br /> **AllowBlankPassword = false :** mots de passe vides ne sont pas autorisées.|  
  
### <a name="externalaccess"></a>EXTERNAL_ACCESS  
 Assemblys EXTERNAL_ACCESS ont les mêmes autorisations que **SAFE** assemblys, avec possibilité en prime d’accéder aux ressources système externes telles que des fichiers, des réseaux, des variables d’environnement et le Registre.  
  
 **EXTERNAL_ACCESS** assemblys ont également les autorisations et les valeurs suivantes :  
  
|Autorisation|Valeur(s)/description|  
|----------------|-----------------------------|  
|**DistributedTransactionPermission**|**Unrestricted :** les transactions distribuées sont autorisées.|  
|**DNSPermission**|**Unrestricted :** autorisation pour demander des informations à partir des serveurs de noms de domaine.|  
|**EnvironmentPermission**|**Unrestricted :** complète l’accès aux variables d’environnement système et utilisateur est autorisé.|  
|**EventLogPermission**|**Administrer :** les actions suivantes sont autorisées : création d’une source d’événement, la lecture des journaux existants, suppression des sources d’événements ou de journaux, réponse aux entrées, effacement d’un journal des événements, écouter des événements et l’accès à une collection de tous les journaux des événements.|  
|**FileIOPermission**|**Unrestricted :** accès complet aux fichiers et dossiers n’est autorisé.|  
|**KeyContainerPermission**|**Unrestricted :** complète l’accès aux conteneurs de clé est autorisé.|  
|**NetworkInformationPermission**|**Accès :** instruction ping est autorisée.|  
|**RegistryPermission**|Permet de droits de lecture pour **HKEY_CLASSES_ROOT**, **HKEY_LOCAL_MACHINE**, **HKEY_CURRENT_USER**, **HKEY_CURRENT_CONFIG**et  **HKEY_USERS.**|  
|**SecurityPermission**|**Assertion :** possibilité de déclarer que tous les appelants de ce code ont l’autorisation requise pour l’opération.<br /><br /> **ControlPrincipal :** possibilité de manipuler l’objet principal.<br /><br /> **L’exécution :** l’autorisation d’exécuter du code managé.<br /><br /> **SerializationFormatter :** capacité à fournir des services de sérialisation.|  
|**SmtpPermission**|**Accès :** les connexions sortantes vers le port 25 hôte SMTP sont autorisées.|  
|**SocketPermission**|**Se connecter :** des connexions sortantes (tous les ports, tous les protocoles) sur une adresse de transport sont autorisées.|  
|**SqlClientPermission**|**Unrestricted :** complète l’accès à la source de données est autorisé.|  
|**StorePermission**|**Unrestricted :** d’un accès complet à un certificat X.509 magasins est autorisée.|  
|**WebPermission**|**Se connecter :** des connexions sortantes aux ressources web sont autorisées.|  
  
### <a name="unsafe"></a>UNSAFE  
 UNSAFE offre aux assemblies un accès sans restriction aux ressources, à la fois à l'intérieur et à l'extérieur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Code qui s’exécute dans un **UNSAFE** assembly peut également appeler de code non managé.  
  
 **UNSAFE** assemblys figurent **FullTrust**.  
  
> [!IMPORTANT]  
>  **SAFE** est le paramètre d’autorisation recommandé pour les assemblys qui effectuent des tâches de gestion de calcul et des données sans accéder à des ressources en dehors de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **EXTERNAL_ACCESS** est recommandée pour les assemblys qui accèdent aux ressources en dehors de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **EXTERNAL_ACCESS** par défaut, les assemblys sont exécutées en tant que le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] compte de service. Il est possible pour **EXTERNAL_ACCESS** code explicitement emprunter l’identité de contexte de sécurité de l’authentification Windows de l’appelant. Étant donné que la valeur par défaut consiste à exécuter en tant que le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] compte de service, l’autorisation d’exécuter **EXTERNAL_ACCESS** ne devrait être donnée qu’aux connexions approuvées pour exécuter en tant que le compte de service. Du point de vue de la sécurité, **EXTERNAL_ACCESS** et **UNSAFE** assemblys sont identiques. Toutefois, **EXTERNAL_ACCESS** assemblys fournissent diverses protections de fiabilité et de robustesse qui ne sont pas **UNSAFE** assemblys. Spécification de **UNSAFE** permet au code de l’assembly peut effectuer des opérations illégales par rapport à la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] espace de processus et donc de compromettre la robustesse et l’évolutivité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la création d’assemblys CLR dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [la gestion des assemblys d’intégration du CLR](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md).  
  
## <a name="accessing-external-resources"></a>Accès aux ressources externes  
 Si un type défini par l’utilisateur (UDT), une procédure stockée ou autre type d’assembly de construction est inscrit auprès du **SAFE** de jeu d’autorisations, puis dans la construction de l’exécution de code managé ne peut pas accéder aux ressources externes. Toutefois, si le **EXTERNAL_ACCESS** ou **UNSAFE** jeux d’autorisations sont spécifiés, et le code managé tente d’accéder aux ressources externes, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] applique les règles suivantes :  
  
|Si|Alors|  
|--------|----------|  
|Le contexte d'exécution correspond à une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|Les tentatives d'accéder aux ressources externes sont refusées et une exception de sécurité est levée.|  
|Le contexte d'exécution correspond à une connexion Windows et le contexte d'exécution est l'appelant d'origine.|La ressource externe est accédée sous le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|L'appelant n'est pas l'appelant d'origine.|L'accès est refusé et une exception de sécurité est levée.|  
|Le contexte d’exécution correspond à une connexion Windows et le contexte d’exécution est l’appelant d’origine et l’appelant n’a pas été empruntée.|L'accès s'effectue en utilisant le contexte de sécurité de l'appelant, et non le compte de service.|  
  
## <a name="permission-set-summary"></a>Synthèse des jeux d'autorisations  
 Le graphique suivant résume les restrictions et les autorisations accordées à la **SAFE**, **EXTERNAL_ACCESS**, et **UNSAFE** jeux d’autorisations.  
  
|||||  
|-|-|-|-|  
||**SAFE**|**EXTERNAL_ACCESS**|**UNSAFE**|  
|**Autorisations de sécurité d’accès au code**|Exécution uniquement|Exécution + accès aux ressources externes|Illimité (y compris P/Invoke)|  
|**Restrictions du modèle de programmation**|Oui|Oui|Aucune restriction|  
|**Vérifiabilité requise**|Oui|Oui|non|  
|**Accès aux données locales**|Oui|Oui|Oui|  
|**Possibilité d’appeler du code natif**|non|Non|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité d’intégration du CLR](../../../relational-databases/clr-integration/security/clr-integration-security.md)   
 [Attributs de Protection d’hôte et de la programmation de l’intégration CLR](../../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md)   
 [Restrictions du modèle de programmation CLR Integration](../../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md)   
 [Environnement hébergé CLR](../../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md)  
  
  
