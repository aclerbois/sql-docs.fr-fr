---
title: Configurer et afficher SharePoint et la journalisation des Diagnostics | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9d36c65115f1ad786340ec8a4058bd20c52cb6a1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-and-view-sharepoint-and-diagnostic-logging"></a>Configurer et afficher SharePoint et la journalisation des Diagnostics
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] sont enregistrés dans les fichiers journaux SharePoint. Utilisez les informations de cette rubrique pour configurer les niveaux de journalisation et afficher les informations du fichier journal. Vous pouvez contrôler les événements du serveur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] qui sont enregistrés dans le fichier. Vous pouvez également contrôler la gravité des messages enregistrés. Pour plus d’informations, consultez [Configurer la collecte des données d’utilisation &#40;PowerPivot pour SharePoint)](../../analysis-services/power-pivot-sharepoint/configure-usage-data-collection-for-power-pivot-for-sharepoint.md).  
  
 Dans cette rubrique :  
  
-   [Emplacement du fichier journal](#bkmk_filelocation)  
  
-   [Modifier les niveaux de journalisation des diagnostics pour des catégories d'événements](#bkmk_modifyloglevels)  
  
-   [Procédure d'affichage des fichiers journaux SharePoint](#bkmk_how2viewlogfiles)  
  
##  <a name="bkmk_filelocation"></a> Emplacement du fichier journal  
 Par défaut, les fichiers journaux SharePoint sont enregistrés à l'emplacement suivant :  
  
 `C:\Program files\Common Files\Microsoft Shared\Web Server Extensions\14\LOGS`  
  
 Le dossier LOGS contient des fichiers journaux (`.log`), des fichiers de données (`.txt`) et des fichiers d'utilisation (`.usage`). La convention d'affectation des noms de fichier pour un journal des traces SharePoint est le nom du serveur suivi de la date et de l'heure. Les journaux des traces SharePoint sont créés à intervalles réguliers et à chaque occurrence d'IISRESET. Le nombre journaux de traces créés sur une période de 24 heures est généralement élevé.  
  
##  <a name="bkmk_modifyloglevels"></a> Modifier les niveaux de journalisation des diagnostics pour des catégories d'événements  
 Par défaut, la journalisation ULS des événements [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] est définie sur *Moyenne*. Ce paramètre est nouveau pour SQL Server 2012. Si vous avez mis à niveau un serveur à partir de la version antérieure, le niveau de journalisation peut toujours être défini sur *Commentaires*, le niveau par défaut dans SQL Server 2008 R2. Si vous êtes habitué à consulter les journaux ULS pour examiner les informations d’utilisation du serveur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] , vous noterez qu’en raison de cette modification, il y a moins d’informations sur les opérations de serveur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] .  
  
 Sauf pour les exceptions, qui sont de type *Élevé*, tous les messages [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] sont classés dans la catégorie Commentaires. Si vous souhaitez enregistrer les entrées de journal pour les opérations courantes de serveur, telles que les connexions, les requêtes ou la création de rapports de requête, vous devez remplacer le niveau de journalisation par Commentaires.  
  
 Pour modifier les niveaux de journalisation des diagnostics pour des catégories d'événements :  
  
1.  Dans l'Administration centrale de SharePoint, cliquez sur **Analyse**.  
  
2.  Cliquez sur **Configurer la journalisation des diagnostics**.  
  
3.  Accédez à **[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] Service**.  
  
4.  Développez la catégorie et sélectionnez des catégories.  
  
     **Demande de page d’application** spécifie les événements déclenchés par l’application de service lors de la recherche d’un [!INCLUDE[ssGeminiSrv_md](../../includes/ssgeminisrv-md.md)] pour le chargement d'une source de données [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] et la communication avec d’autres serveurs de la batterie.  
  
     **Traitement des demandes** spécifie les événements déclenchés par des requêtes sur une base de données [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] chargée sur un serveur de la batterie.  
  
     **Utilisation** spécifie un événement lié à la collecte des données d’utilisation [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] .  
  
5.  Dans Événement le moins critique à enregistrer dans le journal d'événements, sélectionnez **Aucun** pour désactiver la journalisation pour la catégorie, ou **Erreur** pour limiter la journalisation aux erreurs exclusivement.  
  
6.  Sélectionnez **Commentaires** pour enregistrer tous les événements dans le journal des événements.  
  
7.  Dans Événement le moins critique à enregistrer dans le journal des traces, sélectionnez **Aucun** pour désactiver le suivi pour la catégorie, ou **Erreur** pour limiter le suivi aux erreurs exclusivement.  
  
8.  Sélectionnez **Commentaires** pour enregistrer tous les événements dans le journal des traces.  
  
9. Cliquez sur **OK**.  
  
##  <a name="bkmk_how2viewlogfiles"></a> Procédure d'affichage des fichiers journaux SharePoint  
 Les fichiers journaux sont des fichiers texte. Vous pouvez les ouvrir dans n'importe quel éditeur de texte. Vous pouvez également utiliser des applications de visionneuse de journaux tierces.  
  
#### <a name="use-a-text-editor"></a>Utiliser un éditeur de texte  
 Si vous utilisez un éditeur de texte pour dépanner une erreur de serveur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] , les conseils suivants peuvent vous aider à trouver les informations pertinentes dans le fichier :  
  
-   Pour les erreurs de publication, d'affichage ou d'actualisation d'un classeur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] , recherchez le nom du classeur dans le fichier journal.  
  
-   Pour les erreurs qui fournissent un ID de corrélation, copiez l'ID et utilisez-le comme un terme recherché dans le fichier journal.  
  
-   Recherchez le statut d'erreur « Élevé » ou « Exception ». Recherche de « Service[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] ».  
  
-   Si vous savez à quel moment l'erreur s'est produite, utilisez les informations d'horodatage pour limiter l'étendue des entrées à parcourir.  
  
#### <a name="use-a-log-viewer-application"></a>Utiliser une application de visionneuse de journaux  
 Bien que vous puissiez utiliser un éditeur de texte pour consulter les journaux de traces individuellement, une application de visionneuse de journaux qui permet de consulter plusieurs fichiers journaux à la fois peut être plus utile. Heureusement, plusieurs applications de visionneuse de journaux tierces disponibles en téléchargement sur le site Codeplex peuvent vous aider à consulter plusieurs journaux de traces dans un seul espace de travail.  
  
 Les instructions suivantes incluent des liens vers des visionneuses de journaux ULS SharePoint populaires que vous pouvez télécharger à partir de Codeplex.  
  
1.  Accédez à la page [Visionneuse de journaux SharePoint](http://sharepointlogviewer.codeplex.com) ou [Visionneuse de journaux ULS SharePoint](http://go.microsoft.com/fwlink/?LinkId=150052) sur le site Codeplex.  
  
2.  Cliquez sur l'onglet **Downloads** (Téléchargements).  
  
3.  Cliquez sur le fichier exécutable. Il s'agit de **SharePointLogViewer.exe** ou de **ULS Viewer 2.0 Binary**.  
  
4.  Lisez et acceptez les termes du contrat de licence.  
  
5.  Cliquez sur **Save** (Enregistrer) pour télécharger le logiciel sur votre système local.  
  
     Si vous téléchargez la visionneuse de journaux ULS SharePoint, enregistrez ULSViewer.zip dans le dossier Téléchargements.  
  
6.  Dans le dossier Téléchargements, cliquez avec le bouton droit sur ULSViewer.zip et sélectionnez **Extraire tout**.  
  
7.  Spécifiez un dossier de destination, puis cliquez sur **Extraire**.  
  
8.  Dans le dossier qui contient les fichiers programme extraits, double-cliquez sur **ULSViewer** et exécutez l’application.  
  
9. Cliquez sur l'icône de dossier dans l'angle supérieur gauche de la fenêtre d'application.  
  
10. Accédez à \Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\Logs.  
  
11. Sélectionnez un ou plusieurs journaux des traces. Par défaut, les noms des fichiers journaux des traces contiennent le nom de l'ordinateur et des informations d'horodatage. Le type de fichier est Document texte.  
  
12. Cliquez sur **Ouvrir** et attendez que les fichiers soient chargés.  
  
13. Utilisez les filtres intégrés pour sélectionner des enregistrements en fonction de la gravité, du processus, de la catégorie ou d'un fichier texte défini par l'utilisateur. Vous pouvez également cliquer sur les en-têtes de colonne pour modifier l'ordre de tri.  
  
#### <a name="entries-for-power-pivot-services"></a>Entrées pour les services Power Pivot  
 Le tableau suivant décrit les entrées d'opérations de serveur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] susceptibles de se trouver dans un fichier journal SharePoint.  
  
|Traiter|Domaine|Catégorie|Niveau|Boîte de|Détails|  
|-------------|----------|--------------|-----------|-------------|-------------|  
|w3wp.exe|[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] |Utilisation|Commentaires|There are no current request statistics, nothing to log.|À intervalles prédéfinis, le service fournit les statistiques de réponse aux requêtes en tant qu'événement d'utilisation au système de collecte des données d'utilisation. Ce message indique qu'il n'existe aucune statistique sur les requêtes à signaler.|  
|w3wp.exe|[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] Service|Web frontal|Commentaires|Démarrage de localiser un serveur d’applications pour la source de données =\<*chemin d’accès*>|Lorsqu'il reçoit une demande de connexion, le service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] identifie un [!INCLUDE[ssGeminiSrv_md](../../includes/ssgeminisrv-md.md)] disponible pour gérer la demande. Si la batterie de serveurs ne contient qu'un seul serveur, le serveur local accepte toujours la demande.|  
|w3wp.exe|[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] Service|Web frontal|Commentaires|Locating the application server succeeded.|La requête a été allouée à une application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] .|  
|w3wp.exe|[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] Service|Web frontal|Commentaires|Demande de redirection pour la \< *PowerPivotdata source*> à la [!INCLUDE[ssGeminiSrv_md](../../includes/ssgeminisrv-md.md)].|La demande a été envoyée au [!INCLUDE[ssGeminiSrv_md](../../includes/ssgeminisrv-md.md)].|  
|w3wp.exe|[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] |Traitement des demandes|Commentaires|Demande de redirection pour le nom d’utilisateur\<*utilisateur SharePoint*> à la base de données|Une connexion avec emprunt d'identité à la source de données [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] a été créée au nom de l'utilisateur SharePoint.|  
  
## <a name="see-also"></a>Voir aussi  
 [Collecte des données d’utilisation Power Pivot](../../analysis-services/power-pivot-sharepoint/power-pivot-usage-data-collection.md)   
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Configurer la collecte des données d’utilisation &#40;PowerPivot pour SharePoint](../../analysis-services/power-pivot-sharepoint/configure-usage-data-collection-for-power-pivot-for-sharepoint.md)  
  
  
