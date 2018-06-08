---
title: Destination OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.oledbdest.f1
- sql13.dts.designer.oledbdestadapter.connection.f1
- sql13.dts.designer.oledbdestadapter.mappings.f1
- sql13.dts.designer.oledbdestadapter.errorhandling.f1
helpviewer_keywords:
- fast-load data access mode [Integration Services]
- OLE DB destination [Integration Services]
- fast load options [Integration Services]
- fast-load options [Integration Services]
- destinations [Integration Services], OLE DB
- fast load data access mode [Integration Services]
- inserting data
ms.assetid: 873a2fa0-2a02-41fc-a80a-ec9767f36a8a
caps.latest.revision: 79
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6dfed499dd91ae4113671bf88f23e5ac1b0d8792
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-destination"></a>Destination OLE DB
  La destination OLE DB charge des données dans différentes bases de données compatibles OLE DB à l'aide d'une table ou d'une vue de base de données ou d'une commande SQL. Par exemple, la source OLE DB peut charger des données dans des tables de bases de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Si la source de données est [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, la source de données requiert un gestionnaire de connexions différent des versions antérieures d'Excel. Pour plus d’informations, consultez [Établir une connexion à un classeur Excel](../../integration-services/connection-manager/connect-to-an-excel-workbook.md).  
  
 La destination OLE DB propose cinq modes d'accès différents pour charger les données :  
  
-   Une table ou une vue. Vous pouvez indiquer une table ou une vue existante, ou créer une table ;  
  
-   une table ou une vue et des options de chargement rapide. Vous pouvez indiquer une table existante ou en créer une ;  
  
-   Une table ou une vue spécifiée dans une variable.  
  
-   une table ou une vue spécifiée dans une variable et des options de chargement rapide ;  
  
-   Les résultats d'une instruction SQL.  
  
> [!NOTE]  
>  La destination OLE DB ne prend pas en charge les paramètres. Si vous devez exécuter une instruction INSERT paramétrable, envisagez d'utiliser la transformation de commande OLE DB. Pour plus d’informations, consultez [OLE DB Command Transformation](../../integration-services/data-flow/transformations/ole-db-command-transformation.md).  
  
 Lorsque la destination OLE DB charge des données qui utilisent un jeu de caractères codés sur deux octets (DBCS), les données risquent d'être endommagées si le mode d'accès aux données n'utilise pas l'option de chargement rapide et si le gestionnaire de connexions OLE DB utilise le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour SQL Server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQLOLEDB). Pour garantir l’intégrité des données DBCS, vous devez configurer le gestionnaire de connexions OLE DB pour utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ou l’un des modes d’accès avec chargement rapide : **Table ou vue - chargement rapide** ou **Variable de nom de table ou de vue - chargement rapide**. Ces deux options sont disponibles dans la boîte de dialogue **Éditeur de destination OLE DB** . Quand vous programmez le modèle d’objet [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous devez définir la propriété AccessMode sur **OpenRowset à l’aide de FastLoad**ou **OpenRowset à l’aide de FastLoad à partir de Variable**.  
  
> [!NOTE]  
>  Si vous utilisez la boîte de dialogue **Éditeur de destination OLE DB** dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] pour créer la table de destination dans laquelle la destination OLE DB insère des données, vous devrez peut-être sélectionner la table que vous venez de créer manuellement. La sélection manuelle est nécessaire lorsqu'un fournisseur OLE DB tel que le fournisseur OLE DB pour DB2, ajoute manuellement des identificateurs de schéma au nom de la table.  
  
> [!NOTE]  
>  L'instruction CREATE TABLE que la boîte de dialogue **Éditeur de destination OLE DB** génère peut nécessiter une modification selon le type de destination. Par exemple, certaines destinations ne prennent pas en charge les types de données que l'instruction CREATE TABLE utilise.  
  
 Cette destination utilise un gestionnaire de connexions OLE DB pour se connecter à une source de données et le gestionnaire de connexions indique le fournisseur OLE DB à utiliser. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 Un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contient également l'objet de source de données à partir duquel vous pouvez créer un gestionnaire de connexions OLE DB afin de rendre les sources de données et les vues de sources de données disponibles pour la destination OLE DB.  
  
 Une destination OLE DB inclut des mappages entre les colonnes d'entrée et les colonnes de la source de données de destination. Vous n'avez pas besoin de mapper les colonnes d'entrée à toutes les colonnes de destination, mais en fonction des propriétés des colonnes de destination, des erreurs peuvent se produire si aucune colonne d'entrée n'est mappée aux colonnes de destination. Par exemple, si une colonne de destination n'autorise pas les valeurs null, une colonne d'entrée doit être mappée à cette colonne. Par ailleurs, les types de données des colonnes mappées doivent être compatibles. Par exemple, vous ne pouvez pas mapper une colonne d'entrée avec un type de données string à une colonne de destination avec un type de données numeric.  
  
 La destination OLE DB comporte une entrée normale et une sortie d'erreur.  
  
 Pour plus d'informations sur les types de données, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="fast-load-options"></a>Options de chargement rapide  
 Si la destination OLE DB utilise un mode d’accès aux données par chargement rapide, vous pouvez spécifier les options de chargement rapide dans l’interface utilisateur, **Éditeur de destination OLE DB**, pour la destination :  
  
-   Conserver les valeurs d'identité du fichier de données importé ou utiliser les valeurs uniques affectées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Conserver une valeur null pendant l'opération de chargement en masse.  
  
-   Vérifier les contraintes sur la table ou la vue cible au cours de l'opération d'importation en masse.  
  
-   Acquisition d'un verrou au niveau de la table pour la durée de l'opération de chargement en masse.  
  
-   Spécifier le nombre de lignes dans le traitement et la taille de validation.  
  
 Certaines options de chargement rapide sont stockées dans des propriétés spécifiques de la destination OLE DB. Par exemple, FastLoadKeepIdentity spécifie s’il convient de conserver des valeurs d’identification, FastLoadKeepNulls précise s’il faut conserver des valeurs NULL et FastLoadMaxInsertCommitSize spécifie le nombre de lignes à valider en tant que traitement. D’autres options de chargement rapide sont stockées dans une liste séparée par des virgules dans la propriété FastLoadOptions. Si la destination OLE DB utilise toutes les options de chargement rapide qui sont stockées dans FastLoadOptions et répertoriées dans la boîte de dialogue **Éditeur de destination OLE DB** , la propriété prend la valeur **TABLOCK, CHECK_CONSTRAINTS, ROWS_PER_BATCH=1000**. La valeur 1 000 indique que la destination est configurée pour utiliser des traitements de 1 000 lignes.  
  
> [!NOTE]  
>  Tout échec de contrainte à la destination entraîne l’échec de la totalité du lot de lignes définies par FastLoadMaxInsertCommitSize.  
  
 Outre les options de chargement rapide dévoilées dans la boîte de dialogue **Éditeur de destination OLE DB** , vous pouvez configurer la destination OLE DB afin d’utiliser les options de chargement en masse suivantes en tapant les options dans la propriété FastLoadOptions, dans la boîte de dialogue **Éditeur avancé** .  
  
|Option de chargement rapide|Description|  
|----------------------|-----------------|  
|KILOBYTES_PER_BATCH|Indique la taille à insérer en kilo-octets. L’option a la forme **KILOBYTES_PER_BATCH** = \<valeur entière positive**>**.|  
|FIRE_TRIGGERS|Spécifie si des déclencheurs sont activés sur la table d'insertion. L’option a la forme **FIRE_TRIGGERS**. La présence de l'option indique que des déclencheurs sont activés.|  
|ORDER|Spécifie comment les données d'entrée sont triées. L’option a la forme ORDER \<nom de colonne> ASC&#124;DESC. Il n'y a pas de limite quant au nombre de colonnes indiquées et la spécification de l'ordre de tri est facultative. Si l'ordre de tri est omis, l'opération d'insertion part du principe que les données ne sont pas triées.<br /><br /> Remarque : les performances peuvent être améliorées si vous utilisez l’option ORDER pour trier les données d’entrée selon l’index cluster de la table.|  
  
 Les mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)] sont traditionnellement tapés en majuscules, mais ils ne tiennent pas compte de la casse.  
  
 Pour en savoir plus sur les options de chargement rapide, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
## <a name="troubleshooting-the-ole-db-destination"></a>Résolution des problèmes liés à la destination OLE DB  
 Vous pouvez consigner les appels que la destination OLE DB effectue vers des fournisseurs de données externes. Cette fonctionnalité de journalisation permet de résoudre des problèmes liés à l'enregistrement de données vers des sources de données externes que réalise la destination OLE DB. Pour consigner les appels que la destination OLE DB effectue vers des fournisseurs de données externes, activez la journalisation de package et sélectionnez l'événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="configuring-the-ole-db-destination"></a>Configuration de la destination OLE DB  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées OLE DB](../../integration-services/data-flow/ole-db-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Charger des données à l'aide de la destination OLE DB](../../integration-services/data-flow/load-data-by-using-the-ole-db-destination.md)  
  
-   [Définir les propriétés d'un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="ole-db-destination-editor-connection-manager-page"></a>Éditeur de destination OLE DB (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination OLE DB** pour sélectionner la connexion OLE DB de la destination. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.  
  
> [!NOTE]  
>  Si la source de données est [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, la source de données requiert un gestionnaire de connexions différent des versions antérieures d'Excel. Pour plus d’informations, consultez [Établir une connexion à un classeur Excel](../../integration-services/connection-manager/connect-to-an-excel-workbook.md).  
  
> [!NOTE]  
>  La propriété **CommandTimeout** de la destination OLE DB n’est pas disponible dans **l’Éditeur de destination OLE DB**, mais peut être définie à l’aide de **l’Éditeur avancé**. De plus, certaines options de chargement rapide sont uniquement disponibles dans **l’Éditeur avancé**. Pour plus d’informations sur ces propriétés, consultez la section sur la destination OLE DB dans [Propriétés personnalisées OLE DB](../../integration-services/data-flow/ole-db-custom-properties.md).  
  
### <a name="static-options"></a>Options statiques  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez une nouvelle connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** .  
  
 **Mode d'accès aux données**  
 Spécifiez la méthode de chargement des données dans la destination. Le chargement de données du jeu de caractères codés sur deux octets (DBCS) nécessite l'utilisation d'une des options de chargement rapide. Pour plus d’informations sur les modes d’accès aux données à chargement rapide, qui sont optimisés pour les insertions en bloc, consultez [Destination OLE DB](../../integration-services/data-flow/ole-db-destination.md).  
  
|Option|Description|  
|------------|-----------------|  
|Table ou vue|Chargez les données dans une table ou une vue de la destination OLE DB.|  
|Table ou vue - chargement rapide|Chargez les données dans une table ou une vue de la destination OLE DB et utilisez l'option de chargement rapide. Pour plus d’informations sur les modes d’accès aux données à chargement rapide, qui sont optimisés pour les insertions en bloc, consultez [Destination OLE DB](../../integration-services/data-flow/ole-db-destination.md).|  
|Variable de nom de table ou de vue|Spécifiez le nom de la table ou de la vue dans une variable.<br /><br /> **Informations connexes**: [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)|  
|Variable de nom de table ou de vue - chargement rapide|Spécifiez le nom de la table ou de la vue dans une variable et chargez les données à l'aide de l'option de chargement rapide. Pour plus d’informations sur les modes d’accès aux données à chargement rapide, qui sont optimisés pour les insertions en bloc, consultez [Destination OLE DB](../../integration-services/data-flow/ole-db-destination.md).|  
|Commande SQL|Chargez les données dans la destination OLE DB à l'aide d'une requête SQL.|  
  
 **Aperçu**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Visualiser les résultats de la requête** . L'aperçu peut afficher jusqu'à 200 lignes.  
  
### <a name="data-access-mode-dynamic-options"></a>Options dynamiques du mode d'accès aux données  
 Chacun des paramètres **Mode d’accès aux données** affiche un jeu dynamique d’options spécifiques au paramètre en question. Les sections suivantes décrivent chacune des options dynamiques disponibles pour chaque paramètre **Mode d’accès aux données** .  
  
#### <a name="data-access-mode--table-or-view"></a>Mode d'accès aux données = Table ou vue  
 **Nom de la table ou de la vue**  
 Sélectionnez le nom de la table ou de la vue dans la liste de celles qui sont disponibles dans la source de données.  
  
 **Nouveau**  
 Utilisez la boîte de dialogue **Créer une table** pour créer une table.  
  
> [!NOTE]  
>  Quand vous cliquez sur **Nouvelle**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] génère une instruction CREATE TABLE par défaut, basée sur la source de données connectée. Cette instruction CREATE TABLE par défaut n'inclut pas l'attribut FILESTREAM, même si la table source inclut une colonne dans laquelle l'attribut FILESTREAM est déclaré. Pour exécuter un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec l'attribut FILESTREAM, implémentez d'abord le stockage FILESTREAM sur la base de données de destination. Ajoutez ensuite l’attribut FILESTREAM à l’instruction CREATE TABLE dans la boîte de dialogue **Créer une table**. Pour plus d’informations, consultez [Données blob &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
#### <a name="data-access-mode--table-or-view--fast-load"></a>Mode d'accès aux données = Table ou vue - chargement rapide  
 **Nom de la table ou de la vue**  
 Sélectionnez une table ou une vue dans la base de données à l’aide de cette liste, ou créez une table en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Utilisez la boîte de dialogue **Créer une table** pour créer une table.  
  
> [!NOTE]  
>  Quand vous cliquez sur **Nouvelle**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] génère une instruction CREATE TABLE par défaut, basée sur la source de données connectée. Cette instruction CREATE TABLE par défaut n'inclut pas l'attribut FILESTREAM, même si la table source inclut une colonne dans laquelle l'attribut FILESTREAM est déclaré. Pour exécuter un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec l'attribut FILESTREAM, implémentez d'abord le stockage FILESTREAM sur la base de données de destination. Ajoutez ensuite l’attribut FILESTREAM à l’instruction CREATE TABLE dans la boîte de dialogue **Créer une table**. Pour plus d’informations, consultez [Données blob &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Conserver l'identité**  
 Spécifiez si les valeurs d'identité doivent être copiées lors du chargement des données. Cette propriété n'est disponible qu'avec l'option de chargement rapide. La valeur par défaut de cette propriété est **false**.  
  
 **Conserver les valeurs NULL**  
 Spécifiez si les valeurs NULL doivent être copiées lors du chargement des données. Cette propriété n'est disponible qu'avec l'option de chargement rapide. La valeur par défaut de cette propriété est **false**.  
  
 **Verrou de table**  
 Spécifiez si la table est verrouillée pendant le chargement. La valeur par défaut de cette propriété est **true**.  
  
 **Contraintes de validation**  
 Spécifiez si la destination vérifie les contraintes lors du chargement des données. La valeur par défaut de cette propriété est **true**.  
  
 **Lignes par lot**  
 Permet d'indiquer le nombre de lignes contenues dans un traitement. La valeur par défaut de cette propriété est **–1**, ce qui signifie qu’aucune valeur n’a été attribuée.  
  
> [!NOTE]  
>  Effacez la zone de texte dans **l’Éditeur de destination OLE DB** pour indiquer que vous ne voulez pas assigner de valeur personnalisée à cette propriété.  
  
 **Taille de validation d'insertion maximale**  
 Spécifiez la taille du lot que la destination OLE DB tente de valider pendant les opérations de chargement rapide. La valeur **0** indique que toutes les données sont validées dans un seul lot après traitement de toutes les lignes.  
  
> [!NOTE]  
>  Une valeur de **0** peut faire en sorte que le package en cours d’exécution cesse de répondre si la destination OLE DB et un autre composant de flux de données mettent à jour la même table source. Pour empêcher que le package ne s’arrête, affectez la valeur **2147483647** à l’option **Taille de validation d’insertion maximale**.  
  
 Si vous spécifiez une valeur pour cette propriété, la destination valide les lignes par lot qui ont la plus petite taille entre (a) la **Taille de validation d’insertion maximale**et (b) les lignes restantes dans le tampon qui sont en cours de traitement.  
  
> [!NOTE]  
>  Tout échec de contrainte à la destination entraîne l’échec de la totalité du lot de lignes définies par la **Taille de validation d’insertion maximale** .  
  
#### <a name="data-access-mode--table-name-or-view-name-variable"></a>Mode d'accès aux données = Variable de nom de table ou de vue  
 **Nom de la variable**  
 Sélectionnez la variable qui contient le nom de la table ou de la vue.  
  
#### <a name="data-access-mode--table-name-or-view-name-variable--fast-load"></a>Mode d'accès aux données = Variable de nom de table ou de vue - chargement rapide  
 **Nom de la variable**  
 Sélectionnez la variable qui contient le nom de la table ou de la vue.  
  
 **Nouveau**  
 Utilisez la boîte de dialogue **Créer une table** pour créer une table.  
  
> [!NOTE]  
>  Quand vous cliquez sur **Nouvelle**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] génère une instruction CREATE TABLE par défaut, basée sur la source de données connectée. Cette instruction CREATE TABLE par défaut n'inclut pas l'attribut FILESTREAM, même si la table source inclut une colonne dans laquelle l'attribut FILESTREAM est déclaré. Pour exécuter un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec l'attribut FILESTREAM, implémentez d'abord le stockage FILESTREAM sur la base de données de destination. Ajoutez ensuite l’attribut FILESTREAM à l’instruction CREATE TABLE dans la boîte de dialogue **Créer une table**. Pour plus d’informations, consultez [Données blob &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Conserver l'identité**  
 Spécifiez si les valeurs d'identité doivent être copiées lors du chargement des données. Cette propriété n'est disponible qu'avec l'option de chargement rapide. La valeur par défaut de cette propriété est **false**.  
  
 **Conserver les valeurs NULL**  
 Spécifiez si les valeurs NULL doivent être copiées lors du chargement des données. Cette propriété n'est disponible qu'avec l'option de chargement rapide. La valeur par défaut de cette propriété est **false**.  
  
 **Verrou de table**  
 Spécifiez si la table est verrouillée pendant le chargement. La valeur par défaut de cette propriété est **false**.  
  
 **Contraintes de validation**  
 Spécifiez si la tâche vérifie les contraintes. La valeur par défaut de cette propriété est **false**.  
  
 **Lignes par lot**  
 Permet d'indiquer le nombre de lignes contenues dans un traitement. La valeur par défaut de cette propriété est **–1**, ce qui signifie qu’aucune valeur n’a été attribuée.  
  
> [!NOTE]  
>  Effacez la zone de texte dans **l’Éditeur de destination OLE DB** pour indiquer que vous ne voulez pas assigner de valeur personnalisée à cette propriété.  
  
 **Taille de validation d'insertion maximale**  
 Spécifiez la taille du lot que la destination OLE DB tente de valider pendant les opérations de chargement rapide. La valeur par défaut, **2147483647** , indique que toutes les données sont validées dans un seul lot après traitement de toutes les lignes.  
  
> [!NOTE]  
>  Une valeur de **0** peut faire en sorte que le package en cours d’exécution cesse de répondre si la destination OLE DB et un autre composant de flux de données mettent à jour la même table source. Pour empêcher que le package ne s’arrête, affectez la valeur **2147483647** à l’option **Taille de validation d’insertion maximale**.  
  
#### <a name="data-access-mode--sql-command"></a>Mode d'accès aux données = Commande SQL  
 **Texte de la commande SQL**  
 Entrez le texte d’une requête SQL, générez la requête en cliquant sur **Générer une requête**ou recherchez le fichier qui contient le texte de la requête en cliquant sur **Parcourir**.  
  
> [!NOTE]  
>  La destination OLE DB ne prend pas en charge les paramètres. Si vous devez exécuter une instruction INSERT paramétrable, envisagez d'utiliser la transformation de commande OLE DB. Pour plus d’informations, consultez [OLE DB Command Transformation](../../integration-services/data-flow/transformations/ole-db-command-transformation.md).  
  
 **Construire une requête**  
 Utilisez la boîte de dialogue **Générateur de requêtes** pour construire la requête SQL visuellement.  
  
 **Parcourir**  
 Dans la boîte de dialogue **Ouvrir** , localisez le fichier qui contient le texte de la requête SQL.  
  
 **Analyser la requête**  
 Vérifiez la syntaxe du texte de la requête.  
  
## <a name="ole-db-destination-editor-mappings-page"></a>Éditeur de destination OLE DB (page Mappages)
  La page **Mappages** de la boîte de dialogue **Éditeur de destination OLE DB** vous permet de mapper les colonnes d’entrée aux colonnes de destination.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Au moyen du glisser-déplacer, mappez les colonnes d'entrée disponibles dans la table sur des colonnes de destination.  
  
 **Colonnes de destination disponibles**  
 Affichez la liste des colonnes de destination disponibles. Utilisez une opération de glisser-déplacer pour mapper les colonnes de destination disponibles dans la table aux colonnes d'entrée.  
  
 **Colonne d'entrée**  
 Affichez les colonnes d’entrée que vous avez sélectionnées. Vous pouvez supprimer des mappages en sélectionnant **\<ignorer>** de manière à exclure des colonnes de la sortie.  
  
 **Colonne de destination**  
 Indique chaque colonne de destination disponible, qu'elle soit mappée ou non.  
  
## <a name="ole-db-destination-editor-error-output-page"></a>Éditeur de destination OLE DB (page Sortie d'erreur)
  Utilisez la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de destination OLE DB** pour définir les options de gestion des erreurs.  
  
### <a name="options"></a>Options  
 **Entrée/sortie**  
 Affichez le nom de l'entrée.  
  
 **Colonne**  
 Non utilisé.  
  
 **Erreur**  
 Indiquez ce qui doit se produire lorsqu'une erreur se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Rubriques connexes :** [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Troncation**  
 Non utilisé.  
  
 **Description**  
 Affichez la description de l'opération.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Indiquez ce qui doit se produire pour l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez l'option de gestion des erreurs aux cellules sélectionnées.  
  
## <a name="related-content"></a>Contenu associé  
 [Source OLE DB](../../integration-services/data-flow/ole-db-source.md)  
  
 [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)  
  
 [Flux de données](../../integration-services/data-flow/data-flow.md)  
  
  
