---
title: Écran 1 (le pilote ODBC pour SQL Server) de l’Assistant Source de données | Documents Microsoft
ms.custom: ''
ms.date: 09/27/2017
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
ms.author: genemi
manager: craigg
ms.openlocfilehash: d97cbdf8e73254b790ff0c8f965fa8b6d647951a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-source-wizard-screen-1"></a>1 de l’écran de l’Assistant Source de données

Spécifiez le nom et la description de la source de données et le nom du serveur exécutant SQL Server auquel se connecte la source de données. 
    
## <a name="options"></a>Options

### <a name="name"></a>Nom

Nom de la source de données utilisé par une application ODBC lorsqu'elle demande une connexion à la source de données. Par exemple, "Personnel". Le nom de la source de données est affiché dans la boîte de dialogue Administrateur de la source de données ODBC.

### <a name="description"></a> Description

(Facultatif) Description de la source de données. Par exemple, « date d'embauche, historique de salaire et examen actuel de tous les employés. »

### <a name="select-or-enter-a-server-name"></a>Sélectionnez ou entrez un nom de serveur.

Le nom d’une instance de SQL Server sur votre réseau. Vous devrez spécifier un serveur dans la prochaine zone d'édition.

Dans la plupart des cas, le pilote ODBC peut se connecter à l’aide de l’ordre des protocoles par défaut et le nom du serveur fourni dans cette zone. Utilisez le Gestionnaire de Configuration SQL Server si vous souhaitez créer un alias pour le serveur ou configurer les bibliothèques réseau clientes.

Vous pouvez entrer « (local) » dans la zone serveur lorsque vous utilisez le même ordinateur que SQL Server. L’utilisateur se connecte ensuite à l’instance locale de SQL Server, même lors de l’exécution d’une version non connecté au réseau de SQL Server. Il peuvent exécuter plusieurs instances de SQL Server sur le même ordinateur. Pour spécifier une instance nommée de SQL Server, le nom du serveur est spécifié en tant que _nom_serveur_\\_InstanceName_.

Pour plus d’informations sur les noms de serveur pour différents types de réseaux, consultez la documentation d’installation de SQL Server dans la documentation en ligne de SQL Server.

### <a name="finish"></a>Terminer

Si les informations spécifiées dans cet écran sont tout ce qui est nécessaire pour se connecter à SQL Server, vous pouvez cliquer sur **Terminer**. Des valeurs par défaut sont utilisées pour tous les attributs spécifiés sur d'autres écrans de l'Assistant.

### <a name="next"></a>Suivant

Pour procéder à l’écran suivant de l’Assistant, cliquez sur **suivant**.

## <a name="next-steps"></a>Étapes suivantes

[2 de l’écran de l’Assistant Source de données](../../../connect/odbc/windows/dsn-wizard-2.md)
