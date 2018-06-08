---
title: Rôles d’utilisateur | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: be0ec384-e03b-4483-96ca-02b289804d6a
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c96d380d5168a936dc5f4db51f2b52f68e30f49d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="user-roles"></a>Rôles d'utilisateur
  Cette section décrit les rôles d'utilisateur du service de capture de données modifiées pour Oracle par Attunity. Les rôles décrits sont des rôles de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les rôles Windows ou des rôles de base de données Oracle.  
  
## <a name="windows-user-roles"></a>Rôles d'utilisateur Windows  
 La section suivante décrit les rôles d'utilisateur Windows utilisés par le service de capture de données modifiées Oracle.  
  
### <a name="computer-administrator-oracle-cdc-service"></a>Administrateur de l'ordinateur : service de capture de données modifiées Oracle  
 L'administrateur de l'ordinateur est un utilisateur Windows chargé de créer et de gérer le service de capture de données modifiées sur l'ordinateur. Cet utilisateur doit appartenir au groupe des administrateurs de l'ordinateur local.  
  
 Les tâches effectuées par l'administrateur de l'ordinateur de service de capture de données modifiées Oracle sont les suivantes :  
  
-   installation du service de capture de données modifiées pour le logiciel Oracle ;  
  
-   création d'un service Windows de capture de données modifiées Oracle ;  
  
-   configuration de la connexion du service de capture de données modifiées à l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible (chaîne de connexion et informations d’identification) ;  
  
-   vérification du mot de passe principal du service de capture de données modifiées avec lequel les informations d'identification d'exploration de données de journaux Oracle sont protégées ;  
  
-   suppression d'un service Windows de capture de données modifiées ;  
  
-   désinstallation du service de capture de données modifiées pour le logiciel Oracle ;  
  
-   maintenance du service de capture de données modifiées pour le logiciel Oracle (par exemple, installation des mises à jour) ;  
  
-   démarrage et arrêt d'un service Windows de capture de données modifiées.  
  
 Lorsque vous utilisez des fonctionnalités à haute disponibilité, telles que les clusters de basculement Microsoft, l'administrateur de l'ordinateur doit avoir des responsabilités et des autorisations supplémentaires comme suit :  
  
-   Installation et maintenance du service de capture de données modifiées pour le logiciel Oracle sur tous les nœuds de cluster.  
  
-   Définition des ressources génériques du service de cluster pour le service Windows de capture de données modifiées sur les nœuds de cluster individuels.  
  
-   Agir en tant qu'administrateur de l'ordinateur autorisé en tant qu'administrateur sur l'ordinateur où le service de capture de données modifiées pour Oracle est installé. Cette personne installe le service de capture de données modifiées pour Oracle et utilise la console de configuration du service de capture de données modifiées pour configurer un service de capture de données modifiées pour Oracle sur un ordinateur local.  
  
### <a name="service-account-oracle-cdc-service"></a>Compte de service : service de capture de données modifiées Oracle  
 Il s'agit d'un compte de service Windows de capture de données modifiées Oracle utilisé pour exécuter le service de capture de données modifiées Oracle (le compte de service).  
  
 Le seul privilège obligatoire nécessaire pour le compte de service est de pouvoir utiliser le client Oracle et le fournisseur ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Ce compte n’a pas besoin d’accéder aux fichiers à moins que cela ne soit requis par des fournisseurs spécifiques (par exemple, si la chaîne de connexion du client Oracle référence des instances de base de données Oracle dans un fichier **tnsnames.ora** , puis ce fichier doit être accessible en lecture au compte de service).  
  
 Lors de la création d'un service de capture de données modifiées Oracle sur Windows Vista ou Windows Server 2008, le compte de service par défaut est le compte NETWORK SERVICE.  
  
 Sur Windows 7, Windows Server 2008 R2 et versions ultérieures, le compte de service par défaut est NT Service\\<nom-service>.  
  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sur un autre ordinateur ou est une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cluster et le service doit se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible à l'aide de l'authentification Windows, le compte de service doit être un compte de domaine.  
  
## <a name="sql-server-user-roles"></a>Rôles d'utilisateur SQL Server  
 La section suivante décrit les rôles d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisés par le service de capture de données modifiées Oracle.  
  
### <a name="oracle-cdc-service-administrator"></a>Administrateur de service de capture de données modifiées Oracle  
 L'administrateur de service de capture de données modifiées est un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec contrôle total sur les artefacts de service de capture de données modifiées Oracle dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible. L'administrateur de service de capture de données modifiées utilise la console du concepteur de capture de données modifiées Oracle pour concevoir des instances Oracle CDC.  
  
 L'administrateur de service de capture de données modifiées doit disposer des rôles de serveur fixes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **public** et **dbcreator**.  
  
 Les tâches effectuées par l'administrateur de service de capture de données modifiées sont les suivantes :  
  
-   Préparation d’une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour héberger les instances Oracle CDC (qui sont des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ). Dans cette tâche, une base de données spéciale appelée MSXDBCDC est créée dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Création d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'instance Oracle CDC. Cette tâche inclut l’activation de la base de données nouvellement créée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la capture de données modifiées, qui exige un administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (**sysadmin**).  
  
-   Conception d'une instance Oracle CDC. Cette tâche inclut la fourniture des informations sur la base de données Oracle source et les tables capturées, ce qui requiert un administrateur de base de données Oracle.  
  
-   Maintenance de l'instance Oracle CDC dans le temps, ce qui inclut l'ajout/la suppression des instances de capture et la mise à jour de la configuration.  
  
-   Activation ou désactivation d'une instance Oracle CDC.  
  
-   Analyse de l'état d'une instance Oracle CDC.  
  
-   Résolution des problèmes qui affectent l'instance Oracle CDC.  
  
 L’administrateur de service de capture de données modifiées a, au moins au départ, le rôle de base de données fixe **db_owner** pour la base de données CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associée à l’instance Oracle CDC. Cela donne à l'administrateur de service de capture de données modifiées accès aux données modifiées stockées dans la base de données CDC. Une fois créé, le rôle **db_owner** de la base de données CDC peut être affecté à un autre utilisateur qui peut effectuer toutes les tâches répertoriées ci-dessus à l’exception de la préparation d’une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de la création d’une autre instance Oracle CDC.  
  
 L'administrateur de service de capture de données modifiées n'a pas besoin de connaître le mot de passe principal spécifié lors de la création du service Windows de capture de données modifiées Oracle.  
  
### <a name="system-administrator"></a>Administrateur système  
 L’administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et doit disposer du rôle serveur fixe **sysadmin** sur l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associée à un ou plusieurs services de capture de données modifiées Oracle.  
  
 Il existe une seule tâche spécifique de capture de données modifiées Oracle effectuée avec l'administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et qui consiste à activer la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une instance Oracle CDC pour la capture de données modifiées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette tâche est effectuée à l'aide de la console du concepteur de capture de données modifiées Oracle en créant une nouvelle instance Oracle CDC.  
  
### <a name="oracle-cdc-service-user"></a>Utilisateur du service de capture de données modifiées Oracle  
 L'utilisateur du service de capture de données modifiées Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée par le service de capture de données modifiées Oracle pour effectuer ses tâches dans la base de données MSXDBCDC et toutes les instances Oracle CDC (bases de données CDC) gérées par ce service.  
  
 L'utilisateur du service de capture de données modifiées Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit :  
  
-   être membre des rôles de base de données fixes **db_dlladmin**, **db_datareader**et **db_datawriter** pour toutes les bases de données CDC gérées par le serveur ;  
  
-   être membre des rôles de base de données fixes **db_datareader** et **db_datawriter** pour la base de données MSXDBCDC.  
  
 Le service de capture de données modifiées Oracle utilisant une seule connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour utiliser toutes les bases de données CDC et la base de données MSXDBCDC, cette connexion doit être mappée dans toutes ces bases de données.  
  
### <a name="oracle-cdc-change-consumer"></a>Consommateur de modifications de capture de données modifiées Oracle  
 Le consommateur de modifications de capture de données modifiées Oracle est un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui consomme des modifications stockées dans les tables de capture de données modifiées dans la base de données de l'instance Oracle CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Cet utilisateur détermine le rôle d'utilisateur requis pour accéder à chacune des tables de capture de données modifiées via les fonctions de capture de données modifiées générées par l'infrastructure de capture de données modifiées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si aucun rôle d’utilisateur n’est spécifié quand une instance de capture est spécifiée, l’accès aux modifications est limité au membre du rôle de base de données fixe **db_owner** de la base de données CDC.  
  
## <a name="oracle-user-roles"></a>Rôles d'utilisateur Oracle  
 La section suivante décrit les rôles d'utilisateur Oracle utilisés par le service de capture de données modifiées Oracle.  
  
### <a name="database-administrator-dba"></a>Administrateur de base de données (DBA)  
 L'administrateur de base de données Oracle est un utilisateur de base de données Oracle. Les tâches effectuées par l'administrateur de base de données Oracle incluent :  
  
-   configuration de la base de données Oracle source pour travailler en mode ARCHIVELOG ;  
  
-   configuration d'un utilisateur d'exploration de données de journaux avec les autorisations requises ;  
  
-   définition de journalisation supplémentaire pour les tables capturées ;  
  
-   aide pour restaurer les fichiers journaux des transactions archivés qui ne sont plus disponibles de façon à ce qu'ils puissent être traités.  
  
 L'administrateur de base de données Oracle peut obtenir les scripts Oracle SQL à exécuter de façon à les évaluer avant de les exécuter. L'administrateur de base de données Oracle peut également exécuter directement des scripts Oracle SQL à partir de la console du concepteur de capture de données modifiées Oracle.  
  
 Si l'administrateur de base de données Oracle choisit d'utiliser la console du concepteur de capture de données modifiées Oracle, les informations d'identification de l'administrateur ne sont pas conservées sauf le contexte (dialogue) dans lequel elles ont été utilisées.  
  
 L'administrateur de base de données Oracle travaille de concert avec l'administrateur de service de capture de données modifiées Oracle sur la configuration des instances Oracle CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="log-mining-user"></a>Utilisateur d'exploration de données de journaux  
 L'utilisateur d'exploration de données de journaux Oracle est un utilisateur de base de données spécial qui dispose des privilèges nécessaires pour accéder aux journaux des transactions Oracle et les traiter.  
  
 Les informations d'identification pour cet utilisateur sont stockées dans la base de données d'instance Oracle CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide du chiffrement à clé asymétrique. Elles sont accessibles uniquement au service de capture de données modifiées Oracle, mais pas au propriétaire de la base de données d'instance Oracle CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 La liste suivante décrit les privilèges requis pour l'utilisateur d'exploration de données de journaux :  
  
-   SELECT sur \<tout_table_capturée>  
  
-   SELECT ANY TRANSACTION  
  
-   EXECUTE sur DBMS_LOGMNR  
  
-   SELECT sur V$LOGMNR_CONTENTS  
  
-   SELECT sur V$ARCHIVED_LOG  
  
-   SELECT sur V$LOG  
  
-   SELECT sur V$LOGFILE  
  
-   SELECT sur V$DATABASE  
  
-   SELECT sur V$THREAD  
  
-   SELECT sur ALL_INDEXES  
  
-   SELECT sur ALL_OBJECTS  
  
-   SELECT sur DBA_OBJECTS  
  
-   SELECT sur ALL_TABLES  
  
 Si l'un de ces privilèges ne peut pas être accordé à V$xxx, il doit être accordé à V $xxx.  
  
### <a name="schema-user"></a>Utilisateur de schéma  
 L'utilisateur de schéma Oracle est un utilisateur Oracle avec un accès en lecture au schéma des tables Oracle à capturer. Cet utilisateur est nécessaire lors de l'utilisation de la console du concepteur de capture de données modifiées Oracle pour récupérer la liste des schémas Oracle, de tables à capturer et de leurs colonnes, index et clés.  
  
 Les informations d'identification pour cet utilisateur ne sont jamais stockées. Elles sont requises par la console du concepteur CDC chaque fois qu'elles sont nécessaires et sont conservées pour les autres sessions d'interface utilisateur.  
  
  
