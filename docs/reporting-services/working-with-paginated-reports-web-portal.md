---
title: Utilisation des rapports paginés (portail web) | Microsoft Docs
ms.custom: ''
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: fb0bc38f-dc56-4350-8457-cd135c0346e1
caps.latest.revision: 6
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: e7036050c715f1c53275cdad4cd2f3069d7b6f8b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-paginated-reports-web-portal"></a>Utilisation des rapports paginés (portail web)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)])

Vous pouvez afficher et gérer les propriétés d’un rapport paginé dans le portail web. À partir du portail web, vous pouvez lancer le Générateur de rapports pour créer ou modifier des rapports paginés.  
   
## <a name="create-a-paginated-report"></a>Créer un rapport paginé  
  
Procédez comme suit pour créer un dataset partagé.  
  
1.  Sélectionnez Nouveau dans la barre de menus.  
  
2.  Sélectionnez **Rapport paginé**.  
  
    ![ssRSWebPortal-new-report](../reporting-services/media/ssrswebportal-new-report.png)  
  
3.  L’opération lance alors le Générateur de rapports, ou vous êtes invité à le télécharger.  
  
4.  Créez votre rapport, puis sélectionnez l’icône **Enregistrer** en haut à gauche pour enregistrer le rapport paginé dans le serveur de rapports.  
  
## <a name="manage-an-existing-paginated-report"></a>Gérer un rapport paginé existant  
  
Procédez comme suit pour gérer un rapport paginé existant.  
  
> [!NOTE]
> Si vous ne voyez pas de rapports paginés dans le dossier, vérifiez que vous affichez bien les rapports paginés. Vous pouvez sélectionner **Afficher** dans la barre de menus dans le coin supérieur droit du portail web. Vérifiez que **Rapports paginés** est coché.  
  
1.  Sélectionnez les **points de suspension (...)** correspondant au dataset que vous souhaitez gérer.  
      
    ![ssRSWebPortal-manage-report1](../reporting-services/media/ssrswebportal-manage-report1.png)  
  
2.  Sélectionnez **Gérer** . L’écran de modification s’affiche alors.  
    
    ![ssRSWebPortal-manage-report2](../reporting-services/media/ssrswebportal-manage-report2.png)  
  
## <a name="properties"></a>Propriétés  
  
Dans la fenêtre des propriétés, vous pouvez modifier le **nom** et la **description** du rapport paginé. Vous pouvez également utiliser **Supprimer**, **Déplacer**, **Créer un rapport lié**, **Modifier dans le Générateur de rapports**, **Télécharger** ou **Remplacer**.  
    
![ssRSWebPortal-report-properties](../reporting-services/media/ssrswebportal-report-properties.png)  
   
## <a name="parameters"></a>Paramètres  
  
Vous pouvez modifier les paramètres existants d’un rapport paginé. Pour ajouter un nouveau paramètre, vous devez modifier le rapport dans le Générateur de rapports ou dans SQL Server Data Tools.  
  
![ssRSWebPortal-report-parameters](../reporting-services/media/ssrswebportal-report-parameters.png)  
   
## <a name="data-source"></a>Source de données  
Vous pouvez pointer sur une source de données partagée, ou entrer les informations de connexion pour une source de données personnalisée.  
  
![ssRSWebPortal-report-datasource](../reporting-services/media/ssrswebportal-report-datasource.png)  
  
Les options suivantes sont utilisées pour spécifier une connexion à une source de données personnalisée.  
  
**Type**  
  
Spécifiez une extension pour le traitement des données utilisée pour traiter les données de la source de données. Pour obtenir une liste des extensions de données intégrées, consultez [Sources de données prises en charge par Reporting Services (SSRS)]. Des extensions supplémentaires peuvent être disponibles auprès d'éditeurs tiers.  
  
**Chaîne de connexion**  
  
Spécifiez la chaîne de connexion utilisée par le serveur de rapports pour se connecter à la source de données. Le type de connexion détermine la syntaxe à utiliser. Par exemple, une chaîne de connexion pour une extension pour le traitement des données XML est une URL vers un document XML. Dans la plupart des cas, une chaîne de connexion type spécifie le serveur de bases de données et un fichier de données. L’exemple suivant montre une chaîne de connexion utilisée pour se connecter à une base de données SQL Server nommée MyData :  
  
    data source=(a SQL Server instance);initial catalog=MyData  
  
Une chaîne de connexion peut être configurée sous la forme d'une expression pour vous permettre de spécifier la source de données lors de l'exécution. Les expressions de sources de données sont définies dans le rapport du Concepteur de rapports. Les expressions de sources de données ne peuvent pas être définies, visualisées ou modifiées dans le portail web. Vous pouvez cependant remplacer une expression de source de données en cliquant sur **Remplacer l’option par défaut** pour entrer une chaîne de connexion statique. Pour revenir à l’expression, cliquez sur **Revenir à l’option par défaut**. Le serveur de rapports enregistre la chaîne de connexion initiale au cas où vous devriez la restaurer. Pour utiliser les expressions de sources de données, vous devez employer les informations de connexion à la source de données publiées à l'origine dans le rapport. Les sources de données partagées ne prennent pas en charge l'utilisation d'expressions dans la chaîne de connexion.  
  
**Informations d'identification**  
  
Vous pouvez spécifier l’option qui détermine la façon dont les informations d’identification sont obtenues.  
  
> [!IMPORTANT]
> Si les informations d'identification sont fournies dans la chaîne de connexion, les options et les valeurs indiquées dans cette section sont ignorées. Notez que si vous spécifiez les informations d'identification sur la chaîne de connexion, les valeurs sont affichées en texte en clair et sont visibles par tous les utilisateurs qui affichent cette page.  
  
**En tant qu’utilisateur affichant le rapport**  
  
Utilise les informations d'identification Windows de l'utilisateur actuel pour accéder à la source de données. Sélectionnez cette option lorsque les informations d'identification utilisées pour accéder à la source de données sont identiques à celles utilisées pour ouvrir une session sur le domaine réseau. Cette option convient mieux lorsque l'authentification Kerberos est activée pour votre domaine ou que la source de données se trouve sur le même ordinateur que le serveur de rapports. Si Kerberos n’est pas activé, les informations d’identification Windows ne peuvent pas être passées à un autre service ou à un ordinateur distant. Si des connexions supplémentaires à des ordinateurs sont requises, vous obtiendrez une erreur à la place des données attendues.  
  
Un administrateur du serveur de rapports peut désactiver l'utilisation de la sécurité intégrée de Windows pour accéder aux sources de données de rapports. Si cette valeur est grisée, la fonctionnalité n'est pas disponible.  
  
N'utilisez pas cette option si vous projetez de planifier ce rapport ou de vous y abonner. Le traitement de rapports sans assistance ou planifié requiert des informations d'identification qui peuvent être obtenues sans entrée d'utilisateur ou le contexte de sécurité d'un utilisateur actuel. Seules les informations d'identification stockées fournissent cette fonction. Pour cette raison, le serveur de rapports vous empêche de planifier le traitement d'un rapport ou d'un abonnement si le rapport est configuré pour le type d'informations d'identification de sécurité intégrée de Windows. Si vous choisissez cette option pour un rapport qui fait déjà l'objet d'un abonnement ou pour lequel des opérations sont planifiées, les abonnements et opérations planifiées s'arrêteront.  
  
**À l’aide de ces informations d’identification**  
  
Stocke un nom d'utilisateur et un mot de passe chiffrés dans la base de données du serveur de rapports. Sélectionnez cette option pour exécuter un rapport sans assistance (par exemple, des rapports qui sont démarrés par des planifications ou des événements au lieu des actions utilisateur).   
  
Vous pouvez également choisir de quel type d’informations d’identification il s’agirait. L’authentification Windows (nom d’utilisateur et mot de passe Windows) ou des informations d’identification d’une base de données spécifique (nom d’utilisateur et mot de passe de la base de données), comme l’authentification SQL.  
  
Si le compte correspond à des informations d’identification Windows, le compte que vous spécifiez doit disposer d’autorisations d’ouverture d’une session locale sur l’ordinateur qui héberge la source de données utilisée par le rapport.  
  
Sélectionnez **Se connecter avec ces informations d’identification, mais essayer ensuite d’emprunter l’identité de l’utilisateur qui affiche le rapport** pour autoriser la délégation des informations d’identification, mais seulement si la source de données prend en charge l’emprunt d’identité. Pour les bases de données SQL Server, cette option définit la fonction SETUSER. Pour Analysis Services, EffectiveUserName est utilisé.  
  
**En invitant l’utilisateur qui affiche le rapport à fournir ses informations d’identification**  
  
Chaque utilisateur doit fournir un nom d'utilisateur et un mot de passe pour accéder à la source de données. Vous pouvez définir le texte de l'invite qui demande les informations d'identification aux utilisateurs. Par exemple, « Entrez un nom d’utilisateur et un mot de passe pour accéder à la source de données ».  
  
Vous pouvez également choisir de quel type d’informations d’identification il s’agirait. L’authentification Windows (nom d’utilisateur et mot de passe Windows) ou des informations d’identification d’une base de données spécifique (nom d’utilisateur et mot de passe de la base de données), comme l’authentification SQL.  
  
**Sans informations d’identification**  
  
Ceci vous permet de ne pas fournir d’informations d’identification pour la source de données. Si la source de données nécessite une ouverture de session utilisateur, cette option est sans effet. Vous devez choisir cette option uniquement si la connexion de source de données ne nécessite pas d'informations d'identification des utilisateurs.  
  
Pour utiliser cette option, vous devez avoir précédemment configuré le compte d’exécution sans assistance pour votre serveur de rapports. Le compte d’exécution sans assistance est utilisé pour se connecter à des sources de données externes quand d’autres sources d’informations d’identification ne sont pas disponibles. Si vous spécifiez cette option et que le compte n'est pas configuré, la connexion à la source de données de rapports échouera et le traitement de rapports ne se produira pas. Pour plus d’informations sur ce compte, consultez [Configurer le compte d’exécution sans assistance (Gestionnaire de configuration de SSRS)](../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
## <a name="subscriptions"></a>Abonnements  
Un abonnement Reporting Services est une configuration qui remet un rapport à une heure donnée ou en réponse à un événement, et dans un format de fichier que vous spécifiez. Par exemple, tous les mercredis, enregistrer le rapport MonthlySales.rdl au format de document Microsoft Word sur un partage de fichiers. Vous pouvez utiliser des abonnements pour planifier et automatiser la remise d'un rapport avec un ensemble de valeurs de paramètres de rapport spécifique. Pour plus d’informations, consultez [Utilisation des abonnements](working-with-subscriptions-web-portal.md).
  
![ssRSWebPortal-report-subscription1](../reporting-services/media/ssrswebportal-report-subscription1.png)
   
## <a name="dependent-items"></a>Éléments dépendants  
Utilisez la page Éléments dépendants pour afficher une liste d’éléments qui font référence à ce rapport. L’icône de chaque type d’élément indique ce qu’il est. Vous pouvez ensuite sélectionner les **points de suspension (...)** sur chaque élément pour gérer plus avant ces éléments.  
  
## <a name="caching"></a>Mise en cache  
Vous disposez de différentes options de mise en cache des données pour un rapport paginé. Vous pouvez commencer avec une simple sélection.  
  
1.  **Toujours exécuter ce rapport avec les données les plus récentes** interroge la source de données chaque fois que vous exécutez le rapport. Ceci aboutit à un rapport à la demande qui contient les données les plus récentes. Une nouvelle instance du rapport est créée à chaque fois que le rapport est ouvert, qui contient les résultats d’une nouvelle requête. Avec cette méthode, si dix utilisateurs ouvrent le rapport en même temps, dix requêtes sont envoyées à la source de données en vue d'un traitement.  
  
2.  **Mettre en cache des copies de ce rapport et les utiliser en cas de disponibilité** place une copie temporaire des données dans un cache pour une utilisation ultérieure. La mise en cache améliore habituellement les performances, car les données sont retournées à partir du cache ; la requête de dataset n'est pas réexécutée. Avec cette approche, si dix utilisateurs ouvrent le rapport, seule la première demande génère une requête sur la source de données. Le rapport est ensuite placé dans la mémoire cache pour être affiché par les neuf autres utilisateurs.  
  
3.  **Toujours exécuter ce rapport sur les instantanés prégénérés** met en cache la présentation du rapport et les données pendant une période de temps donnée. Vous pouvez exécuter un rapport en tant qu'instantané de rapport afin d'éviter qu'il soit exécuté à des moments inopportuns (par exemple, pendant une sauvegarde programmée). L’instantané peut être actualisé selon une planification. [En savoir plus]  
  
![ssRSWebPortal-report-caching1](../reporting-services/media/ssrswebportal-report-caching1.png)  
   
Vous disposerez d’options supplémentaires en sélectionnant **Mettre en cache des copies de ce rapport et les utiliser en cas de disponibilité** .  
  
![ssRSWebPortal-report-caching2](../reporting-services/media/ssrswebportal-report-caching2.png)  

Pour plus d’informations, consultez [Utilisation des instantanés](working-with-snapshots-web-portal.md).
  
### <a name="cache-expiration"></a>Expiration du cache  
  
Vous pouvez déterminer si vous voulez faire expirer le cache pour le rapport paginé après une certaine quantité de temps ou si vous préférez le faire selon une planification. Vous pouvez utiliser une planification partagée.  
  
> [!NOTE]
> Cela n’actualise pas le cache.  
  
### <a name="cache-refresh-plans"></a>Plans d'actualisation du cache  
  
Vous pouvez utiliser des plans d’actualisation du cache pour créer des planifications de préchargement de copies temporaires des données pour un rapport paginé dans le cache. Un plan d'actualisation inclut une planification et la possibilité de spécifier ou de remplacer des valeurs pour les paramètres. Vous ne pouvez pas remplacer les valeurs des paramètres marqués en lecture seule. Vous pouvez créer et utiliser plusieurs plans d'actualisation.  
   
Les affectations de rôles par défaut qui vous permettent d’ajouter, de supprimer et de modifier des rapports paginés pour des plans d’actualisation du cache sont les suivants : Gestionnaire de contenu, Mes Rapports et Serveur de publication.  
  
Une fois l’option de cache activée ci-dessus, vous pouvez ensuite définir un plan d’actualisation du cache. Pour cela, sélectionnez le lien **Gérer les plans d’actualisation** qui apparaît une fois que vous appliquez les paramètres de cache. Vous serez alors dirigé vers la page de planification d’actualisation du cache.   
  
Pour créer un plan d’actualisation du cache, cliquez sur **Nouveau plan d'actualisation du cache**. Vous pouvez ensuite entrer un nom pour le plan et spécifier une planification. Si le dataset comporte des paramètres définis, ils seront répertoriés et vous serez en mesure de leur attribuer des valeurs, sauf s’ils sont marqués en lecture seule.  
  
Une fois que vous avez terminé, vous pouvez sélectionner **Créer un plan d’actualisation du Cache**.  
  
![ssRSWebPortal-report-caching3](../reporting-services/media/ssrswebportal-report-caching3.png)  
  
> [!NOTE]
> L’agent SQL Server doit être en cours d’exécution pour créer un plan d’actualisation du cache.  
  
Vous pouvez ensuite **Modifier** ou **Supprimer** des plans répertoriés. L’option **Créer à partir d'un élément existant** est activée lorsqu'un seul et unique plan d'actualisation du cache est sélectionné. Cette option crée un nouveau plan d'actualisation copié à partir du plan d'origine. La page du plan d'actualisation du cache qui s'ouvre contient les détails du plan sélectionné. Vous pouvez alors modifier les options du plan d'actualisation et enregistrer ce dernier en utilisant une nouvelle description.  
  
## <a name="history-snapshots"></a>Instantanés d’historique  
  
Utilisez la page Instantanés d’historique pour afficher des instantanés de rapport qui sont générés et stockés dans le temps. Selon les options définies, l’historique de rapport peut contenir seulement les instantanés les plus récents.  
  
Un historique de rapport est toujours affiché dans le contexte du rapport dont il est issu. Vous ne pouvez pas afficher l'historique de tous les rapports sur un serveur de rapports dans un seul emplacement.  
  
Pour générer un instantané, le rapport doit pouvoir s’exécuter sans assistance (c’est-à-dire qu’il doit utiliser les informations d’identification stockées ; les rapports paramétrables doivent contenir des valeurs de paramètres par défaut pour tous les paramètres). Les instantanés peuvent être générés manuellement ou comme opération planifiée.   
  
Vous pouvez cliquer sur un instantané d'historique de rapport pour l'afficher. Les instantanés qui apparaissent dans l'historique de rapport ne sont différenciés que par la date et l'heure de leur création. Il n'y a aucun moyen de déterminer si un instantané a été généré par une opération manuelle ou planifiée.  
  
## <a name="security"></a>Sécurité  
Utilisez la page Propriétés de sécurité pour afficher ou modifier les paramètres de sécurité qui déterminent l’accès au rapport. Cette page est disponible pour les éléments que vous êtes autorisé à sécuriser.  
  
L'accès aux éléments est défini par l'intermédiaire des attributions de rôle qui spécifient les tâches que peuvent effectuer un groupe ou un utilisateur. Une attribution de rôle est composée d'un nom d'utilisateur ou de groupe et d'une ou plusieurs définitions de rôles qui spécifient une collection de tâches.  
  
**Modifier la sécurité de l'élément**  
  
Sélectionnez cet élément pour modifier la façon dont la sécurité est définie pour l’élément actif.

## <a name="next-steps"></a>Étapes suivantes

[Portail Web](../reporting-services/web-portal-ssrs-native-mode.md)  
[Utiliser des datasets partagés](../reporting-services/work-with-shared-datasets-web-portal.md)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
