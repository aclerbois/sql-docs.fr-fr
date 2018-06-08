---
title: Créer une Source de données de rapport | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bf4b8048392121c37b6f4f13584aa2fd18a90984
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-report-data-source"></a>Créer une source de données de rapport
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pour permettre à Power View de se connecter à un modèle multidimensionnel, vous devez créer une définition de source de données de rapport partagée, également appelée fichier .rsds, dans une bibliothèque SharePoint. Le fichier .rsds spécifie le nom d'une instance de serveur Analysis Services, le type de connexion, la chaîne de connexion et les informations d'identification utilisées pour la connexion à la source de données. Lorsqu'un utilisateur clique sur le fichier .rsds, un nouveau rapport Power View vide (un fichier .rdlx) s'ouvre dans le navigateur.  
  
 Pour créer une connexion .rsds, vous devez disposer de SQL Server 2012 (ou version ultérieure) Reporting Services et du complément Reporting Services pour SharePoint 2010 ou SharePoint 2013.  
  
## <a name="create-a-report-data-source-rsds-connection-to-a-multidimensional-model"></a>Créer une connexion de source de données de rapport (.rsds) à un modèle multidimensionnel  
 Avant de commencer, vous devez connaître les éléments suivants :  
  
-   Le nom de l'instance de serveur Analysis Services exécutée en mode multidimensionnel.  
  
-   Le nom de la base de données multidimensionnelle à laquelle vous souhaitez vous connecter.  
  
-   Le nom du cube, s'il en existe plusieurs.  
  
-   (Facultatif) Nom de la perspective.  
  
-   (Facultatif) Identificateur de paramètres régionaux.  
  
#### <a name="to-create-a-shared-report-data-source-rsds-file-sharepoint-2010"></a>Pour créer un fichier de source de données de rapport partagée (.rsds) (SharePoint 2010)  
  
1.  Cliquez sur l'onglet **Documents** dans le ruban de la bibliothèque.  
  
2.  Cliquez sur **Nouveau document** > **Source de données du rapport**.  
  
    > [!NOTE]  
    >  Si l’élément **Source de données du rapport** ne figure pas dans le menu, cela signifie que le type de contenu de la source de données du rapport n’a pas été activé pour cette bibliothèque. Pour plus d’informations, consultez [Ajouter des types de contenus Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
3.  Dans la page **Propriétés de la source de données** , dans la zone **Nom**, entrez un nom pour le fichier .rsds de connexion.  
  
4.  Dans la zone **Type de source de données**, sélectionnez **Modèle sémantique Microsoft BI pour Power View**.  
  
5.  Dans **Chaîne de connexion**, spécifiez le nom du serveur Analysis Services, le nom de la base de données, le nom du cube et d’autres paramètres facultatifs.  
  
     Chaîne de connexion : `Data source=<servername>;initial catalog=<multidimensionaldatabasename>-ee;cube='<cubename>’`  
  
    > [!NOTE]  
    >  S'il existe plusieurs cubes, spécifiez un nom de cube.  
  
     (Facultatif) Les cubes peuvent contenir des perspectives qui fournissent aux utilisateurs une vue sélectionnée dans laquelle seules certaines dimensions ou seuls certains groupes de mesures sont visibles dans le client. Pour spécifier une perspective, entrez le nom de la perspective en tant que valeur de la propriété Cube : `Data source=<servername>;initial catalog=<multidimensionaldatabasename>-ee;cube='<perspectivename>’`  
  
     (Facultatif) Les cubes peuvent disposer de traductions de données et de métadonnées, spécifiées pour différentes langues au sein du modèle. Pour voir les traductions (données et métadonnées), vous devez ajouter la propriété « Locale Identifier » à la chaîne de connexion : `Data source=<servername>;initial catalog=<multidimensionaldatabasename>-ee;cube='<cubename>’; Locale Identifier=<identifier number>`  
  
6.  Dans **Informations d'identification**, spécifiez comment le serveur de rapports obtient les informations d'identification permettant d'accéder à la source de données externe.  
  
    -   Sélectionnez **Authentification Windows (intégrée)** pour pouvoir accéder aux données à l’aide des informations d’identification de l’utilisateur qui a ouvert le rapport. Ne sélectionnez pas cette option si la batterie de serveurs ou le site SharePoint utilise l'authentification par formulaire ou si la connexion au serveur de rapports s'effectue via un compte approuvé. Ne sélectionnez pas cette option si vous voulez planifier un abonnement ou un traitement de données pour ce rapport. Cette option convient mieux lorsque l'authentification Kerberos est activée pour votre domaine ou que la source de données se trouve sur le même ordinateur que le serveur de rapports. Si l'authentification Kerberos n'est pas activée, les informations d'identification Windows ne peuvent être transmises qu'à un seul autre ordinateur. En d'autres termes, si la source de données externe se trouve sur un autre ordinateur et si elle nécessite une connexion supplémentaire, vous recevrez une erreur au lieu des données attendues.  
  
    -   Sélectionnez **Demander des informations d'identification** pour que l'utilisateur entre ses informations d'identification chaque fois qu'il exécute le rapport. Ne sélectionnez pas cette option si vous voulez planifier un abonnement ou un traitement de données pour ce rapport.  
  
    -   Sélectionnez **Informations d'identification stockées** , si vous souhaitez accéder aux données via un jeu unique d'informations d'identification. Les informations d'identification sont chiffrées avant d'être stockées. Vous pouvez sélectionner les options appropriées pour déterminer le mode de stockage des informations d'identification. Sélectionnez l'option Utiliser les informations d'identification Windows si les informations d'identification stockées appartiennent à un compte d'utilisateur Windows. Sélectionnez l'option **Définir le contexte d'exécution pour ce compte** pour définir le contexte d'exécution sur le serveur de base de données.  
  
    -   Sélectionnez **Informations d’identification non requises** si vous souhaitez spécifier les informations d’identification dans la chaîne de connexion ou si vous souhaitez exécuter le rapport à l’aide d’un compte avec privilèges de bas niveau.  
  
7.  Cliquez sur **Tester la connexion** pour valider.  
  
8.  Sélectionnez **Activer cette source de données** si vous souhaitez que la source de données soit active. Si la source de données est configurée mais qu'elle n'est pas active, les utilisateurs recevront un message d'erreur lorsqu'ils essaieront de créer un rapport.  
  
  
