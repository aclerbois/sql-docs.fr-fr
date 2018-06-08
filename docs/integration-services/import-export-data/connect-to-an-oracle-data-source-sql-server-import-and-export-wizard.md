---
title: Se connecter à une source de données Oracle (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b0bd1f5a-34dd-4be3-9ac8-f9f87727781b
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1f15cffa118a8783b02a1d04fcd953aa4dbb14b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-an-oracle-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données Oracle (Assistant Importation et Exportation SQL Server)
Cette rubrique vous montre comment se connecter à une source de données **Oracle** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server. Vous pouvez utiliser plusieurs fournisseurs de données pour vous connecter à Oracle.

> [!IMPORTANT]
> Les exigences et prérequis détaillés pour la connexion à une base de données Oracle n’entrent pas dans le cadre de cet article Microsoft. Cet article suppose que le logiciel client Oracle est déjà installé et que vous pouvez aussi vous connecter à la base de données Oracle cible. Pour plus d’informations, consultez votre administrateur de base de données Oracle ou la documentation Oracle.

## <a name="connect-to-oracle-with-the-net-framework-data-provider-for-oracle"></a>Se connecter à Oracle au moyen du fournisseur de données .NET Framework pour Oracle
Après avoir sélectionné le **Fournisseur de données .NET Framework pour Oracle** dans la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant, la page présente une liste groupée d’options pour le fournisseur. Nombre d’entre elles portent des noms inconnus et ont des paramètres inhabituels. Heureusement, vous ne devez fournir que deux ou trois éléments d’information. Vous pouvez ignorer les valeurs par défaut des autres paramètres.

> [!NOTE]
> Les options de connexion de ce fournisseur de données sont les mêmes, qu’Oracle soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

|Informations nécessaires|Fournisseur de données .NET Framework pour la propriété Oracle|
|---|---|
|Nom du serveur|**Source de données**|
|Informations d’authentification (connexion)|**ID d’utilisateur** et **Mot de passe**, sinon **Sécurité intégrée**|

Il n’est pas nécessaire d’entrer la chaîne de connexion dans le champ **ConnectionString** de la liste. Une fois que vous avez entré les valeurs individuelles pour le nom du serveur Oracle (**Source de données**) et les informations de connexion, l’Assistant assemble la chaîne de connexion à partir des propriétés individuelles et de leur valeur. 

![Connexion à Oracle avec le fournisseur .NET](../../integration-services/import-export-data/media/connect-to-oracle-with-net-provider.jpg)

## <a name="connect-to-oracle-with-the-microsoft-odbc-driver-for-oracle"></a>Se connecter à Oracle au moyen de Microsoft ODBC Driver for Oracle
Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des sources de données. Pour vous connecter avec un pilote ODBC, commencez par sélectionner le **Fournisseur de données .NET Framework pour ODBC** comme source de données dans la page **Choisir une source de données** ou **Choisir une destination**. Ce fournisseur agit comme un wrapper autour du pilote ODBC.

Voici l’écran générique que vous voyez immédiatement après avoir sélectionné le fournisseur de données .NET Framework pour ODBC.

![Connexion à Oracle avec ODBC avant](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-before.jpg)

### <a name="options-to-specify-odbc-driver-for-oracle"></a>Options à spécifier (pilote ODBC pour Oracle)

> [!NOTE]
> Les options de connexion de ce fournisseur de données et de ce pilote ODBC sont les mêmes, qu’Oracle soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

Pour vous connecter à Oracle au moyen du pilote ODBC pour Oracle, assemblez une chaîne de connexion qui inclut les paramètres suivants accompagnés de leur valeur. Le format d’une chaîne de connexion complète est donné immédiatement après la liste des paramètres.

> [!TIP]
> Obtenez de l’aide pour l’assemblage d’une chaîne de connexion correcte. Au lieu de produire une chaîne de connexion, vous pouvez fournir un nom de source de données (DSN, data source name) existant ou en créer un. Pour obtenir plus d’informations sur ces options, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

**Driver**  
Nom du pilote ODBC, **Microsoft ODBC for Oracle**.

**Server**  
Nom du serveur Oracle. 

**UID** et **Pwd**   
Id d’utilisateur et mot de passe pour se connecter.

### <a name="connection-string-format"></a>Format de la chaîne de connexion
Voici le format d’une chaîne de connexion standard.

    ```
    Driver={Microsoft ODBC for Oracle};Server=myServerAddress;Uid=myUsername;Pwd=myPassword;
    ```

### <a name="enter-the-connection-string"></a>Entrer la chaîne de connexion
Indiquez la chaîne de connexion dans le champ **ConnectionString** ou entrez le nom de la source de données dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une Destination**. Une fois que vous avez entré la chaîne de connexion, l’Assistant analyse cette chaîne et affiche les propriétés individuelles avec leur valeur dans la liste.

Voici l’écran que vous voyez après avoir entré la chaîne de connexion.

![Connexion à Oracle avec ODBC](../../integration-services/import-export-data/media/connect-to-oracle-with-odbc.jpg)

## <a name="whats-my-oracle-server-name"></a>Quel est le nom de mon serveur Oracle ?
Exécutez une des requêtes suivantes pour obtenir le nom de votre serveur Oracle.

`SELECT host_name FROM v$instance`

ou Gestionnaire de configuration

`SELECT sys_context('USERENV','SERVER_HOST') FROM dual`

## <a name="other-data-providers-and-more-info"></a>Autres fournisseurs de données et renseignements complémentaires
Pour plus d’informations sur la façon de se connecter à Oracle au moyen d’un fournisseur de données qui n’est pas répertorié ici, consultez [Chaînes de connexion Oracle](https://www.connectionstrings.com/oracle/). Ce site tiers contient également des informations complémentaires sur les fournisseurs de données et les paramètres de connexion décrits dans cette page.

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)

