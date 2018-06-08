---
title: Se connecter à une source de données ODBC (Assistant Importation et Exportation SQL Server) | Microsoft Docs
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
ms.assetid: e6318776-a188-48a7-995d-9eafd7148ff2
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: dc12f0380f64fd818de9caed35ffa5d146eabeff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-an-odbc-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données ODBC (Assistant Importation et Exportation SQL Server)
Cette rubrique vous montre comment se connecter à une source de données **ODBC** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server.

Vous devrez peut-être télécharger le pilote de données dont vous avez besoin auprès de Microsoft ou d’un tiers.

Vous devrez peut-être également rechercher les informations de connexion requises que vous devez fournir. Ce site tiers ([The Connection Strings Reference](https://www.connectionstrings.com/)) contient des exemples de chaînes de connexion et des renseignements complémentaires sur les fournisseurs de données et les informations de connexion dont ils ont besoin.

## <a name="make-sure-the-driver-you-want-is-installed"></a>Vérifier que le pilote de votre choix est installé
1.  Recherchez l’applet **Sources de données ODBC (64 bits)** dans le Panneau de configuration. Si vous avez un pilote 32 bits, ou si vous savez que vous devez utiliser un pilote 32 bits, recherchez l’applet **Sources de données ODBC (32 bits)**.
2.  Lancez l’applet. La fenêtre **Administrateur de source de données ODBC** s’ouvre.
3.  Sous l’onglet **Pilotes**, vous trouverez la liste de tous les pilotes ODBC installés sur votre ordinateur. (Les noms de certains pilotes peuvent figurer dans plusieurs langues.)

    Voici un exemple de liste des pilotes 64 bits installés.

    ![Pilotes ODBC 64 bits installés](../../integration-services/import-export-data/media/installed-64-bit-odbc-drivers.png)

> [!TIP]
> Si vous savez que votre pilote est installé mais que vous ne le voyez dans l’applet 64 bits, recherchez-le dans l’applet 32 bits. Cela vous indique également si vous devez exécuter l’Assistant Importation et Exportation SQL Server 64 bits ou 32 bits.
>
> Pour utiliser la version 64 bits de l’Assistant Importation et Exportation SQL Server, vous devez installer SQL Server. SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) sont des applications 32 bits qui installent uniquement des fichiers 32 bits, y compris la version 32 bits de l’Assistant.
    
## <a name="step-1---select-the-data-source"></a>Étape 1 : sélectionner la source de données
Les pilotes ODBC installés sur votre ordinateur ne sont pas répertoriés dans la liste déroulante des sources de données. Pour vous connecter avec un pilote ODBC, commencez par sélectionner le **Fournisseur de données .NET Framework pour ODBC** comme source de données dans la page **Choisir une source de données** ou **Choisir une Destination** de l’Assistant. Ce fournisseur agit comme un wrapper autour du pilote ODBC.

Voici l’écran générique qui s’affiche immédiatement après que vous avez sélectionné le fournisseur de données .NET Framework pour ODBC.

![Connexion à SQL avec ODBC avant](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-before.jpg)

## <a name="step-2---provide-the-connection-info"></a>Étape 2 : fournir les informations de connexion
L’étape suivante consiste à fournir les informations de connexion pour votre pilote ODBC et votre source de données. Vous avez le choix entre deux options.
1.  Fournissez un **nom de source de données** (DSN, Data Source Name) qui existe déjà ou que vous créez dans l’applet **Administrateur de source de données ODBC** du Panneau de configuration. Un nom de source de données est la collection enregistrée des paramètres requis pour se connecter à une source de données ODBC.

    Si vous connaissez déjà le nom de la source de données, ou savez comment créer un nom de source de données, vous pouvez ignorer le reste de cette page. Entrez le nom de la source de données dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une destination**, puis passez à l’étape suivante de l’Assistant.

    [Fournir un nom de source de données](#odbc_dsn)
    
2.  Fournissez une **chaîne de connexion**, que vous pouvez rechercher en ligne, ou bien créer et tester sur votre ordinateur avec l’applet **Administrateur de source de données ODBC**.

    Si vous avez déjà la chaîne de connexion ou vous savez comment la créer, vous pouvez ignorer le reste de cette page. Entrez la chaîne de connexion dans le champ **ConnectionString** de la page **Choisir une source de données** ou **Choisir une destination**, puis passez à l’étape suivante de l’Assistant.

    [Fournir une chaîne de connexion](#odbc_connstring)

Si vous fournissez une chaîne de connexion, la page **Choisir une source de données** ou **Choisir une destination** affiche toutes les informations de connexion que l’Assistant va utiliser pour se connecter à votre source de données, par exemple, le nom du serveur et de la base de données, ainsi que la méthode d’authentification. Si vous fournissez un nom de source de données, ces informations ne sont pas visibles.

## <a name="odbc_dsn"></a> Option 1 : fournir un nom de source de données
Si vous souhaitez fournir les informations de connexion à l’aide d’un nom de source de données (DSN, data source name), utilisez l’applet **Administrateur de source de données ODBC** pour rechercher le nom de source de données existant ou pour créer un nouveau nom de source de données.
1.  Recherchez l’applet **Sources de données ODBC (64 bits)** dans le Panneau de configuration. Si vous avez un pilote 32 bits, ou si vous devez utiliser un pilote 32 bits, recherchez l’applet **Sources de données ODBC (32 bits)**.
2.  Lancez l’applet. La fenêtre **Administrateur de source de données ODBC** s’ouvre. L’applet se présente comme ceci.

    ![Applet Administrateur ODBC du Panneau de configuration](../../integration-services/import-export-data/media/odbc-administrator-control-panel-applet.png)
    
3.  Si vous souhaitez **utiliser un nom de source de données existant** pour votre source de données, vous pouvez utiliser tout nom de source de données figurant sous les onglets **Source de données utilisateur**, **Source de données système** ou **Source de données fichier**. Vérifiez le nom, puis revenez dans l’Assistant et entrez-le dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une destination**. Ignorez le reste de cette page et passez à l’étape suivante de l’Assistant.
4.  Si vous souhaitez **créer un nom de source de données**, décidez si vous souhaitez qu’il ne soit visible que par vous (Source de données utilisateur), qu’il soit visible par tous les utilisateurs de l’ordinateur, y compris les services Windows (Source de données système) ou qu’il soit enregistré dans un fichier (Source de données fichier). Cet exemple crée un nom de source de données système.
5. Sous l’onglet **Source de données système**, cliquez sur **Ajouter**.

    ![Ajouter un nouveau nom de source de données système ODBC](../../integration-services/import-export-data/media/add-a-new-odbc-system-dsn.png)
    
6.  Dans la boîte de dialogue **Créer une nouvelle source de données**, sélectionnez le pilote de votre source de données, puis cliquez sur **Terminer**.

    ![Choisir le pilote du nouveau nom de source de données système](../../integration-services/import-export-data/media/pick-driver-for-new-system-dsn.png)
    
7. Le pilote affiche maintenant un ou plusieurs écrans spécifiques au pilote dans lesquelles vous entrez les informations nécessaires pour la connexion à votre source de données. (Pour le pilote SQL Server, par exemple, il existe quatre pages de paramètres personnalisés.) Une fois que vous avez terminé, le nouveau nom de source de données système s’affiche dans la liste.

    ![Nouveau nom de source de données système dans la liste](../../integration-services/import-export-data/media/new-system-dsn-in-list.png)
    
8.  Revenez dans l’Assistant et entrez le nom de la source de données dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une destination**. Passez à l’étape suivante de l’Assistant.

## <a name="odbc_connstring"></a> Option 2 : fournir une chaîne de connexion
Si vous souhaitez fournir vos informations de connexion à l’aide d’une chaîne de connexion, le reste de cette rubrique vous aide à obtenir la chaîne de connexion dont vous avez besoin.

Cet exemple utilise la chaîne de connexion suivante qui permet de se connecter à Microsoft SQL Server.

    ```
    Driver={ODBC Driver 13 for SQL Server};server=localhost;database=WideWorldImporters;trusted_connection=Yes;
    ```

Entrez la chaîne de connexion dans le champ **ConnectionString** de la page **Choisir une source de données** ou **Choisir une destination**. Une fois que vous avez entré la chaîne de connexion, l’Assistant analyse cette chaîne et affiche les propriétés individuelles avec leur valeur dans la liste.

Voici l’écran que vous voyez après avoir entré la chaîne de connexion.

![Connexion à SQL avec ODBC après](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-after.jpg)

> [!NOTE]
> Les options de connexion d’un pilote ODBC sont les mêmes que vous configuriez la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

## <a name="get-the-connection-string-online"></a>Obtenir la chaîne de connexion en ligne
Pour rechercher des chaînes de connexion pour votre pilote ODBC en ligne, consultez [The Connection Strings Reference](https://www.connectionstrings.com/). Ce site tiers contient des exemples de chaînes de connexion et des renseignements complémentaires sur les fournisseurs de données et les informations de connexion dont ils ont besoin.

## <a name="get-the-connection-string-with-an-app"></a>Obtenir la chaîne de connexion en ligne avec une application
Pour générer et tester la chaîne de connexion pour votre pilote ODBC sur votre ordinateur, vous pouvez utiliser l’applet **Administrateur de source de données ODBC** du Panneau de configuration. Créez un nom de source de données fichier pour votre connexion, puis copiez les paramètres du nom de source de données fichier afin d’assembler la chaîne de connexion. Cela nécessite plusieurs étapes, mais garantit que la chaîne de connexion est valide.

1.  Recherchez l’applet **Sources de données ODBC (64 bits)** dans le Panneau de configuration. Si vous avez un pilote 32 bits, ou si vous devez utiliser un pilote 32 bits, recherchez l’applet **Sources de données ODBC (32 bits)**.
2.  Lancez l’applet. La fenêtre **Administrateur de source de données ODBC** s’ouvre.
3.  Cliquez sur l’onglet **Source de données fichier** de l’applet. Cliquez sur **Ajouter**.

    Pour les besoins de cet exemple, créez un nom de source de données fichier plutôt qu’utilisateur ou système, car celui-ci enregistre les paires nom-valeur dans le format spécifique requis pour la chaîne de connexion.

    ![Ajouter un nouveau nom de source de données fichier ODBC](../../integration-services/import-export-data/media/add-a-new-odbc-file-dsn.png)

4.  Dans la boîte de dialogue **Créer une nouvelle source de données**, sélectionnez votre pilote dans la liste, puis cliquez sur **suivant**. Cet exemple va créer un nom de source de données qui contient les arguments de chaîne de connexion dont nous avons besoin pour nous connecter à Microsoft SQL Server.

    ![Créer une nouvelle source de données ODBC](../../integration-services/import-export-data/media/create-new-odbc-data-source.png)
    
5.  Sélectionnez un emplacement et entrez un nom de fichier pour le nouveau nom de source de données fichier, puis cliquez sur **Suivant**. Rappelez-vous de l’emplacement où vous enregistrez le fichier afin de pouvoir le trouver et l’ouvrir dans une étape ultérieure.

    ![Enregistrer le nouveau nom de source de données fichier](../../integration-services/import-export-data/media/save-new-file-dsn.png)

6.  Passez en revue le récapitulatif de vos sélections, puis cliquez sur **Terminer**.

7.  Après avoir cliqué sur **Terminer**, le pilote que vous avez sélectionné affiche un ou plusieurs écrans propriétaires permettant de rassembler les informations dont il a besoin pour se connecter. En général, ces informations incluent le serveur, les informations de connexion et la base de données pour les sources de données sur serveur, ou encore le fichier, le format et la version pour les sources de données sur fichiers.

8. Une fois que vous avez configuré votre source de données et cliqué sur **Terminer**, un récapitulatif de vos sélections s’affiche généralement et vous avez la possibilité de les tester.

    ![Tester le nouveau nom de source de données fichier](../../integration-services/import-export-data/media/test-new-file-dsn.png)

9. Une fois que vous avez testé votre source de données et fermé les boîtes de dialogue, recherchez le nom de source de données fichier dans lequel vous l’avez enregistrée dans le système de fichiers. Si vous n’avez pas modifié l’extension de fichier, l’extension par défaut est .DSN.

10. Ouvrez le fichier enregistré dans le Bloc-notes ou un autre éditeur de texte. Voici le contenu de notre exemple SQL Server.

    ```   
    [ODBC]  
    DRIVER=ODBC Driver 13 for SQL Server  
    TrustServerCertificate=No  
    DATABASE=WideWorldImporters    
    WSID=<local computer name>  
    APP=Microsoft® Windows® Operating System  
    Trusted_Connection=Yes  
    SERVER=localhost   
    ```
        
11. Copiez et collez les valeurs nécessaires dans une chaîne de connexion dans laquelle les paires nom-valeur sont séparées par des points-virgules.

    Une fois que vous avez assemblé les valeurs nécessaires à partir de l’exemple de nom de source de données fichier, vous disposez de la chaîne de connexion suivante.
    
        ```
        DRIVER=ODBC Driver 13 for SQL Server;SERVER=localhost;DATABASE=WideWorldImporters;Trusted_Connection=Yes
        ```

    Vous n’avez généralement pas besoin de tous les paramètres d’un nom de source de données créé par l’Administrateur de source de données ODBC pour créer une chaîne de connexion qui fonctionne.  
    -   Vous devez toujours spécifier le pilote ODBC.
    -   Pour une source de données sur serveur tel que SQL Server, vous avez en général besoin des informations de Server, Database et des informations de connexion. Ainsi, dans l’exemple de nom de source de données, vous n’avez pas besoin des informations de TrustServerCertificate, WSID ou Application.
    -   Pour une source de données sur fichier, vous avez au minimum besoin du nom de fichier et de l’emplacement.
    
12. Copiez la chaîne de connexion dans le champ **ConnectionString** de la page **Choisir une source de données** ou **Choisir une Destination de l’Assistant**. L’Assistant analyse la chaîne et vous êtes prêt à continuer.

    ![Connexion à SQL avec ODBC après](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-after.jpg)

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)


