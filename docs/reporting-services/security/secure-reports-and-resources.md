---
title: Sécuriser des rapports et des ressources | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- security [Reporting Services], reports
- security [Reporting Services], resources
- reports [Reporting Services], security
- confidential reports [Reporting Services]
- resources [Reporting Services], security
ms.assetid: 63cd55c7-fd2a-49e3-a3f8-59eb1a1c6e83
caps.latest.revision: 47
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f4a41d9238315719bc6e47fb3bc9c598222673a6
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34550770"
---
# <a name="secure-reports-and-resources"></a>Sécuriser des rapports et des ressources
  Vous pouvez définir la sécurité pour des rapports et des ressources individuels afin de contrôler le degré d'accès dont disposent les utilisateurs à ces éléments. Par défaut, seuls les utilisateurs qui sont membres du groupe intégré **Administrateurs** peuvent exécuter les rapports, afficher les ressources, modifier les propriétés et supprimer les éléments. Tous les autres utilisateurs possèdent des attributions de rôles créées pour eux qui autorisent l'accès à un rapport ou une ressource.  
  
## <a name="role-based-access-to-reports-and-resources"></a>Accès aux rapports et aux ressources en fonction des rôles  
 Pour accorder l'accès aux rapports et aux ressources, vous pouvez autoriser les utilisateurs à hériter des attributions de rôles existantes d'un dossier parent ou à créer une nouvelle attribution de rôle sur l'élément proprement dit.  
  
 Dans la plupart des cas, vous voudrez probablement utiliser les autorisations qui sont héritées d'un dossier parent. Il est nécessaire d'établir des paramètres de sécurité sur les ressources et les rapports uniquement si vous voulez masquer le rapport ou la ressource aux utilisateurs qui n'ont pas besoin de savoir que le rapport ou la ressource existe, ou pour augmenter le niveau d'accès d'un rapport ou d'un élément. Ces objectifs ne sont pas mutuellement exclusifs. Vous pouvez limiter l'accès à un rapport à un plus petit nombre d'utilisateurs, et leur fournir - à tous ou à certains - des privilèges étendus pour gérer le rapport.  
  
 Il est possible que vous deviez créer plusieurs attributions de rôles pour atteindre vos objectifs. Par exemple, supposons que vous ayez un rapport que vous vouliez rendre accessible à deux utilisateurs, Anne et Fernand, et au groupe des responsables des ressources humaines. Anne et Fernand doivent être en mesure de gérer le rapport, mais les membres responsables des ressources humaines ont uniquement besoin de l'exécuter. Pour répondre aux besoins de ces différents utilisateurs, vous devez créer trois attributions de rôles distinctes : une qui donne à Anne un rôle de gestionnaire de contenu du rapport, une qui donne à Fernand un rôle de gestionnaire de contenu du rapport et une qui permette au groupe des responsables des ressources humaines d'effectuer des tâches en affichage seul.  
  
 Une fois que vous avez défini la sécurité sur un rapport ou une ressource, ceux-ci conservent ces paramètres même si vous déplacez l'élément à un autre endroit. Par exemple, si vous déplacez un rapport auquel seules quelques personnes sont autorisées à accéder, le rapport continue à n'être accessible qu'à ces mêmes personnes même si vous le déplacez dans un dossier à stratégie de sécurité relativement ouverte.  
  
## <a name="mitigating-html-injection-attacks-in-a-published-report-or-document"></a>Limitation des attaques par injection HTML dans un document ou un rapport publié  
 Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], les rapports et les ressources sont traités sous l’identité de sécurité de l’utilisateur qui exécute le rapport. Si le rapport contient des expressions, un script, des éléments de rapports personnalisés ou des assemblys personnalisés, le code s'exécute sous les informations d'identification de l'utilisateur. Si une ressource est un document HTML qui contient un script, le script sera exécuté lorsque l'utilisateur ouvrira le document sur le serveur de rapports. La possibilité d'exécuter un script ou du code dans un rapport est une fonction puissante associée à un certain degré de risque. Si le code est malveillant, le serveur de rapports et l'utilisateur qui exécute le rapport sont vulnérables à une attaque.  
  
 Lorsque vous accordez l'accès à des rapports et des ressources qui sont traités en tant que code HTML, vous devez garder en mémoire le fait que les rapports sont traités en confiance totale et qu'un script potentiellement malveillant peut être envoyé au client. En fonction des paramètres du navigateur, le client exécutera le code HTML au niveau de confiance qui est spécifié dans le navigateur.  
  
 Vous pouvez limiter le risque d'exécution de script malveillant en prenant les précautions suivantes :  
  
-   Soyez sélectif lorsque vous décidez quelles sont les personnes pouvant publier du contenu sur un serveur de rapports. Dans la mesure où il existe un risque de publication de contenu malveillant, vous devez restreindre les utilisateurs pouvant publier du contenu à un petit nombre d'utilisateurs de confiance.  
  
-   Tous les éditeurs doivent éviter de publier des rapports et des ressources provenant de sources inconnues ou non approuvées. Si nécessaire, ouvrez le fichier dans un éditeur de texte et vérifiez qu'il ne contient pas d'URL et de script suspects.  
  
## <a name="report-parameters-and-script-injection"></a>Paramètres de rapport et injection de script  
 Les paramètres de rapport apportent une grande souplesse au niveau de la création et de l'exécution générale du rapport. Toutefois, cette souplesse peut, dans certains cas, être utilisée par un intrus au cours d'attaques par ruse. Pour réduire le risque d'exécution accidentelle de scripts malveillants, ouvrez les rapports rendus seulement à partir de sources approuvées. Il est recommandé de tenir compte du scénario suivant présentant une attaque potentielle par injection de script de convertisseur HTML :  
  
1.  Un rapport contient une zone de texte dont l'action du lien hypertexte est définie sur la valeur d'un paramètre comportant éventuellement un texte malveillant.  
  
2.  Le rapport est publié sur un serveur de rapports ou mis à disposition de telle façon que la valeur du paramètre de rapport est contrôlable par l'URL d'une page Web.  
  
3.  Un intrus crée un lien vers la page web ou le serveur de rapports en spécifiant la valeur du paramètre sous la forme « javascript:\<script malveillant ici> » et envoie ce lien à une autre personne en vue d’une attaque par ruse.  
  
## <a name="mitigating-script-injection-attacks-in-a-hyperlink-in-a-published-report-or-document"></a>Limitation des attaques par injection de script dans un lien hypertexte figurant dans un rapport ou un document publié  
 Les rapports peuvent contenir des liens hypertexte incorporés dans la valeur de la propriété Action sur un élément de rapport ou une partie d’un élément de rapport. Les liens hypertexte peuvent être liés aux données extraites d'une source de données externe au moment du traitement du rapport. Si un utilisateur mal intentionné modifie les données sous-jacentes, le lien hypertexte risque d'être utilisé pour écrire des exploits. Si un utilisateur clique sur le lien dans le rapport publié ou exporté, le script malveillant peut ensuite s'exécuter.  
  
 Pour limiter le risque d'insertion de liens dans un rapport qui, par inadvertance, exécuterait des scripts malveillants, n'associez des liens hypertexte qu'avec des données de sources fiables. Vérifiez que les données issues des résultats de la requête et des expressions qui lient des données aux liens hypertexte ne créent pas de liens susceptibles d'être utilisés frauduleusement. Ainsi, ne fondez pas un lien hypertexte sur une expression qui concatène des données à partir de plusieurs champs de dataset. Si nécessaire, accédez au rapport et utilisez la commande « Afficher la source » pour rechercher la présence éventuelle d'URL et de scripts suspects.  
  
## <a name="mitigating-sql-injection-attacks-in-a-parameterized-report"></a>Limitation des attaques par injection SQL dans un rapport paramétré  
 Dans un rapport qui inclut un paramètre de type **String**, veillez à utiliser une liste de valeurs disponibles (également appelée liste de valeurs valides) et vérifiez que l’utilisateur qui exécute le rapport dispose uniquement des autorisations nécessaires à l’affichage des données du rapport. Quand vous définissez un paramètre de type **Chaîne**, la zone de texte qui apparaît vous permet d’entrer n’importe quelle valeur. Une liste de valeurs disponibles limite les valeurs susceptibles d'être entrées. Si le paramètre de rapport est lié à un paramètre de requête et vous n'utilisez pas une liste de valeurs disponibles, l'utilisateur d'un rapport peut taper la syntaxe SQL dans la zone de texte, ce qui peut exposer le rapport et votre serveur de rapports au risque d'une attaque par injection SQL. Si l'utilisateur dispose d'autorisations suffisantes pour exécuter la nouvelle instruction SQL, cela risque de générer des résultats indésirables sur le serveur.  
  
 Si un paramètre de rapport n'est pas lié à un paramètre de requête et les valeurs de paramètre sont incluses dans le rapport, l'utilisateur d'un rapport peut taper la syntaxe de l'expression ou une URL dans la valeur de paramètre et rendre le rapport au format Excel ou HTML. Si un autre utilisateur affiche ensuite le rapport et clique sur le contenu du paramètre de rendu, celui-ci peut exécuter accidentellement le lien ou le script malveillant.  
  
 Pour réduire le risque d'exécution accidentelle de scripts malveillants, ouvrez les rapports rendus uniquement à partir de sources approuvées.  
  
> [!NOTE]  
>  Les versions précédentes de la documentation contenaient un exemple de création d'une requête dynamique en tant qu'expression. Ce type de requête crée une vulnérabilité aux attaques par injection SQL ; il est par conséquent déconseillé.  
  
## <a name="securing-confidential-reports"></a>Sécurisation des rapports confidentiels  
 Les rapports qui contiennent des informations confidentielles doivent être sécurisés au niveau de l'accès aux données, en exigeant des utilisateurs qu'ils s'identifient pour accéder à des données sensibles. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md). Vous pouvez également sécuriser un dossier pour le rendre inaccessible aux utilisateurs non autorisés. Pour plus d’informations, consultez [Dossiers sécurisés](../../reporting-services/security/secure-folders.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et gérer des attributions de rôles](../../reporting-services/security/create-and-manage-role-assignments.md)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Sécuriser les éléments de source de données partagée](../../reporting-services/security/secure-shared-data-source-items.md)   
 [Stocker des informations d’identification dans une source de données Reporting Services](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)  
  
  
