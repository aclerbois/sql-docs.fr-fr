---
title: Exemples d’URL pour les éléments sur un serveur de rapports (mode SharePoint) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 54cb861a-8cec-445c-875d-599fb9bd1973
caps.latest.revision: 5
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 42638cca55a7d567ebe3ec3bfad37880cca6aebe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="url-examples-for-items-on-a-report-server---sharepoint-mode"></a>Exemples d’URL pour les éléments sur un serveur de rapports (mode SharePoint)
  Pour publier des rapports et des éléments associés dans une bibliothèque SharePoint, vous pouvez soit publier le contenu à l’aide des outils de création de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] tels que le Concepteur de rapports, soit télécharger le contenu à l’aide des actions de site SharePoint.  
  
 Les sites SharePoint utilisent des adresses web différentes d’un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif. Une arborescence Web de site SharePoint inclut l'application Web SharePoint, un site de niveau supérieur, des sous-sites facultatifs et des bibliothèques. Vous devez savoir comment créer une adresse URL qui spécifie le serveur SharePoint et connaître l'emplacement dans l'arborescence de site SharePoint où publier un rapport ou des éléments connexes.  
  
 Les éléments connexes d'un rapport incluent les sources de données partagées, les sous-rapports, les rapports d'extraction et les ressources telles que les fichiers image Web. Un rapport qui été publié dans une bibliothèque SharePoint doit spécifier ces éléments connexes en indiquant leur emplacement dans la bibliothèque SharePoint.  
  
 Utilisez les exemples de cette rubrique pour créer des URL pour les rapports et les éléments associés dans vos solutions de rapports.  
  
## <a name="site-hierarchy"></a>Arborescence des sites  
 Lorsque vous configurez un serveur de rapports pour qu'il s'exécute en mode intégré SharePoint, l'arborescence web SharePoint est utilisée pour adresser les éléments qui sont traités et gérés sur un serveur de rapports.  
  
 Les éléments répertoriés ci-dessous de l'arborescence Web peuvent être utilisés pour accéder au contenu du serveur de rapports et pour sécuriser ce contenu. Les autres objets, tels que les listes et les pages, ne sont pas utilisés pour accéder au contenu du serveur de rapports et ne sont par conséquent pas décrits dans le tableau suivant.  
  
|Object|Description|  
|------------|-----------------|  
|Application web SharePoint|Une application web SharePoint peut être installée comme serveur autonome ou dans une batterie de serveurs contenant une collection de serveurs virtuels. Une application web possède une URL (par exemple, `http:*//servername*`) et peut contenir plusieurs sites.|  
|Site|Un site peut être un site parent d'une application web ou un sous-site.|  
|Bibliothèque SharePoint|Une bibliothèque contient des documents ou des dossiers. Une bibliothèque ou un dossier de bibliothèque représente le seul objet de site qui peut stocker des rapports, des modèles de rapport, des sources de données partagées et des images externes.|  
|Élément|Les éléments de serveur de rapports auxquels vous pouvez faire référence dans une URL incluent une définition de rapport pour un rapport ou un sous-rapport, un modèle de rapport, une source de données partagée ou une image externe.|  
  
## <a name="url-syntax-and-rules"></a>Syntaxe et règles des URL  
 Chaque élément de serveur de rapports d'une bibliothèque est identifié par une URL complète qui inclut le préfixe du protocole, le nom du serveur, le site, la bibliothèque, le nom de fichier et l'extension de nom de fichier pour le type de fichier.  
  
### <a name="url-for-a-sharepoint-server"></a>URL d'un serveur SharePoint  
 Vous devez utiliser une URL vers le serveur SharePoint lorsque vous déployez un projet de serveur de rapports ou de modèle de rapport dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] sur le serveur de rapports.  
  
 Pour trouver le nom du serveur à utiliser, ouvrez un navigateur et localisez la bibliothèque SharePoint où vous souhaitez publier un rapport. Le nom du serveur apparaît juste après le préfixe du protocole ; par exemple, `http:*//servername*`.  
  
 L'utilisation du point de terminaison du proxy URL [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n'est pas prise en charge. Un point de terminaison de proxy inclut un numéro de port ; par exemple, `http:*//servername:8080/reportserver*`.  
  
### <a name="url-for-a-sharepoint-server-site-or-subsite"></a>URL d'un site ou sous-site de serveur SharePoint  
 Lorsque vous déployez un rapport ou une source de données de rapport, vous devez utiliser une URL vers un site et un sous-site SharePoint, le cas échéant. Dans l’URL, le nom du site apparaît juste après le nom du serveur ; par exemple, `http://*servername/site*` ou `http://*servername/site/subsite*`.  
  
 Dans une application web [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007 ou [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] , le site et le sous-site correspondent le plus souvent aux onglets du site principal. Pour trouver le nom du site ou du sous-site, cliquez sur **Accueil**, puis sur **Tout le contenu du site**. Faites défiler vers le bas et recherchez **Sites et espaces de travail**. La liste des sites s'affiche dans cette section.  
  
### <a name="url-for-a-sharepoint-library"></a>URL d'une bibliothèque SharePoint  
 Si vous déployez un rapport ou un élément connexe vers une bibliothèque SharePoint, vous devez utiliser une URL vers la bibliothèque SharePoint. L'URL à utiliser pour une bibliothèque varie selon votre version de SharePoint.  
  
 Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] 3.0 ou [!INCLUDE[SPF2010](../../includes/spf2010-md.md)], la bibliothèque apparaît après le nom du serveur ; par exemple, `http://*servername/*Shared Documents`.  
  
 Dans [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007 ou [!INCLUDE[SPS2010](../../includes/sps2010-md.md)], la bibliothèque apparaît après le site et le sous-site. Par exemple, `http://*servername/site/*Documents`.  
  
 Pour rechercher les informations de chemin d'accès pour une nouvelle bibliothèque SharePoint ou pour un site inconnu, ouvrez un navigateur et localisez la bibliothèque SharePoint où vous souhaitez publier vos rapports. Si la bibliothèque est vide, téléchargez un fichier. Cliquez avec le bouton droit sur le fichier et sélectionnez **Propriétés** pour ouvrir la fenêtre **Propriétés** . L'adresse du fichier contient les valeurs URL nécessaires à une opération de publication.  
  
### <a name="fully-qualified-urls-for-items-on-a-sharepoint-site"></a>URL complètes des éléments sur un site SharePoint  
 Les éléments stockés dans une bibliothèque SharePoint sont toujours traités par le biais d’une URL complète qui commence par l’application web (`http://*server*`) comme nœud racine, et se termine par le nom du fichier auquel vous faites référence.  
  
 Les noms de fichiers dans l'URL doivent inclure une extension de nom de fichier.  
  
 Vous ne pouvez pas utiliser des URL relatives pour les éléments dépendants dans les rapports que vous publiez sur un site SharePoint. Par exemple, vous ne pouvez pas utiliser une URL relative pour faire référence à une source de données partagée, à un modèle de rapport ou à un sous-rapport. Vous devez toujours spécifier l'URL complète vers une bibliothèque SharePoint pour chaque élément. Il est impossible de prédire où un fichier dépendant peut se trouver, car il n'existe pas d'arborescence prédéfinie de sites que vous pouvez utiliser pour analyser un format d'URL.  
  
 Lorsque vous publiez ou téléchargez un rapport qui contient des éléments dépendants, vous devez définir les références aux éléments dépendants après avoir publié le rapport. Des références qui fonctionnaient correctement en mode aperçu dans le Concepteur de rapports ne fonctionneront pas nécessairement une fois le rapport publié. Pour plus d’informations, consultez [Publication dans une bibliothèque SharePoint à partir d’un outil de création](#publishingToDocLib) dans cette rubrique.  
  
### <a name="urls-for-external-images"></a>URL pour des images externes  
 Une définition de rapport peut inclure un fichier image stocké comme fichier externe. Vous pouvez faire référence à ce fichier dans la définition de rapport en définissant une URL complète vers le fichier image. Il peut être stocké sur un site SharePoint ou sur un ordinateur distant.  
  
> [!IMPORTANT]  
>  Si l'URL externe correspond à une image située sur un site SharePoint, l'icône d'image rompue apparaît lorsque vous affichez un aperçu du rapport dans le Générateur de rapports. Lorsque vous téléchargez le rapport sur le site SharePoint, puis affichez le rapport en mode connecté, l’icône d’image rompue apparaît si vous disposez seulement des autorisations **Afficher les éléments** .  
  
 Quel que soit le mode de serveur de rapports, les références à un fichier image externe au sein d'un rapport doivent être une URL complète. De plus, la référence à un fichier image externe nécessite généralement que vous configuriez le compte de traitement de rapport sans assistance.  
  
### <a name="specifying-subreports-and-drillthrough-reports"></a>Spécification de sous-rapports et de rapports d'extraction  
 Les sous-rapports doivent résider dans le même dossier que le rapport principal. Vous ne pouvez pas spécifier de dossier relatif.  
  
 Pour spécifier des rapports d'extraction, incluez l'URL dans une expression. Par exemple, pour spécifier le rapport nommé SalesDetails comme un rapport d'extraction, dans la zone Action de la zone de texte ou du texte d'espace réservé, définissez ReportName sur l'expression suivante :  
  
```  
="http://site/subsite/documentlibrary/SalesDetails.rdl"  
```  
  
### <a name="reserved-names-on-sharepoint-sites"></a>Noms réservés sur des sites SharePoint  
 Si vous créez ou construisez une URL pour un élément situé sur un site SharePoint, n’oubliez pas que les mots **Personnel** et **Sites** sont des noms réservés sous le site par défaut.  
  
## <a name="examples-of-urls"></a>Exemples d'URL  
 Lors de la publication d'éléments dans une bibliothèque SharePoint, il est important de spécifier des URL complètes vers la bibliothèque cible. Une URL SharePoint complète inclut l'application Web SharePoint, le site, la bibliothèque, le dossier (facultatif), le fichier et l'extension de nom de fichier. Les exemples ci-dessous illustrent la syntaxe à utiliser.  
  
|Cible|Exemple d'URL|  
|------------|-----------------|  
|Serveur SharePoint.|`http://TestServer`|  
|Site ou sous-site de serveur SharePoint.|`http://TestServer/toplevelsite/subsite`|  
|Exemple de rapport Company Sales dans le dossier **Documents partagés** d’un déploiement [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] ou [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] .|`http://TestServer/TestSite/Shared%20Documents/Company%20Sales.rdl`|  
|Exemple de rapport Company Sales dans le dossier **Documents/Doc** d’une instance [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] ou [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] .|`http://TestServer/TestSite/Documents/Doc/Company%20Sales.rdl`|  
|Exemple de rapport Company Sales dans le dossier **Report Center** d’une instance [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] ou [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] .|`http://TestServer/TestSite/Reports/Doc/Company%20Sales.rdl`|  
  
##  <a name="publishingToDocLib"></a> Publication dans une bibliothèque SharePoint à partir d’un outil de création  
 Lorsque vous utilisez un outil de création de rapports pour publier des rapports et des fichiers associés dans une bibliothèque, les fichiers sont validés avant d'être ajoutés. Si vous téléchargez les rapports et les fichiers associés via l’action **Télécharger** dans une bibliothèque SharePoint, aucune vérification de validation n’a lieu. Vous ne saurez pas si le fichier est valide tant que vous n'accéderez pas au rapport en le gérant, le modifiant ou l'exécutant.  
  
> [!NOTE]  
>  Pour publier des rapports sur un site SharePoint à partir de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], il peut s'avérer nécessaire d'ajouter le site SharePoint à votre liste d’emplacements approuvés dans le navigateur Internet Explorer.  
  
### <a name="shared-data-sources"></a>Sources de données partagées  
 Lorsque vous publiez une source de données partagée à l’aide d’un outil de création de rapports, vous devez définir la propriété de projet **TargetDataSourceFolder**. Le dossier source des données cibles doit être une URL vers une bibliothèque SharePoint. À la différence du mode natif [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous ne pouvez pas spécifier un dossier relatif ; les chemins d'accès relatifs ne sont pas valides. Un dossier dans le chemin d'accès de la bibliothèque de documents est créé s'il n'existe pas déjà.  
  
 Lorsque vous publiez un fichier de source de données partagée (.rds) sur un site SharePoint, cela modifie le fichier de source de données en lui attribuant l'extension de nom de fichier .rsds. Le fichier .rsds ne peut pas être enregistré localement à partir d’un site SharePoint et importé dans un projet [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] existant. Les sources de données partagées avec les extensions de nom de fichier .rds et .rsds ne sont pas interchangeables.  
  
#### <a name="shared-data-sources-from-report-designer"></a>Sources de données partagées du Concepteur de rapports  
 Si vous publiez des sources de données partagées à partir d'un projet Concepteur de rapports, vous pouvez utiliser une URL qui spécifie la bibliothèque cible ou vous pouvez laisser la propriété vide. À la différence du mode natif [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous ne pouvez pas spécifier un dossier relatif ; les chemins d'accès relatifs ne sont pas valides. Un dossier dans le chemin d'accès de la bibliothèque de documents est créé s'il n'existe pas déjà. Si vous laissez vide le dossier de la source de données cible, la source de données sera publiée dans le dossier de rapports cible.  
  
### <a name="file-names"></a>Noms de fichiers  
 Les noms de fichiers dans une URL pour des éléments de rapport doivent inclure une extension de nom de fichier. L'extension de nom de fichier détermine le type de fichier. Lorsque vous publiez des éléments de rapport à partir d'un outil de création de rapports, l'extension de nom de fichier est incluse automatiquement. Si vous téléchargez un élément de rapport vers une bibliothèque SharePoint, vous devez inclure une extension de nom de fichier.  
  
 Si vous ne spécifiez pas d’extension de nom de fichier pour les éléments que vous chargez sur un site SharePoint, l’erreur **rsInvalidDataSourceReference** se produit. Les noms de fichiers ne doivent pas inclure les caractères qui ne sont pas reconnus comme des caractères valides de nom de fichier par les applications SharePoint. N'incluez pas les caractères suivants : # % & * : < > ? / { | }.  
  
## <a name="differences-between-uploading-and-publishing"></a>Différences entre le téléchargement et la publication  
 Lorsque vous utilisez le Concepteur de rapports ou le Générateur de rapports pour publier des rapports et des fichiers associés dans une bibliothèque, les fichiers sont validés avant d'être ajoutés. Si vous téléchargez les rapports et les fichiers associés via l’action **Télécharger** dans une bibliothèque SharePoint, aucune vérification de validation n’a lieu. Vous ne saurez pas si le fichier est valide tant que vous n'accéderez pas au rapport en le gérant, le modifiant ou l'exécutant.  
  
## <a name="updating-a-published-item"></a>Mise à jour d'un élément publié  
 Après avoir publié ou téléchargé un élément dans une bibliothèque SharePoint, vous devez extraire l'élément de la bibliothèque avant de le mettre à jour. Lorsque le rapport est extrait pour vous, vous êtes le seul utilisateur autorisé à le modifier. Une fois que vous avez terminé, archivez-le de nouveau.  
  
 Si vous téléchargez ou publiez un rapport sans extraire auparavant le document (par exemple, en téléchargeant un élément qui a le même nom qu'un élément existant), le serveur de rapports l'extraira pour vous, ajoutera le rapport mis à jour comme nouvelle version de l'élément existant, puis archivera de nouveau le document.  
  
## <a name="external-images-as-resources"></a>Images externes en tant que ressources  
 Un serveur de rapports qui s'exécute en mode natif prend en charge le concept d'une ressource, qui est définie comme un fichier quelconque qui est stocké et sécurisé sur le serveur de rapports, mais qui n'est pas traité par le serveur de rapports. En mode natif, il peut s'agir d'un type quelconque de fichier.  
  
 Lorsqu'un serveur de rapports s'exécute en mode intégré SharePoint, le concept de ressource présente une définition moins large. Le serveur de rapports conserve le concept de ressource pour le stockage des rapports qui font référence à une image externe. Cela s'applique si le rapport est un instantané ou une copie conservée pour un usage interne.  
  
## <a name="see-also"></a> Voir aussi  
 [publier un rapport dans une bibliothèque SharePoint](../../reporting-services/reports/publish-a-report-to-a-sharepoint-library.md)   
 [Publier une source de données partagée sur une bibliothèque SharePoint](../../reporting-services/reports/publish-a-shared-data-source-to-a-sharepoint-library.md)   
 [Pages de propriétés du projet, boîte de dialogue](../../reporting-services/tools/project-property-pages-dialog-box.md)  
  
  
