---
title: Boîte de dialogue de connexion SQL Server (ODBC) | Documents Microsoft
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
ms.openlocfilehash: 3dcd7f9d5d3807858ae13a9ded3a2164eca20b45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-login-dialog-box-odbc"></a>Compte de connexion SQL Server, boîte de dialogue (ODBC)

Lorsque vous appelez une connexion ODBC sans spécifier suffisamment d’informations pour le pilote pour se connecter à un serveur SQL Server, le pilote ODBC s’affiche la **connexion SQL Server** boîte de dialogue.

## <a name="options"></a>Options

### <a name="server"></a>Server

Le nom d’une instance de SQL Server sur votre réseau. Sélectionnez le nom d’un serveur dans la liste ou tapez le nom de serveur\instance dans la **Server** boîte. Si vous le souhaitez, vous pouvez créer un alias de serveur sur l’ordinateur client à l’aide **Gestionnaire de Configuration SQL Server**, tapez ce nom dans la **Server** boîte.

Lorsque vous utilisez le même ordinateur que SQL Server, vous pouvez entrer « (local) ». Vous pouvez ensuite vous connecter à une instance locale de SQL Server, même lors de l’exécution d’une version non connecté au réseau de SQL Server.

Pour plus d’informations sur les noms de serveur pour différents types de réseaux, consultez la documentation d’installation de SQL Server dans la documentation en ligne de SQL Server.

### <a name="authentication-mode"></a>Mode d'authentification

Sélectionne le mode d’authentification à partir d’une des opérations suivantes :
- **SQL Server** avec l’ID de connexion et mot de passe
- **Intégrée de Windows** actuellement connecté en compte de l’utilisateur à l’aide de l’authentification
- **Mot de passe Active Directory** avec l’ID de connexion et mot de passe
- **Intégrée à Active Directory** actuellement connecté en compte de l’utilisateur à l’aide de l’authentification
- **Active Directory interactif** l’authentification avec l’ID de connexion

Consultez [Data Source Assistant écran 2](../../../connect/odbc/windows/dsn-wizard-2.md) pour plus d’informations sur les modes d’authentification.

### <a name="server-spn"></a>SPN du serveur

Si vous utilisez une connexion approuvée, vous pouvez spécifier un nom de principal de service (SPN) pour le serveur.

### <a name="login-id"></a>Nom d'accès

Spécifie l’ID de connexion SQL Server ou Azure Active Directory à utiliser pour la connexion si **Mode d’authentification** a la valeur **SQL Server** ou **mot de passe Active Directory** ou **Active Directory Interactive**. Dans le cas contraire, le **ID de connexion** à cocher est désactivée.

### <a name="password"></a>Mot de passe

Spécifie le mot de passe pour l’ID de connexion SQL Server ou Azure Active Directory utilisé pour la connexion si **Mode d’authentification** a la valeur **SQL Server** ou **passe Active Directory**. Dans le cas contraire, le **mot de passe** à cocher est désactivée.

### <a name="options"></a>Options

Affiche ou masque le **Options** groupe. Le **Options** bouton est activé si **Server** a une valeur.

### <a name="change-password"></a>Modification du mot de passe

Lorsque cette case est activée, affiche le **nouveau mot de passe** et **confirmer le nouveau mot de passe** cases.

### <a name="new-password"></a>Nouveau mot de passe

Spécifie le nouveau mot de passe.

### <a name="confirm-new-password"></a>Confirmer le nouveau mot de passe

Spécifie une deuxième fois le nouveau mot de passe, à des fins de confirmation.

### <a name="database"></a>Base de données

Spécifie la base de données par défaut à utiliser sur la connexion. Ce paramètre remplace la base de données par défaut spécifiée pour le nom de connexion sur le serveur. Si aucune base de données n'est spécifiée, la connexion utilise la base de données par défaut spécifiée pour le nom de connexion sur le serveur.

### <a name="mirror-server"></a>Serveur miroir

Indique le nom du partenaire de basculement de la base de données à mettre en miroir.

### <a name="mirror-spn"></a>SPN miroir

Facultativement, vous pouvez spécifier un SPN pour le serveur miroir. Le SPN du serveur miroir est utilisé pour l'authentification mutuelle entre client et serveur.

### <a name="language"></a>Langage

Spécifie la langue nationale à utiliser pour les messages du système SQL Server. L’ordinateur exécutant SQL Server doit avoir la langue installée. Ce paramètre remplace la langue par défaut spécifiée pour le nom de connexion sur le serveur. Si aucune langue n'est spécifiée, la connexion utilise la langue par défaut spécifiée pour le nom de connexion sur le serveur.

### <a name="application-name"></a>Application Name

(Facultatif) Spécifie le nom de l’application à stocker dans le **nom_programme** colonne dans la ligne de cette connexion dans **sys.sysprocesses**.

### <a name="workstation-id"></a>ID Station de travail

(Facultatif) Spécifie l’ID de station de travail à stocker dans le **nom d’hôte** colonne dans la ligne de cette connexion dans **sys.sysprocesses**.

### <a name="use-strong-encryption-for-data"></a>Utiliser le chiffrement renforcé pour les données

Lorsque sélectionné, les données passées via la connexion seront chiffrées. Les noms de connexion sont chiffrés par défaut, même si la case à cocher est désactivée.

### <a name="trust-server-certificate"></a>Approuver le certificat de serveur

Cette option est applicable uniquement lorsque **utiliser un chiffrement renforcé pour les données** est activé. Lorsque sélectionné, il est possible que le certificat du serveur ne sera pas validé pour ont le nom d’hôte correct du serveur et être émis par une autorité de certification approuvée.

## <a name="see-also"></a>Voir aussi

[Pilote Microsoft ODBC pour SQL Server sur Windows](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)
