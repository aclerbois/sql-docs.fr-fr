---
title: Créer, modifier, puis supprimer des sources de données partagées (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- modifying data source properties
- shared data sources [Reporting Services]
- removing shared data sources
- roles [Reporting Services], shared data sources
- data sources [Reporting Services], shared
- data sources [Reporting Services], modifying properties
- deleting shared data sources
ms.assetid: 1e58c1c2-5ecf-4ce6-9d04-0a8acfba17be
caps.latest.revision: 53
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f68f43c6b004219977aed509286c8d56fdca1afe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-modify-and-delete-shared-data-sources-ssrs"></a>Créer, modifier, puis supprimer des sources de données partagées (SSRS)
  Une source de données partagée est un ensemble de propriétés de connexion à la source de données pouvant être référencées par plusieurs rapports, modèles et abonnements pilotés par les données qui s’exécutent sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  Les sources de données partagées permettent de gérer facilement des propriétés de source de données qui changent souvent dans le temps. Si le compte ou le mot de passe d'un utilisateur change ou bien si vous déplacez la base de données sur un serveur différent, vous pouvez mettre à jour les informations de connexion à un seul endroit.  
  
 L'icône suivante indique une source de données partagée dans l'arborescence des dossiers du Gestionnaire de rapports :  
  
 ![Icône Source de données partagée](../../reporting-services/report-data/media/hlp-16datasource.png "Icône Source de données partagée")  
icône de source de données partagée  
  
 Les sources de données partagées sont obligatoires pour les modèles de rapports et facultatives pour les rapports et les abonnements pilotés par les données. Si vous envisagez d'utiliser des modèles de rapports pour la génération d'états ad hoc, vous devez créer et gérer un élément de source de données partagée pour fournir des informations de connexion au modèle.  
  
 Une source de données partagée se compose des éléments suivants :  
  
|Élément|Description|  
|----------|-----------------|  
|Nom   |Nom qui identifie l'élément au sein de la hiérarchie des dossiers du serveur de rapports.|  
|Description|Description qui apparaît avec l'élément dans le Gestionnaire de rapports lorsque vous consultez le contenu du dossier.|  
|Type de connexion|Extension pour le traitement des données utilisée avec la source de données. Vous ne pouvez utiliser que les extensions pour le traitement des données qui sont déployées sur le serveur de rapports. Pour plus d’informations sur les extensions pour le traitement des données incluses dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).|  
|Chaîne de connexion|Chaîne de connexion pour la base de données. Pour plus d’informations et pour consulter des exemples de chaînes de connexion aux sources de données fréquemment utilisées, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).|  
|Type d'informations d'identification|Spécifie la façon dont les informations d'identification sont obtenues pour la connexion et si elles doivent être utilisées une fois la connexion établie. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).|  
  
 Une source de données partagée ne contient pas d'informations de requête utilisées pour récupérer des données. La requête est toujours conservée dans une définition de rapport.  
  
## <a name="creating-and-modifying-shared-data-sources"></a>Création et modification de sources de données partagées  
 Pour créer une source de données partagée ou modifier ses propriétés, vous devez disposer d'autorisations **Gérer les sources de données** sur le serveur de rapports. Si le serveur de rapports s'exécute en mode natif, vous pouvez utiliser le Gestionnaire de rapports pour créer et configurer la source de données partagée. Si le serveur de rapports s'exécute en mode intégré SharePoint, vous pouvez utiliser les pages d'application sur un site SharePoint. Pour n'importe quel serveur de rapports et quel que soit son mode, vous pouvez créer une source de données partagée dans le Concepteur de rapports, puis la publier sur un serveur cible.  
  
 Après avoir créé une source de données partagée sur le serveur de rapports, vous pouvez créer des attributions de rôle pour y contrôler l'accès, la déplacer à un autre endroit, la renommer ou la mettre hors ligne pour empêcher le traitement des rapports pendant que des opérations de maintenance sont effectuées sur la source de données externe. Si vous renommez ou déplacez un élément de source de données partagée à un autre endroit de la hiérarchie des dossiers du serveur de rapports, les informations de chemin de tous les rapports ou abonnements qui font référence à cette source de données partagée sont corrigées en conséquence. Si vous mettez la source de données partagée hors ligne, aucun rapport, modèle ou abonnement ne s'exécutera tant que vous n'aurez pas réactivé la source de données.  
  
 Pour plus d’informations sur le contrôle de l’accès aux sources de données partagées dans l’arborescence des dossiers du serveur de rapports, consultez [Sécuriser les éléments de source de données partagée](../../reporting-services/security/secure-shared-data-source-items.md).  
  
 **Pour créer une source de données partagée dans le Concepteur de rapports**  
  
1.  Dans la barre d'outils du volet des données de rapport, cliquez sur **Nouveau** , puis sur **Source de données**. La boîte de dialogue **Propriétés de la source de données** s'ouvre.  
  
    > [!NOTE]  
    >  Si le volet des données de rapport n’est pas visible, cliquez sur l’option **Données du rapport** du menu **Affichage** .  
  
2.  Dans la zone de texte **Nom** , tapez un nom pour la source de données ou acceptez la valeur par défaut. Le nom de la source de données est utilisé en interne dans le rapport. Par souci de clarté, il est recommandé que le nom de la source de données contienne le nom de la base de données spécifiée dans la chaîne de connexion.  
  
3.  Vérifiez que l’option **Utiliser une référence de source de données partagée** est sélectionnée, puis procédez comme suit.  
  
    1.  Cliquez sur **Nouveau**. Dans la boîte de dialogue de propriétés **Source de données partagée** , suivez les étapes 2 et 3 pour créer une source de données.  
  
    2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
         La nouvelle source de données partagée apparaît dans le dossier Sources de données partagées, dans l'Explorateur de solutions.  
  
4.  Cliquez sur Informations d’identification.  
  
     Spécifiez les informations d'identification à utiliser pour cette source de données. Le propriétaire de la source de données choisit le type d'informations d'identification pris en charge.  
  
 **Pour créer une source de données partagée dans le Gestionnaire de rapports**  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Dans le Gestionnaire de rapports, parcourez l'arborescence jusqu'à la page **Contenu** .  
  
3.  Cliquez sur **Nouvelle source de données**. La page **Nouvelle source de données** s’ouvre.  
  
4.  Tapez le nom de l'élément. Le nom doit contenir au moins un caractère et il doit commencer par une lettre. Il peut également comprendre des symboles, à l'exception des espaces ou des caractères ; ? : @ & = + , $ / * < > | " /.  
  
5.  Si vous le souhaitez, entrez une description renseignant les utilisateurs sur la connexion. Ce descriptif s'affiche dans la page **Contenu** du Gestionnaire de rapports.  
  
6.  Dans la liste **Type de source de données** , spécifiez l'extension pour le traitement des données qui est utilisée en vue d'exploiter les informations à partir de la source de données.  
  
7.  Dans la zone **Chaîne de connexion**, spécifiez la chaîne de connexion utilisée par le serveur de rapports pour se connecter à la source de données. Il est recommandé de n'indiquer aucune information d'identification ici.  
  
     L'exemple suivant illustre l'utilisation d'une chaîne de connexion pour se connecter à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] locale :  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2012  
    ```  
  
8.  Pour **Se connecter en utilisant**, précisez comment les informations d'identification sont obtenues lorsque le rapport s'exécute :  
  
    -   Si vous voulez demander à l'utilisateur un nom de connexion et un mot de passe, cliquez sur **Informations d'identification fournies par l'utilisateur qui exécute le rapport**. Pour utiliser les informations d'identification fournies par l'utilisateur en tant qu'informations d'identification Windows, activez la case à cocher **Utiliser comme informations d'identification Windows lors de la connexion à la source de données**. Si le nom d'utilisateur et le mot de passe sont des informations d'identification de base de données, ne sélectionnez pas cette option.  
  
    -   Si vous avez l’intention d’utiliser la source de données comme source de données partagée avec des informations d’identification enregistrées gérées par le propriétaire de la source de données ou pour des rapports prenant en charge les abonnements ou d’autres opérations planifiées (par exemple, la création automatique d’un historique de rapport), cliquez sur **Informations d’identification stockées de manière sécurisée sur le serveur de rapports**. Si le serveur de base de données prend en charge l'emprunt d'identité ou la délégation, sélectionnez **Emprunter l'identité de l'utilisateur authentifié une fois la connexion établie à la source de données**.  
  
    -   Si vous souhaitez que le serveur de rapports transmette les informations d'identification de l'utilisateur du rapport au serveur qui héberge la source de données externe, cliquez sur **Sécurité intégrée de Windows**. Dans ce cas, l'utilisateur n'est pas invité à taper son nom d'utilisateur ou son mot de passe.  
  
    -   Si la source de données n’utilise pas d’informations d’identification (par exemple, si la source de données est un fichier XML accessible à partir du le système de fichiers), cliquez sur **Informations d’identification non requises**. Spécifiez uniquement ce type d'informations d'identification s'il est valide pour la source de données. Si vous sélectionnez cette option pour une source de données qui requiert l'authentification, la connexion échoue. Si vous sélectionnez cette option, veillez à configurer le compte d'exécution sans assistance, qui permet au serveur de rapports de se connecter à d'autres ordinateurs pour récupérer des données ou des fichiers lorsque les informations d'identification de l'utilisateur ne sont pas disponibles.  
  
         Pour plus d’informations sur la configuration des informations d’identification, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md). Pour plus d’informations sur le compte d’exécution sans assistance, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
9. Cliquez sur le bouton **Tester la connexion** pour valider la configuration de la source de données.  
  
    > [!NOTE]  
    >  Le bouton Tester la connexion n'est pas pris en charge pour le type de source de données XML.  
  
10. Cliquez sur **OK**  
  
 **Pour modifier une source de données partagée dans le Gestionnaire de rapports**  
  
1.  Dans le Gestionnaire de rapports, parcourez l'arborescence jusqu'à la page Contenu.  
  
2.  Accédez à l’élément de source de données partagée, pointez sur l’élément, cliquez sur la liste déroulante, puis dans le menu contextuel, cliquez sur **Gérer**. La page de **propriétés** s'ouvre.  
  
3.  Modifiez la source de données, puis cliquez sur **Appliquer**.  
  
## <a name="deleting-shared-data-sources"></a>suppression des sources de données partagées  
 Vous pouvez supprimer une source de données partagée de la même manière que vous supprimez un élément quelconque du serveur de rapports.  
  
 **Pour supprimer une source de données partagée**  
  
1.  Dans le Gestionnaire de rapports, parcourez l'arborescence jusqu'à la page **Contenu** et effectuez l'une des opérations suivantes :  
  
    -   Naviguez jusqu'à l'élément de source de données partagée.  
  
         Cliquez sur l'élément pour l'ouvrir. La page des propriétés générales s'ouvre.  
  
         Cliquez sur **Supprimer**, puis sur **OK**.  
  
    -   Dans la page **Contenu** , parcourez l'arborescence jusqu'au dossier contenant la source de données à supprimer.  
  
         Pointez sur l’élément, cliquez sur la liste déroulante, puis dans le menu contextuel, cliquez sur **Supprimer**.  
  
         [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 La suppression d'une source de données partagée désactive tout rapport, modèle ou abonnement piloté par les données qui l'utilise. Sans informations de connexion à la source de données, les éléments ne s'exécuteront plus. Pour activer ces éléments, vous devez ouvrir chacun d'eux et effectuer les opérations suivantes :  
  
-   Pour les rapports et abonnements pilotés par les données qui référencent la source de données partagée, vous pouvez spécifier les informations de connexion à la source de données dans les propriétés du rapport ou l'abonnement au rapport. Vous pouvez aussi sélectionner une nouvelle source de données partagée possédant les valeurs que vous souhaitez utiliser.  
  
-   Pour les modèles et les rapports du Générateur de rapports qui utilisent ce modèle, vous devez spécifier une nouvelle source de données partagée. Les modèles obtiennent uniquement des informations de connexion à la source de données par le biais des sources de données partagées.  
  
 Il n'existe pas d'opération d'annulation pour la suppression d'une source de données partagée. Toutefois, si vous supprimez par erreur une source de données partagée, vous pouvez en créer une autre en utilisant les mêmes valeurs de propriété que celles de la source que vous avez supprimée. Vous devrez ouvrir chaque rapport, modèle et abonnement piloté par les données pour relier la source de données partagée à l'élément qui l'utilise, mais tant que les propriétés de source de données sont les mêmes qu'avant, les rapports, modèles et abonnements continueront de fonctionner comme avant.  
  
## <a name="importing-shared-data-sources"></a>Importation de sources de données partagées  
 **Pour importer une source de données existante dans le Concepteur de rapports**  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier **Sources de données partagées** dans le projet Report Server, puis cliquez sur **Ajouter un élément existant**. La boîte de dialogue **Ajouter un élément existant** s'ouvre.  
  
2.  Accédez à un fichier de source de données partagée (rds) de définition de rapport existant, puis cliquez sur **Ouvrir**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="shared-data-sources-in-sharepoint"></a>Sources de données partagées dans SharePoint  
 Lorsque vous exécutez un rapport à partir d'une bibliothèque SharePoint, les informations de connexion peuvent être définies dans le rapport ou dans un fichier externe lié au rapport. Si les informations de connexion sont incorporées dans le rapport, elles constituent une source de données personnalisée. Si les informations de connexion sont définies dans un fichier externe, elles constituent une source de données partagée. Le fichier externe peut être un fichier de source de données du serveur de rapports (.rsds) ou un fichier de connexion de données Office (.odc).  
  
 Un fichier .rsds est similaire à un fichier .rds, mais possède un schéma différent. Pour créer un fichier .rsds, vous pouvez publier un fichier .rds issu du Concepteur de rapports ou du Générateur de modèles dans une bibliothèque SharePoint (un nouveau fichier .rsds est créé à partir du fichier .rds d'origine). Vous pouvez également créer un fichier dans une bibliothèque d'un site SharePoint.  
  
 Une fois que vous avez créé ou publié une source de données partagée, vous pouvez modifier les propriétés de connexion ou supprimer le fichier s'il n'est plus utilisé. Avant de supprimer une source de données partagée, vous devez déterminer si elle est utilisée par des rapports et des modèles de rapport. Pour cela, vous pouvez examiner les éléments dépendants qui font référence à la source de données partagée.  
  
 Bien que la liste des éléments dépendants indique si la source de données partagée est référencée, elle n'indique pas si l'élément est utilisé de manière active. Pour déterminer si la source de données partagée ou le modèle est utilisé de manière active, vous pouvez examiner les fichiers journaux du serveur de rapports. Si vous n'avez pas accès aux fichiers journaux ou si les fichiers ne contiennent pas les informations souhaitées, songez à déplacer le rapport vers un dossier inaccessible le temps de déterminer son état réel.  
  
 **Pour créer un fichier de source de données partagée (.rsds) (SharePoint 2010)**  
  
1.  Cliquez sur l'onglet **Documents** dans le ruban de la bibliothèque.  
  
2.  Dans le menu **Nouveau document** , cliquez sur **Source de données du rapport**.  
  
    > [!NOTE]  
    >  Si l'élément **Source de données du rapport** ne figure pas dans le menu, cela signifie que le type de contenu de la source de données du rapport n'a pas été activé. Pour plus d’informations, consultez [Ajouter des types de contenus Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
3.  Dans **Nom**, entrez un nom descriptif pour le fichier .rsds.  
  
4.  Dans **Type de source de données**, sélectionnez le type de source de données dans la liste. Pour plus d’informations, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
5.  Dans **Chaîne de connexion**, spécifiez un pointeur vers la source de données, ainsi que les autres paramètres nécessaires à l'établissement d'une connexion à la source de données externe. Le type de source de données utilisé détermine la syntaxe de la chaîne de connexion. Pour obtenir plus d’informations et des exemples, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
6.  Dans **Informations d'identification**, spécifiez comment le serveur de rapports obtient les informations d'identification permettant d'accéder à la source de données externe. Les informations d'identification peuvent être stockées, demandées, intégrées ou configurées pour le traitement autonome des rapports.  
  
    -   Sélectionnez **Authentification Windows (intégrée)** pour pouvoir accéder aux données à l’aide des informations d’identification de l’utilisateur qui a ouvert le rapport. Ne sélectionnez pas cette option si la batterie de serveurs ou le site SharePoint utilise l'authentification par formulaire ou si la connexion au serveur de rapports s'effectue via un compte approuvé. Ne sélectionnez pas cette option si vous voulez planifier un abonnement ou un traitement de données pour ce rapport. Cette option convient mieux lorsque l'authentification Kerberos est activée pour votre domaine ou que la source de données se trouve sur le même ordinateur que le serveur de rapports. Si l'authentification Kerberos n'est pas activée, les informations d'identification Windows ne peuvent être transmises qu'à un seul autre ordinateur. En d'autres termes, si la source de données externe se trouve sur un autre ordinateur et si elle nécessite une connexion supplémentaire, vous recevrez une erreur au lieu des données attendues.  
  
    -   Sélectionnez **Demander des informations d'identification** pour que l'utilisateur entre ses informations d'identification chaque fois qu'il exécute le rapport. Ne sélectionnez pas cette option si vous voulez planifier un abonnement ou un traitement de données pour ce rapport.  
  
    -   Sélectionnez **Informations d'identification stockées** , si vous souhaitez accéder aux données via un jeu unique d'informations d'identification. Les informations d'identification sont chiffrées avant d'être stockées. Vous pouvez sélectionner les options appropriées pour déterminer le mode de stockage des informations d'identification. Sélectionnez l'option Utiliser les informations d'identification Windows si les informations d'identification stockées appartiennent à un compte d'utilisateur Windows. Sélectionnez l'option **Définir le contexte d'exécution pour ce compte** pour définir le contexte d'exécution sur le serveur de base de données. Pour les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette option définit la fonction SETUSER. Pour plus d’informations, consultez [SETUSER &#40;Transact-SQL&#41;](../../t-sql/statements/setuser-transact-sql.md).  
  
    -   Sélectionnez **Informations d’identification non requises** , si vous voulez spécifier les informations d’identification dans la chaîne de connexion, ou si vous voulez exécuter le rapport à l’aide d’un compte doté de privilèges minimaux et configuré sur le serveur de rapports. Si ce compte n'est pas configuré sur le serveur de rapports, les utilisateurs sont invités à fournir des informations d'identification ; par ailleurs, les opérations planifiées définies pour le rapport ne s'exécutent pas.  
  
7.  Sélectionnez **Activer cette source de données** si vous souhaitez que la source de données soit active. Si la source de données est configurée mais qu'elle n'est pas active, les utilisateurs verront un message d'erreur lorsqu'ils essaieront d'utiliser un rapport basé sur la source de données.  
  
8.  Cliquez sur le bouton **Tester la connexion** pour valider la configuration de la source de données.  
  
    > [!NOTE]  
    >  Le bouton Tester la connexion n'est pas pris en charge pour le type de source de données XML.  
  
9. Cliquez sur **OK** pour créer la source de données partagée.  
  
 **Pour supprimer un fichier de source de données partagée (.rsds)**  
  
1.  Ouvrez la bibliothèque qui contient le fichier .rsds.  
  
2.  Pointez sur la source de données partagée.  
  
3.  Cliquez pour afficher une flèche orientée vers le bas et cliquez sur **Supprimer**.  
  
 Si vous supprimez par erreur une source de données partagée à conserver, vous pouvez en créer une autre qui contient les mêmes informations de connexion. Après avoir recréé la source de données partagée, ouvrez chaque rapport et modèle ayant utilisé cette source de données, puis sélectionnez la source de données partagée. Le nom, les informations d'identification ou la syntaxe de la chaîne de connexion du nouvel élément de source de données partagée peuvent être différents de ceux que vous supprimez. Tant que la connexion correspond à la même source de données, les propriétés de la source de données peuvent différer des valeurs d'origine.  
  
 Soyez vigilant lorsque vous supprimez un modèle de rapport. Si vous supprimez un modèle, vous ne pouvez plus ouvrir et modifier les rapports qui en dépendent dans le Générateur de rapports. Si vous supprimez par inadvertance un modèle utilisé par des rapports existants, vous devez générer de nouveau le modèle, recréer et enregistrer tous les rapports qui utilisent ce modèle, puis spécifier de nouveau la sécurité des éléments de modèle à utiliser. Vous ne pouvez pas simplement générer de nouveau le modèle, puis l'associer à un rapport existant.  
  
## <a name="dependent-items"></a>Éléments dépendants  
 Pour afficher la liste des rapports et modèles qui utilisent la source de données, ouvrez la page Éléments dépendants de la source de données partagée. Vous pouvez accéder à cette page lorsque vous ouvrez la source de données dans le Gestionnaire de rapports ou dans une page d'application SharePoint. Notez que la page Éléments dépendants n'indique pas les abonnements pilotés par les données. Si une source de données partagée est utilisée par un abonnement, celui-ci ne figurera pas dans la liste Éléments dépendants.  
  
 **Pour afficher les éléments dépendants dans SharePoint**  
  
1.  Ouvrez la bibliothèque qui contient le fichier .rsds.  
  
2.  Pointez sur la source de données partagée.  
  
3.  Cliquez pour afficher une flèche orientée vers le bas, puis sélectionnez **Afficher les éléments dépendants**.  
  
     Pour les modèles de rapports, la liste des éléments dépendants affiche les rapports créés dans le Générateur de rapports. Pour les sources de données partagées, la liste des éléments dépendants peut inclure à la fois les rapports et les modèles de rapports.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et gérer des sources de données partagées &#40;Reporting Services en mode intégré SharePoint&#41;](http://msdn.microsoft.com/library/2d3428e4-a810-4e66-a287-ff18e57fad76)   
 [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Gérer des sources de données de rapports](../../reporting-services/report-data/manage-report-data-sources.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Connexions de données ou sources de données incorporées et partagées &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/f417782c-b85a-4c4d-8a40-839176daba56)   
 [Page des propriétés des sources de données &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/f37edda0-19e6-489e-b544-8751fa6b6cfb)   
 [Créer, supprimer ou modifier une source de données partagée &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/cd7bace3-f8ec-4ee3-8a9f-2f217cdca9f2)   
 [Configurer les propriétés de la source de données d’un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-data/configure-data-source-properties-for-a-report-report-manager.md)  
  
  
