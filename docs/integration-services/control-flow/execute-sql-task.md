---
title: Exécuter des requêtes SQL, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executesqltask.f1
- sql13.dts.designer.executesqltask.general.f1
- sql13.dts.designer.executesqltask.parametermapping.f1
- sql13.dts.designer.executesqltask.resultset.f1
helpviewer_keywords:
- Transact-SQL statements, SSIS
- statements [Integration Services]
- batches [Integration Services]
- Execute SQL task [Integration Services]
ms.assetid: bebb2e8c-0410-43b2-ac2f-6fc80c8f2e9e
caps.latest.revision: 115
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 48c90af75a51d0b849f1ce7b0a714bd403e9018d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-sql-task"></a>Tâche d’exécution de requêtes SQL
  La tâche d'exécution SQL exécute des instructions ou des procédures stockées SQL à partir d'un package. La tâche peut contenir une seule ou plusieurs instructions SQL s'exécutant de façon séquentielle. Vous pouvez utiliser la tâche d'exécution SQL aux fins suivantes :  
  
-   Tronquer une table ou une vue pour la préparer à l'insertion de données.  
  
-   Créer, modifier et supprimer des objets de base de données tels que des tables et des vues.  
  
-   Recréer des tables de faits et de dimension avant d'y charger des données.  
  
-   Exécuter des procédures stockées. Si l'instruction SQL appelle une procédure stockée qui retourne des résultats à partir d'une table temporaire, utilisez l'option WITH RESULT SETS afin de définir les métadonnées du jeu de résultats.  
  
-   Enregistrer dans une variable l'ensemble de lignes retourné par une requête.  
  
 Vous pouvez combiner la tâche d'exécution SQL et les conteneurs de boucles Foreach et For pour exécuter plusieurs instructions SQL. Ces conteneurs mettent en œuvre des flux de contrôle répétitifs dans un package et peuvent exécuter la tâche d'exécution SQL de façon répétée. Par exemple, à l'aide du conteneur de boucles Foreach, un package peut passer en revue les fichiers d'un dossier et exécuter une tâche d'exécution SQL de façon répétée afin d'exécuter l'instruction SQL stockée dans chaque fichier.  
  
## <a name="connect-to-a-data-source"></a>Connexion à une source de données  
 La tâche d'exécution SQL peut utiliser différents types de gestionnaires de connexions pour se connecter à la source de données dans laquelle elle exécute l'instruction ou la procédure stockée SQL. La tâche peut utiliser les types de connexion décrits dans le tableau suivant.  
  
|Type de connexion|Gestionnaire de connexions|  
|---------------------|------------------------|  
|EXCEL|[Gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager.md)|  
|OLE DB|[Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)|  
|ODBC|[Gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager.md)|  
|ADO|[Gestionnaire de connexions ADO](../../integration-services/connection-manager/ado-connection-manager.md)|  
|ADO.NET|[Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md)|  
|SQLMOBILE|[Gestionnaire de connexions de SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|  
  
## <a name="create-sql-statements"></a>Créer des instructions SQL  
 La source des instructions SQL utilisée par cette tâche peut être une propriété de tâche contenant une instruction, une connexion à un fichier contenant une ou plusieurs instructions ou le nom d'une variable contenant une instruction. Les instructions SQL doivent être écrites dans le langage du système de gestion de bases de données (SGBD) source. Pour plus d’informations, consultez [Requêtes Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-queries.md).  
  
 Si les instructions SQL sont stockées dans un fichier, la tâche utilise un gestionnaire de connexions de fichiers pour se connecter au fichier. Pour plus d’informations, consultez [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 Dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous pouvez utiliser la boîte de dialogue **Éditeur de tâche d’exécution de requêtes SQL** pour taper des instructions SQL ou utiliser le **Générateur de requêtes**, une interface graphique utilisateur permettant de créer des requêtes SQL. 
  
> [!NOTE]  
>  Les instructions SQL valides écrites en dehors de la tâche d'exécution SQL peuvent ne pas être analysées correctement par celle-ci.  
  
> [!NOTE]  
>  La tâche d’exécution de requêtes SQL utilise la valeur d’énumération **RecognizeAll** ParseMode. Pour plus d’informations, consultez [ManagedBatchParser Namespace](http://go.microsoft.com/fwlink/?LinkId=223617)(Espace de noms ManagedBatchParser).  
  
## <a name="send-multiple-statements-in-a-batch"></a>Envoyer plusieurs instructions dans un traitement  
 Si vous incluez plusieurs instructions dans une tâche d'exécution SQL, vous pouvez les regrouper et les exécuter sous forme de traitement. Pour indiquer la fin d'un traitement, utilisez la commande GO. Toutes les instructions SQL comprises entre deux commandes GO sont envoyées dans un traitement au fournisseur OLE DB afin d'être exécutées. La commande SQL peut comprendre plusieurs traitements séparés par des commandes GO.  
  
 Il existe des restrictions sur les types d'instructions SQL pouvant être regroupées dans un traitement. Pour plus d’informations, consultez [Lots d’instructions](../../relational-databases/native-client-odbc-queries/executing-statements/batches-of-statements.md).  
  
 Si la tâche d'exécution SQL exécute un traitement d'instructions SQL, les règles suivantes s'appliquent à celui-ci :  
  
-   Une seule instruction peut retourner un ensemble de résultats et il doit s'agir de la première instruction du traitement.  
  
-   Si l'ensemble de résultats utilise des liaisons de résultats, les requêtes doivent retourner le même nombre de colonnes. Si les requêtes retournent un nombre différent de colonnes, la tâche échoue. Toutefois, même si la tâche échoue, les requêtes qu'elle exécute, telles que les requêtes DELETE ou INSERT, peuvent réussir.  
  
-   Si les liaisons de résultats utilisent des noms de colonne, la requête doit retourner des colonnes portant les mêmes noms que l'ensemble de résultats utilisé par la tâche. Si les colonnes sont manquantes, la tâche échoue.  
  
-   Si la tâche utilise la liaison de paramètre, toutes les requêtes du traitement doivent avoir le même nombre et les mêmes types de paramètres.  
  
## <a name="run-parameterized-sql-commands"></a>Exécuter des commandes SQL paramétrables  
 Les instructions et les procédures stockées SQL utilisent fréquemment des paramètres d'entrée, des paramètres de sortie et des codes de retour. La tâche d’exécution de requêtes SQL prend en charge les types de paramètres **Input**, **Output**et **ReturnValue** . Vous utilisez le type **Input** pour les paramètres d’entrée, **Output** pour les paramètres de sortie et **ReturnValue** pour les codes de retour.  
  
> [!NOTE]  
>  Vous ne pouvez utiliser des paramètres dans une tâche d'exécution SQL que si le fournisseur de données les prend en charge.  
  
## <a name="specify-a-result-set-type"></a>Spécifier un type d’ensemble de résultats  
 Selon le type de commande SQL, un ensemble de résultats peut ou non être retourné à la tâche d'exécution SQL. Par exemple, une instruction SELECT retourne généralement un ensemble de résultats, contrairement à une instruction INSERT. L'ensemble de résultats issu d'une instruction SELECT peut contenir un nombre de lignes quelconque (aucune ligne, une ligne ou de nombreuses lignes). Les procédures stockées peuvent également retourner une valeur entière, appelée « code de retour », qui indique l'état de leur exécution. Dans ce cas, l'ensemble de résultats comprend une seule ligne.  
  
## <a name="configure-the-execute-sql-task"></a>Configurer la tâche Exécuter des requêtes SQL  
 Vous pouvez configurer la tâche d'exécution SQL comme suit :  
  
-   Spécifiez le type de gestionnaire de connexions à utiliser pour établir la connexion à une base de données.  
  
-   Spécifiez le type d'ensemble de résultats retourné par l'instruction SQL.  
  
-   Spécifiez un délai d'expiration pour les instructions SQL.  
  
-   Spécifiez la source de l'instruction SQL.  
  
-   Indiquez si la tâche passe la phase de préparation de l'instruction SQL.  
  
-   Si vous utilisez le type de connexion ADO, indiquez si l'instruction SQL est une procédure stockée. Pour d’autres types de connexion, cette propriété est définie en lecture seule et sa valeur est toujours **false**.  
  
 Vous pouvez définir les propriétés par programmation ou par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  

## <a name="general-page---execute-sql-task-editor"></a>Page Général - Éditeur de tâche d’exécution de requêtes SQL
 Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche d'exécution SQL** pour configurer la tâche d'exécution SQL et indiquer l'instruction SQL que la tâche exécuter.  

Pour plus d’informations sur le langage Transact-SQL, consultez [Référence Transact-SQL &#40;moteur de base de données&#41;](../../t-sql/transact-sql-reference-database-engine.md).  
  
### <a name="static-options"></a>Options statiques  
 **Nom**  
 Donnez un nom unique à la tâche d'exécution SQL dans le flux de travail. Le nom fourni est affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la tâche d'exécution SQL. Pour réaliser des packages autodocumentés plus faciles à maintenir, fournissez une description de la tâche en rapport avec son objectif.  
  
 **TimeOut**  
 Spécifiez le nombre maximal de secondes d'exécution de la tâche au terme duquel le délai d'attente expire. La valeur 0 indique un délai infini. La valeur par défaut est 0.  
  
> [!NOTE]  
>  Les procédures stockées ne sont par concernées par le délai d'expiration si elles émulent la fonctionnalité de veille en laissant le temps nécessaire à l'établissement des connexions et à la réalisation des transactions qui est supérieur au nombre de secondes spécifié par l'option **TimeOut**. Cependant, les procédures stockées qui exécutent des requêtes sont toujours soumises à la limitation de temps spécifiée dans **TimeOut**.  
  
 **CodePage**  
 Spécifiez la page de codes à utiliser pour la traduction des valeurs Unicode en variables. Il s'agit par défaut de la page de codes de l'ordinateur local.  
  
> [!NOTE]  
>  Lorsque la tâche d'exécution SQL utilise un gestionnaire de connexions ADO ou ODBC, la propriété **CodePage** n'est pas disponible. Si votre solution requiert l'utilisation d'une page de codes, utilisez un gestionnaire de connexions OLE DB ou ADO.NET avec la tâche d'exécution SQL.  
  
 **TypeConversionMode**  
 Quand vous définissez cette propriété avec la valeur **Allowed**, la tâche d’exécution SQL tente de convertir le paramètre de sortie et les résultats de la requête dans le type de données de la variable à laquelle les résultats sont affectés. Cela s'applique au type de jeu de résultats **Ligne unique** .  
  
 **ResultSet**  
 Spécifiez le type de résultats attendu par l'instruction SQL en cours d'exécution. Choisissez parmi les options suivantes : **Ligne unique**, **Ensemble de résultats complet**, **XML**ou **Aucun**.  
  
 **ConnectionType**  
 Choisissez le type de gestionnaire de connexions à utiliser pour vous connecter à la source de données. Les types de connexions disponibles sont **OLE DB**, **ODBC**, **ADO**, **ADO.NET** et **SQLMOBILE**.  
  
 **Rubriques connexes :** [Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md), [Gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager.md), [Gestionnaire de connexions ADO](../../integration-services/connection-manager/ado-connection-manager.md), [Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md), [Gestionnaire de connexions de SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)  
  
 **Connexion**  
 Choisissez la connexion dans la liste des gestionnaires de connexions définis. Pour créer une connexion, sélectionnez \<**Nouvelle connexion...**>.  
  
 **SQLSourceType**  
 Sélectionnez le type de source de l'instruction SQL qui exécute la tâche.  
  
 Selon le type de gestionnaire de connexions que la tâche d'exécution SQL utilise, vous devez utiliser des marqueurs de paramètres spécifiques dans les instructions SQL paramétrables.    
  
 Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Définissez la source d'une instruction Transact-SQL. Sélectionnez cette valeur pour afficher l'option dynamique **SQLStatement**.|  
|**Connexion de fichiers**|Sélectionnez un fichier qui contient une instruction Transact-SQL. Configurez cette option pour afficher l'option dynamique **FileConnection**.|  
|**Variable**|Définissez la source sur une variable qui définit l'instruction Transact-SQL. Sélectionnez cette valeur pour afficher l'option dynamique **SourceVariable**.|  
  
 **QueryIsStoredProcedure**  
 Indique si l'instruction SQL spécifiée à exécuter est une procédure stockée. Cette propriété est en lecture/écriture uniquement si la tâche utilise le gestionnaire de connexions ADO. Sinon, elle est en lecture seule ; sa valeur est alors **false**.  
  
 **BypassPrepare**  
 Indiquez si l'instruction SQL est préparée.  **true** ignore la préparation ; **false** prépare l'instruction SQL avant de l'exécuter. Cette option est disponible uniquement avec les connexions OLE DB qui prennent en charge la préparation.  
  
 **Rubriques connexes :**  [Exécution préparée](../../relational-databases/native-client-odbc-queries/executing-statements/prepared-execution.md)  
  
 **Parcourir**  
 Utilisez la boîte de dialogue **Ouvrir** pour rechercher un fichier qui contient une instruction SQL. Sélectionnez un fichier dont vous voulez copier le contenu en tant qu'instruction SQL dans la propriété **SQLStatement** .  
  
 **Générer la requête**  
 Créez une instruction SQL au moyen de la boîte de dialogue **Générateur de requêtes** : il s’agit d’un outil graphique de création de requêtes. Cette option est disponible lorsque l'option **SQLSourceType** est configurée avec **Entrée directe**.  
  
 **Analyser la requête**  
 Valide la syntaxe de l’instruction SQL.  
  
### <a name="sqlsourcetype-dynamic-options"></a>Options dynamiques SQLSourceType  
  
#### <a name="sqlsourcetype--direct-input"></a>SQLSourceType = Entrée directe  
 **SQLStatement**  
 Dans la zone des options, tapez l’instruction SQL à exécuter ou cliquez sur le bouton d’exploration (…) pour taper l’instruction SQL dans la boîte de dialogue **Entrer une requête SQL** . Vous pouvez également cliquer sur **Générer la requête** pour composer l’instruction à l’aide de la boîte de dialogue **Générateur de requêtes** .  
  
 **Rubriques connexes :** [Générateur de requêtes](http://msdn.microsoft.com/library/780752c9-6e3c-4f44-aaff-4f4d5e5a45c5)  
  
#### <a name="sqlsourcetype--file-connection"></a>SQLSourceType = Connexion de fichiers  
 **FileConnection**  
 Sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**Nouvelle connexion...**> pour en créer un.  
  
 **Rubriques connexes :** [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md), [File Connection Manager Editor](../../integration-services/connection-manager/file-connection-manager-editor.md)  
  
#### <a name="sqlsourcetype--variable"></a>SQLSourceType = Variable  
 **SourceVariable**  
 Sélectionnez une variable existante ou cliquez sur \<**Nouvelle variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](http://msdn.microsoft.com/library/d09b5d31-433f-4f7c-8c68-9df3a97785d5)  
 
## <a name="parameter-mapping-page---execute-sql-task-editor"></a>Page Mappage de paramètre - Éditeur de tâche d’exécution de requêtes SQL
Utilisez la page **Mappage de paramètre** de la boîte de dialogue **Éditeur de tâche d’exécution de requêtes SQL** pour associer des variables à des paramètres dans une instruction SQL.  
  
### <a name="options"></a>Options  
 **Nom de la variable**  
 Après avoir ajouté un mappage de paramètre en cliquant sur **Ajouter**, sélectionnez une variable système ou une variable définie par l’utilisateur dans la liste, ou cliquez sur \<**Nouvelle variable...**> pour ajouter une nouvelle variable via la boîte de dialogue **Ajouter une variable**.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)  
  
 **Sens**  
 Sélectionnez le sens du paramètre. Associez chaque variable à un paramètre d'entrée, un paramètre de sortie ou un code de retour.  
  
 **Type de données**  
 Sélectionnez le type de données du paramètre. La liste des types de données disponibles est propre au fournisseur sélectionné dans le gestionnaire de connexions utilisé par la tâche.  
  
 **Nom du paramètre**  
 Fournissez un nom de paramètre.  
  
 En fonction du type de gestionnaire de connexions que la tâche utilise, vous devez utiliser des nombres ou des noms de paramètres. Avec certaines types de gestionnaires de connexions, le premier caractère du nom de paramètre doit être le signe @, des noms spécifiques tels que @Param1 doivent être utilisés ou les noms de colonnes doivent être utilisés comme noms de paramètres.  
  
 **Taille de paramètre**  
 Indiquez la taille des paramètres qui ont une longueur variable, par exemple les chaînes et champs binaires.  
  
 Ce paramètre garantit que le fournisseur alloue l'espace suffisant pour les valeurs de paramètre à longueur variable.  
  
 **Ajouter**  
 Cliquez pour ajouter une association de paramètre.  
  
 **Supprimer**  
 Sélectionnez une association de paramètre dans la liste et cliquez sur **Supprimer**.  
 
## <a name="result-set-page---execute-sql-task-editor"></a>Page Ensemble de résultats - Éditeur de tâche d’exécution SQL
Utilisez la page **Jeu de résultats** de la boîte de dialogue **Éditeur de tâche d’exécution de requêtes SQL** pour mapper le résultat de l’instruction SQL à des variables nouvelles ou existantes. Les options de cette boîte de dialogue sont désactivées si **ResultSet** dans la page Général est défini sur **Aucun**.  
  
### <a name="options"></a>Options  
 **Nom de résultat**  
 Après avoir ajouté un ensemble de mappages d’un jeu de résultats en cliquant sur **Ajouter**, donnez un nom au résultat. Selon le type de jeu de résultats, vous devez utiliser des noms de résultats spécifiques.  
  
 Si le type de jeu de résultats est **Ligne unique**, le nom peut être celui d’une colonne retournée par la requête ou le numéro qui, dans la liste des colonnes, représente la position d’une colonne retournée par la requête.  
  
 Si le type de l'ensemble de résultats est **Ensemble de résultats complet** ou **XML**, vous devez utiliser 0 comme nom de jeu de résultats.  
 
  
 **Nom de la variable**  
 Mappez le jeu de résultats à une variable en sélectionnant celle-ci, ou cliquez sur \<**Nouvelle variable...**> pour ajouter une nouvelle variable via la boîte de dialogue **Ajouter une variable**.  
  
 **Ajouter**  
 Ajoute une correspondance de jeu de résultats.  
  
 **Supprimer**  
 Sélectionnez un mappage de jeu de résultats dans la liste, puis cliquez sur **Supprimer**.  
 
## <a name="parameters-in-the-execute-sql-task"></a>Paramètres de la tâche d’exécution SQL
Les instructions et les procédures stockées SQL utilisent fréquemment des paramètres **d’entrée** , des paramètres de **sortie** et des codes de retour. Dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], la tâche d’exécution SQL prend en charge les types de paramètres **Input**, **Output** et **ReturnValue**. Vous utilisez le type **Input** pour les paramètres d’entrée, **Output** pour les paramètres de sortie et **ReturnValue** pour les codes de retour.  
  
> [!NOTE]  
>  Vous ne pouvez utiliser des paramètres dans une tâche d'exécution SQL que si le fournisseur de données les prend en charge.  
  
 Les paramètres des commandes SQL, notamment les requêtes et les procédures stockées, sont mappés à des variables définies par l'utilisateur créées dans l'étendue de la tâche d'exécution SQL, un conteneur parent ou dans l'étendue du package. Les variables peuvent être définies au moment de la conception ou être remplies dynamiquement lors de l'exécution. Vous pouvez également mapper des paramètres à des variables système. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Variables système](../../integration-services/system-variables.md).  
  
 Toutefois, l'utilisation de paramètres et de codes de retour dans une tâche d'exécution SQL ne permet pas uniquement de savoir quels types de paramètres sont pris en charge par la tâche et de quelle manière ces paramètres seront mappés. D'autres indications et spécifications d'utilisation permettent d'utiliser avec succès des paramètres et des codes de retour dans la tâche d'exécution SQL. Le reste de cette rubrique traite de ces indications et spécifications d'utilisation.  
  
-   [Utiliser des marqueurs et des noms de paramètres](#Parameter_names_and_markers)  
  
-   [Utiliser des paramètres avec les types de données de date et d’heure](#Date_and_time_data_types)  
  
-   [Utiliser des paramètres dans les clauses WHERE](#WHERE_clauses)  
  
-   [Utiliser des paramètres avec les procédures stockées](#Stored_procedures)  
  
-   [Obtenir les valeurs de codes de retour](#Return_codes)    
  
###  <a name="Parameter_names_and_markers"></a> Utiliser des marqueurs et des noms de paramètres  
 Selon le type de connexion que la tâche d'exécution SQL utilise, la syntaxe de la commande SQL utilise différents marqueurs de paramètres. Par exemple, le type de gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] exige que la commande SQL utilise un marqueur de paramètre au format **@varParameter**, tandis que le type de connexion OLE DB requiert le marqueur de paramètre point d’interrogation (?).  
  
 Les noms que vous pouvez utiliser comme noms de paramètres dans les mappages entre variables et paramètres varient également selon le type de gestionnaire de connexions. Par exemple, le type de gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] utilise un nom défini par l'utilisateur à préfixe @, tandis que le type de gestionnaire de connexions OLE DB impose l'utilisation de la valeur numérique d'un ordinal de base 0 comme nom de paramètre.  
  
 Le tableau suivant indique les conditions requises des commandes SQL pour les types de gestionnaires de connexions que la tâche d'exécution SQL peut utiliser.  
  
|Type de connexion|Marqueur de paramètre|Nom du paramètre|Exemple de commande SQL|  
|---------------------|----------------------|--------------------|-------------------------|  
|ADO|?|Param1, Param2, …|SELECT FirstName, LastName, Title FROM Person.Contact WHERE ContactID = ?|  
|[!INCLUDE[vstecado](../../includes/vstecado-md.md)]|@\<nom du paramètre>|@\<nom du paramètre>|SELECT FirstName, LastName, Title FROM Person.Contact WHERE ContactID = @parmContactID|  
|ODBC|?|1, 2, 3, …|SELECT FirstName, LastName, Title FROM Person.Contact WHERE ContactID = ?|  
|EXCEL et OLE DB|?|0, 1, 2, 3, …|SELECT FirstName, LastName, Title FROM Person.Contact WHERE ContactID = ?|  
  
#### <a name="use-parameters-with-adonet-and-ado-connection-managers"></a>Utiliser des paramètres avec les gestionnaires de connexions ADO.NET et ADO  
 Les gestionnaires de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] et ADO ont des spécifications particulières pour les commandes SQL qui utilisent des paramètres :  
  
-   Les gestionnaires de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] exigent que la commande SQL utilise des noms de paramètres comme marqueurs de paramètres. Cela signifie que des variables peuvent être mappées directement à des paramètres. Par exemple, la variable `@varName` est mappée au paramètre nommé `@parName` et fournit une valeur au paramètre `@parName`.  
  
-   Les gestionnaires de connexions ADO.NET imposent que la commande SQL utilise des points d'interrogation (?) comme marqueurs de paramètres. Toutefois, vous pouvez utiliser les noms définis par l'utilisateur, à l'exception des valeurs entières, comme noms de paramètres.  
  
 Pour fournir des valeurs aux paramètres, les variables sont mappées à des noms de paramètres. Puis, la tâche d'exécution SQL utilise la valeur ordinale du nom de paramètre dans la liste des paramètres pour charger des valeurs de variables dans des paramètres.  
  
#### <a name="use-parameters-with-excel-odbc-and-ole-db-connection-managers"></a>Utiliser des paramètres avec les gestionnaires de connexions EXCEL, ODBC et OLE DB  
 Les gestionnaires de connexions EXCEL, ODBC et OLE DB imposent que la commande SQL utilise des points d'interrogation (?) comme marqueurs de paramètres et des valeurs numériques de base 0 et de base 1 comme noms de paramètres. Si la tâche d'exécution SQL utilise le gestionnaire de connexions ODBC, le nom de paramètre qui mappe au premier paramètre dans la requête est nommé 1 ; sinon, le paramètre est nommé 0. Pour les paramètres suivants, la valeur numérique du nom de paramètre indique le paramètre dans la commande SQL à laquelle le nom de paramètre mappe. Par exemple, le paramètre nommé 3 est mappé au troisième paramètre, qui est représenté par le troisième point d'interrogation (?) dans la commande SQL.  
  
 Pour fournir des valeurs aux paramètres, les variables sont mappées à des noms de paramètres et la tâche d'exécution SQL utilise la valeur ordinale du nom du paramètre pour charger des valeurs de variables dans des paramètres.  
  
 Selon le fournisseur que le gestionnaire de connexions utilise, certains types de données OLE DB peuvent ne pas être pris en charge. Par exemple, le pilote Excel ne reconnaît qu'un ensemble limité de types de données. Pour plus d’informations sur le comportement du fournisseur Jet avec le pilote Excel, consultez [Source Excel](../../integration-services/data-flow/excel-source.md).  
  
#### <a name="use-parameters-with-ole-db-connection-managers"></a>Utiliser des paramètres avec les gestionnaires de connexions OLE DB  
 Quand la tâche d’exécution SQL utilise le gestionnaire de connexions OLE DB, la propriété BypassPrepare de la tâche est disponible. Vous devez définir cette propriété à **true** si la tâche d’exécution SQL utilise des instructions SQL avec des paramètres.  
  
 Lorsque vous utilisez un gestionnaire de connexions OLE DB, vous ne pouvez pas utiliser de sous-requêtes paramétrables, car la tâche d'exécution SQL ne peut pas dériver d'informations de paramètre par le biais du fournisseur OLE DB. Toutefois, vous pouvez utiliser une expression pour concaténer les valeurs des paramètres dans la chaîne de requête et définir la propriété SqlStatementSource de la tâche.  
  
###  <a name="Date_and_time_data_types"></a> Utiliser des paramètres avec les types de données de date et d’heure  
  
#### <a name="use-date-and-time-parameters-with-adonet-and-ado-connection-managers"></a>Utiliser des paramètres de date et d’heure avec les gestionnaires de connexions ADO.NET et ADO  
 Au moment de la lecture des données des types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **time** et **datetimeoffset**, une tâche d’exécution SQL qui utilise un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] ou ADO a les spécifications supplémentaires suivantes :  
  
-   Concernant les données de type **time**, un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] impose que ces données soient stockées dans un paramètre de type **Input** ou **Output**, et dont le type de données est **string**.  
  
-   Pour les données **datetimeoffset**, un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] impose que ces données soient stockées dans l’un des paramètres suivants :  
  
    -   Un paramètre de type **Input** et dont le type de données est **string**.  
  
    -   Un paramètre de type **Output** ou **ReturnValue**, et dont le type de données est **datetimeoffset**, **string**ou **datetime2**. Si vous sélectionnez un paramètre dont le type de données est **string** ou **datetime2**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] convertit les données en string ou datetime2.  
  
-   Un gestionnaire de connexions ADO impose que les données **time** ou **datetimeoffset** soient stockées dans un paramètre de type **Input** ou **Output** et dont le type de données est **adVarWchar**.  
  
 Pour plus d’informations sur les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et leur mappage aux types de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) et [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
#### <a name="use-date-and-time-parameters-with-ole-db-connection-managers"></a>Utiliser des paramètres de date et d’heure avec les gestionnaires de connexions OLE DB  
 Pendant l’utilisation d’un gestionnaire de connexions OLE DB, une tâche d’exécution SQL a des spécifications de stockage particulières pour les données des types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **date**, **time**, **datetime**, **datetime2** et **datetimeoffset**. Vous devez stocker ces données dans l'un des types de paramètres suivants :  
  
-   Un paramètre d'entrée doté du type de données NVARCHAR.  
  
-   Un paramètre de sortie doté du type de données approprié, tel que répertorié dans le tableau suivant.  
  
    |Type de paramètre de **sortie**|Type de données Date|  
    |-------------------------------|--------------------|  
    |DBDATE|**date**|  
    |DBTIME2|**time**|  
    |DBTIMESTAMP|**datetime**, **datetime2**|  
    |DBTIMESTAMPOFFSET|**datetimeoffset**|  
  
 Si les données ne sont pas stockées dans le paramètre d'entrée ou de sortie approprié, le package échoue.  
  
#### <a name="use-date-and-time-parameters-with-odbc-connection-managers"></a>Utiliser des paramètres de date et d’heure avec les gestionnaires de connexions ODBC  
 Pendant l’utilisation d’un gestionnaire de connexions ODBC, une tâche d’exécution SQL a des spécifications de stockage particulières pour les données de l’un des types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **date**, **time**, **datetime**, **datetime2** ou **datetimeoffset**. Vous devez stocker ces données dans l'un des types de paramètres suivants :  
  
-   Un paramètre **d’entrée** doté du type de données SQL_WVARCHAR  
  
-   Un paramètre de **sortie** doté du type de données approprié, tel que répertorié dans le tableau suivant.  
  
    |Type de paramètre de**sortie** |Type de données Date|  
    |-------------------------------|--------------------|  
    |SQL_DATE|**date**|  
    |SQL_SS_TIME2|**time**|  
    |SQL_TYPE_TIMESTAMP<br /><br /> -ou-<br /><br /> SQL_TIMESTAMP|**datetime**, **datetime2**|  
    |SQL_SS_TIMESTAMPOFFSET|**datetimeoffset**|  
  
 Si les données ne sont pas stockées dans le paramètre d'entrée ou de sortie approprié, le package échoue.  
  
###  <a name="WHERE_clauses"></a> Utiliser des paramètres dans les clauses WHERE  
 Les commandes SELECT, INSERT, UPDATE et DELETE incluent fréquemment des clauses WHERE pour spécifier des filtres qui définissent les conditions auxquelles chaque ligne des tables sources doit satisfaire pour se qualifier pour une commande SQL. Les paramètres fournissent les valeurs de filtre dans les clauses WHERE.  
  
 Vous pouvez utiliser des marqueurs de paramètres pour fournir dynamiquement des valeurs de paramètres. Les règles pour lesquelles des marqueurs de paramètres et des noms de paramètres peuvent être utilisés dans l'instruction SQL varient selon le type de gestionnaire de connexions que la tâche d'exécution SQL utilise.  
  
 Le tableau suivant présente des exemples de la commande SELECT par type de gestionnaire de connexions. Les instructions INSERT, UPDATE et DELETE sont similaires. Les exemples utilisent la commande SELECT pour retourner les produits de la table **Product** dans [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] qui ont un **ProductID** supérieur et inférieur aux valeurs spécifiées par deux paramètres.  
  
|Type de connexion|Syntaxe SELECT|  
|---------------------|-------------------|  
|EXCEL, ODBC et OLEDB|`SELECT* FROM Production.Product WHERE ProductId > ? AND ProductID < ?`|  
|ADO|`SELECT* FROM Production.Product WHERE ProductId > ? AND ProductID < ?`|  
|[!INCLUDE[vstecado](../../includes/vstecado-md.md)]|`SELECT* FROM Production.Product WHERE ProductId > @parmMinProductID AND ProductID < @parmMaxProductID`|  
  
 Les exemples requièrent des paramètres avec les noms suivants :  
  
-   Les gestionnaires de connexions EXCEL et OLED DB utilisent les noms de paramètres 0 et 1. Le type de connexion ODBC utilise 1 et 2.  
  
-   Le type de connexion ADO peut utiliser deux noms de paramètres (par exemple, Param1 et Param2), mais les paramètres doivent être mappés selon leur position ordinale dans la liste des paramètres.  
  
-   Le type de connexion [!INCLUDE[vstecado](../../includes/vstecado-md.md)] utilise les noms de paramètres @parmMinProductID et @parmMaxProductID.  
  
###  <a name="Stored_procedures"></a> Utiliser des paramètres avec les procédures stockées  
 Les commandes SQL qui exécutent des procédures stockées peuvent également utiliser le mappage de paramètres. Les règles d'utilisation des marqueurs de paramètres et des noms de paramètres varient selon le type de gestionnaire de connexions que la tâche d'exécution SQL utilise, tout comme les règles des requêtes paramétrables.  
  
 Le tableau suivant présente des exemples de la commande EXEC par type de gestionnaire de connexions. Les exemples exécutent la procédure stockée **uspGetBillOfMaterials** dans [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)]. La procédure stockée utilise les paramètres `@StartProductID` d’entrée `@CheckDate` **et** .  
  
|Type de connexion|Syntaxe EXEC|  
|---------------------|-----------------|  
|EXCEL et OLEDB|`EXEC uspGetBillOfMaterials ?, ?`|  
|ODBC|`{call uspGetBillOfMaterials(?, ?)}`<br /><br /> Pour plus d’informations sur la syntaxe d’appel ODBC, consultez la rubrique [Paramètres de procédure](http://go.microsoft.com/fwlink/?LinkId=89462)dans le Guide de référence du programmeur ODBC publié dans MSDN Library.|  
|ADO|Si IsQueryStoredProcedure est défini sur **False**, `EXEC uspGetBillOfMaterials ?, ?`<br /><br /> Si IsQueryStoredProcedure est défini sur **True**, `uspGetBillOfMaterials`|  
|[!INCLUDE[vstecado](../../includes/vstecado-md.md)]|Si IsQueryStoredProcedure est défini sur **False**, `EXEC uspGetBillOfMaterials @StartProductID, @CheckDate`<br /><br /> Si IsQueryStoredProcedure est défini sur **True**, `uspGetBillOfMaterials`|  
  
 Pour utiliser des paramètres de sortie, la syntaxe impose que le mot clé OUTPUT suive chaque marqueur de paramètre. Par exemple, la syntaxe de paramètre de sortie suivante est correcte : `EXEC myStoredProcedure ? OUTPUT`.  
  
 Pour plus d’informations sur l’utilisation de paramètres d’entrée et de sortie avec des procédures stockées Transact-SQL, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
 
### <a name="map-query-parameters-to-variables"></a>Mapper des paramètres à des variables
Cette section décrit comment utiliser une instruction SQL paramétrable dans la tâche d’exécution SQL et créer des mappages entre des variables et les paramètres de l’instruction SQL.  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que vous voulez utiliser.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Si le package ne contient pas déjà une tâche d'exécution SQL, ajoutez-en une au flux de contrôle du package. Pour plus d’informations, consultez [Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
5.  Double-cliquez sur la tâche d'exécution SQL.  
  
6.  Indiquez une commande SQL paramétrable de l'une des manières suivantes :  
  
    -   Utilisez l’entrée directe et tapez la commande SQL dans la propriété SQLStatement.  
  
    -   Utilisez l’entrée directe, cliquez sur **Générer la requête**, puis créez une commande SQL à l’aide des outils graphiques fournis par le Générateur de requêtes.  
  
    -   Utilisez un fichier de connexion, puis référencez le fichier contenant la commande SQL.  
  
    -   Utilisez une variable, puis référencez la variable contenant la commande SQL.  
  
     Les marqueurs de paramètres que vous utilisez dans les instructions SQL paramétrables sont liés au type de connexion que la tâche d'exécution SQL utilise.  
  
    |Type de connexion|Marqueur de paramètre|  
    |---------------------|----------------------|  
    |ADO|?|  
    |ADO.NET et SQLMOBILE|@\<nom du paramètre>|  
    |ODBC|?|  
    |EXCEL et OLE DB|?|  
  
     Le tableau suivant présente des exemples de la commande SELECT par type de gestionnaire de connexions. Les paramètres fournissent les valeurs de filtre dans les clauses WHERE. Les exemples utilisent la commande SELECT pour retourner les produits de la table **Product** dans [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] qui ont un **ProductID** supérieur et inférieur aux valeurs spécifiées par deux paramètres.  
  
    |Type de connexion|Syntaxe SELECT|  
    |---------------------|-------------------|  
    |EXCEL, ODBC et OLEDB|`SELECT* FROM Production.Product WHERE ProductId > ? AND ProductID < ?`|  
    |ADO|`SELECT* FROM Production.Product WHERE ProductId > ? AND ProductID < ?`|  
    |[!INCLUDE[vstecado](../../includes/vstecado-md.md)]|`SELECT* FROM Production.Product WHERE ProductId > @parmMinProductID AND ProductID < @parmMaxProductID`|  
   
7.  Cliquez sur **Mappage de paramètre**.  
  
8.  Pour ajouter un mappage de paramètre, cliquez sur **Ajouter**.  
  
9. Fournissez un nom dans la zone **Nom du paramètre** .  
  
     Les noms de paramètres que vous utilisez sont liés au type de connexion que la tâche d'exécution SQL utilise.  
  
    |Type de connexion|Nom du paramètre|  
    |---------------------|--------------------|  
    |ADO|Param1, Param2, …|  
    |ADO.NET et SQLMOBILE|@\<nom du paramètre>|  
    |ODBC|1, 2, 3, …|  
    |EXCEL et OLE DB|0, 1, 2, 3, …|  
  
10. Dans la liste **Nom de variable** , sélectionnez une variable. Pour plus d’informations, consultez [Ajouter, supprimer, modifier l’étendue de la variable définie par l’utilisateur dans un package](http://msdn.microsoft.com/library/cbf40c7f-3c8a-48cd-aefa-8b37faf8b40e).  
  
11. Dans la liste **Direction** , indiquez si le paramètre est une entrée, une sortie ou une valeur de retour.  
  
12. Dans la liste **Type de données** , définissez le type de données du paramètre.  
  
    > [!IMPORTANT]  
    >  Le type de données du paramètre doit être compatible avec le type de données de la variable.  
  
13. Répétez les étapes 8 à 11 pour chaque paramètre de l'instruction SQL.  
  
    > [!IMPORTANT]  
    >  L'ordre de mappage des paramètres doit être identique à leur ordre d'apparition dans l'instruction SQL.  
  
14. Cliquez sur **OK**.  

##  <a name="Return_codes"></a> Obtenir les valeurs de codes de retour  
 Une procédure stockée peut retourner une valeur entière appelée « code de retour » pour indiquer l'état d'exécution d'une procédure. Pour implémenter des codes de retour dans la tâche d’exécution SQL, vous utilisez des paramètres du type **ReturnValue** .  
  
 Le tableau suivant présente par type de connexion des exemples de commandes EXEC qui implémentent des codes de retour. Tous les exemples utilisent un paramètre **d’entrée** . Les règles d’utilisation des marqueurs de paramètres et des noms de paramètres sont les mêmes pour tous les types de paramètre :**Input**, **Output**et **ReturnValue**.  
  
 Certaines syntaxes ne prennent pas en charge les littéraux de paramètres. Dans ce cas, vous devez fournir la valeur du paramètre en utilisant une variable.  
  
|Type de connexion|Syntaxe EXEC|  
|---------------------|-----------------|  
|EXCEL et OLEDB|`EXEC ? = myStoredProcedure 1`|  
|ODBC|`{? = call myStoredProcedure(1)}`<br /><br /> Pour plus d’informations sur la syntaxe d’appel ODBC, consultez la rubrique [Paramètres de procédure](http://go.microsoft.com/fwlink/?LinkId=89462)dans le Guide de référence du programmeur ODBC publié dans MSDN Library.|  
|ADO|Si IsQueryStoreProcedure est défini sur **False**, `EXEC ? = myStoredProcedure 1`<br /><br /> Si IsQueryStoreProcedure est défini sur **True**, `myStoredProcedure`|  
|[!INCLUDE[vstecado](../../includes/vstecado-md.md)]|Définissez IsQueryStoreProcedure sur **True**.<br /><br /> `myStoredProcedure`|  
  
 Dans la syntaxe affichée dans la table précédente, la tâche d’exécution SQL utilise le type de source **Entrée directe** pour exécuter la procédure stockée. La tâche d’exécution SQL peut aussi utiliser le type de source **Connexion de fichiers** pour exécuter une procédure stockée. Que la tâche d’exécution SQL utilise le type de source **Entrée directe** ou **Connexion de fichiers** , utilisez un paramètre de type **ReturnValue** pour implémenter le code de retour.
  
 Pour plus d’informations sur l’utilisation de codes de retour avec des procédures stockées Transact-SQL, consultez [RETURN &#40;Transact-SQL&#41;](../../t-sql/language-elements/return-transact-sql.md).  
  
## <a name="result-sets-in-the-execute-sql-task"></a>Ensembles de résultats dans la tâche d’exécution SQL
 Dans un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , le retour d'un jeu de résultats à la tâche d'exécution SQL dépend du type de commande SQL que la tâche utilise. Par exemple, une instruction SELECT retourne généralement un ensemble de résultats, contrairement à une instruction INSERT.  
  
 Le contenu du jeu de résultats varie également selon la commande SQL. Par exemple, l'ensemble de résultats issu d'une instruction SELECT peut contenir un nombre de lignes quelconque (aucune ligne, une ligne ou de nombreuses lignes). Toutefois, le jeu de résultats d'une instruction SELECT qui retourne un nombre ou une somme contient une seule ligne.  
  
 L'utilisation d'ensembles de résultats dans une tâche d'exécution SQL ne permet pas uniquement de savoir si la commande SQL retourne un ensemble de résultats et ce que celui-ci contient. D'autres indications et spécifications d'utilisation permettent d'utiliser avec succès des jeux de résultats dans la tâche d'exécution SQL. Le reste de cette rubrique traite de ces indications et spécifications d'utilisation.  
  
-   [Spécification d'un type d'ensemble de résultats](#Result_set_type)  
  
-   [Remplir une variable à l’aide d’un jeu de résultats](#Populate_variable_with_result_set)  
  
###  <a name="Result_set_type"></a> Spécifier un type d’ensemble de résultats  
 La tâche d'exécution SQL prend en charge les types de jeux de résultats suivants :  
  
-   L'ensemble de résultats **Aucun** est utilisé lorsque la requête ne retourne aucun résultat. Par exemple, cet ensemble de résultats est utilisé pour les requêtes qui ajoutent, modifient et suppriment des enregistrements dans une table.  
  
-   L'ensemble de résultats **Ligne unique** est utilisé lorsque la requête ne retourne qu'une seule ligne. Par exemple, ce jeu de résultats est utilisé pour une instruction SELECT qui retourne un nombre ou une somme.  
  
-   Le jeu de résultats **Ensemble de résultats complet** est utilisé lorsque la requête retourne plusieurs lignes. Par exemple, ce jeu de résultats est utilisé pour une instruction SELECT qui extrait toutes les lignes d'une table.  
  
-   L'ensemble de résultats **XML** est utilisé lorsque la requête retourne un ensemble de résultats dans un format XML. Par exemple, ce jeu de résultats est utilisé pour une instruction SELECT qui comprend une clause FOR XML.  
  
 Si la tâche d'exécution SQL utilise l'ensemble de résultats **Ensemble de résultats complet** et que la requête retourne plusieurs ensemble de lignes, la tâche ne retourne que le premier. Si cet ensemble de lignes génère une erreur, la tâche la signale. En revanche, si d'autres ensembles de lignes génèrent des erreurs, la tâche ne les signale pas.  
  
###  <a name="Populate_variable_with_result_set"></a> Remplir une variable à l’aide d’un jeu de résultats  
 Vous pouvez lier le jeu de résultats retourné par une requête à une variable définie par l'utilisateur si le type du jeu de résultats est une ligne unique, un ensemble de lignes ou des données XML.  
  
 Si le type de l'ensemble de résultats est **Ligne unique**, vous pouvez lier une colonne du résultat obtenu à une variable en utilisant le nom de colonne comme nom d'ensemble de résultats. Vous pouvez également utiliser comme nom la position ordinale de la colonne dans la liste des colonnes. Par exemple, le nom de l'ensemble de résultats de la requête `SELECT Color FROM Production.Product WHERE ProductID = ?` pourrait être **Color** ou **0**. Si la requête retourne plusieurs colonnes et que vous souhaitez accéder aux valeurs de toutes les colonnes, vous devez lier chaque colonne à une variable différente. Si vous mappez des colonnes à des variables en utilisant des numéros comme noms de jeux de résultats, ces numéros reflètent l'ordre d'apparition des colonnes dans la liste des colonnes de la requête. Par exemple, dans la requête `SELECT Color, ListPrice, FROM Production.Product WHERE ProductID = ?`, vous utilisez 0 pour la colonne **Color** et 1 pour la colonne **ListPrice** . La possibilité d'utiliser un nom de colonne comme nom d'ensemble de résultats dépend du fournisseur que la tâche a été configurée pour utiliser. Tous les fournisseurs ne rendent pas les noms de colonnes disponibles.  
  
 Certaines requêtes qui retournent une valeur unique peuvent ne pas inclure de noms de colonnes. Par exemple, l'instruction `SELECT COUNT (*) FROM Production.Product` ne retourne aucun nom de colonne. Vous pouvez accéder à l'ensemble de résultats en utilisant la position ordinale, 0, comme nom de résultat. Pour accéder au résultat de retour par nom de colonne, la requête doit inclure une clause AS \<nom alias> pour fournir un nom de colonne. L'instruction `SELECT COUNT (*)AS CountOfProduct FROM Production.Product`, fournit la colonne **CountOfProduct** . Vous pouvez ensuite accéder à la colonne de résultat de retour en utilisant le nom de colonne **CountOfProduct** ou la position ordinale, 0.  
  
 Si le type de l'ensemble de résultats est **Ensemble de résultats complet** ou **XML**, vous devez utiliser 0 comme nom de jeu de résultats.  
  
 Lorsque vous associez une variable à un ensemble de résultats à l'aide du type **Ligne unique** , la variable doit être d'un type de données compatible avec celui de la colonne contenue dans l'ensemble de résultats. Par exemple, vous ne pouvez pas associer un ensemble de résultats contenant un type de données **String** à une variable de type de données numérique. Lorsque vous définissez la propriété **TypeConversionMode** sur **Allowed**, la tâche d'exécution SQL tente de convertir le paramètre de sortie et les résultats de la requête dans le type de données de la variable à laquelle les résultats sont affectés.  
  
 Un ensemble de résultats XML ne peut être associé qu'à une variable de type de données **String** ou **Object** . Si la variable est de type de données **String** , la tâche d'exécution SQL retourne une chaîne et la source XML peut exploiter les données XML. Si le type de données de la variable est **Object** , la tâche d’exécution SQL retourne un objet DOM (Document Object Model).  
  
 Un **ensemble de résultats complet** doit correspondre à une variable du type de données **Object** . Le résultat obtenu est un objet d'ensemble de lignes. Vous pouvez utiliser un conteneur de boucles Foreach pour extraire les valeurs de ligne de table qui sont stockées dans la variable Object dans les variables de package, et utiliser une tâche de script pour écrire dans un fichier les données stockées dans les variables de package. Pour une démonstration de l'utilisation d'un conteneur de boucles Foreach et d'une tâche de script, voir l'exemple CodePlex, [Execute SQL Parameters and Result Sets](http://go.microsoft.com/fwlink/?LinkId=157863)(en anglais), sur msftisprodsamples.codeplex.com.  
  
 Le tableau suivant récapitule les types de données des variables pouvant correspondre à des ensembles de résultats.  
  
|Type d'ensemble de résultats|Type de données de la variable|Type d'objet|  
|---------------------|---------------------------|--------------------|  
|Ligne unique|Tout type compatible avec la colonne de type contenue dans l'ensemble de résultats.|Non applicable|  
|Ensemble de résultats complet|**Objet**|Si la tâche utilise un gestionnaire de connexions natif, tel que les gestionnaires de connexions ADO, OLE DB, Excel et ODBC, l'objet retourné est un **Recordset**ADO.<br /><br /> Si la tâche utilise un gestionnaire de connexions managées, tel que le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)], l’objet retourné est un **System.Data.DataSet**.<br /><br /> Vous pouvez utiliser une tâche de script pour accéder à l'objet **System.Data.DataSet** , comme le montre l'exemple suivant.<br /><br /> `Dim dt As Data.DataTable`<br /><br /> `Dim ds As Data.DataSet = CType(Dts.Variables("Recordset").Value, DataSet) dt = ds.Tables(0)`|  
|XML|**String**|**String**|  
|XML|**Objet**|Si la tâche utilise un gestionnaire de connexions natif, tel que les gestionnaires de connexions ADO, OLE DB, Excel et ODBC, l'objet retourné est un **MSXML6.IXMLDOMDocument**.<br /><br /> Si la tâche utilise un gestionnaire de connexions managées, tel que le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)], l’objet retourné est un **System.Xml.XmlDocument**.|  
  
 Vous pouvez définir la variable dans l'étendue de la tâche d'exécution SQL ou dans celle du package. Si la variable a l'étendue d'un package, le jeu de résultats est disponible pour les autres tâches et conteneurs figurant dans le package, ainsi que pour les packages exécutés par les tâches d'exécution de package ou d'exécution de package DTS 2000.  
  
 Quand vous mappez une variable à un jeu de résultats **Ligne unique** , les valeurs qui ne sont pas des chaînes et qui sont retournées par l’instruction SQL sont converties en chaînes quand les conditions suivantes sont réunies :  
  
-   La propriété **TypeConversionMode** a la valeur true. Définissez la valeur de propriété dans la fenêtre Propriétés ou à l'aide de l' **éditeur de tâche d'exécution de requêtes SQL**.  
  
-   La conversion n'entraîne pas de troncation des données.  
  
## <a name="map-result-sets-to-variables-in-an-execute-sql-task"></a>Mapper des ensembles de résultats à des variables dans une tâche d'exécution SQL
Cette section décrit comment créer un mappage entre un jeu de résultats et une variable dans une tâche d’exécution SQL. Le mappage d'un jeu de résultats à une variable rend le jeu de résultats disponible aux autres éléments du package. Par exemple, un script dans une tâche de script peut lire la variable, puis utiliser les valeurs du jeu de résultats ou une source XML pour consommer le jeu de résultats stocké dans une variable. Si le jeu de résultats est généré par un package parent, il est possible de le rendre disponible à un package enfant appelé par une tâche d'exécution de package en mappant le jeu de résultats à une variable dans le package parent, puis en créant une configuration de variable de package parent dans le package enfant pour stocker la valeur de la variable parent.  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans **l’Explorateur de solutions**, double-cliquez sur le package pour l’ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Si le package ne contient pas déjà une tâche d'exécution SQL, ajoutez-en une au flux de contrôle du package. Pour plus d’informations, consultez [Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
5.  Double-cliquez sur la tâche d'exécution SQL.  
  
6.  Dans la boîte de dialogue **Éditeur de tâche d’exécution SQL** , dans la page **Général** , sélectionnez le type de jeu de résultats **Ligne unique**, **Ensemble de résultats complet**ou **XML** .  

7.  Cliquez sur **Ensemble de résultats**.  
  
8.  Pour ajouter un mappage d’un jeu de résultats, cliquez sur **Ajouter**.  
  
9. Dans la liste **Nom de variable** , sélectionnez une variable ou créez-en une. Pour plus d’informations, consultez [Ajouter, supprimer, modifier l’étendue de la variable définie par l’utilisateur dans un package](http://msdn.microsoft.com/library/cbf40c7f-3c8a-48cd-aefa-8b37faf8b40e).  
  
10. Dans la liste **Nom de résultat** , éventuellement, modifiez le nom du jeu de résultats.  
  
     En général vous pouvez utiliser le nom de colonne comme nom de jeu de résultats, ou vous pouvez utiliser la position ordinale de la colonne dans la liste de colonnes en tant que jeu de résultats. La possibilité d'utiliser un nom de colonne comme nom du jeu de résultats dépend du fournisseur que la tâche a été configurée pour utiliser. Tous les fournisseurs ne rendent pas les noms de colonnes disponibles.  
  
11. Cliquez sur **OK**.  

## <a name="troubleshoot-the-execute-sql-task"></a>Résoudre les problèmes liés à la tâche d’exécution SQL  
 Vous pouvez consigner les appels que la tâche d'exécution SQL effectue auprès de fournisseurs de données externes. Vous pouvez utiliser cette fonctionnalité de journalisation pour résoudre les problèmes liés aux commandes SQL qu'exécute la tâche d'exécution SQL. Pour consigner les appels que la tâche d’exécution SQL passe à des fournisseurs de données externes, activez la journalisation des packages et sélectionnez l’événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
 Parfois, une commande SQL ou une procédure stockée retourne plusieurs jeux de résultats. Ces jeux de résultats incluent non seulement des ensembles de lignes qui sont le résultat de requêtes **SELECT** , mais également des valeurs uniques qui sont le résultat d’erreurs d’instructions **RAISERROR** ou **PRINT** . Le fait que la tâche ignore les erreurs dans des jeux de résultats qui se produisent après le premier jeu de résultats dépend du type de gestionnaire de connexions utilisé :  
  
-   Lorsque vous utilisez les gestionnaires de connexions OLE DB et ADO, la tâche ignore les jeux de résultats qui se produisent après le premier jeu de résultats. Par conséquent, avec ces gestionnaires de connexions, la tâche ignore une erreur retournée par une commande SQL ou une procédure stockée lorsque l'erreur ne fait pas partie du premier jeu de résultats.  
  
-   Lorsque vous utilisez les gestionnaires de connexions ODBC et ADO.NET, la tâche n'ignore pas les jeux de résultats qui se produisent après le premier jeu de résultats. Avec ces gestionnaires de connexions, la tâche échoue avec une erreur quand un jeu de résultats autre que le premier jeu de résultats contient une erreur.  
  
### <a name="custom-log-entries"></a>Entrées de journal personnalisées  
 Le tableau suivant décrit les entrées de journal personnalisées pour la tâche d'exécution SQL. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ExecuteSQLExecutingQuery**|Fournit des informations sur les phases d'exécution de l'instruction SQL. Des entrées de journal sont écrites lorsque la tâche acquiert la connexion à la base de données, lorsqu'elle commence à préparer l'instruction SQL et à la fin de l'exécution de l'instruction SQL. L'entrée de journal concernant la phase de préparation inclut l'instruction SQL que la tâche utilise.|  

