---
title: Procédures stockées (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stored-Procs
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- storing programs as stored procedures
- stored procedures [SQL Server], about stored procedures
ms.assetid: cc6daf62-9663-4c3e-950a-ab42e2830427
caps.latest.revision: 35
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: be6ef66f23a8bea4172466373bf6fb261ddb0cb9
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708907"
---
# <a name="stored-procedures-database-engine"></a>Procédures stockées (moteur de base de données)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Une procédure stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un groupe d’une ou de plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou une référence à une méthode CLR (Common Runtime Language) [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Les procédures ressemblent à des constructions d'autres langages de programmation, car elles peuvent :  
  
-   accepter des paramètres d'entrée et retourner plusieurs valeurs sous la forme de paramètres de sortie au programme appelant ;  
  
-   contenir des instructions de programmation qui effectuent des opérations dans la base de données. Cela inclut l'appel d'autres procédures ;  
  
-   retourner une valeur d'état à un programme appelant pour indiquer une réussite ou un échec (et la raison de l'échec).  
  
## <a name="benefits-of-using-stored-procedures"></a>Avantages de l'utilisation des procédures stockées  
 La liste suivante décrit certains avantages de l'utilisation des procédures.  
  
 Trafic réseau serveur/client réduit  
 Les commandes d'une procédure sont exécutées comme un seul lot de codes. Cela peut réduire considérablement le trafic réseau entre le serveur et le client, car seul l'appel pour exécuter la procédure est envoyé sur le réseau. Sans encapsulation de code fournie par une procédure, chaque ligne de code individuelle doit être transmise sur le réseau.  
  
 Sécurité renforcée  
 Plusieurs utilisateurs et programmes clients peuvent effectuer des opérations sur les objets de base de données sous-jacents par le biais d'une procédure, même si les utilisateurs et les programmes n'ont pas d'autorisations directes sur ces objets sous-jacents. La procédure contrôle les processus et activités effectués et protège les objets de base de données sous-jacents. Cela élimine la nécessité d'accorder des autorisations au niveau de l'objet individuel et simplifie les couches de sécurité.  
  
 La clause [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) peut être spécifiée dans l'instruction CREATE PROCEDURE pour permettre l'emprunt de l'identité d'un autre utilisateur, ou pour permettre aux utilisateurs ou aux applications d'effectuer certaines activités de base de données sans avoir besoin d'autorisations directes sur les commandes et les objets sous-jacents. Par exemple, il n'est pas possible d'accorder des autorisations sur certaines actions comme TRUNCATE TABLE. Pour exécuter TRUNCATE TABLE, l'utilisateur doit disposer d'autorisations ALTER sur la table spécifiée. L'octroi à un utilisateur des autorisations ALTER sur une table est critiquable, car celui-ci disposera en réalité d'autorisations plus étendues que celles lui permettant de tronquer une table. En intégrant l'instruction TRUNCATE TABLE dans un module et en spécifiant que ce module s'exécute en tant qu'utilisateur disposant des autorisations de modifier la table, vous pouvez étendre les autorisations de tronquer la table à l'utilisateur auquel vous accordez les autorisations EXECUTE sur le module.  
  
 En appelant une procédure sur le réseau, seul l'appel pour exécuter la procédure est visible. Par conséquent, les utilisateurs malveillants ne peuvent pas voir les noms des objets de table et de base de données, incorporer leurs propres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou rechercher des données critiques.  
  
 L'utilisation des paramètres de procédure permet de se prémunir contre les attaques par injection de code SQL. Dans la mesure où l’entrée de paramètre est traitée comme une valeur littérale et non en tant que code exécutable, il est plus difficile à un intrus d’insérer une commande dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de la procédure et de compromettre la sécurité.  
  
 Les procédures peuvent être chiffrées, ce qui permet d'obfusquer le code source. Pour plus d’informations, consultez [SQL Server Encryption](../../relational-databases/security/encryption/sql-server-encryption.md)  
  
 Réutilisation du code  
 Le code de toute opération de base de données répétitive est le candidat parfait pour une encapsulation dans les procédures. Cela élimine les réécritures inutiles du même code, réduit les incohérences du code et permet l'accès et l'exécution du code par tout utilisateur ou toute application disposant des autorisations nécessaires.  
  
 Maintenance plus simple  
 Lorsque les applications clientes appellent des procédures et conservent les opérations de base de données dans la couche Données, seules les procédures doivent être mises à jour pour toutes les modifications apportées dans la base de données sous-jacente. La couche application reste distincte et peut ignorer les modifications apportées à la mise en page, aux relations et aux processus de base de données.  
  
 Performances améliorées  
 Par défaut, une procédure est compilée à sa première exécution et crée un plan d'exécution qui est réutilisé pour les exécutions suivantes. Étant donné que le processeur de requêtes ne doit pas créer de plan, le traitement de la procédure dure généralement moins longtemps.  
  
 Si des modifications significatives ont été apportées aux tables ou aux données référencés par la procédure, le plan précompilé peut ralentir la procédure. Dans ce cas, la recompilation de la procédure et l'application forcée d'un nouveau plan d'exécution peuvent améliorer les performances.  
  
## <a name="types-of-stored-procedures"></a>Types de procédures stockées  
 Définie par l'utilisateur  
 Une procédure définie par l’utilisateur peut être créée dans une base de données définie par l’utilisateur ou dans toutes les bases de données système à l’exception de la base de données **Resource** . La procédure peut être développée dans [!INCLUDE[tsql](../../includes/tsql-md.md)] ou en tant que référence à une méthode CLR (Common Language Runtime) [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .  
  
 Temporaire  
 Les procédures temporaires sont une forme de procédures définies par l'utilisateur. Les procédures temporaires sont semblables à une procédure permanente, sauf qu'elles sont stockées dans **tempdb**. Il en existe deux types : locale et globale. Elles se différencient par leur nom, leur visibilité et leur disponibilité. Le premier caractère du nom des procédures temporaires locales est un signe dièse (#) unique. Ces procédures sont visibles uniquement à la connexion actuelle de l'utilisateur et sont supprimées dès que la connexion est fermée. En revanche, le nom des procédures temporaires globales commence par deux signes dièse (##) ; ces procédures sont visibles à tout utilisateur après avoir été créées et sont supprimées à la fin de la dernière session qui utilise la procédure.  
  
 Système  
 Les procédures système sont incluses dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles sont stockées physiquement dans la base de données **Resource** interne et masquée, mais elles apparaissent logiquement dans le schéma **sys** de chaque base de données définie par le système et définie par l’utilisateur. En outre, la base de données **msdb** contient également les procédures stockées système dans le schéma **dbo** , utilisées pour planifier les alertes et les travaux. Compte tenu du fait que les procédures système commencent par le préfixe **sp_**, nous vous recommandons de ne pas utiliser ce préfixe quand vous nommez des procédures définies par l’utilisateur. Pour obtenir la liste complète des procédures système, consultez [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les procédures système qui assurent l’interface entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les programmes externes pour diverses activités de maintenance. Ces procédures étendues utilisent le préfixe xp_. Pour obtenir la liste complète des procédures étendues, consultez [Procédures stockées étendues générales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md).  
  
 Étendue définie par l'utilisateur  
 Les procédures étendues vous permettent de créer des routines externes dans un langage de programmation comme le langage C. Ce sont des DLL qu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut charger et exécuter dynamiquement.  
  
> [!NOTE]  
>  Les procédures stockées étendues seront supprimées dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et modifiez dès que possible les applications qui utilisent actuellement cette fonctionnalité. Créez des procédures CLR à la place. Cette méthode offre une alternative plus robuste et plus sûre à l'écriture des procédures étendues.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|||  
|-|-|  
|**Description de la tâche**|**Rubrique**|  
|Explique comment créer une procédure stockée.|[Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)|  
|Explique comment modifier une procédure stockée.|[Modifier une procédure stockée](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)|  
|Explique comment supprimer une procédure stockée.|[Supprimer une procédure stockée](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)|  
|Explique comment exécuter une procédure stockée.|[Exécuter une procédure stockée](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)|  
|Explique comment accorder des autorisations sur une procédure stockée.|[Accorder des autorisations sur une procédure stockée](../../relational-databases/stored-procedures/grant-permissions-on-a-stored-procedure.md)|  
|Explique comment retourner des données d'une procédure stockée à une application.|[Retour de données à partir d’une procédure stockée](../../relational-databases/stored-procedures/return-data-from-a-stored-procedure.md)|  
|Explique comment recompiler une procédure stockée.|[Recompiler une procédure stockée](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md)|  
|Explique comment renommer une procédure stockée.|[Renommer une procédure stockée](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)|  
|Explique comment afficher la définition d'une procédure stockée.|[Afficher la définition d’une procédure stockée](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)|  
|Explique comment consulter les dépendances d'une procédure stockée.|[Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)|  
|Décrit la façon dont les paramètres sont utilisés dans une procédure stockée.|[Paramètres](../../relational-databases/stored-procedures/parameters.md)|  
  
## <a name="related-content"></a>Contenu associé  
 [Procédures stockées du CLR](http://msdn.microsoft.com/library/bbdd51b2-a9b4-4916-ba6f-7957ac6c3f33)  
  
  
