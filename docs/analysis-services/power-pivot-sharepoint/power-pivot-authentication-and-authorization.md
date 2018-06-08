---
title: Power Pivot Authentication and Authorization | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2ebb63d1ad381982ab4313ee6d980c5a919d6122
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-pivot-authentication-and-authorization"></a>Authentification et autorisation PowerPivot
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un déploiement de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint qui s’exécute dans une batterie de serveurs SharePoint 2010 utilise le sous-système d’authentification et le modèle d’autorisation fournis par les serveurs SharePoint. L’infrastructure de sécurité SharePoint s’étend au contenu et aux opérations [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , car l’ensemble du contenu relatif à [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]est stocké dans des bases de données de contenu SharePoint, et l’ensemble des opérations relatives à [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]est effectué par des services partagés [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie de serveurs. Les utilisateurs qui demandent un classeur contenant des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont authentifiés à l’aide d’une identité d’utilisateur SharePoint basée sur leur identité d’utilisateur Windows. Les autorisations d'affichage sur le classeur déterminent si la demande est accordée ou refusée.  
  
 Étant donné que l’intégration à Excel Services est nécessaire pour les analyses de données libre-service, pour sécuriser un serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] vous devez également bien comprendre le fonctionnement de la sécurité dans Excel Services. Quand un utilisateur interroge un tableau croisé dynamique comportant une connexion de données à des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , Excel Services transmet une demande de connexion de données à un serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie pour charger les données. Du fait de cette interaction entre les serveurs, il est indispensable que vous compreniez comment configurer des paramètres de sécurité pour les deux serveurs.  
  
 Cliquez sur les liens suivants pour accéder à des sections spécifiques de cette rubrique :  
  
 [Authentification Windows à l'aide de la spécification de connexion en mode classique](../../analysis-services/power-pivot-sharepoint/power-pivot-authentication-and-authorization.md#bkmk_auth)  
  
 [Opérations PowerPivot nécessitant une autorisation de l’utilisateur](#UserConnections)  
  
 [Autorisations SharePoint pour l’accès aux données PowerPivot](#Permissions)  
  
 [Considérations relatives à la sécurité Excel Services pour les classeurs PowerPivot](#excel)  
  
##  <a name="bkmk_auth"></a> Authentification Windows à l'aide de la spécification de connexion en mode classique  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint prend en charge un ensemble réduit d’options d’authentification disponibles dans SharePoint. Parmi les options d’authentification disponibles, seule l’authentification Windows est prise en charge pour un déploiement de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. En outre, l'application Web via laquelle la connexion se produit doit être configurée pour le mode classique.  
  
 L’authentification Windows est requise car le moteur de données Analysis Services dans un déploiement [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint prend uniquement en charge l’authentification Windows. Excel Services établit les connexions à Analysis Services via le fournisseur OLE DB MSOLAP à l'aide d'une identité d'utilisateur Windows qui a été authentifiée via NTLM ou du protocole Kerberos.  
  
 La deuxième spécification, l’authentification en mode classique sur l’application web, est nécessaire pour garantir l’opérabilité du service web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Le service web est un composant qui s’exécute sur un serveur web frontal et fournit la redirection HTTP vers un serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint dans la batterie. Tandis que le service web prend en charge les revendications pour les communications entre services, il ne les prend pas en charge pour les demandes de connexion de données qu’il achemine vers un service partagé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie de serveurs. Les demandes de chargement de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont prises en charge uniquement pour les connexions authentifiées émanant d’IIS à l’aide d’une identité Windows. La connexion en mode classique sur l’application web permet une connexion réussie du service web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] aux services partagés [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie.  
  
 Bien que la connexion en mode classique ne soit pas obligatoire pour le scénario d’accès aux données le plus courant (où les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont extraites du même classeur Excel qui les restitue), n’essayez pas d’utiliser [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint avec des applications web SharePoint configurées pour utiliser d’autres fournisseurs d’authentification. Cette action entraîne un échec de connexion chaque fois que les utilisateurs essaieront de se connecter aux classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] en tant que source de données externe.  
  
 Sans connexion en mode classique, les types suivants de demandes traitées par le service web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] échouent :  
  
-   Toute demande de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] provenant de l’extérieur de la batterie (par exemple, quand vous créez un rapport dans le Concepteur de rapports ou le Générateur de rapports, où la source de données est une URL SharePoint d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ).  
  
-   Les demandes dans la batterie d’une application cliente ou d’un rapport qui utilise le classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] comme source de données externe (par exemple, quand vous créez un classeur dans l’application de bureau Excel, en utilisant comme source de données un deuxième classeur Excel publié contenant des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ).  
  
### <a name="how-to-check-the-authentication-provider-for-your-application"></a>Comment vérifier le fournisseur d'authentification pour votre application  
 Lors de la création d'applications Web, veillez à sélectionner l'option **Authentification en mode classique** dans la page Créer une application Web.  
  
 Pour les applications Web existantes, utilisez les instructions suivantes pour vérifier si l'application Web est configurée pour utiliser l'authentification Windows.  
  
1.  Dans Administration Centrale, sous Gestion des applications, cliquez sur **Gérer les applications Web**.  
  
2.  Sélectionnez l'application Web.  
  
3.  Cliquez sur **Fournisseurs d'authentification**.  
  
4.  Vérifiez que vous avez un fournisseur pour chaque zone et que la zone par défaut a la valeur Windows.  
  
##  <a name="UserConnections"></a> Opérations PowerPivot nécessitant une autorisation de l’utilisateur  
 L’autorisation SharePoint est utilisée exclusivement pour tous les niveaux d’accès au traitement de données et de requêtes [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Le modèle d'autorisation basé sur les rôles Analysis Services n'est pas pris en charge. Il n’y a aucune autorisation basée sur les rôles pour des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] au niveau de la cellule, de la ligne ou de la table. Vous ne pouvez pas sécuriser des parties différentes du classeur pour accorder ou refuser à des utilisateurs spécifiques l'accès à des données sensibles qu'il contient. Les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] incorporées sont entièrement disponibles pour les utilisateurs disposant d’autorisations d’affichage sur le classeur Excel dans une bibliothèque SharePoint.  
  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint emprunte l’identité d’un utilisateur SharePoint dans les cas suivants :  
  
-   Requêtes adressées à des tableaux croisés dynamiques ou graphiques croisés dynamiques qui ont des connexions de données à une base de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , où une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] établit pour le compte d’un utilisateur des connexions à une instance spécifique du service partagé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui traite les données.  
  
-   Chargement de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] à partir du cache ou d’une bibliothèque si les données ne sont pas disponibles autrement. Si une demande de connexion de données concerne des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui ne sont pas encore chargées dans le système, l’instance [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] utilise l’identité de l’utilisateur SharePoint pour récupérer la source de données dans une bibliothèque de contenu et la charger en mémoire.  
  
-   Opérations d'actualisation des données qui enregistrent une copie mise à jour de la source de données dans le classeur d'une bibliothèque de contenu. Dans ce cas, un journal réel de l'opération est créé avec le nom d'utilisateur et le mot de passe récupérés d'une application cible dans le service Banque d'informations sécurisé. Les informations d’identification peuvent correspondre au compte d’actualisation des données sans assistance [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , mais aussi aux informations d’identification stockées avec la planification de l’actualisation des données lors de sa création. Pour plus d’informations, consultez [Configurer les informations d’identification stockées pour l’actualisation des données Power Pivot (Power Pivot pour SharePoint)](http://msdn.microsoft.com/en-us/987eff0f-bcfe-4bbd-81e0-9aca993a2a75) et [Configurer le compte d’actualisation des données PowerPivot sans assistance (PowerPivot pour SharePoint)](http://msdn.microsoft.com/en-us/81401eac-c619-4fad-ad3e-599e7a6f8493).  
  
##  <a name="Permissions"></a> Autorisations SharePoint pour l’accès aux données PowerPivot  
 La publication, la gestion et la sécurisation d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont prises en charge uniquement via l’intégration SharePoint. Les serveurs SharePoint fournissent des sous-systèmes d'authentification et d'autorisation qui garantissent un accès légitime aux données. Il n’existe pas de scénarios pris en charge pour le déploiement sûr d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] en dehors d’une batterie de serveurs SharePoint.  
  
 L’accès utilisateur aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est en lecture seule sur le serveur par le biais des autorisations d’affichage ou supérieures. Les autorisations de collaboration permettent d'ajouter et de modifier le fichier. Les modifications apportées aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] requièrent le téléchargement du classeur dans une application bureautique Excel pour laquelle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel est installé. Les autorisations de collaboration sur le fichier déterminent si l'utilisateur peut télécharger le fichier localement, puis enregistrer les modifications dans SharePoint.  
  
 Ainsi, les niveaux d’autorisation Collaboration et Vue seule définissent le jeu d’autorisations réel pour l’accès utilisateur aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . D'autres niveaux d'autorisation fonctionnent dans la mesure où ils présentent les mêmes autorisations que Collaboration et Vue seule (par exemple, puisque Lecture inclut les autorisations Vue seule, un utilisateur auquel ont été octroyées les autorisations Lecture a le même niveau d'accès qu'avec Vue seule).  
  
 Le tableau suivant résume les niveaux d’autorisation qui déterminent l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et aux opérations de serveur :  
  
|Niveau d'autorisation|Autorise les tâches suivantes|  
|----------------------|------------------------|  
|Administrateur de batteries de serveurs ou de services|Installation, activation et configuration de services et d'applications.<br /><br /> Utilisation du tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et affichage des rapports d’administration.|  
|Contrôle total|Activation de l’intégration des fonctionnalités [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] au niveau de la collection de sites.<br /><br /> Création d’une bibliothèque [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .<br /><br /> Création d'une bibliothèque de flux de données.|  
|Collaboration|Ajout, modification, suppression et téléchargement de classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .<br /><br /> Configuration de l'actualisation des données.<br /><br /> Création de classeurs et rapports basés sur des classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur un site SharePoint.<br /><br /> Création de documents de service de données dans une bibliothèque de flux de données|  
|Lecture|Accès à des classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] comme source de données externe, où l’URL du classeur est explicitement entrée dans une boîte de dialogue de connexion (par exemple, dans l’Assistant Connexion de données Excel).|  
|Vue seule|Affichage de classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .<br /><br /> Affichage de l'historique d'actualisation des données.<br /><br /> Connexion d’un classeur local à un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur un site SharePoint pour réutiliser ses données d’une autre façon.<br /><br /> Téléchargement d'un instantané du classeur. L'instantané est une copie statique des données, sans segments, filtres, formules ou connexions de données. Le contenu de l'instantané est similaire à la copie de valeurs de cellules de la fenêtre de navigateur.|  
  
##  <a name="excel"></a> Considérations relatives à la sécurité Excel Services pour les classeurs PowerPivot  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Le traitement des requêtes côté serveur est étroitement lié à Excel Services. L’intégration de produit commence au niveau du document, du fait que les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont des fichiers de classeur Excel (.xlsx) qui contiennent ou font référence à des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Il n’existe pas d’extension de fichier distincte pour un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Quand un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est ouvert sur un site SharePoint, Excel Services lit la chaîne de connexion des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] intégrée et transmet la requête au fournisseur OLE DB Analysis Services SQL Server local. Le fournisseur transmet ensuite les informations de connexion à un serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie. Pour que les requêtes transitent de façon transparente entre les deux serveurs, Excel Services doit être configuré de façon à utiliser les paramètres requis par [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.  
  
 Dans Excel Services, les paramètres de configuration de sécurité sont spécifiés sur des emplacements, des fournisseurs de données approuvés et des bibliothèques de connexions de données approuvés. Le tableau suivant décrit les paramètres qui activent ou améliorent l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Si un paramètre n’apparaît pas ici, c’est qu’il n’a aucun effet sur les connexions de serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Pour obtenir des instructions sur la façon de spécifier ces paramètres étape par étape, consultez la section « Activer Excel Services » dans [Configuration initiale (Power Pivot pour SharePoint)](http://msdn.microsoft.com/en-us/3a0ec2eb-017a-40db-b8d4-8aa8f4cdc146).  
  
> [!NOTE]  
>  La plupart des paramètres relatifs à la sécurité s'appliquent à des emplacements approuvés. Si vous voulez conserver les valeurs par défaut ou utiliser des valeurs différentes pour des sites différents, vous pouvez créer un emplacement approuvé supplémentaire pour les sites qui contiennent les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , puis configurer les paramètres suivants uniquement pour ce site. Pour plus d’informations, voir [Create a trusted location for Power Pivot sites in Central Administration](../../analysis-services/power-pivot-sharepoint/create-a-trusted-location-for-power-pivot-sites-in-central-administration.md).  
  
|Domaine|Paramètre| Description|  
|----------|-------------|-----------------|  
|application Web|Fournisseur d'authentification Windows|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] convertit un jeton de revendications qu’il obtient d’Excel Services en une identité d’utilisateur Windows. Toutes les applications Web qui utilisent Excel Services comme ressource doivent être configurées pour utiliser le fournisseur d'authentification Windows.|  
|Emplacement approuvé|Type d'emplacement|Cette valeur doit être paramétrée sur **Microsoft SharePoint Foundation**. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] récupèrent une copie du fichier .xlsx et la chargent sur un serveur Analysis Services dans la batterie. Le serveur ne peut récupérer que des fichiers .xlsx d'une bibliothèque de contenu.|  
||Autoriser les données externes|Ce paramètre doit avoir la valeur **Bibliothèques de connexions de données approuvées et incorporées**. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont incorporées au classeur. Si vous interdisez les connexions incorporées, les utilisateurs peuvent consulter le cache [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , mais ne sont pas en mesure d’interagir avec les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .|  
||Avertir lors de l'actualisation|Cette valeur doit être désactivée si vous utilisez la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour stocker des classeurs et des rapports. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] inclut une fonctionnalité d’aperçu des documents qui fonctionne mieux si les paramètres Actualisation à l’ouverture et Avertir lors de l’actualisation sont désactivés.|  
|Fournisseurs de données approuvés|MSOLAP.4<br /><br /> MSOLAP.5|MSOLAP.4 est inclus par défaut, mais l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] requiert que le fournisseur MSOLAP.4 soit la version SQL Server 2008 R2.<br /><br /> MSOLAP.5 est installé avec la version [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.<br /><br /> Ne supprimez pas ces fournisseurs de la liste des fournisseurs de données approuvés. Dans certains cas, vous devrez peut-être installer des copies supplémentaires de ce fournisseur sur d'autres serveurs SharePoint dans votre batterie de serveurs. Pour plus d’informations, voir [Install the Analysis Services OLE DB Provider on SharePoint Servers](http://msdn.microsoft.com/en-us/2c62daf9-1f2d-4508-a497-af62360ee859)(Installer le fournisseur OLE DB Analysis Services sur les serveurs SharePoint).|  
|Bibliothèques de connexions de données approuvées|Ce paramètre est facultatif.|Vous pouvez utiliser des fichiers Office Data Connection (.odc) dans des classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Si vous utilisez des fichiers .odc pour fournir les informations de connexion aux classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] locaux, vous pouvez ajouter les mêmes fichiers .odc à cette bibliothèque.|  
|Assembly de fonction défini par l'utilisateur|Non applicable.|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint ignore les assemblys de fonction définis par l’utilisateur que vous générez pour Excel Services. Si vous comptez sur les assemblys définis par l’utilisateur pour un comportement spécifique, sachez que le traitement des requêtes [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’utilise pas les fonctions définies par l’utilisateur que vous avez créées.|  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration des comptes de service Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md)   
 [Configurer le Power Pivot (Power Pivot pour SharePoint) de compte d’actualisation des données sans assistance](http://msdn.microsoft.com/en-us/81401eac-c619-4fad-ad3e-599e7a6f8493)   
 [Create a trusted location for Power Pivot sites in Central Administration](../../analysis-services/power-pivot-sharepoint/create-a-trusted-location-for-power-pivot-sites-in-central-administration.md)   
 [Architecture de sécurité PowerPivot](http://go.microsoft.com/fwlink/?linkID=220970)  
  
  
