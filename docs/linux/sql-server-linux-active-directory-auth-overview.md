---
title: L’authentification Active Directory pour SQL Server sur Linux | Documents Microsoft
description: Cet article fournit une vue d’ensemble de l’authentification Active Directory pour SQL Server sur Linux.
author: rothja
ms.date: 02/23/2018
ms.author: jroth
manager: craigg
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: 7f34cda192cbd909ac6c2392ab49acb58038b416
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="active-directory-authentication-for-sql-server-on-linux"></a>Authentification Active Directory pour SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article fournit une vue d’ensemble de l’authentification d’Active Directory (AD) pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux. L’authentification Active Directory est aussi appelée authentification intégrée dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

## <a name="ad-authentication-overview"></a>Vue d’ensemble de l’authentification Active Directory

L’authentification Active Directory permet appartenant au domaine des clients sur Windows ou Linux s’authentifier auprès de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de leurs informations d’identification de domaine et le protocole Kerberos.

L’authentification Active Directory offre les avantages suivants sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] l’authentification :

- Les utilisateurs s’authentifient via l’authentification unique, sans être invité à entrer un mot de passe.   
- En créant des comptes de connexion des groupes Active Directory, vous pouvez gérer l’accès et les autorisations dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide d’appartenances Active Directory.  
- Chaque utilisateur possède une identité unique dans votre organisation, donc vous n’êtes pas obligé d’effectuer le suivi de quelles connexions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] correspondent à quelles personnes.   
- Active Directory vous permet d’appliquer une stratégie de mot de passe centralisée de votre organisation.   

## <a name="configuration-steps"></a>Étapes de configuration

Pour pouvoir utiliser l’authentification Active Directory, vous devez disposer d’un contrôleur de domaine Active Directory (Windows) sur votre réseau.

Les détails pour savoir comment configurer l’authentification Active Directory sont fournis dans le didacticiel, [didacticiel : l’authentification d’utilisation d’Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md). La liste suivante fournit un résumé avec un lien vers chaque section dans le didacticiel :

1. [Joindre un ordinateur hôte de SQL Server à un domaine Active Directory](sql-server-linux-active-directory-authentication.md#join).
1. [Créer un utilisateur Active Directory pour SQL Server et définissez ServicePrincipalName](sql-server-linux-active-directory-authentication.md#createuser).
1. [Configurer le service de SQL Server fichier keytab](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Créer des connexions basée sur Active Directory de SQL Server dans Transact-SQL](sql-server-linux-active-directory-authentication.md#createsqllogins).
1. [Se connecter à SQL Server à l’aide de l’authentification Active Directory](sql-server-linux-active-directory-authentication.md#connect).

## <a name="known-issues"></a>Problèmes connus

- À ce stade, la seule méthode d’authentification prise en charge pour le point de terminaison de mise en miroir de base de données est le certificat. La méthode d’authentification WINDOWS sera activée dans une version ultérieure.
- Les annonces outils tiers tels que Centrify, Powerbroker, et Vintela ne sont pas pris en charge.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la façon d’implémenter l’authentification Active Directory pour SQL Server sur Linux, consultez [didacticiel : l’authentification d’utilisation d’Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md).