---
title: Partager des flux de données à l’aide d’une bibliothèque de flux de données (PowerPivot pour SharePoint) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f262fe19ba39f217135908173189bf93a7e68817
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="share-data-feeds-using-a-data-feed-library-power-pivot-for-sharepoint"></a>Partager des flux de données à l’aide d’une bibliothèque de flux de données (Power Pivot pour SharePoint)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un flux de données est un flux de données XML généré à partir d'un service ou d'une application qui expose des données au format câble Atom. Son utilisation pour transporter des données entre applications et vers des visionneuses côté client est de plus en plus répandue. Dans un déploiement [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, les flux de données sont utilisés pour remplir une source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] avec les données d’une application ou d’un service prenant en charge Atom.  
  
 Si vous utilisez déjà une combinaison d'applications compatibles Atom, il ne vous sera peut-être jamais nécessaire de savoir comment les flux sont générés et consommés, car le transfert de données entre applications est transparent. Toutefois, les organisations qui utilisent des solutions personnalisées pour publier des flux Atom ont souvent besoin d'un moyen de rendre ces sources disponibles pour les travailleurs de l'information. L'une des méthodes pour y parvenir consiste à créer et partager des fichiers de document de service de données (.atomsvc) qui fournissent des connexions aux sources en ligne qui produisent les flux. Une bibliothèque spéciale, appelée « bibliothèque de source de données », prend en charge la création et le partage des documents de service de données dans une application Web SharePoint.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Conditions préalables](#prereq)  
  
 [Créer un document de service de données](#createdsdoc)  
  
 [Sécuriser un document de service de données](#securedsdoc)  
  
 [Modifier un document de service de données](#modifydsdoc)  
  
 [Étape suivante : utiliser un document de service de données](#usedsdoc)  
  
> [!NOTE]  
>  Bien que les flux de données soient utilisés pour ajouter des données web à une source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] que vous créez dans [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)], toute application cliente qui peut lire un flux Atom peut traiter un document de service de données.  
  
##  <a name="prereq"></a> Configuration requise  
 Vous devez disposer d’un déploiement de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint qui ajoute le traitement de requêtes [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] à une batterie de serveurs SharePoint. La prise en charge des flux de données est déployée via le package de solution [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Vous devez posséder une bibliothèque SharePoint qui prend en charge le type de contenu du document de service de données. Il est à cet effet recommandé d'utiliser une bibliothèque de source de données par défaut, mais il est aussi possible d'ajouter manuellement le type de contenu à n'importe quelle bibliothèque. Pour plus d’informations, consultez [Créer ou personnaliser une bibliothèque de flux de données &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/create-or-customize-a-data-feed-library-power-pivot-for-sharepoint.md).  
  
 Vous devez disposer d'un service de données ou d'une source de données en ligne qui fournit des données tabulaires XML au format Atom 1.0.  
  
 Vous devez disposer d'autorisations Collaboration sur un site SharePoint pour créer ou gérer un document de service de données dans une bibliothèque SharePoint.  
  
##  <a name="createdsdoc"></a> Créer un document de service de données  
 Un document de service de données est une requête active qui permet, à la demande, de transmettre en continu des données à partir d'une source de données ou application en ligne fournissant des données dans un format de flux. Lorsque vous créez un document de service de données, vous spécifiez un pointeur vers un ou plusieurs services de données adressables par URL qui fournissent des données tabulaires XML au format syndiqué Atom.  
  
 Un même document peut spécifier plusieurs flux de données. Cela peut être utile si vous souhaitez récupérer un jeu de charges utiles de données du même service, voire de différents services, en une seule opération d'importation.  
  
1.  Sur un site SharePoint, ouvrez la bibliothèque de source de données ou une autre bibliothèque de documents dans laquelle vous avez ajouté et configuré le type de contenu du service de données. Pour rechercher une bibliothèque de source de données précédemment créée, cliquez sur **Afficher tout** dans Lancement rapide.  
  
2.  Sur le ruban du haut de la page, dans les Outils de document, cliquez sur **Documents**.  
  
3.  Cliquez sur **Nouveau document** , puis sélectionnez **Document de service de données**.  
  
4.  Dans la page Nouveau document de service de données, entrez les informations suivantes :  
  
    1.  Nom et description du document de service de données. Veillez à fournir un niveau de détail suffisant pour que les utilisateurs puissent décider d'utiliser ou non le flux.  
  
    2.  Dans Source de données, entrez l'URL d'un service de données ou d'une application Web qui fournit des données au format Atom 1.0.  
  
         L'URL doit être résolue en un service qui retourne des données structurées ou semi-structurées en lignes et colonnes. Ce service doit retourner des données de façon anonyme ou via les informations d'identification de sécurité de l'utilisateur actuel.  
  
         L'URL doit être résolue en un service qui prend en charge l'authentification Windows, l'authentification de base ou l'accès anonyme. L'utilisateur qui importe le flux spécifie la méthode à utiliser. La sécurité intégrée est sélectionnée par défaut.  
  
         Une URL de flux de données peut inclure des paramètres. Différents types de technologies de service des données prennent en charge des modèles d'adressage par URL avancés qui vous permettent de sélectionner précisément les données à utiliser. Par exemple, un service de données ADO.NET fournit des paramètres d'URL pour la spécification des entités, associations et chemins de navigation dans les données sous-jacentes. En spécifiant une URL complexe comme source d'un flux de données, vous pouvez spécifier précisément le dataset à utiliser.  
  
    3.  Pour le même flux de données, entrez un nom de table qui identifiera par la suite le dataset dans une application cliente. Dans [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)], chaque flux de données que vous importez est placé dans son propre contrôle de table dans une source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Vous devez spécifier le nom de la table qui reçoit les données importées lorsque vous configurez le flux de données.  
  
5.  Cliquez sur Ajouter une autre source de données pour répéter les étapes précédentes afin de spécifier d'autres flux du même service ou d'un autre service.  
  
     Chaque document de service de données est traité au cours d'une même opération. Tous les flux de données du document seront générés de façon asynchrone et retournés à une application cliente dans la même opération. Il convient par conséquent de ne spécifier que les paires URL/table des flux de données que vous souhaitez utiliser ensemble.  
  
     Étant donné que les schémas d'authentification sont définis au niveau du document de service de données, chaque flux de données supplémentaire doit provenir d'un service ou d'une application qui prend en charge le même schéma d'authentification que le premier flux. Tous les flux d'un document de service de données seront authentifiés à l'aide de la même méthode au moment de l'exécution.  
  
6.  Enregistrez le document. Le document de service de données est stocké sous la forme d'un fichier physique (.atomsvc) dans une bibliothèque de contenu configurée pour ce type de contenu.  
  
 Pour utiliser le document de service de données, vous pouvez ouvrir un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)] et choisir l'option **À partir de flux de données** dans l'Assistant Importer des données. Lorsqu'il y sera invité, un utilisateur spécifiera l'URL SharePoint du document de service de données pour démarrer une opération d'importation de données. Pour plus d’informations, consultez [Utiliser des flux de données &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/use-data-feeds-power-pivot-for-sharepoint.md).  
  
##  <a name="securedsdoc"></a> Sécuriser un document de service de données  
 Un document de service de données hérite des autorisations de la bibliothèque qui le contient. Les autorisations que vous définissez sur l'élément déterminent si un utilisateur peut ouvrir, modifier ou supprimer le document de service de données.  
  
 Pour utiliser un document de service de données en tant qu’importation de flux de données dans l’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , les autorisations d’affichage sur le document sont les seules qui soient nécessaires à l’utilisateur. Les autorisations d'affichage sont suffisantes pour résoudre l'URL dans l'Assistant Importation.  
  
 Les autorisations d'affichage sur un document de service de données sont vérifiées uniquement lorsqu'une opération d'importation de flux de données commence. Après importation, les autorisations sur le document ne sont pas vérifiées en continu ; les flux ajoutés à une source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] existent sous forme de données statiques, déconnectées du document de service de données qui a fourni les informations de connexion d’origine.  
  
 De même, les opérations d'actualisation de données que vous planifiez par la suite excluent également le document de service de données. Au moment de l’importation, les informations de connexion de chaque flux sont copiées dans la source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] à des fins d’actualisation. En tant que telles, les autorisations sur un document de service de données ne sont pas vérifiées pour l'actualisation des données, car le document lui-même n'est jamais référencé dans une opération d'actualisation.  
  
|Tâche|Spécifications relatives aux autorisations SharePoint|  
|----------|----------------------------------------|  
|Importer des flux de données dans un classeur compatible [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].|Autorisations d'affichage sur le document de service de données dans une bibliothèque.|  
|Dans l’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , actualiser des données précédemment récupérées au moyen d’un flux|Non applicable. L’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] utilise les informations de connexion HTTP incorporées pour se connecter directement aux services et applications de données qui fournissent le flux. L’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’utilise pas le document de service de données.|  
|Dans une batterie de serveurs SharePoint, actualiser des données dans le cadre d'une tâche planifiée, sans intervention requise de l'utilisateur|Non applicable. Le service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] utilise les informations de connexion HTTP incorporées pour se connecter directement aux services et applications de données qui fournissent le flux. Le service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’utilise pas le document de service de données.|  
|Supprimer un document de service de données dans une bibliothèque|Autorisations Collaboration sur la bibliothèque.|  
  
##  <a name="modifydsdoc"></a> Modifier un document de service de données  
 Vous pouvez ajouter, modifier ou supprimer des entrées URL/table individuelles dans un document de service de données. Une fois que vous avez enregistré vos modifications, les utilisateurs qui sélectionnent le document de service dans une nouvelle opération d'importation obtiennent les flux de données que vous avez spécifiés.  
  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui ont utilisé une version précédente du document ne sont pas affectés par les modifications que vous apportez. En effet, un document de service de données est lu une seule fois au cours de l'opération d'importation initiale. Pendant l'importation, les URL de service et noms de tables sont copiés et stockés en interne dans le classeur. Ces valeurs internes sont ensuite utilisées dans les opérations d'actualisation suivantes pour obtenir des données mises à jour.  
  
 Étant donné qu'il n'existe pas de lien persistant entre un document de service de données situé sur un site SharePoint et le classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui contient le flux importé, la modification d'une partie quelconque d'un document de service de données est sans effet sur les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] existants.  
  
> [!IMPORTANT]  
>  Bien que le document de service de données ne soit lu qu'une seule fois, les services de données qui fournissent les données réelles peuvent faire l'objet d'accès à intervalles réguliers pour obtenir des flux plus récents. Pour plus d’informations sur l’actualisation des données, consultez [Actualisation des données Power Pivot](../../analysis-services/power-pivot-sharepoint/power-pivot-data-refresh.md).  
  
##  <a name="usedsdoc"></a> Étape suivante : utiliser un document de service de données  
 Pour utiliser un document de service de données que vous avez créé dans une bibliothèque SharePoint, utilisez l’option d’importation **À partir de flux de données** dans une source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Pour obtenir des instructions, consultez [Utiliser des flux de données &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/use-data-feeds-power-pivot-for-sharepoint.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Flux de données Power Pivot](../../analysis-services/power-pivot-sharepoint/power-pivot-data-feeds.md)  
  
  
