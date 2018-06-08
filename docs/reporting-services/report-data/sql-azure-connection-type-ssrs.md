---
title: Type de connexion SQL Azure (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 02/24/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c84def6c-e8cf-43d9-9912-098171a7ce79
caps.latest.revision: 17
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3183a2d7ec717cfd5f00768c16a142727d3fc058
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-azure-connection-type-ssrs"></a>Type de connexion SQL Azure (SSRS)
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] est une base de données relationnelle hébergée basée sur le cloud qui repose sur les technologies [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour inclure des données de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] dans votre rapport, vous devez avoir un dataset basé sur une source de données de rapport de type [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Ce type de source de données intégré est basé sur l’extension de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Utilisez ce type de source de données pour vous connecter à et récupérer des données de [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Cette extension de données prend en charge des paramètres à valeurs multiples, agrégats de serveur et informations d'identification gérés séparément de la chaîne de connexion.  
  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] est semblable à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans vos locaux et l'obtention de données de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] est, à quelques exceptions près, identique à l'obtention de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Quand vous ouvrez une connexion à [!INCLUDE[ssSDS](../../includes/sssds-md.md)], définissez le délai de connexion à 30 secondes.  
  
 Pour plus d’informations, consultez [Microsoft Azure SQL Database sur docs.microsoft.com](https://docs.microsoft.com/azure/sql-database/).  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="Connection"></a> Chaîne de connexion  
 Lorsque vous vous connectez à [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vous vous connectez à un objet de base de données dans le nuage. Comme pour les bases de données sur site, la base de données hébergée peut comporter plusieurs schémas qui ont plusieurs tables, vues et procédures stockées. Vous spécifiez l'objet de base de données à utiliser dans le concepteur de requêtes. Si vous ne spécifiez pas de base de données dans la chaîne de connexion, vous vous connectez à la base de données par défaut que l'administrateur vous a affectée.  
  
 Contactez l'administrateur de votre base de données pour connaître les informations de connexion et d'identification à utiliser pour se connecter à la source de données. L'exemple de chaîne de connexion suivant spécifie un exemple de base de données hébergé nommé AdventureWorks.  
  
```  
Data Source=<host>;Initial Catalog=AdventureWorks; Encrypt=True;  
```  
  
 De plus, vous utilisez la boîte de dialogue **Propriétés de la source de données** pour fournir des informations d'identification telles que le nom d'utilisateur et le mot de passe. Les options `User Id` et `Password` sont ajoutées automatiquement à la chaîne de connexion ; vous n'avez pas besoin de les taper dans la chaîne de connexion.  
  
 Pour obtenir plus d’informations et des exemples de chaînes de connexion, consultez [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34).  
  
##  <a name="Credentials"></a> Informations d'identification  
 L'authentification Windows (sécurité intégrée) n'est pas prise en charge. Si vous essayez de vous connecter à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] à l'aide de l'authentification Windows, une erreur se produit. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] prend en charge uniquement l’authentification SQL Server (nom d’utilisateur et mot de passe) ; en outre, les utilisateurs doivent fournir des informations d’identification (nom de connexion et mot de passe) chaque fois qu’ils se connectent à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Les informations d'identification doivent être suffisantes pour accéder à la base de données. Selon votre requête, vous pouvez avoir besoin d'autres autorisations, par exemple des autorisations suffisantes pour exécuter des procédures stockées et accéder aux tables et aux vues. Le propriétaire de la source de données externe doit configurer des informations d'identification suffisantes pour fournir un accès en lecture seule aux objets de base de données nécessaires.  
  
 Sur un client de création de rapports, les options suivantes sont disponibles pour spécifier des informations d'identification :  
  
-   Utiliser un nom d'utilisateur et un mot de passe enregistrés. Pour négocier le double tronçon qui se produit lorsque la base de données qui contient les données de rapport est différente du serveur de rapports, sélectionnez les options permettant d'utiliser les informations d'identification en tant qu'informations d'identification Windows. Vous pouvez également choisir d'emprunter l'identité de l'utilisateur authentifié après la connexion à la source de données.  
  
-   Aucune information d'identification n'est requise. Pour utiliser cette option, vous devez avoir configuré le compte d'exécution sans assistance sur le serveur de rapports. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md) dans la [documentation Reporting Services](http://go.microsoft.com/fwlink/?linkid=121312) sur msdn.microsoft.com.  
  
 Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
  
##  <a name="Query"></a> Requêtes  
 Une requête spécifie les données à récupérer pour un dataset de rapport. Les colonnes dans le jeu de résultats d'une requête remplissent la collection de champs pour un dataset. Si la requête retourne plusieurs jeux de résultats, le rapport traite uniquement le premier jeu de résultats que la requête récupère. Bien qu'il y ait des différences entre les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)], notamment les tailles des bases de données prises en charge, l'écriture de requêtes pour des bases de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)]est semblable à l'écriture de requêtes pour des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] telles que BACKUP ne sont pas prises en charge dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], mais il ne s'agit pas de celles que vous utilisez dans les requêtes de rapport. Pour plus d’informations, consultez [Type de connexion SQL Server &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md).  
  
 Par défaut, si vous créez une requête ou si vous ouvrez une requête existante qui peut être représentée dans le concepteur de requêtes graphique, le concepteur de requêtes relationnelles est disponible. Vous pouvez spécifier une requête de plusieurs façons :  
  
-   Générer une requête de manière interactive. Utilisez le concepteur de requêtes relationnelles, qui affiche selon un mode hiérarchique les tables, vues, procédures stockées et autres éléments de base de données, organisés par schéma de base de données. Sélectionnez des colonnes à partir des tables ou des vues, ou spécifiez des procédures stockées ou des fonctions table. Limitez le nombre de lignes de données à récupérer en spécifiant des critères de filtre. Personnalisez le filtre lorsque le rapport s'exécute en définissant l'option de paramètre.  
  
-   Taper ou coller une requête. Utilisez le concepteur de requêtes textuel pour entrer directement du texte [!INCLUDE[tsql](../../includes/tsql-md.md)] , coller du texte de requête à partir d’une autre source, entrer des requêtes complexes impossibles à créer à l’aide du concepteur de requêtes relationnelles, ou entrer des expressions basées sur des requêtes.  
  
-   Importe une requête existante à partir d'un fichier ou d'un rapport. Utilisez le bouton **Importer une requête** à partir de l'un des concepteurs de requêtes afin de naviguer jusqu'à un fichier .sql ou .rdl, et d'importer une requête.  
  
 Le concepteur de requêtes textuel prend en charge les deux modes suivants :  
  
-   [Texte](#QueryText) Commandes de type [!INCLUDE[tsql](../../includes/tsql-md.md)] qui sélectionnent des données à partir de la source de données.  
  
-   [Procédure stockée](#QueryStoredProcedure) Faites votre choix parmi une liste de procédures stockées.  
  
 Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes relationnelles &#40;Générateur de rapports&#41;](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) et [Interface utilisateur du Concepteur de requêtes textuel &#40;Générateur de rapports&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 Le concepteur de requêtes graphique utilisé par [!INCLUDE[ssSDS](../../includes/sssds-md.md)] fournit une prise en charge intégrée du regroupement et des agrégats pour vous aider à écrire des requêtes qui récupèrent uniquement les données de synthèse. Les fonctionnalités du langage [!INCLUDE[tsql](../../includes/tsql-md.md)] sont les suivantes : clause GROUP BY, mot clé DISTINCT et agrégats tels que SUM et COUNT. Le concepteur de requêtes textuel fournit une prise en charge complète du langage [!INCLUDE[tsql](../../includes/tsql-md.md)] , notamment en matière de regroupement et d’agrégats. Pour plus d’informations sur [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Informations de référence sur Transact-SQL &#40;moteur de base de données&#41;](../../t-sql/transact-sql-reference-database-engine.md) dans la [documentation en ligne](http://go.microsoft.com/fwlink/?LinkId=141687) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur msdn.microsoft.com.  
  
###  <a name="QueryText"></a> Utilisation du type de requête Texte  
 Dans le concepteur de requêtes textuel, vous tapez des commandes [!INCLUDE[tsql](../../includes/tsql-md.md)] pour définir les données d’un dataset. Par exemple, la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sélectionne les noms de tous les employés qui occupent la fonction d'assistants marketing :  
  
```  
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```  
  
 Cliquez sur le bouton **Exécuter** (**!**) de la barre d’outils pour exécuter la requête et afficher un jeu de résultats.  
  
 Pour paramétrer cette requête, ajoutez un paramètre de requête. Par exemple, modifiez la clause WHERE comme suit :  
  
```  
WHERE HumanResources.Employee.JobTitle = (@JobTitle)  
```  
  
 Lorsque vous exécutez la requête, les paramètres de rapport qui correspondent aux paramètres de requête sont créés automatiquement. Pour plus d'informations, consultez [Paramètres de requête](#Parameters) plus loin dans cette rubrique.  
  
  
###  <a name="QueryStoredProcedure"></a> Utilisation du type de requête StoredProcedure  
 Vous pouvez spécifier une procédure stockée pour une requête de dataset en procédant de l'une des manières suivantes :  
  
-   Dans la boîte de dialogue **Propriétés du dataset** , définissez l'option **Procédure stockée** . Effectuez un choix dans la zone de liste déroulante de procédures stockées et de fonctions table.  
  
-   Dans le Concepteur de requêtes relationnelles, dans le volet Base de données, sélectionnez une procédure stockée ou une fonction table.  
  
-   Dans le Concepteur de requêtes textuel, sélectionnez **StoredProcedure** dans la barre d’outils.  
  
 Après avoir sélectionné une procédure stockée ou une fonction table, vous pouvez exécuter la requête. Vous serez invité à fournir les valeurs des paramètres d'entrée. Lorsque vous exécutez la requête, les paramètres de rapport qui correspondent aux paramètres d'entrée sont créés automatiquement. Pour plus d'informations, consultez [Paramètres de requête](#Parameters) plus loin dans cette rubrique.  
  
 Seul le premier jeu de résultats extrait pour une procédure stockée est pris en charge. Si une procédure stockée retourne plusieurs jeux de résultats, seul le premier est utilisé.  
  
 Si une procédure stockée possède un paramètre doté d'une valeur par défaut, vous pouvez accéder à cette dernière en utilisant le mot clé DEFAULT comme valeur pour le paramètre. Si le paramètre de requête est lié à un paramètre de rapport, l'utilisateur peut taper ou sélectionner le mot DEFAULT dans la zone d'entrée pour le paramètre de rapport.  
  
 Pour plus d’informations sur les procédures stockées, consultez « Procédures stockées (Moteur de base de données) » dans la [documentation en ligne de SQL Server](http://go.microsoft.com/fwlink/?linkid=98335) sur msdn.microsoft.com.  
  
  
##  <a name="Parameters"></a> Paramètres  
 Lorsque le texte de requête contient des variables de requête ou des procédures stockées dotées de paramètres d'entrée, les paramètres de requête correspondants pour le dataset et les paramètres de rapport pour le rapport sont automatiquement générés. Le texte de requête ne doit pas inclure l'instruction DECLARE pour chaque variable de requête.  
  
 Par exemple, la requête SQL suivante crée un paramètre de rapport nommé **EmpID**:  
  
```  
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```  
  
 Par défaut, chaque paramètre de rapport a le type de données Texte ; en outre, un dataset est créé automatiquement pour fournir une liste déroulante de valeurs disponibles. Après avoir créé les paramètres de rapport, vous devrez peut-être modifier les valeurs par défaut. Pour plus d'informations, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
  
##  <a name="Remarks"></a> Notes  
  
###### <a name="alternate-data-extensions"></a>Autres extensions de données  
 Vous pouvez également récupérer des données à partir d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide d'un type de source de données ODBC. La connexion à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] via OLE DB n'est pas prise en charge.  
  
 Pour plus d’informations, consultez [Type de connexion ODBC &#40;SSRS&#41;](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
###### <a name="platform-and-version-information"></a>Informations sur les plateformes et les versions  
 Pour plus d’informations sur la prise en charge des plateformes et des versions, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
##  <a name="HowTo"></a> Rubriques de procédures  
 Cette section contient des instructions pas à pas sur l'utilisation des connexions de données, des sources de données et des datasets.  
  
 [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="Related"></a> Sections connexes  
 Ces sections de la documentation fournissent des informations de fond d'ordre conceptuel sur les données de rapport, ainsi que des informations sur les procédures de définition, de personnalisation et d'utilisation des parties d'un rapport qui sont liées aux données.  
  
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Fournit une vue d'ensemble de l'accès aux données pour votre rapport.  
  
 [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34)  
 Fournit des informations sur les connexions de données et les sources de données.  
  
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Fournit des informations sur les datasets incorporés et partagés.  
  
 [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Fournit des informations sur la collection de champs de dataset générée par la requête.  
  
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.  
  
  
## <a name="see-also"></a> Voir aussi  
[Microsoft Azure SQL Database sur docs.microsoft.com](https://docs.microsoft.com/azure/sql-database/)  
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
 D’autres questions ? [Essayez le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
  
  
