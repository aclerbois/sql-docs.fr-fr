---
title: Créer une connexion de modèle sémantique BI vers une base de données de modèle tabulaire | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d17f5d68c1ea60c413a631ba0d734768c61fa5b2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-bi-semantic-model-connection-to-a-tabular-model-database"></a>Créer une connexion de modèle sémantique BI à une base de données model tabulaire
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Utilisez les informations de cette rubrique pour configurer une connexion de modèle sémantique BI qui redirige vers une base de données model tabulaire qui s'exécute sur une instance Analysis Services en dehors de la batterie SharePoint.  
  
 Après avoir créé une connexion de modèle sémantique BI et configuré les autorisations SharePoint et Analysis Services, les utilisateurs peuvent l'utiliser comme source de données pour les rapports Excel ou [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] .  
  
 Cette rubrique comprend les sections suivantes. Effectuez chaque tâche dans l'ordre indiqué.  
  
 [Examiner la configuration requise](#bkmk_prereq)  
  
 [Accorder les autorisations d'administration Analysis Services aux applications de services partagés](#bkmk_ssas)  
  
 [Accorder les autorisations de lecture sur la base de données model tabulaire](#bkmk_BISM)  
  
 [Créer une connexion de modèle sémantique BI à une base de données model tabulaire](#bkmk_connect)  
  
 [Configurer les autorisations SharePoint sur la connexion de modèle sémantique BI](#bkmk_permissions)  
  
 [Étapes suivantes](#bkmk_next)  
  
##  <a name="bkmk_prereq"></a> Examiner la configuration requise  
 Vous devez disposer d'autorisations Collaboration ou supérieures pour créer un fichier de connexion de modèle sémantique BI.  
  
 Vous devez disposer d'une bibliothèque qui prend en charge le type de contenu Connexion de modèle sémantique BI. Pour plus d’informations, consultez [Ajouter un type de contenu de connexion du modèle sémantique BI à une bibliothèque &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/add-bi-semantic-model-connection-content-type-to-library.md).  
  
 Vous devez connaître le serveur et le nom de la base de données pour lesquels vous configurez une connexion de modèle sémantique BI. Analysis Services doit être configuré pour le mode tabulaire. Les bases de données qui s'exécutent sur le serveur doivent être des bases de données model tabulaires. Pour obtenir des instructions sur la recherche du mode serveur, consultez [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md).  
  
 Dans certains scénarios, les services partagés dans un environnement SharePoint doivent avoir des autorisations d'administrateur sur l'instance Analysis Services. Ces services incluent des applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , Reporting Services et PerformancePoint. Avant de pouvoir accorder des autorisations d'administrateur, vous devez connaître l'identité de ces applications de service. Vous pouvez utiliser l'Administration centrale pour déterminer l'identité.  
  
 Vous devez être administrateur de service SharePoint pour afficher les informations de sécurité dans l'Administration centrale.  
  
 Vous devez être administrateur système Analysis Services pour accorder des droits d'administration dans Management Studio.  
  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint doit être accessible par le biais des applications web qui utilisent un mode d’authentification classique. Les connexions de modèles sémantiques BI aux sources de données externes dépendent de la connexion en mode classique. Pour plus d’informations, voir [Power Pivot Authentication and Authorization](../../analysis-services/power-pivot-sharepoint/power-pivot-authentication-and-authorization.md).  
  
 Tous les ordinateurs et utilisateurs qui participent à la séquence de connexion doivent être dans le même domaine ou dans un domaine approuvé (approbation bidirectionnelle).  
  
##  <a name="bkmk_ssas"></a> Accorder les autorisations d'administration Analysis Services aux applications de services partagés  
 Les connexions entre SharePoint et une base de données model tabulaire sur un serveur Analysis Services sont parfois générées par un service partagé pour le compte de l'utilisateur qui demande les données. Le service qui exécute la requête peut être une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , une application de service Reporting Services ou une application de service PerformancePoint. Pour que la connexion réussisse, le service doit disposer d'autorisations administratives sur le serveur Analysis Services. Dans Analysis Services, seul un administrateur est autorisé à établir une connexion avec emprunt d'identité pour le compte d'un autre utilisateur.  
  
 Les autorisations administratives sont nécessaires lorsque la connexion est utilisée dans ces conditions :  
  
-   Lors de la vérification des informations de connexion au cours de la configuration d'un fichier de connexion de modèle sémantique BI.  
  
-   Lors du démarrage d'un rapport Power View à l'aide d'une connexion de modèle sémantique BI.  
  
-   Lors du remplissage d'un composant WebPart PerformancePoint à l'aide d'une connexion de modèle sémantique BI.  
  
 Pour vous assurer que ces comportements fonctionnent comme prévu, accordez à chaque identité de service des autorisations d'administrateur sur l'instance Analysis Services. Utilisez les instructions suivantes pour accorder les autorisations requises.  
  
 **Ajouter des identités de service au rôle d'administrateur de serveur**  
  
1.  Dans SQL Server Management Studio, connectez-vous à l'instance d'Analysis Services.  
  
2.  Cliquez avec le bouton droit sur le nom du serveur, puis sélectionnez **Propriétés**.  
  
3.  Cliquez sur **Sécurité**, puis cliquez sur **Ajouter**. Entrez le compte d'utilisateur Windows utilisé pour exécuter l'application de service.  
  
     Vous pouvez utiliser l'Administration centrale pour déterminer l'identité. Dans la section Sécurité, ouvrez **Configurer les comptes de service** pour afficher le compte Windows associé au pool d'applications de service utilisé pour chaque application, puis suivez les instructions fournies dans cette rubrique pour accorder des autorisations administratives au compte.  
  
##  <a name="bkmk_BISM"></a> Accorder les autorisations de lecture sur la base de données model tabulaire  
 Étant donné que la base de données s'exécute sur un serveur externe à la batterie, une partie de la configuration de vos connexions consiste à accorder aux utilisateurs de base de données des autorisations sur le serveur Analysis Services principal. Analysis Services utilise un modèle d'autorisation basé sur les rôles. Les utilisateurs qui se connectent aux bases de données model doivent avoir des autorisations de lecture ou supérieures, via un rôle qui accorde l'accès en lecture à ses membres.  
  
 Les rôles, et parfois l'appartenance au rôle, sont définis lorsque le modèle est créé dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Vous ne pouvez pas utiliser SQL Server Management Studio pour créer des rôles, mais vous pouvez l'utiliser pour ajouter des membres à un rôle qui est déjà défini. Pour plus d’informations sur la création de rôles, consultez [créer et gérer les rôles](../../analysis-services/tabular-models/create-and-manage-roles-ssas-tabular.md).  
  
#### <a name="assign-role-membership"></a>Assigner l'appartenance au rôle  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], développez la base de données dans l'Explorateur d'objets, puis développez **Rôles**. Vous devez voir un rôle qui est déjà défini. S'il n'existe aucun rôle, contactez l'auteur du modèle et demandez l'ajout d'un rôle. Le modèle doit être redéployé avant que le rôle soit visible dans Management Studio.  
  
2.  Cliquez avec le bouton droit sur le rôle, puis sélectionnez **Propriétés**.  
  
3.  Dans la page Appartenance, ajoutez les comptes d'utilisateurs et de groupe Windows qui requièrent l'accès.  
  
##  <a name="bkmk_connect"></a> Créer une connexion de modèle sémantique BI à une base de données model tabulaire  
 Après avoir défini les autorisations dans Analysis Services, vous pouvez revenir à SharePoint et créer une connexion de modèle sémantique BI.  
  
1.  Dans la bibliothèque qui contiendra la connexion de modèle sémantique BI, cliquez sur **Documents** sur le ruban SharePoint.  
  
2.  Cliquez sur la flèche de déroulement de Nouveau document, puis sélectionnez **Fichier de connexion de modèle sémantique BI** pour ouvrir la page Nouvelle connexion de modèle sémantique BI.  
  
3.  Définissez à la fois les propriétés du **Serveur** et de la **Base de données** . Si vous n'êtes pas sûr du nom de la base de données, utilisez SQL Server Management Studio pour consulter la liste des bases de données déployées sur le serveur.  
  
     **Nom du serveur** est le nom réseau du serveur, l’adresse IP ou le nom de domaine complet (par exemple, myserver.mydomain.corp.adventure-works.com). Si le serveur est installé comme une instance nommée, entrez le nom du serveur selon ce format : nomordinateur\nominstance.  
  
     **Base de données** doit être une base de données tabulaire actuellement disponible sur le serveur. Ne spécifiez pas un autre fichier de connexion de modèle sémantique BI, un fichier Office Data Connection (.odc), une base de données OLAP Analysis Services ni un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Pour obtenir le nom de la base de données, vous pouvez utiliser Management Studio pour vous connecter au serveur et afficher la liste des bases de données disponibles. Utilisez la page de propriétés de la base de données pour vous assurer d'utiliser le nom correct.  
  
4.  Cliquez sur **OK** pour enregistrer la page. À ce stade, l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] vérifie la connexion.  
  
     La vérification réussit si les informations de connexion sont correctes et que vous avez accordé des autorisations d’administrateur à l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour qu’elle puisse se connecter à Analysis Services en tant qu’utilisateur actuel.  
  
     La vérification échoue si les informations de connexion sont incorrectes, ou si l'application de service ne possède pas les autorisations. Un message de validation s'affiche dans la page et vous demande si vous souhaitez enregistrer le fichier. Si vous savez que la connexion est valide, vous devriez sauvegarder le fichier, car l'erreur résulte du fait qu'il manque des autorisations plutôt que du fait que les informations de connexion ne sont pas valides.  
  
     Vous pouvez vérifier la connexion en l'utilisant dans Excel ou Power View pour vous connecter à la base de données model tabulaire. Si la connexion à la source de données réussit, la connexion est valide en dépit de l'avertissement.  
  
##  <a name="bkmk_permissions"></a> Configurer les autorisations SharePoint sur la connexion de modèle sémantique BI  
 La possibilité d'utiliser une connexion de modèle sémantique BI comme source de données pour un classeur Excel ou un rapport Reporting Services requiert des autorisations de **Lecture** sur l'élément de connexion de modèle sémantique BI dans une bibliothèque SharePoint. Le niveau d'autorisation Lecture inclut l'autorisation **Ouvrir les éléments** qui permet le téléchargement des informations de connexion de modèle sémantique BI vers une application bureautique Excel.  
  
 Il existe plusieurs manières d'accorder des autorisations dans SharePoint. L’instruction suivante explique comment créer un groupe appelé **Utilisateurs BISM** qui dispose du niveau d’autorisation **Read** .  
  
 Vous devez être le propriétaire du site pour modifier des autorisations.  
  
1.  Dans Actions du site, cliquez sur **Autorisations de site**.  
  
2.  Cliquez sur **Créer un groupe** et nommez le nouveau groupe **Utilisateurs BISM**.  
  
3.  Choisissez le niveau d'autorisation **Lecture** et cliquez sur **Créer**.  
  
4.  Sélectionnez **Utilisateurs BISM** dans Personnes et groupes.  
  
5.  Pointez sur Nouveau, cliquez sur **Ajouter des utilisateurs**, puis ajoutez des comptes d'utilisateur ou de groupe.  
  
     Ces utilisateurs et groupes bénéficient maintenant d'autorisations de lecture sur l'ensemble du site, notamment les bibliothèques et listes qui héritent des autorisations au niveau du site. Si ces autorisations sont trop élevées, vous pouvez sélectivement supprimer ce groupe de bibliothèques, de listes ou d'éléments spécifiques.  
  
 Pour supprimer sélectivement des autorisations au niveau de l'élément, procédez comme suit :  
  
1.  Dans une bibliothèque, sélectionnez un document. Cliquez sur la flèche vers le bas, puis sur **Gérer les autorisations**.  
  
2.  Par défaut, un élément hérite des autorisations. Pour modifier les autorisations de documents individuels dans cette bibliothèque, cliquez sur **Arrêter l’héritage des autorisations**.  
  
3.  Cochez la case en regard d’ **Utilisateurs BISM**.  
  
4.  Cliquez sur **Supprimer les autorisations des utilisateurs**.  
  
##  <a name="bkmk_next"></a> Étapes suivantes  
 Après avoir créé et sécurisé une connexion de modèle sémantique BI, vous pouvez la spécifier en tant que source de données. Pour plus d’informations, consultez [Utiliser une connexion de modèle sémantique BI dans Excel ou Reporting Services](../../analysis-services/power-pivot-sharepoint/use-a-bi-semantic-model-connection-in-excel-or-reporting-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion de modèle sémantique BI Power Pivot &#40;.bism&#41;](../../analysis-services/power-pivot-sharepoint/power-pivot-bi-semantic-model-connection-bism.md)   
 [Créer une connexion de modèle sémantique BI à un classeur PowerPivot](../../analysis-services/power-pivot-sharepoint/create-a-bi-semantic-model-connection-to-a-power-pivot-workbook.md)  
  
  
