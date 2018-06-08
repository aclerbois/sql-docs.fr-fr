---
title: Écran 2 (le pilote ODBC pour SQL Server) de l’Assistant Source de données | Documents Microsoft
ms.custom: ''
ms.date: 03/21/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
caps.latest.revision: 22
author: MightyPen
ms.author: v-jizho2
manager: craigg
ms.openlocfilehash: 96907675f7bdb1072923da9ad56412dd7dcd59c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-source-wizard-screen-2"></a>Assistant Source de données - Écran 2

Spécifiez la méthode d’authentification et configurer les entrées de client avancé Microsoft SQL Server et la connexion et le mot de passe que le pilote ODBC pour SQL Server utilisera pour se connecter à SQL Server lors de la configuration de la source de données.

## <a name="options"></a>Options

### <a name="with-integrated-windows-authentication"></a>Avec l’authentification Windows intégrée

Indique que le pilote demande une connexion sécurisée (ou approuvée) à un serveur SQL Server. Si cette option est sélectionnée, SQL Server utilise la sécurité de connexion intégrée pour établir des connexions à l'aide de cette source de données, quel que soit le mode de sécurité de connexion en vigueur au niveau du serveur. Tout ID de connexion ou mot de passe fourni est ignoré. L’administrateur système SQL Server doit avoir associé votre nom d’utilisateur Windows à un ID de connexion SQL Server (par exemple, en utilisant SQL Server Management Studio).

Vous pouvez éventuellement spécifier un nom de principal du service (SPN) pour le serveur.

### <a name="with-active-directory-integrated-authentication"></a>Avec l’authentification intégrée à Active Directory

Spécifie que le pilote de s’authentifier à SQL Server à l’aide d’Azure Active Directory. Lorsque sélectionnée, SQL Server utilise la sécurité de connexion intégrée à Azure Active Directory pour établir une connexion à l’aide de cette source de données, quel que soit le mode de sécurité de connexion en cours au niveau du serveur.

### <a name="with-sql-server-authentication"></a>Avec l’authentification SQL Server

Spécifie que le pilote de s’authentifier à SQL Server à l’aide d’un ID de connexion et un mot de passe.

### <a name="with-active-directory-password-authentication"></a>Avec l’authentification du mot de passe Active Directory

Spécifie que le pilote de s’authentifier à SQL Server à l’aide d’un ID de connexion d’Azure Active Directory et le mot de passe.

### <a name="with-active-directory-interactive-authentication"></a>Avec l’authentification Interactive d’Active Directory

Spécifie que le pilote de s’authentifier à SQL Server à l’aide du mode interactif Azure Active Directory en fournissant l’ID de connexion. Cette opération déclenche la boîte de dialogue invite de commandes de l’authentification Windows Azure.

### <a name="login-id"></a>Nom d'accès

Spécifie l’ID de connexion que le pilote utilise lors de la connexion à SQL Server si **avec l’authentification SQL Server à l’aide d’un ID de connexion et un mot de passe entré par l’utilisateur** ou **avec Active Directory authentification mot de passe à l’aide d’un ID de connexion et le mot de passe entré par l’utilisateur** ou **avec Active Directory authentification Interactive à l’aide d’un ID de connexion entré par l’utilisateur** est sélectionnée. Cela s'applique uniquement à la connexion établie pour déterminer les paramètres serveur par défaut, pas aux connexions établies ultérieurement à l'aide de la source de données, après sa création.

### <a name="password"></a>Mot de passe

Spécifie le mot de passe que le pilote utilise lors de la connexion à SQL Server si **avec l’authentification SQL Server à l’aide d’un ID de connexion et un mot de passe entré par l’utilisateur** ou **avec Active Directory authentification mot de passe à l’aide d’un ID de connexion et le mot de passe entré par l’utilisateur** est sélectionnée. Cela s'applique uniquement à la connexion établie pour déterminer les paramètres serveur par défaut, pas aux connexions établies ultérieurement à l'aide de la nouvelle source de données.

Les deux le **ID de connexion** et **mot de passe** sont désactivées si **avec intégré l’authentification Windows** ou **avec Active Directory intégré authentification** est sélectionnée.

### <a name="next"></a>Suivant

Passe à l’écran suivant de l’Assistant.

### <a name="back"></a>Précédent

Retourne à l’écran précédent de l’Assistant.

## <a name="next-steps"></a>Étapes suivantes

[1 de l’écran de l’Assistant Source de données](../../../connect/odbc/windows/dsn-wizard-1.md)

[3 de l’écran de l’Assistant Source de données](../../../connect/odbc/windows/dsn-wizard-3.md)

