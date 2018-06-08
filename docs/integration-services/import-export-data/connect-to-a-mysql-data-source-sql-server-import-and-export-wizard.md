---
title: Se connecter à une source de données MySQL (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3d7c5a38-18d3-4cc9-a241-04422cb250d3
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7a21c4b78fdd7632e6e4b39a38c8f22bd3c2b84a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-a-mysql-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données MySQL (Assistant Importation et Exportation SQL Server)
Cette rubrique explique comment se connecter à une source de données **MySQL** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server. Vous pouvez utiliser plusieurs fournisseurs de données pour vous connecter à MySQL.

> [!IMPORTANT]
> Les exigences et prérequis détaillés pour la connexion à une base de données MySQL n’entrent pas dans le cadre de cet article Microsoft. Il est supposé dans cet article que le logiciel client MySQL est déjà installé et que vous pouvez vous connecter à la base de données MySQL cible. Pour plus d’informations, consultez votre administrateur de base de données MySQL ou la documentation MySQL.

## <a name="get-the-mysql-connectors"></a>Se procurer les connecteurs MySQL
Téléchargez les fournisseurs et les pilotes décrits dans cette rubrique depuis la page [Connecteurs MySQL](https://dev.mysql.com/downloads/connector/).

## <a name="connect-to-mysql-with-the-net-framework-data-provider-for-mysql"></a>Se connecter à MySQL au moyen du fournisseur de données .NET Framework pour MySQL
Après avoir sélectionné le **Fournisseur de données .NET Framework pour MySQL** dans la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant, la page présente une liste groupée d’options pour le fournisseur. Nombre d’entre elles portent des noms inconnus et ont des paramètres inhabituels. Heureusement, vous ne devez fournir que peu d’éléments d’information. Vous pouvez ignorer les valeurs par défaut des autres paramètres.

> [!NOTE]
> Les options de connexion de ce fournisseur de données sont les mêmes que MySQL soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

|Informations nécessaires|Fournisseur de données .NET Framework pour la propriété MySQL|
|---|---|
|Nom du serveur|**Server**|
|Nom de la base de données|**Sauvegarde de la base de données**|
|Informations d’authentification (connexion)|**ID d’utilisateur** et **Mot de passe**|

Il n’est pas nécessaire d’entrer la chaîne de connexion dans le champ **ConnectionString** de la liste. Une fois que vous avez entré les valeurs individuelles pour le nom du serveur MySQL (**Serveur**) et les informations de connexion, l’Assistant assemble la chaîne de connexion à partir des propriétés individuelles et de leurs valeurs. 

![Se connecter à MySQL à l’aide du fournisseur .NET, 1 sur 2](../../integration-services/import-export-data/media/connect-to-mysql-with-the-net-provider-1-of-2.png)

![Se connecter à MySQL à l’aide du fournisseur .NET, 2 sur 2](../../integration-services/import-export-data/media/connect-to-mysql-with-the-net-provider-2-of-2.png)

## <a name="connect-to-mysql-with-the-mysql-odbc-driver"></a>Se connecter à MySQL à l’aide du pilote ODBC MySQL
Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des sources de données. Pour vous connecter avec un pilote ODBC, commencez par sélectionner le **Fournisseur de données .NET Framework pour ODBC** comme source de données dans la page **Choisir une source de données** ou **Choisir une destination**. Ce fournisseur agit comme un wrapper autour du pilote ODBC.

Voici l’écran générique qui s’affiche immédiatement après que vous avez sélectionné le fournisseur de données .NET Framework pour ODBC.

![Connexion à SQL avec ODBC avant](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-before.jpg)

### <a name="options-to-specify-mysql-odbc-driver"></a>Options à spécifier (pilote ODBC MySQL)

> [!NOTE]
> Les options de connexion de ce fournisseur de données et de ce pilote ODBC sont les mêmes que MySQL soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

Pour vous connecter à MySQL au moyen du pilote ODBC MySQL, assemblez une chaîne de connexion qui inclut les paramètres suivants et leurs valeurs. Le format d’une chaîne de connexion complète est donné immédiatement après la liste des paramètres.

> [!TIP]
> Obtenez de l’aide pour l’assemblage d’une chaîne de connexion correcte. Au lieu de produire une chaîne de connexion, vous pouvez fournir un nom de source de données (DSN, data source name) existant ou en créer un. Pour obtenir plus d’informations sur ces options, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

**Driver**  
Nom du pilote ODBC.

**Server**  
Nom du serveur MySQL. 

**Sauvegarde de la base de données**  
Nom de la base de données MySQL.

**UID** et **Pwd**   
ID d’utilisateur et mot de passe pour se connecter.

### <a name="connection-string-format"></a>Format de la chaîne de connexion
Voici le format d’une chaîne de connexion standard.

    ```
    Driver={MySQL ODBC 5.3 Unicode Driver};Server=<server>;Database=<database>;UID=<user id>;PWD=<password>
    ```

### <a name="enter-the-connection-string"></a>Entrer la chaîne de connexion
Indiquez la chaîne de connexion dans le champ **ConnectionString** ou entrez le nom de la source de données dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une Destination**. Une fois que vous avez entré la chaîne de connexion, l’Assistant analyse cette chaîne et affiche les propriétés individuelles avec leur valeur dans la liste.

L’exemple suivant utilise cette chaîne de connexion.

    ```
    Driver={MySQL ODBC 5.3 Unicode Driver};Server=127.0.0.1;Database=world;UID=root;PWD=********
    ```

Voici l’écran que vous voyez après avoir entré la chaîne de connexion.

![Se connecter à MySQL avec ODBC](../../integration-services/import-export-data/media/connect-to-mysql-with-odbc.png)

## <a name="other-data-providers-and-more-info"></a>Autres fournisseurs de données et renseignements complémentaires
Pour plus d’informations sur la connexion à MySQL au moyen d’un fournisseur de données qui n’est pas répertorié ici, consultez [Chaînes de connexion MySQL](https://www.connectionstrings.com/mysql/). Ce site tiers contient également des informations complémentaires sur les fournisseurs de données et les paramètres de connexion décrits dans cette page.

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)

