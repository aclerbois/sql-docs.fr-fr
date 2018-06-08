---
title: Source OLE DB | Microsoft Docs
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
- sql13.dts.designer.oledbsource.f1
- sql13.dts.designer.oledbsourceadapter.connection.f1
- sql13.dts.designer.oledbsourceadapter.columns.f1
- sql13.dts.designer.oledbsourceadapter.errorhandling.f1
helpviewer_keywords:
- sources [Integration Services], OLE DB
- OLE DB source [Integration Services]
ms.assetid: f87cc5f6-b078-40f3-9d87-7a65e13e4c86
caps.latest.revision: 69
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d4b4e9cd95db8a42f4a3e13962770aa018deeec2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-source"></a>Source OLE DB
  La source OLE DB extrait des données d'une série de bases de données relationnelles compatibles OLE DB à l'aide d'une table de base de données, d'une vue ou d'une commande SQL. Par exemple, la source OLE DB peut extraire des données de tables de bases de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Si la source de données est [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, la source de données requiert un gestionnaire de connexions différent des versions antérieures d'Excel. Pour plus d’informations, consultez [Établir une connexion à un classeur Excel](../../integration-services/connection-manager/connect-to-an-excel-workbook.md).  
  
 La source OLE DB fournit quatre modes d'accès aux données différents pour l'extraction des données :  
  
-   Une table ou une vue.  
  
-   Une table ou une vue spécifiée dans une variable.  
  
-   Les résultats d'une instruction SQL. La requête peut être une requête paramétrable.  
  
-   Les résultats d'une instruction SQL stockée dans une variable.  
  
> [!NOTE]  
>  Lorsque vous utilisez une instruction SQL pour appeler une procédure stockée qui retourne des résultats à partir d'une table temporaire, utilisez l'option WITH RESULT SETS afin de définir les métadonnées du jeu de résultats.  
  
 Si vous utilisez une requête paramétrable, vous pouvez mapper des variables à des paramètres pour spécifier les valeurs de paramètres individuels dans les instructions SQL.  
  
 Cette source utilise un gestionnaire de connexions OLE DB pour se connecter à une source de données et le gestionnaire de connexions spécifie le fournisseur OLE DB à utiliser. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 Un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit également l'objet de source de données à partir duquel vous pouvez créer un gestionnaire de connexions OLE DB et rendre ainsi les sources de données et les vues de source de données disponibles pour la source OLE DB.  
  
 Selon le fournisseur OLE DB, certaines contraintes s'appliquent à la source OLE DB :  
  
-   Le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour Oracle ne prend pas en charge les types de données Oracle BLOB, CLOB, NCLOB, BFILE ou UROWID et la source OLE DB ne peut pas extraire de données des tables qui contiennent des colonnes de ces types de données.  
  
-   Le fournisseur IBM OLE DB DB2 et le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB DB2 ne prennent pas en charge l'utilisation d'une commande SQL qui appelle une procédure stockée. Lorsque ce type de commande est utilisé, la source OLE DB ne peut pas créer les métadonnées de colonne ; par conséquent, les composants de flux de données qui suivent la source OLE DB dans le flux de données ne disposent pas de données de colonnes et l'exécution du flux de données échoue.  
  
 La source OLE DB a une sortie normale et une sortie d'erreur.  
  
## <a name="using-parameterized-sql-statements"></a>Utilisation d'instructions SQL paramétrables  
 La source OLE DB peut utiliser une instruction SQL pour extraire des données. L'instruction peut être une instruction SELECT ou EXEC.  
  
 La source OLE DB utilise un gestionnaire de connexions OLE DB pour établir une connexion à la source de données à partir de laquelle elle extrait des données. Selon le fournisseur que le gestionnaire de connexions OLE DB utilise et le système de gestion de bases de données relationnelles (SGBDR) auquel le gestionnaire de connexions se connecte, différentes règles s'appliquent à la dénomination des paramètres et à la constitution de leur liste. Si les noms des paramètres sont retournés à partir du SGBDR, vous pouvez utiliser des noms de paramètres pour mapper les paramètres d'une liste de paramètres aux paramètres d'une instruction SQL ; sinon, les paramètres sont mappés aux paramètres de l'instruction SQL en fonction de leur position ordinale dans la liste de paramètres. Les types de noms de paramètres pris en charge varient selon le fournisseur. Par exemple, certains fournisseurs imposent l'utilisation de noms de variables ou de colonnes, d'autres exigent l'emploi de noms symboliques tels que 0 ou Param0. Il convient de consulter la documentation spécifique du fournisseur pour obtenir des informations sur les noms de paramètres à utiliser dans les instructions SQL.  
  
 Lorsque vous utilisez un gestionnaire de connexions OLE DB, vous ne pouvez pas utiliser de sous-requêtes paramétrables, car la source OLE DB ne peut pas dériver d'informations de paramètre par le biais du fournisseur OLE DB. Vous pouvez toutefois utiliser une expression pour concaténer les valeurs de paramètre dans la chaîne de requête et définir la propriété SqlCommand de la source. Dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous configurez une source OLE DB à l’aide de la boîte de dialogue **Éditeur de source OLE DB** et mappez les paramètres à des variables dans la boîte de dialogue **Définition des paramètres de la requête** .  
  
### <a name="specifying-parameters-by-using-ordinal-positions"></a>Spécifications de paramètres à l'aide de positions ordinales  
 Si aucun nom de paramètre n’est retourné, l’ordre selon lequel les paramètres sont répertoriés dans la liste **Paramètres** dans la boîte de dialogue **Définition des paramètres de la requête** régit le marqueur de paramètre auquel ils sont mappés au moment de l’exécution. Le premier paramètre de la liste est-il mappé sur le premier dans l'instruction SQL, le deuxième sur le deuxième ?, etc.  
  
 L’instruction SQL suivante sélectionne des lignes dans la table **Product** de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Le premier paramètre dans la liste **Mappages** est mappé au premier paramètre de la colonne **Color** , le deuxième paramètre à la colonne **Size** .  
  
 `SELECT * FROM Production.Product WHERE Color = ? AND Size = ?`  
  
 Les noms de paramètres n'ont aucune incidence. Par exemple, si un paramètre porte le même nom que la colonne à laquelle il s’applique, mais qu’il n’est pas placé à la bonne position ordinale dans la liste **Paramètres** , le mappage de paramètres effectué au moment de l’exécution utilise la position ordinale du paramètre, pas le nom du paramètre.  
  
 La commande EXEC impose généralement l'utilisation des noms des variables qui fournissent des valeurs de paramètres dans la procédure comme noms de paramètres.  
  
### <a name="specifying-parameters-by-using-names"></a>Spécification de paramètres à l'aide de noms  
 Si les noms de paramètres réels sont retournés à partir du SGBDR, les paramètres utilisés par une instruction SELECT et EXEC sont mappés par nom. Les noms de paramètres doivent correspondre aux noms que la procédure stockée, exécutée par l'instruction SELECT ou l'instruction EXEC, attend.  
  
 L’instruction SQL suivante exécute la procédure stockée **uspGetWhereUsedProductID** , disponible dans la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] .  
  
 `EXEC uspGetWhereUsedProductID ?, ?`  
  
 La procédure stockée attend les variables, `@StartProductID` et `@CheckDate`, pour fournir des valeurs de paramètres. L’ordre dans lequel les paramètres apparaissent dans la liste **Mappages** n’est pas pertinent. Le seul impératif est que les noms de paramètres correspondent aux noms de variables dans la procédure stockée, notamment le signe @.  
  
### <a name="mapping-parameters-to-variables"></a>Mappage de paramètres à des variables  
 Les paramètres sont mappés à des variables qui fournissent les valeurs de paramètres au moment de l'exécution. Les variables sont généralement des variables définies par l’utilisateur, bien que vous puissiez utiliser les variables système fournies par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Si vous utilisez des variables définies par l'utilisateur, assurez-vous que vous choisissez un type de données compatible avec celui de la colonne référencée par le paramètre mappé. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
## <a name="troubleshooting-the-ole-db-source"></a>Résolution des problèmes liés à la source OLE DB  
 Vous pouvez consigner les appels que la source OLE DB effectue vers des fournisseurs de données externes. Cette fonctionnalité de journalisation permet de résoudre des problèmes liés au chargement de données que réalise la source OLE DB depuis des sources de données externes. Pour consigner les appels que la source OLE DB effectue vers des fournisseurs de données externes, activez la journalisation de package et sélectionnez l’événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="configuring-the-ole-db-source"></a>Configuration de la source OLE DB  
 Vous pouvez définir les propriétés par programmation ou par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées OLE DB](../../integration-services/data-flow/ole-db-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Extraire des données à l'aide de la source OLE DB](../../integration-services/data-flow/extract-data-by-using-the-ole-db-source.md)  
  
-   [Mapper des paramètres de requête à des variables dans un composant de flux de données](../../integration-services/data-flow/map-query-parameters-to-variables-in-a-data-flow-component.md)  
  
-   [Définir les propriétés d'un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
-   [Trier des données pour les transformations de fusion et de jointure de fusion](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-content"></a>Contenu associé  
 Article Wiki, [SSIS with Oracle Connectors](http://go.microsoft.com/fwlink/?LinkId=220670)(SSIS avec connecteurs Oracle) sur social.technet.microsoft.com.  
  
## <a name="ole-db-source-editor-connection-manager-page"></a>Éditeur de source OLE DB (page Gestionnaire de connexions)
  La page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source OLE DB** vous permet de sélectionner le gestionnaire de connexions OLE DB pour la source. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.  
  
> [!NOTE]  
>  Pour charger des données à partir d’une source de données qui utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, utilisez une source OLE DB. Vous ne pouvez pas utiliser une source Excel pour charger des données à partir d'une source de données Excel 2007. Pour plus d’informations, consultez [Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md).  
>   
>  Pour charger des données à partir d'une source de données qui utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2003 ou une version antérieure, utilisez une source Excel. Pour plus d’informations, consultez [Éditeur de source Excel &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/excel-source-editor-connection-manager-page.md).  
  
> [!NOTE]  
>  La propriété **CommandTimeout** de la source OLE DB n’est pas disponible dans **l’Éditeur de source OLE DB**, mais peut être définie à l’aide de **l’Éditeur avancé**. Pour plus d’informations sur cette propriété, consultez la section sur la source Excel dans [Propriétés personnalisées OLE DB](../../integration-services/data-flow/ole-db-custom-properties.md).  
  
### <a name="open-the-ole-db-source-editor-connection-manager-page"></a>Ouvrir l’Éditeur de source OLE DB (page Gestionnaire de connexions)  
  
1.  Ajoutez la source OLE DB au package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
2.  Cliquez avec le bouton droit sur le composant de la source, puis cliquez sur **Modifier**.  
  
3.  Cliquez sur **Gestionnaire de connexions**.  
  
### <a name="static-options"></a>Options statiques  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez une nouvelle connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** .  
  
 **Mode d'accès aux données**  
 Spécifiez la méthode de sélection des données dans la source.  
  
|Option|Description|  
|------------|-----------------|  
|Table ou vue|Permet de récupérer les données d'une table ou d'une vue dans la source de données OLE DB.|  
|Variable de nom de table ou de vue|Spécifiez le nom de la table ou de la vue dans une variable.<br /><br /> **Informations connexes :** [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)|  
|Commande SQL|Récupérez les données de la source de données OLE DB à l'aide d'une requête SQL.|  
|Commande SQL à partir d'une variable|Spécifiez le texte de la requête SQL dans une variable.|  
  
 **Aperçu**  
 Affichez un aperçu des résultats à partir de la boîte de dialogue **Vue de données** . Le mode**Aperçu** peut afficher jusqu’à 200 lignes.  
  
> [!NOTE]  
>  Lorsque vous affichez l'aperçu des données, les colonnes ayant un type CLR défini par l'utilisateur ne contiennent pas de données. Les valeurs \<valeur trop grande pour être affichée> ou System.Byte[] s’affichent à la place. La première s’affiche lorsque le fournisseur SQL OLE DB accède à la source de données, la seconde lorsque vous utilisez le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
### <a name="data-access-mode-dynamic-options"></a>Options dynamiques du mode d'accès aux données  
  
#### <a name="data-access-mode--table-or-view"></a>Mode d'accès aux données = Table ou vue  
 **Nom de la table ou de la vue**  
 Sélectionnez le nom de la table ou de la vue dans la liste de celles qui sont disponibles dans la source de données.  
  
#### <a name="data-access-mode--table-name-or-view-name-variable"></a>Mode d'accès aux données = Variable de nom de table ou de vue  
 **Nom de la variable**  
 Sélectionnez la variable qui contient le nom de la table ou de la vue.  
  
#### <a name="data-access-mode--sql-command"></a>Mode d'accès aux données = Commande SQL  
 **Texte de la commande SQL**  
 Entrez le texte d’une requête SQL, générez la requête en cliquant sur **Générer une requête**ou recherchez le fichier qui contient le texte de la requête en cliquant sur **Parcourir**.  
  
 **Paramètres**  
 Si vous avez entré une requête paramétrable en spécifiant ? comme espace réservé de paramètre dans le texte de la requête, utilisez la boîte de dialogue **Définition des paramètres de la requête** pour mapper des paramètres d’entrée de la requête à des variables du package.  
  
 **Build query**  
 Utilisez la boîte de dialogue **Générateur de requêtes** pour construire la requête SQL visuellement.  
  
 **Parcourir**  
 Dans la boîte de dialogue **Ouvrir** , localisez le fichier qui contient le texte de la requête SQL.  
  
 **Analyser la requête**  
 Vérifiez la syntaxe du texte de la requête.  
  
#### <a name="data-access-mode--sql-command-from-variable"></a>Mode d'accès aux données = Commande SQL à partir d'une variable  
 **Nom de la variable**  
 Sélectionnez la variable qui contient le texte de la requête SQL.  
  
## <a name="ole-db-source-editor-columns-page"></a>Éditeur de source OLE DB (page Colonnes)
  Utilisez la page **Colonnes** de la boîte de dialogue **Éditeur de source OLE DB** pour mapper une colonne de sortie à chaque colonne externe (source).  
  
### <a name="options"></a>Options  
 **Colonnes externes disponibles**  
 Affiche la liste des colonnes externes disponibles dans la source de données. Vous ne pouvez pas ajouter ou supprimer des colonnes à l'aide de cette table.  
  
 **Colonne externe**  
 Affiche les colonnes externes (sources) selon l'ordre dans lequel vous les visualisez lorsque vous configurez des composants qui consomment des données à partir de cette source. Vous pouvez modifier cet ordre en supprimant d'abord les colonnes sélectionnées dans la table, puis en choisissant des colonnes externes dans la liste selon un ordre différent.  
  
 **Colonne de sortie**  
 Spécifiez un nom unique pour chaque colonne de sortie. Le nom par défaut est celui de la colonne externe (source) sélectionnée ; vous pouvez néanmoins choisir n'importe quel nom unique et significatif. Le nom spécifié s'affiche dans le concepteur SSIS.  
  
## <a name="ole-db-source-editor-error-output-page"></a>Éditeur de source OLE DB (page Sortie d'erreur)
  La page **Sortie d’erreur** de la boîte de dialogue **Éditeur de source OLE DB** vous permet de sélectionner les options de gestion des erreurs et de définir les propriétés sur les colonnes de sortie d’erreur.  
  
### <a name="options"></a>Options  
 **Entrée/sortie**  
 Affichez le nom de la source de données.  
  
 **Colonne**  
 Indique les colonnes (sources) externes que vous avez sélectionnées dans la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source OLE DB**.  
  
 **Erreur**  
 Indiquez ce qui doit se produire lorsqu'une erreur se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Rubriques connexes :** [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Troncation**  
 Indiquez ce qui doit se produire lorsqu'une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Description**  
 Affiche la description de l'erreur.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Indiquez ce qui doit se produire pour l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez l'option de gestion des erreurs aux cellules sélectionnées.  
  
## <a name="see-also"></a> Voir aussi  
 [Destination OLE DB](../../integration-services/data-flow/ole-db-destination.md)   
 [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)   
 [Flux de données](../../integration-services/data-flow/data-flow.md)  
  
  
